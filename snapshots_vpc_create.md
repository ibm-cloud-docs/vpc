---

copyright:
  years: 2021, 2022
lastupdated: "2022-12-09"

keywords:
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating Snapshots
{: #snapshots-vpc-create}

With the UI, CLI, or API, you can create a snapshot of a {{site.data.keyword.block_storage_is_short}} volume that is attached to a virtual server instance. You can create a snapshot of a boot or data volume.
{: shortdesc}

Before you take a snapshot, make sure that all cached data is present on disk - which applies to instances with Windows and Linux&reg; operating systems. For example, on Linux&reg; operating systems, run the `sync` command to force an immediate write of all cached data to disk.
{: note}

## Create a snapshot in the UI
{: #snapshots-vpc-create-ui}
{: ui}

In the UI, you can create a snapshot of a {{site.data.keyword.block_storage_is_short}} volume that you attached to a running virtual server instance.

### Create a snapshot from the list of snapshots
{: #snapshots-vpc-create-from-list}

Follow these steps to create a snapshot from the list of snapshots.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**.

2. From the list of snapshots (initially empty), click **Create**.

3. Enter the information in Table 1 to define your snapshot and select the {{site.data.keyword.block_storage_is_short}} volume that you want to copy.

4. Click **Create snapshot**. You're returned to the list of snapshots. Messages display while snapshot is being created and when ready, the snapshot displays first in the list of snapshots. You can then [view details of your snapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).

| Field | Value |
|-------|-------|
| Location | Specify the geography and region for this snapshot. |
| Name  | Provide a unique name for the snapshot. The UI verifies the name for proper conventions and identifies duplicate names. For suggestions about how to name your snapshots, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-naming). |
| Resource group | Select a [resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups) for the snapshot, or use the default. You can't change the resource group after the snapshot is created. |
| Tags | Specify any [user tags](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#apply-tags-volumes-ui) you want to identify this resource. |
| Access management tags | Specify any [access management tags](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#storage-add-access-mgt-tags) for this resource. |
| Block storage volume | Select a volume from the drop-down list. The boot or data volume must be attached to a running virtual server instance. |
| Encryption | Encryption information for the volume that you selected, either [provider-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-provider-managed-encryption) or [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-customer-managed-encryption). The snapshot inherits the encryption of the source volume. You can't change the encryption type.
{: caption="Table 1. Selections for creating a snapshot" caption-side="bottom"}

### Create a snapshot from the list of {{site.data.keyword.block_storage_is_short}} volumes
{: #snapshots-vpc-create-from-volume-list}

Follow these steps to create a snapshot from the list of {{site.data.keyword.block_storage_is_short}} volumes. The volume must be attached to a virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.

2. From the list of volumes, locate a boot or data volume that is attached to an instance.

3. Click the overflow menu (...) and select **Create snapshot**.

4. On the snapshots list page, messages display while snapshot is being created. When ready, the snapshot displays first in the list of snapshots. You can then [view details of your snapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).

### Create a snapshot from an attached {{site.data.keyword.block_storage_is_short}} volume
{: #snapshots-vpc-create-from-vol-details}

For a volume that's attached to an instance, you can create a snapshot from the volume details.

1. Go to the volume details page in either of these ways:

    * From the [instance details page](#view-vol-details-instance-ui), scroll to the list of attached volumes and click the link.
    * From the [list of block storage volumes](/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui), select a volume attached to an instance.

2. On the volume details page, select **Create snapshot** from the **Actions** menu.

3. Provide a snapshot name and optionally, a resource group and user tags.

4. Under **Volume**, click **Edit** to select a volume form the list that displays, then click **Save**. A summary displays in the side panel.

5. Click **Create block storage snapshot**. A message indicates the snapshot is being created and you're returned to the volume details page.

6. To view the snapshot, click the **Backups and Snapshots** tab.

## Create a snapshot from the CLI
{: #snapshots-vpc-create-cli}
{: cli}

### Gather information to create a snapshot from the CLI
{: #snapshots-vpc-getinfo-cli}

Before you run the `ibmcloud is snapshots-create` command, you can view the details about the volume and instance.

Gather the following information:

|     Details   |  Listing options  | What it provides  |
| --------------------- | --------------------------------|---------------------|
| Volume | `ibmcloud is volumes` | Locate a volume from the list of volumes, verify the volume type, and whether it was attached to an instance. |
| Volume VOLUME_ID  | `ibmcloud is volumes VOLUME_ID` | Review details of a volume. |
| Instances | `ibmcloud is instances` | List all instances. |
| Instance INSTANCE_ID  | `ibmcloud is instance INSTANCE_ID` | View details of an instance to see attached boot and data volumes. |
{: caption="Table 1. Details for creating snapshots" caption-side="bottom"}

### Create a snapshot from the CLI
{: #snapshots-vpc-create-procedure-cli}

1. Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

2. To use the CLI, set the `IBMCLOUD_IS_FEATURE_SNAPSHOT` environment variable to `true`. Copy the following code:

   ```zsh
   export IBMCLOUD_IS_FEATURE_SNAPSHOT=true
   ```
   {: pre}

3. After you install the vpc-infrastructure plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.

4. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli).

5. Run the `snapshot-create` command to create a snapshot.

```zsh
ibmcloud is snapshot-create --volume VOLUME_ID [--name NAME] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--tags  TAG_NAME1,TAG_NAME2,...] [--output JSON] [-q, --quiet]
```
{: pre}

Example:

```zsh
$ ibmcloud is snapshot-create --name demo-snapshot1 --volume c3f9ffa4-6609-4750-ad09-e8caea5d9e5c --tags env:test,env:prod
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
Encryption         provider_managed
Encryption key     -
Minimum Capacity   100
Size               1
Source Image       ID                                          Name
                   c348a188-bc70-4c08-afb7-cbcbde831be3        ibm-centos-7-6-minimal-amd64-2

Resource group     ID                                          Name
                   64e81667-75d8-4803-9935-fb0ee5895c04        Default

Created            2022-12-09T16:18:56+08:00
Tags               env:test,env:prod
```
{: screen}

## Create a snapshot with the API
{: #snapshots-vpc-create-api}
{: api}

You can create a snapshot by calling the [VPC API](/apidocs/vpc).

Make a `POST/snapshots` request to create a snapshot of a boot or data volume. The following example creates a snapshot of a boot volume by using the volume ID, and specifies user tags that can be associated with a [backup policy](/docs/vpc?topic=vpc-backup-service-about).

```curl
curl -X POST \
"$vpc_api_endpoint/v1/snapshots?version=2022-12-09&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "name": "boot-snapshot-1",
      "source_volume": {
        "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8"
      },
      "resource_group": {
        "id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4"
      },
      "user_tags": [
         "env:test",
         "env:prod"
      ]
    }'
```
{: codeblock}

A successful response looks like the following example. The snapshot lifecycle state is `pending` while the snapshot is created. When successfully created, the status changes to `stable`.

```json
{
  "bootable": true,
  "created_at": "2022-12-09T20:18:18Z",
  "crn": "crn:[...]",
  "deletable": false,
  "encryption": "user_managed",
  "encryption_key": {
    "crn": "crn:[...]"
  },
  "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/12917904-3771-424d-8391-53ec9e305d52",
  "id": "12917904-3771-424d-8391-53ec9e305d52",
  "lifecycle_state": "pending",
  "minimum_capacity": 100,
  "name": "boot-snapshot1",
  "operating_system": {
    "architecture": "amd64",
    "dedicated_host_only": false,
    "display_name": "Ubuntu Linux&reg; 20.04 LTS Focal Fossa Minimal Install (amd64)",
    "family": "Ubuntu Linux",
    "gpu_supported": [],
    "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/ubuntu-20-04-amd64",
    "name": "ubuntu-20-04-amd64",
    "resource_type": "operating_system",
    "vendor": "Canonical",
    "version": "20.04 LTS Focal Fossa Minimal Install"
  },
  "resource_group": {
    "crn": "crn:[...]",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/a342dbfb-3ea7-48d1-96e8-2825ec5feab4",
    "id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4",
    "name": "Default"
  },
  "resource_type": "snapshot",
  "service_tags": [],
  "size": 1,
  "source_image": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/images/5e9c6b3c-c8e4-4c14-a6d7-08c1395d9905",
    "id": "5e9c6b3c-c8e4-4c14-a6d7-08c1395d9905",
    "name": "ibm-ubuntu-20-04-minimal-amd64-1",
    "resource_type": "image"
  },
  "source_volume": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/8948ad59-bc0f-7510-812f-5dc64f59fab8",
    "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8",
    "name": "my-instance-data",
    "resource_type": "volume"
  },
  "user_tags": [
    "env:test",
    "env:prod"
  ]
}
```
{: codeblock}

## Next steps
{: #snapshots_vpc_create_next_steps}

[View](/docs/vpc?topic=vpc-snapshots-vpc-view) all the snapshots that you created, or details about a single snapshot.

[Restore a volume](/docs/vpc?topic=vpc-snapshots-vpc-restore) from a snapshot.
