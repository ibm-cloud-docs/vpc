---

copyright:
  years: 2021
lastupdated: "2021-05-05"

keywords: snapshots, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance
subcollection: vpc

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Creating Snapshots
{: #snapshots-vpc-create}

Using the UI, CLI, or API, you can create a snapshot of a {{site.data.keyword.block_storage_is_short}} volume that is attached to a virtual server instance. You can create a snapshot of a boot or data volume.
{:shortdesc}

Before you take a snapshot, make sure all cached data is present on disk. This applies to instances with Windows and Linux operating systems. For example, on Linux operating systems, run the `sync` command to force an immediate write of all cached data to disk.
{:note}

## Create a snapshot by using the UI
{: #snapshots-vpc-create-ui}
{:ui}

In the UI, you can create a snapshot of a {{site.data.keyword.block_storage_is_short}} volume that's attached to a running virtual server instance. 

The snapshots UI is available only in these regions:  Sydney (au-syd), and Japan (jp-osa).

### Create a snapshot from the list of snapshots
{: #snapshots-vpc-create-from-list}

Follow these steps to create a snapshot from the list of snapshots.

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**.

  Be sure to select VPC infrastructure from the Menu icon. If necessary, click the *Switch to Gen 2 compute* link in the top banner to ensure you are creating generation 2 resources.  
  {: tip}

1. From the list of snapshots (initially empty), click **Create**.

1. Enter the information in Table 1 to define your snapshot and select the block storage volume you'll be copying.

1. Click **Create snapshot**. You're returned to the list of snapshots. Messages display while snapshot is being created and when ready, the snapshot displays first in the list of snapshots. You can then [view details of your snapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).

| Field | Value |
|-------|-------|
| Name  | Provide a unique name for the snapshot. The UI verifies the name for proper conventions and identifies duplicate names. For suggestions about how to name your snapshots, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-naming). |
| Resource group | Select a [resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups) for the snapshot, or use the default. You can't change the resource group after the snapshot is created. |
| Block storage volume | Select a volume from the drop down list. The boot or data volume must be attached to a running virtual server instance. |
| Region | The default region for the volume that you selected. |
| Encryption | Encryption information for the volume you selected, either IBM-managed or customer-managed. The snapshot inherits the encryption of the source volume. You can't change the encryption type.
{: caption="Table 1. Selections for creating a snapshot" caption-side="top"}

### Create a snapshot from the list of block storage volumes
{: #snapshots-vpc-create-from-volume-list}

Follow these steps to create a snapshot from the list of block storage volumes. 

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.

  Be sure to select VPC infrastructure from the Menu icon. If necessary, click the *Switch to Gen 2 compute* link in the top banner to ensure you are creating generation 2 resources.  
  {: tip}

1. From the list of volumes, locate a boot or data volume attached to an instance.

1. Click the overflow menu (...) and select  **Create snapshot**.

1.  On the snapshots list page, messages display while snapshot is being created. When ready, the snapshot displays first in the list of snapshots. You can then [view details of your snapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).


## Create a snapshot using the CLI
{: #snapshots-vpc-create-cli}
{:cli}

### Gathering information to create a snapshot using the CLI
{: #snapshots-vpc-getinfo-cli}

Before you run the `ibmcloud is snapshots-create` command, you can view the details about the volume and instance.

Gather the following information:

|     Details   |  Listing options  | What it provides  |
| --------------------- | --------------------------------|---------------------|
| Volume | `ibmcloud is volumes` | Locate a volume from the list of volumes, verify the volume type, and whether it's attached to a instance. |
| Volume VOLUME_ID  | `ibmcloud is volumes VOLUME_ID` | Review details of a volume. |
| Instances | `ibmcloud is instances` | List all instances. |
| Instance INSTANCE_ID  | `ibmcloud is instance INSTANCE_ID` | View details of an instance to see attached boot and data volumes. |
{: caption="Table 1. Details for creating snapshots" caption-side="top"}  

### Procedure for creating a snapshot using the CLI
{: #snapshots-vpc-create-procedure-cli}

1. Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

2. To use the CLI, set the `IBMCLOUD_IS_FEATURE_SNAPSHOT` environment variable to `true`. Copy the following code:

   ```
   export IBMCLOUD_IS_FEATURE_SNAPSHOT=true
   ```
   {:pre}

3. After you install the vpc-infrastructure plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.
   
4. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli).

5. Run the `snapshot-create` command to create a snapshot. 

```
ibmcloud is snapshot-create --volume VOLUME_ID [--name NAME] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
```
{:pre}

Example:

```
$ ibmcloud is snapshot-create --name demo-snapshot1 --volume c3f9ffa4-6609-4750-ad09-e8caea5d9e5c
Creating snapshot demo-snapshot-1 in resource group under account VPC 01 as user rtuser1@mycompany.com...

ID                 b40ecbfa-296b-4592-b959-59459868d683   
Name               my-snapshot1   
CRN                crn:v1:public:is:us-south:a/23db6395-3466-4055-ada1-c072b6b749bf::
                   snapshot:b40ecbfa-296b-4592-b959-59459868d683   
Status             pending   
Source Volume      ID                                          Name      
                   c3f9ffa4-6609-4750-ad09-e8caea5d9e5c        demo-volume1      
                      
Progress           0   
Bootable           false   
Deletable          false   
Encryption         provider_managed   
Encryption key     -   
Minimum Capacity   100   
Size               1   
Source Image       ID                                          Name      
                   c348a188-bc70-4c08-afb7-cbcbde831be3        ibm-centos-7-6-minimal-amd64-2      
                      
Resource group     ID                                          Name      
                   64e81667-75d8-4803-9935-fb0ee5895c04        Default      
                      
Created            2021-02-22T16:18:56+08:00   
```
{:screen}

## Create a snapshot from the API
{: #snapshots-vpc-create-api}
{:api}

You can create a snapshot by calling the [VPC API](https://{DomainName}/apidocs/vpc).

Make a `POST/snapshots` request to create a snapshot of a boot or data volume. The following example creates a snapshot of a boot volume by using the volume ID.

```
curl -X POST \
"$vpc_api_endpoint/v1/snapshots?version=2020-10-12&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "name": "boot-snapshot-1",
      "source_volume": {
        "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8"
      },
      "resource_group": {
        "id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4"
      }
    }'
```
{:codeblock}

A successful response will look like this. The snapshot lifecycle state is `pending` while the snapshot is being created. When successfully created, the status changes to `stable`.

```
{
  "id": "r134-b27aa600-c372-476b-9ecd-b8c7e03818e7",
  "crn": "crn:[...]",
  "href": "https://cloud.ibm.com/v1/snapshots/r134-b27aa600-c372-476b-9ecd-b8c7e03818e7",
  "name": "boot-snapshot-1",
  "progress": 0,
  "resource_group": {
    "id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4",
    "crn": "crn:[...]",
    "href": "https://resource-controller.cloud.ibm.com/v2/
    resource_groups/a342dbfb-3ea7-48d1-96e8-2825ec5feab4",
    "name": "Default"
  },
  "encryption": "provider_managed",
  "encryption_key": null,
  "source_volume": {
    "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8",
    "crn": "crn:[...]",
    "href": "https://cloud.ibm.com/v1/volumes/8948ad59-bc0f-7510-812f-5dc64f59fab8",
    "name": "my-boot-volume-1"
  },
  "created_at": "2021-02-22T16:54:33Z",
  "lifecycle_state": "pending",
  "source_image": {
    "id": "72b27b5c-f4b0-48bb-b954-5becc7c1dcb8"
  },
  "size": 0,
  "deletable": false,
  "resource_type": "snapshot"
}
```
{:codeblock}

## Next steps
{: #snapshots_vpc_create_next_steps}

[View](/docs/vpc?topic=vpc-snapshots-vpc-view) all snapshots you created, or details about a single snapshot.

[Restore a volume](/docs/vpc?topic=vpc-snapshots-vpc-restore) from a snapshot.
