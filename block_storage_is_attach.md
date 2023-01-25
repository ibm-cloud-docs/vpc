---

copyright:
  years: 2019, 2023
lastupdated: "2023-01-25"

keywords: vpc, block storage, block storage for vpc, mounting storing, attaching block storage, vpc instance, data volumes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attaching a {{site.data.keyword.block_storage_is_short}} volume
{: #attaching-block-storage}

When you create a {{site.data.keyword.block_storage_is_full}} volume for a virtual server instance, the volume is attached to the instance by default. When you detach a volume, it exists as an unattached volume that you can later reattach. These available volumes are displayed in the list of [all {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-viewing-block-storage#viewvols). You can attach the volume to another instance from the list of all {{site.data.keyword.block_storage_is_short}} volumes, or when you view details about an instance.
{: shortdesc}

## Volume attachment limits
{: #vol-attach-limits}

You can attach only one {{site.data.keyword.block_storage_is_short}} boot volume to a virtual server instance at a time, but you can attach up to 12 {{site.data.keyword.block_storage_is_short}} data volumes to a single instance.

When you create a {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}} instance, only one data volume can be added currently. Even if you attach more volumes, they are not used by the instance.
{: note}

You can't use the UI to attach {{site.data.keyword.block_storage_is_short}} volumes to {{site.data.keyword.containerfull}} Cluster worker nodes. For more information about using the CLI to attach volumes to cluster nodes, see [Storing data on {{site.data.keyword.block_storage_is_short}}](/docs/containers?topic=containers-vpc-block).


## Attach a {{site.data.keyword.block_storage_is_short}} volume to a virtual server instance
{: #attach}
{: help}
{: support}
{: ui}

From the list of all {{site.data.keyword.block_storage_is_short}} volumes, follow these steps.

1. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, go to the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
1. In the list of volumes, click the ellipsis at the end of a row for an available, unattached volume to display its action menu.
1. Select **Attach to instance**.
1. Select a compute resource (virtual server instance) from the list of available resources, and then click **Attach**.
1. Messages display on the volume details page to indicate that the volume is being attached to the image. When it completes, the image name appears under **Attached instances**.

When you create a {{site.data.keyword.hpvs}} instance and the contract mentions volumes, you can attach a data volume during the instance creation, or within 15 minutes after the instance is created. Failure to do so causes the instance to go into a shutdown state after the 15-minute window.
{: note}

You can also attach a {{site.data.keyword.block_storage_is_short}} volume from the virtual server instance details page.

1. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
1. Select an instance from the list of all virtual server instances. If any {{site.data.keyword.block_storage_is_short}} volumes are attached, they are listed under **Storage volumes**.
1. Select **Attach volume**.
1. Select a volume from the list of available resources and click **Attach**. Messages display on the instance details page to indicate that the volume is being attached. When it completes, the **Storage volumes** list is updated to include the new volume.

## Attaching a {{site.data.keyword.block_storage_is_short}} volume from the CLI
{: #attaching-block-storage-cli}
{: cli}

Follow these instructions to use the CLI to attach a {{site.data.keyword.block_storage_is_short}} volume to a virtual server instance. Each instance can have [many volume attachments](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits), but a single volume attachment connects one volume to one instance.

### Before you begin
{: #before-attaching-block-storage-cli}

1. Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

2. After you install the vpc-infrastructure plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.

3. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli).


### Attach a {{site.data.keyword.block_storage_is_short}} volume from the CLI
{: #attach-block-storage-cli}
{: help}
{: support}

To attach a volume to a virtual server instance in the current resource group, run this command.

```sh
ibmcloud is instance-volume-attachment-add NAME INSTANCE_ID VOLUME_ID [--auto-delete true | false] [--json]
```
{: pre}

`NAME` is the name that you provide for the volume attachment and INSTANCE_ID is the ID of the virtual server instance.

The `VOLUME_ID` specifies the volume that you are attaching.

Specify `--auto-delete true` if you want the volume to automatically delete when the virtual server instance is deleted.

To see a list of available virtual server instances, run the `ibmcloud is instances` command.

Example:

```text
$ ibmcloud is instances
Listing instances under account my-account-01 as user rtuser1@mycompany.com...
ID                                     Name                  Address          Profile   Image                            Created        Status     VPC                               Zone         Resource Group
0738-916e3ccf-b3af-47a5-b549-c9a3b9815559   instance-test2        192.0.2.1        -         ubuntu-16.04-amd64(7eb4e35b-.)   4 hours ago    running    function-test-vpc1(974e258e-.)    us-south-1   -
0738-b762f064-26a6-4ffe-bfe4-4a21d92effaf   instance-test1        192.0.2.2        -         ubuntu-16.04-amd64(7eb4e35b-.)   4 hours ago    running    function-test-vpc2(974e258e-.)    us-south-1   -
0738-ad0ade52-0533-4dc6-a145-f1ad6d5bee2c   vsi-09202             198.51.100.1     -         ubuntu-16.04-amd64(7eb4e35b-.)   5 hours ago    running    vpnaas-test1(2467b0fa-.)          us-south-1   -
0738-e6353eba-c407-4406-b9f6-c50ee1da8d83   vsi-09201             198.51.100.3     -         ubuntu-16.04-amd64(7eb4e35b-.)   5 hours ago    running    vpnaas-test1(2467b0fa-.)          us-south-1   -

```
{: screen}

### Show details of a volume that is attached to a virtual server instance
{: #show-details-attached-vol}

After you attach a volume, you can display details by specifying the instance ID and volume attachment ID in the `instance-volume-attachment` command.

```sh
ibmcloud is instance-volume-attachment INSTANCE_ID VOLUME_ATTACHMENT_ID [--json]
```
{: pre}

### List all volume attachments of a server instance
{: #list-all-attached-vol}

Use the `instance-volume-attachments` command and specify the instance ID to see all volume attachments for an instance.

```sh
ibmcloud is instance-volume-attachments INSTANCE_ID [--json]
```
{: pre}

Do you prefer to use the {{site.data.keyword.cloud}} console? For more information, see [Attaching a {{site.data.keyword.block_storage_is_short}} volume in the UI](/docs/vpc?topic=vpc-attaching-block-storage).
{: tip}

### Create a volume attachment JSON
{: #volume_attachment_json}

When you provision a virtual server instance from the CLI and create a {{site.data.keyword.block_storage_is_short}} volume as part of the process, you must specify a volume attachment JSON. The volume attachment JSON, specified in the command or as a file, defines the volume parameters. When you [create an instance](/docs/vpc?topic=vpc-creating-virtual-servers-cli) and specify the `--volume-attach` parameter, you specify the volume JSON. For example, `--volume-attach @/Users/myname/myvolume-attachment_create.json`.

The following example shows a volume attachment JSON file that defines a custom volume and specifies user tags.

```json
[
    {
        "name": "myvolume-attachment",
        "delete_volume_on_instance_delete": true,
        "volume": {
            "name": "myvolume",
            "capacity": 100,
            "iops": 1000,
            "profile": {
                "name": "custom"
            },
            "user_tags": {
                "env:test"
            }
        }
    }
]
```
{: codeblock}

## Attaching a {{site.data.keyword.block_storage_is_short}} volume with the API
{: #attaching-block-storage-api}
{: api}

### Attach a {{site.data.keyword.block_storage_is_short}} volume with the API
{: #attach-block-storage-api}
{: help}
{: support}

Attach {{site.data.keyword.block_storage_is_short}} volumes to an instance by directly calling the [REST APIs](/apidocs/vpc).

Create a volume attachment for an instance to attach a {{site.data.keyword.block_storage_is_short}} volume. Make a `POST /instances` call and specify `volume_attachments`. This example creates a volume attachment and specifies the volume by ID.

```text
POST/instances/{instance_id}/volume_attachments
```
{: pre}

Example request:

```curl
curl -X POST "$vpc_api_endpoint/v1/instances/$instance_id/volume_attachments?version=2021-04-20&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "delete_volume_on_instance_delete": true,
      "name": "my-volume-attachment-data-5iops",
      "volume": {
        "id": "d8b26921-1409-4c2f-9b46-39b5b6e0b945"
      }
    }'
```
{: codeblock}

A successful response indicates that the volume is attached.

```json
{
  "created_at": "2021-04-21T16:35:47.000Z",
  "delete_volume_on_instance_delete": true,
  "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/8f06378c-ed0e-481e-b98c-9a6dfbee1ed5/volume_attachments/9f2a645e-19c1-4f8f-b062-46b9e0671999",
  "id": "9f2a645e-19c1-4f8f-b062-46b9e0671999",
  "name": "my-volume-attachment-data-5iops",
  "status": "attached",
  "type": "data",
  "volume": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/d8b26921-1409-4c2f-9b46-39b5b6e0b945",
    "id": "d8b26921-1409-4c2f-9b46-39b5b6e0b945",
    "name": "my-volume-data-5iops"
  }
}
```
{: screen}

## Next steps
{: #next-step-attaching-block-storage}

Create more volumes and manage existing ones. See the following information.

* [Creating {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-creating-block-storage)
* [Managing {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-managing-block-storage)
