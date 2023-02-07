---

copyright:
  years: 2021, 2023
lastupdated: "2023-02-07"

keywords:
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating Snapshots
{: #snapshots-vpc-create}

With the UI, CLI, or API, you can create a snapshot of a {{site.data.keyword.block_storage_is_short}} volume that is attached to a virtual server instance. You can create a snapshot of a boot or data volume.
{: shortdesc}

Before you take a snapshot, make sure that all cached data is present on disk, especially when you're taking a snapshot of instances with Windows and Linux&reg; operating systems. For example, on Linux&reg; operating systems, run the `sync` command to force an immediate write of all cached data to disk.
{: note}

## Create a snapshot in the UI
{: #snapshots-vpc-create-ui}
{: ui}

In the console, you can create a snapshot of a {{site.data.keyword.block_storage_is_short}} volume that is attached to a running virtual server instance.

### Create a snapshot from the list of snapshots
{: #snapshots-vpc-create-from-list}

Follow these steps to create a snapshot from the list of snapshots.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**.
2. From the list of snapshots (initially empty), click **Create**.
3. Enter the required information (see Table 1) to define your snapshot and select the {{site.data.keyword.block_storage_is_short}} volume that you want to copy.

   | Field | Value |
   |-------|-------|
   | Location | Specify the geography and region for this snapshot. |
   | Name  | Provide a unique name for the snapshot. The UI verifies the name for proper conventions and identifies duplicate names. For suggestions about how to name your snapshots, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-naming). |
   | Resource group | Select a [resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups) for the snapshot, or use the default. You can't change the resource group after the snapshot is created. |
   | Tags | Specify any [user tags that you want to identify this resource. |
   | Access management tags | Specify any [access management tags](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#storage-add-access-mgt-tags) for this resource. |
   | Block storage volume | Select a volume from the list. The boot or data volume must be attached to a running virtual server instance. |
   | Encryption | Encryption information for the volume that you selected, either [provider-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-provider-managed-encryption) or [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-customer-managed-encryption). The snapshot inherits the encryption of the source volume. You can't change the encryption type.
   {: caption="Table 1. Selections for creating a snapshot" caption-side="bottom"}

4. Click **Create snapshot**. You're returned to the list of snapshots. Messages are displayed while the snapshot is being created and when it's ready, the snapshot displays first in the list of snapshots. You can then [view details of your snapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).

### Create a snapshot from the list of {{site.data.keyword.block_storage_is_short}} volumes
{: #snapshots-vpc-create-from-volume-list}

Follow these steps to create a snapshot from the list of {{site.data.keyword.block_storage_is_short}} volumes. The volume must be attached to a virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
2. From the list of volumes, locate a boot or data volume that is attached to an instance.
3. Click the overflow menu (...) and select **Create snapshot**.
4. On the snapshots list page, messages are displayed while snapshot is being created. When it's ready, the snapshot is displayed first in the list of snapshots. You can then [view details of your snapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).

### Create a snapshot from an attached {{site.data.keyword.block_storage_is_short}} volume
{: #snapshots-vpc-create-from-vol-details}

For a volume that is attached to an instance, you can create a snapshot from the volume details.

1. Go to the volume details page in either of these ways:

    * From the [instance details page](#view-vol-details-instance-ui), scroll to the list of attached volumes and click the link.
    * From the [list of block storage volumes](/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui), select a volume that is attached to an instance.

2. On the volume details page, select **Create snapshot** from the **Actions** menu.
3. Provide a snapshot name and optionally, a resource group and user tags.
4. Under **Volume**, click **Edit** to select a volume form the list that is displayed, then click **Save**. A summary is shown in the side panel.
5. Click **Create block storage snapshot**. A message indicates that the snapshot is being created and you're returned to the volume details page.
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

### Prerequisites for creating a snapshot from the CLI
{: #snapshots-vpc-create-procedure-cli}

1. Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
2. To use the CLI, set the `IBMCLOUD_IS_FEATURE_SNAPSHOT` environment variable to `true`. Copy the following code:

   ```zsh
   export IBMCLOUD_IS_FEATURE_SNAPSHOT=true
   ```
   {: pre}

3. After you install the vpc-infrastructure plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.
4. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli).

### Create a snapshot from the CLI
{: #snapshot-create-cli}

To create a snapshot, run the `snapshot-create` command.

```zsh
ibmcloud is snapshot-create --volume VOLUME_ID [--name NAME] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--tags  TAG_NAME1,TAG_NAME2,...] [--clone-zones ZONE1,ZONE2] [--output JSON] [-q, --quiet]
```
{: pre}

The following example creates a snapshot with the name `cli-snapshot-2` of the boot volume `test-1`. The snapshot is tagged with `env:test` and `env:prod`, and clones fast restore snapshots in `us-south-1` and `us-south-3` zones.

```sh
$ ibmcloud is snapshot-create --volume test-1 --name cli-snapshot-2 --tags env:test,env:prod --clone-zones us-south-1,us-south-3
Creating snapshot cli-snapshot-2 under account VPC 01 as rtuser1@mycompany.com...
                          
ID                     r134-e9c45a8f-3b6e-420f-8147-7dff7582e89f   
Name                   cli-snapshot-2   
CRN                    crn:v1:staging:public:is:us-south:a/efe5afc483594adaa8325e2b4d1290df::snapshot:r134-e9c45a8f-3b6e-420f-8147-7dff7582e89f   
Status                 pending   
Clones                 Zone         Available   Created      
                       us-south-1   false       2022-11-04T12:18:21+05:30      
                       us-south-3   false       2022-11-04T12:18:21+05:30      
                          
Source volume          ID                                          Name      
                       r134-806aaab7-555f-45fd-87ed-b11848c74a55   test-1      
                          
Bootable               true   
Encryption             provider_managed   
Encryption key         -   
Minimum capacity(GB)   100   
Size(GB)               1   
Source image           ID                                          Name      
                       r134-2630c3c2-ed8a-4d82-89b2-facea89d49d3   ibm-centos-7-9-minimal-amd64-7      
                          
Operating system       Name             Vendor   Version                 Family   Architecture   Display name      
                       centos-7-amd64   CentOS   7.x - Minimal Install   CentOS   amd64          CentOS 7.x - Minimal Install (amd64)      
                          
Resource group         ID                                 Name      
                       11caaa983d9c4beb82690daab08717e9   Default      
                          
Created                2022-11-04T12:18:21+05:30   
Captured at            0001-01-01T05:53:28+05:53   
Tags                   env:test,env:prod
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-create`](/docs/cli?topic=cli-vpc-reference#snapshot-create).

### Create a fast restore snapshot clone
{: #snapshots-vpc-create-frclone}
{: cli}

The following example creates a fast restore snapshot clone of the snapshot `r134-502aeb51-38f2-4905-bb50-b0786760d692` in the `us-south-3` zone.

```sh
ibmcloud is snapshot-clc r134-502aeb51-38f2-4905-bb50-b0786760d692  --zone us-south-3
Creating zonal clone of snapshot r134-502aeb51-38f2-4905-bb50-b0786760d692 under account VPCUI-DEV as user Shyam.Venkat.R@ibm.com...
               
Zone        us-south-3   
Available   false   
Created     2022-11-04T12:23:01+05:30   
Href        https://us-south-stage01.iaasdev.cloud.ibm.com/v1/regions/us-south/zones/us-south-3
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-create`](/docs/cli?topic=cli-vpc-reference#snapshot-create).

## Create a snapshot with the API
{: #snapshots-vpc-create-api}
{: api}

You can create a snapshot by calling the [VPC API](/apidocs/vpc). Make a `POST/snapshots` request to create a snapshot of a boot or data volume. The following example creates a snapshot of a boot volume by using the volume ID, and specifies user tags that can be associated with a [backup policy](/docs/vpc?topic=vpc-backup-service-about).

```curl
curl -X POST \
"$vpc_api_endpoint/v1/snapshots?version=2022-12-12&generation=2" \
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
{: curl}

A successful response looks like the following example. The snapshot lifecycle state is `pending` while the snapshot is created. When it is successfully created, the status changes to `stable`.

```json
{
  "bootable": true,
  "clones": [],
  "created_at": "2022-12-12T20:18:18Z",
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

## Create a snapshot and fast restore snapshot clone with the API
{: #snapshots-vpc-create-snaphot-clone-api}
{: api}

When you create a snapshot, you can also create a fast restore snapshot clone in another zone. By cloning a snapshot and keeping it in another zone, you can later use the fast restore feature to quickly provision a new volume with data from a snapshot. For more information, see [Snapshots fast restore feature](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#snapshots_vpc_fast_restore).

Make a `POST/snapshots` request to create a snapshot of a boot or data volume and specify the `clones` property. Indicate a different zone or zones in your region from the zone in which you're creating the snapshot. In the following example, a clone is created in us-south-2, specified by name.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/snapshots?version=2022-12-18\&generation=2" \
-H "Authorization: $iam_token" \
-d '{
    "clones": [
      {
        "zone": {
            "name": "us-south-2"
        }
      }
    ],
    "name": "my-snapshot-1",
    "source_volume": {
        "id": "1a6b7274-678d-4dfb-8981-c71dd9d4daa5"
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
{: curl}

A successful response indicates that the clone was created in the specified zone.

```json
{
  "bootable": true,
  "clones": [
		{
			"available": false,
			"created_at": "2022-12-18T14:58:32Z",
			"zone": {
				"name": "us-south-2",
				"href": "https:ibm.com/v1/regions/us-south/zones/us-south-1",
				"resource_type": "zone"
			}
		}
	],
  "created_at": "2022-12-18T20:18:18Z",
  "crn": "crn:[...]",
  "deletable": false,
  "encryption": "user_managed",
  "encryption_key": {
     "crn": "crn:[...]"
  },
  "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/12917904-3771-424d-8391-53ec9e305d52",
  "id": "12917904-3771-424d-8391-53ec9e305d52",
  "lifecycle_state": "pending",
  .
  .
  .
}  
```
{: codeblock}

## Next steps
{: #snapshots_vpc_create_next_steps}

[View](/docs/vpc?topic=vpc-snapshots-vpc-view) all the snapshots that you created, or details about a single snapshot.

[Restore a volume](/docs/vpc?topic=vpc-snapshots-vpc-restore) from a snapshot.
