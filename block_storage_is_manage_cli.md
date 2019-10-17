---

copyright:
  years: 2019
lastupdated: "2019-09-30"

keywords: block storage, IBM Cloud, VPC, virtual private cloud, volume, volume attachment, data storage, virtual server instance, instance

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:important: .important}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}

# Managing block storage volumes by using the CLI
{: #managing-block-storage-cli}

Manage {{site.data.keyword.block_storage_is_short}} from the command-line interface (CLI). Update a volume name, update a volume attachment, detach a volume, or delete a volume.
{:shortdesc}

## Before you begin
{: #before-managing-block-storage-cli}

1. Make sure that you have downloaded, installed, and initialized the following CLI plug-ins:
    * {{site.data.keyword.cloud_notm}} CLI
    * The infrastructure-service plug-in

   For more information, see the [CLI Reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).
   
   When you install the vpc-infrastructure plug-in for the first time, you must set the target generation to gen 2, `ibmcloud is target --gen 2`.
   {:important}
   
2. Make sure that you have [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-getting-started).

## Update the name of a volume
{: #update-vol-name}

To change a volume name, specify either the volume name or ID and then indicate the new name. The volume name can be up to 63 alpha-numeric characters and include special characters. Follow [these volume name conventions](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-managing-block-storage#volume-name-conventions).

```bash
ibmcloud is volume-update VOLUME_ID [--name NEW_NAME] [--json]
```

Example:

```bash
$ ibmcloud is volume-update 933c8781-f7f5-4a8f-8a2d-3bfc711788ee --name demo-volume-update
Updating volume 933c8781-f7f5-4a8f-8a2d-3bfc711788ee under account MyAccount 01 as user user1@mycompany.com...
ID                                      0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    demo-volume-update
Capacity                                100
IOPS                                    1000
Profile                                 5iops-tier
Encryption                              -
Encryption                              provider managed
Status                                  available
Created                                 2019-07-20 10:09:28
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Zone                                    us-south-2
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Volume Attachment Instance Reference    none
```
{: screen}

## Update a volume attachment using the CLI
{: #update-vol-name-cli}

You can update the volume attachment name and change the default auto delete setting with the `instance-volume-attachment-update` command.

```bash
ibmcloud is instance-volume-attachment-update INSTANCE_ID VOLUME_ATTACHMENT_ID [--name NEW_NAME] [--auto-delete true | false] [--json]
```

Use the `--name` parameter and specify a new name for the volume attachment.

Specify`--auto-delete true` to automatically delete the volume when you delete the instance to which it's attached.

Example showing details for `Volume Attachment Instance Reference`:

```
Vdisk Name    Vdisk ID                               Vdisk Type   Auto Delete   Instance Name   Instance ID
Vdisk-data1   0738-fd146b1f-e1bb-4eab-ba78-3109e6bc3a2d   data         true          vsi-test1       0738-8b56da93-7990-4ccf-9dc5-5aee6a5f08f9
```
{: screen}

## Detach a volume using the CLI
{: #detach-vol-attachment-cli}

Use the `instance-volume-attachment-detach` command to detach a volume from an instance and delete the volume attachment. The block storage volume is not deleted; you can later [attach it to another instance](/docs/vpc?topic=vpc-attaching-block-storage-cli).

```bash
ibmcloud is instance-volume-attachment-detach INSTANCE_ID VOLUME_ATTACHMENT_ID [-f, --force]
```

## Delete a block storage volume using the CLI
{: #delete-vol-cli}

Use the `volume-delete` command and specify the volume ID to delete a block storage volume.

**Note:** You cannot delete an active block storage volume. You must first [detach it from the virtual server](#detach-vol-attachment-cli).

```bash
ibmcloud is volume-delete (VOLUME_NAME | VOLUME_ID) [-f, --force]
```

Example:

```bash
$ ibmcloud is volume-delete 64d85f0f-6c08-4188-9e9a-0057b3aa1b69
This will delete volume 64d85f0f-6c08-4188-9e9a-0057b3aa1b69 and cannot be undone. Continue?> y
Deleting volume 64d85f0f-6c08-4188-9e9a-0057b3aa1b69 under account MyAccount 01 as user user1@mycompany.com...
OK
Volume ID 0738-64d85f0f-6c08-4188-9e9a-0057b3aa1b69 is deleted.
```
{: screen}

Do you prefer to manage block storage volumes using the {{site.data.keyword.cloud}} console? For more information, see [Managing block storage volumes](/docs/vpc?topic=vpc-managing-block-storage).
{:tip}

## Next steps
{: #next-step-managing-block-storage-cli}

[Create more volumes using the CLI](/docs/vpc?topic=vpc-creating-block-storage-cli).

For issues with existing block storage volumes, you might be able to troubleshoot and fix the problems yourself. For more information, see
[Troubleshooting {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-troubleshooting-block-storage).
