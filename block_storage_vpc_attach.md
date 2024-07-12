---

copyright:
  years: 2019, 2024
lastupdated: "2024-07-12"

keywords: vpc, Block Storage, Block Storage for vpc, mounting storing, attaching Block Storage, vpc instance, data volumes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attaching a {{site.data.keyword.block_storage_is_short}} volume
{: #attaching-block-storage}

When you create an {{site.data.keyword.block_storage_is_full}} volume for a virtual server instance, the volume is attached to the instance by default. When you detach a volume, it exists as an unattached volume that you can later reattach. These available volumes are displayed in the list of [all {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-viewing-block-storage#viewvols). You can attach the volume to another instance from the list of all {{site.data.keyword.block_storage_is_short}} volumes, or when you view details about an instance.
{: shortdesc}

## Volume attachment limits
{: #vol-attach-limits}

You can attach only one {{site.data.keyword.block_storage_is_short}} boot volume to a virtual server instance at a time, but you can attach up to 12 {{site.data.keyword.block_storage_is_short}} data volumes to a single instance.

When you create a {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}} instance, only one data volume can be added currently. Even if you attach more volumes, they are not used by the instance.
{: note}

You can't use the UI to attach {{site.data.keyword.block_storage_is_short}} volumes to {{site.data.keyword.containerfull}} Cluster worker nodes. For more information about using the CLI to attach volumes to cluster nodes, see [Storing data on {{site.data.keyword.block_storage_is_short}}](/docs/containers?topic=containers-vpc-block).

## Attaching a {{site.data.keyword.block_storage_is_short}} volume to a virtual server instance
{: #attach}
{: help}
{: support}
{: ui}

You can attach a volume to a virtual server instance from the list of Block Storage volumes, volume details page, or from the instance details page. 

### Attaching a volume to an instance from the list of volumes
{: #attach-from-vol-list}

From the list of all {{site.data.keyword.block_storage_is_short}} volumes, follow these steps.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
1. In the list of volumes, identify an available, unattached volume and click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") at the end of a row.
1. Select **Attach to instance**.
1. Select a virtual server instance from the list of available instances, and then click **Save**.
    If the list shows no virtual server instances, click **Create server** to go to the [instance provisioning](/docs/vpc?topic=vpc-creating-virtual-servers) page.  
1. Messages display on the volume details page to indicate that the volume is being attached to the image. When it completes, the image name appears under **Attached instances**.

When you create a {{site.data.keyword.hpvs}} instance and the contract mentions volumes, you have 15 minutes after the creation of the instance to attach a data volume. Failure to do so causes the instance to go into a shutdown state after the 15-minute window.
{: note}

### Attaching a volume to an instance from the volume details page
{: #attach-from-volume-details}

From the volume details page, follow these steps:

1. Select an unattached volume from the [list of Block Storage volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui). 
1. From the volume details page, in the **Virtual server instances** tile, click **Attach**.
1. On the side panel, select a virtual server instance and click **Save**. 
    If the list shows no virtual server instances, click **Create server** to go to the [instance provisioning](/docs/vpc?topic=vpc-creating-virtual-servers) page. The boot or data volume is shown on the instance provisioning page in their respective fields.
1. Messages display that the volume is being attached to the image. When it completes, the image name appears under **Attached instances**.

### Attaching a volume to an instance from the instance details page
{: #attach-from-instance-details}

Attach a {{site.data.keyword.block_storage_is_short}} volume from the virtual server instance details page.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Compute > Virtual server instances**.
1. Select an instance from the list of all virtual server instances. If any {{site.data.keyword.block_storage_is_short}} volumes are attached, they are listed under **Storage volumes**.
1. Select **Attach volume**.
1. Select a volume from the list of available resources and click **Attach**. Messages display on the instance details page to indicate that the volume is being attached. When it completes, the **Storage volumes** list is updated to include the new volume.

## Attaching a {{site.data.keyword.block_storage_is_short}} volume from the CLI
{: #attaching-block-storage-cli}
{: cli}

Follow these instructions to use the CLI to attach a {{site.data.keyword.block_storage_is_short}} volume to a virtual server instance. Each instance can have [many volume attachments](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits), but a single volume attachment connects one volume to one instance.

### Before you begin
{: #before-attaching-block-storage-cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

2. Select the current generation of VPC. 
   ```sh
   ibmcloud is target --gen 2
   ```
   {: pre}

### Attaching a {{site.data.keyword.block_storage_is_short}} volume from the CLI
{: #attach-block-storage-cli}
{: help}
{: support}

To attach a volume to a virtual server instance in the current resource group, run this command.

```sh
ibmcloud is instance-volume-attachment-add NAME INSTANCE_ID VOLUME_ID [--auto-delete true | false] [--json]
```
{: pre}

`NAME` is the name that you provide for the volume attachment and INSTANCE_ID is the ID of the virtual server instance. The `VOLUME_ID` specifies the volume that you are attaching. If you want the volume to automatically delete when the virtual server instance is deleted, specify `--auto-delete true`.

To see a list of available virtual server instances, run the `ibmcloud is instances` command. Check out the following example.

```sh
$ ibmcloud is instances
Listing instances in all resource groups and region us-east under account Test Account as user test.user@ibm.com...
ID                                          Name                 Status    Reserved IP   Floating IP      Profile    Image                                VPC                              Zone        Resource group   
0757_506a787d-0672-4209-854b-3e280ec50b44    my-rhel-instance    running   10.241.0.4    169.63.96.188    bx2-2x8    ibm-redhat-9-0-minimal-amd64-2       test-pbalak1                     us-east-1   defaults   
0757_4fc00b09-c967-46bf-9203-d17a08078d1d    my-rhel-instance2   running   10.241.0.5    150.239.86.22    bx2-2x8    ibm-redhat-9-0-minimal-amd64-2       test-pbalak1                     us-east-1   defaults   
0757_11f5db7f-35a1-4678-bcbd-c85204e09507    my-test-ro          stopped   10.241.0.5    -                bx2d-2x8   ibm-ubuntu-18-04-5-minimal-amd64-1   test-vpc-blu-wdc                 us-east-1   defaults   
0767_7ac6da13-c16a-4f58-9981-612d6e33ec53    my-ubuntu-vsi       running   10.241.64.4   -                bx2-2x8    ibm-centos-7-9-minimal-amd64-3       test-vpc-blu-wdc                 us-east-2   defaults   
0757_ff64452f-7e81-4a48-b938-7fc601597dcd    my-new-ubuntu-vsi   running   10.241.14.4   52.116.121.167   mx2-2x16   ibm-ubuntu-20-04-6-minimal-amd64-1   test-vpc-do-not-delete-default   us-east-1   Default   
0757_337977c1-f1f0-42ad-9de4-16fe0cda2ba9    my-virtual-server   running   10.241.4.4    52.116.124.193   mx2-2x16   ibm-ubuntu-20-04-6-minimal-amd64-1   test-vpc-do-not-delete-default   us-east-1   Default   
```
{: screen}

Then, select the virtual server instance that is in the same zone as the volume that you want to attach.

```sh
$ ibmcloud is instance-volume-attachment-add otp1 0757_11f5db7f-35a1-4678-bcbd-c85204e09507 demo-volume-update --auto-delete true
Creating volume attachment otp1 for instance my-test-ro under account Test Account as user test.user@ibm.com...
                     
ID                0757-6757e676-0bf5-4b79-9a5b-29c24e17420c   
Name              otp1   
Volume            ID                                          Name      
                  r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac   demo-volume-update      
                     
Status            attaching   
Bandwidth(Mbps)   393   
Type              data   
Device            -   
Auto delete       true   
Created           2023-06-29T18:14:57+00:00
```
{: screen}

For more information about available command options, see [`ibmcloud is instance-volume-attachment-add`](/docs/cli?topic=cli-vpc-reference#instance-volume-attachment-add).

### Show details of a volume that is attached to a virtual server instance
{: #show-details-attached-vol}

After you attach a volume, you can display details by specifying the instance ID or name and the volume attachment ID or name in the `instance-volume-attachment` command.

```sh
ibmcloud is instance-volume-attachment INSTANCE VOLUME_ATTACHMENT [--json]
```
{: pre}

```sh
$ ibmcloud is instance-volume-attachment my-test-ro otp1
Getting volume attachment otp1 of instance my-test-ro under account Test Account as user test.user@ibm.com...
                     
ID                0757-6757e676-0bf5-4b79-9a5b-29c24e17420c   
Name              otp1   
Volume            ID                                          Name      
                  r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac   demo-volume-update      
                     
Status            attached   
Bandwidth(Mbps)   393   
Type              data   
Device            0757-6757e676-0bf5-4b79-9a5b-29c24e17420c-bxsh7   
Auto delete       true   
Created           2023-06-29T18:14:57+00:00   
```
{: screen}

For more information about available command options, see [`ibmcloud is instance-volume-attachment`](/docs/cli?topic=cli-vpc-reference#instance-volume-attachment-view).

### List all volume attachments of a server instance
{: #list-all-attached-vol}

Use the `instance-volume-attachments` command and specify the instance ID or name to see all volume attachments for an instance.

```sh
ibmcloud is instance-volume-attachments INSTANCE [--json]
```
{: pre}

See the following example.

```sh
ibmcloud is instance-volume-attachments my-test-ro
Listing volume attachments of instance my-test-ro under account Test Account as user test.user@ibm.com...
ID                                          Name                                 Volume                          Status     Type   Device                                            Auto delete   
0757-6757e676-0bf5-4b79-9a5b-29c24e17420c   otp1                                 demo-volume-update              attached   data   0757-6757e676-0bf5-4b79-9a5b-29c24e17420c-bxsh7   true   
0757-af470ade-9d5c-491e-97b2-f000ed4ee49b   scheming-tipper-shivering-decrease   my-test-ro-boot-1629867631000   attached   boot   0757-af470ade-9d5c-491e-97b2-f000ed4ee49b-fc4tl   true 
```
{: screen}  

For more information about available command options, see [`ibmcloud is instance-volume-attachments`](/docs/cli?topic=cli-vpc-reference#instance-volume-attachments-list).

### Create a volume attachment JSON
{: #volume_attachment_json}

When you provision a virtual server instance from the CLI and create a {{site.data.keyword.block_storage_is_short}} volume as part of the process, you can specify a volume attachment JSON. The volume attachment JSON, specified in the command or as a file, contains the volume parameters. 

In the CLI command that you use to [create an instance](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli), you specify the `--volume-attach` option and the volume JSON file. For example, `--volume-attach @/Users/myname/myvolume-attachment_create.json`.

The following example shows the content of a volume attachment JSON file that defines a custom volume and specifies user tags.

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

### Attaching a {{site.data.keyword.block_storage_is_short}} volume with the API
{: #attach-block-storage-api}
{: help}
{: support}

Attach {{site.data.keyword.block_storage_is_short}} volumes to an instance by directly calling the [REST APIs](/apidocs/vpc).

Create a volume attachment for an instance to attach a {{site.data.keyword.block_storage_is_short}} volume. Make a `POST /instances` call and specify `volume_attachments`. 

```text
POST/instances/{instance_id}/volume_attachments
```
{: pre}

The following example creates a volume attachment and specifies the volume by ID.

```sh
curl -X POST "$vpc_api_endpoint/v1/instances/$instance_id/volume_attachments?version=2021-04-20&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "delete_volume_on_instance_delete": true,
      "name": "my-volume-attachment-data-5iops",
      "volume": {"id": "d8b26921-1409-4c2f-9b46-39b5b6e0b945"}
    }'
```
{: pre}

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
