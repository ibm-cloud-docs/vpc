---

copyright:
  years: 2019, 2024
lastupdated: "2024-08-09"

keywords: vpc Block Storage, provision Block Storage for vpc, bootable snapshots, create volume from snapshot, fast restore

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating {{site.data.keyword.block_storage_is_short}} volumes
{: #creating-block-storage}

Create a {{site.data.keyword.block_storage_is_short}} volume by using the UI, CLI, API, or Terraform. You can create a volume as part of instance provisioning, as a stand-alone volume that you can later attach to an instance, or by restoring from a snapshot.
{: shortdesc}

Before you get started, make sure that you [created a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{: important}

## Creating {{site.data.keyword.block_storage_is_short}} volumes in the console
{: #creating-block-storage-ui}
{: ui}

Use the {{site.data.keyword.cloud_notm}} console to create a {{site.data.keyword.block_storage_is_short}} volume when you create a virtual server instance or as a stand-alone volume.

### Creating and attaching a {{site.data.keyword.block_storage_is_short}} volume when you create an instance
{: #create-from-vsi}
{: help}
{: support}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Compute > Virtual server instances**.
1. From the list of virtual server instances, click **Create**. 
1. In the Virtual server for VPC provisioning page, select Server type, Location, OS, Profile, Placement Group, and so on. For more information about provisioning the instance, see [Creating a virtual server instance with the UI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui). 
1. The details of the boot volume that is to be created are displayed on the page. You can edit the boot volume by clicking the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to change the size, specify encryption, and add any user tags to identify this resource. 
1. To create a data volume and attach it to the instance, in the **Data volumes** section of the instance provisioning page, click **Create**. In the side panel, specify the volume details.
   1. You have the option to import data from a snaphot by clicking the **Import from snapshot** toggle. Select a snapshot from the list.
   1. Specify a unique, meaningful name for your volume. For example, it can be a name that describes your compute or workload function. The volume name must begin with a lowercase letter. The name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-). Volume names must be unique the entire VPC infrastructure. You can edit the name later if you want. 
   1. Auto-delete feature is disabled by default. If you want the volume to be deleted when the attached virtual server instance is deleted, click the toggle to enable the feature.
   1. Resource group and location are inherited from the instance. These values can't be changed.
   1. You can specify optional user tags to associate with this volume. For more information about organizing resources with user tags, see [Working with tags](/docs/account?topic=account-tag&interface=ui).
   1. Select the encryption type. Provider-managed encryption is enabled by default on all volumes. You can also choose to create an envelop encryption with your own root keys. Encryption keys are created and maintained in Key Management Services ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}). For more information, see [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-encryption-prereqs).
   1. Click **Next**.
   1. Select the storage profile.
      - For [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers), select the tile with the performance level that you require and specify the volume size in GBs. Volume sizes can be 10 - 16,000 GB. 
      - For [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) IOPS, specify the size of your volume and IOPS range based on the size of the volume. As you type the IOPS value, the UI shows the acceptable range. You can also click the **storage size** link to see the table that contains the size and IOPS ranges of the [custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom).
   1. Enter the volume size in GBs. Volume sizes can be 10 - 16,000 GB.
   1. If you selected a profile with custom IOPS, specify an IOPS value within range based on the storage size. For more information, see [Custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#custom).
   1. Click **Save**.
1. You return to the virtual instance provisioning page to finish defining the remaining attributes of your virtual server instance.
1. When you are satisfied with your choices, click **Create virtual server**.

A {{site.data.keyword.block_storage_is_short}} volume can be attached to only one virtual server at a time. On the {{site.data.keyword.block_storage_is_short}} volume summary page, you can view details about the virtual server instance by selecting the instance name under **Attached instances**.

### Creating and attaching a {{site.data.keyword.block_storage_is_short}} volume from an existing instance
{: #create-from-existing-vsi}

You can create a {{site.data.keyword.block_storage_is_short}} volume from an existing instance.

1. Go to the list of virtual server instances. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Compute > Virtual server instances**.
2. Select an instance from the list.
3. On the instance details page, scroll to **Storage volumes** and click **Attach**.
4. In the side panel, click the down arrow under **Block volumes** and select **Create a data volume**. The side panel expands with fields to define the volume.
   1. If you created volume snapshots previously, the option to import one becomes available. Click the toggle to **Import from snapshot**, and select a snapshot from the list.
   1. Specify a unique, meaningful name for your volume. For example, it can be a name that describes your compute or workload function. The volume name must begin with a lowercase letter. The name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-). Volume names must be unique the entire VPC infrastructure. You can edit the name later if you want. 
   1. Auto-delete feature is disabled by default. If you want the volume to be deleted when the attached virtual server instance is deleted, click the toggle to enable the feature.
   1. Resource group and location are inherited from the instance. These values can't be changed.
   1. You can specify optional user tags to associate with this volume. For more information about organizing resources with user tags, see [Working with tags](/docs/account?topic=account-tag&interface=ui).
   1. Select the encryption type. Provider-managed encryption is enabled by default on all volumes. You can also choose to create an envelop encryption with your own root keys. Encryption keys are created and maintained in Key Management Services ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}).
   1. Click **Next**.
   1. Select the storage profile.
      - For [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers), select the tile with the performance level that you require and specify the volume size in GBs. Volume sizes can be 10 - 16,000 GB. 
      - For [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) IOPS, specify the size of your volume and IOPS range based on the size of the volume. As you type the IOPS value, the UI shows the acceptable range. You can also click the **storage size** link to see the table that contains the size and IOPS ranges of the [custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom).
   1. Click **Save**.
5. When you're finished defining the volume, click **Save**. The volume is created and attached to the instance. You're returned to the instance details page. The new volume is shown in the list of storage volumes.

### Creating a stand-alone {{site.data.keyword.block_storage_is_short}} volume
{: #create-standalone-vol}
{: help}
{: support}

You can create a {{site.data.keyword.block_storage_is_short}} volume independent of virtual server provisioning and attach the volume to an instance later.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
1. Click **Create**.
1. On the {{site.data.keyword.block_storage_is_short}} provisioning page, review the **Location** information. The geography, region, and zone are inherited from the VPC (for example, North America, Dallas, Dallas-1). You can select a different zone in your location from the menu by clicking the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit"). 
1. In the **Details** section, you must specify the name of the volume and the resource group that the volume is to be added to. Optionally, you can add user and access management tags.
    1. Specify a meaningful name for your volume. For example, provide a name that describes your compute or workload function.
    
       The volume name must begin with a lowercase letter. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-). Volume names must be unique across the entire VPC infrastructure. You can edit the name later.
       {: important}

    1. Specify a [Resource group](/docs/vpc?topic=vpc-iam-getting-started&interface=ui#iam-resource-groups).
    1. Specify [user tags](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-user-tags) to organize your resources and for use by [backup policies](/docs/vpc?topic=vpc-backup-service-about).
    1. Specify [access management tags](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-mgt-tags) that were created in IAM to help you manage access to your volumes.
1. In the **Optional configurations** section, you can specify whether you want to create the volume with data from a snapshot. For more information, see [Create a stand-alone {{site.data.keyword.block_storage_is_short}} volume from a snapshot](#create-standalone-vol). Also, you can choose to apply a backup policy. Click **Apply** to see available policies and plans.
1. In the **Profile** section, you can specify the performance profile of your volume, its IOPS, and capacity.
    - For [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers), select the tile with the performance level that you require and specify the volume size in GBs. Volume sizes can be 10 - 16,000 GB. 
    - For [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) IOPS, specify the size of your volume and IOPS range based on the size of the volume. As you type the IOPS value, the UI shows the acceptable range. You can also click the **storage size** link to see the table that contains the size and IOPS ranges of the [custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom).
1. In the **Encryption at rest** section, you can choose to keep the encryption with IBM-managed keys that is enabled by default on all volumes. Or you can choose to use [your own encryption key](/docs/vpc?topic=vpc-block-storage-vpc-encryption) by selecting your key management service: {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}. To locate your encryption key, select one of the following options:
    - **Locate by Instance**:
       1. Select the data encryption instance from the list. If you don't have an instance yet, you can click the link to create one.
       1. Select the data encryption key that is stored within the {{site.data.keyword.keymanagementserviceshort}} instance to use for encrypting the volume.
    - **Locate by CRN**: enter the CRN of the customer root key to be used for encrypting the volume.
1. When you're finished, click **Create block storage volume**. 
    - If you're not ready to order yet or just looking for pricing information, you can add the information that you see in the side panel to an Estimate. For more information about how this feature works, see [Estimating your costs](/docs/billing-usage?topic=billing-usage-cost).
1. You're returned to the {{site.data.keyword.block_storage_is_short}} volumes page, where a message indicates that the volume is being created (volume status is _pending_). A second message displays when the volume is created (volume status is _available_).
1. To see details of the new volume, select the **View resource** link in the second message to go to the Volume details page.

When you refresh the list of Block Storage volumes in the console, the new volume appears at the beginning of the list of volumes. For stand-alone volumes, the Attachment Type column is blank (-). The **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions") at the end of a table row provides a link for [attaching a Block Storage volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage).

### Creating a stand-alone {{site.data.keyword.block_storage_is_short}} volume from a snapshot in the console
{: #create-standalone-vol-from-snapshot}

When you [create a {{site.data.keyword.block_storage_is_short}} volume](#create-standalone-vol), you can select a snapshot and use its data to create the new volume. The snapshots that are presented are in a stable state. The data volume that is created from the snapshot is not attached to an instance and appears in the [list of {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui). You can later attach the volume to an instance.

You can restore a boot volume from a "bootable" snapshot. The boot volume is restored when you create a virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
1. Select **Create**.
1. Specify the location of the volume, volume name, optional resource group and tags.
1. In the **Optional configurations** section, click **Import from snapshot**.
1. Click **Select snapshot** to create the volume with data from the selected snapshot. By default, the **Nonbootable** tab is selected that lists all data volume snapshots. To restore a boot volume, click the **Bootable** tab. **Attach volume to new virtual server instance** is selected by default. The boot volume is restored when the instance is created.
1. Click **Save**. Information about the snapshot is shown on the volume provisioning page, including the name, its size, the date when it was created, its source volume, and whether it is a bootable snapshot. Bootable snapshot information includes the operating system and image. The encryption is inherited from the snapshot and shown in the encryption section.
1. Click **Create block storage volume**.

## Creating {{site.data.keyword.block_storage_is_short}} volumes from the CLI
{: #creating-block-storage-cli}
{: cli}

You can create {{site.data.keyword.block_storage_is_short}} volumes by using the command-line interface (CLI).

### Before you begin
{: #before-creating-block-storage-cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

### Creating a {{site.data.keyword.block_storage_is_short}} volume from the CLI
{: #create-vol-cli}
{: help}
{: support}

Run the following command to create a {{site.data.keyword.block_storage_is_short}} volume. Provide a volume name, profile name, and the name of the availability zone in your region. For more information about {{site.data.keyword.block_storage_is_short}} profiles, see [Profiles](/docs/vpc?topic=vpc-block-storage-profiles). Extra options are shown in brackets.

Valid volume names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure. For example, if you create two volumes with the same name in the same account and region, a `volume name duplicate` error is triggered.
{: important}

```sh
ibmcloud is volume-create VOLUME_NAME PROFILE_NAME ZONE_NAME [--capacity CAPACITY] [--iops IOPS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--tags  TAG_NAME1,TAG_NAME2,...] [--json]
```
{: pre}

See the following example.

```bash
$ ibmcloud is volume-create demovolume1 custom us-east-1 --capacity 500 --iops 3000 --tags env:test,env:prod
Creating volume demovolume1 in resource group Default under account VPC 01 as user rtuser1@mycompany.com...

ID                                     r014-e45f9c8c-4655-4a3e-9d90-70c2d64d1746   
Name                                   demovolume1   
CRN                                    crn:v1:bluemix:public:is:us-east-1:a/a1234567::volume:r014-e45f9c8c-4655-4a3e-9d90-70c2d64d1746   
Status                                 pending   
Attachment state                       unattached   
Capacity                               500   
IOPS                                   3000   
Bandwidth(Mbps)                        6291   
Profile                                custom   
Encryption key                         -   
Encryption                             provider_managed   
Resource group                         defaults   
Created                                2023-07-24T16:20:52+00:00   
Zone                                   us-east-1   
Health State                           inapplicable   
Volume Attachment Instance Reference   -   
Active                                 false   
Busy                                   false   
Tags                                   env:test,env:prod 
```
{: screen}

Capacity, indicated in megabytes, can range from 10 - 16,000 GBs. If not specified, the default capacity is 100 GBs. IOPS values can be 100 - 48,000 IOPS, depending on the profile and volume size. If not specified, the IOPS value defaults to the valid configuration per volume profile. For more information, see the table of [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom).

The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure.
{: requirement}

Note the volume ID. You need to specify the ID when you attach {{site.data.keyword.block_storage_is_short}} to a virtual server instance, view {{site.data.keyword.block_storage_is_short}} volume details, or delete volumes.

The volume health state shows `inapplicable` while the volume status is `pending`. After the volume is created, the health state shows `OK`, unless the creation failed. For more information, see [{{site.data.keyword.block_storage_is_short}} volume health states](/docs/vpc?topic=vpc-block-storage-vpc-monitoring&interface=cli#block-storage-vpc-health-states).

User tags are added to identify the volume resource. When these tags are matched with the tags in a backup policy, the volume is backed up according to the schedule in the backup plan. For more information, see [Creating a backup policy](/docs/vpc?topic=vpc-create-backup-policy-and-plan).

### Creating a stand-alone {{site.data.keyword.block_storage_is_short}} volume with customer-managed encryption from the CLI
{: #encrypt-standalone-data-vol-cli}

To create a Block Storage volume with customer-managed encryption from the CLI, use the `ibmcloud is volume-create` command with the `--encryption-key` option. The `encryption_key` option needs a valid CRN for the root key in the key management service.

```sh
ibmcloud is volume-create VOLUME_NAME PROFILE_NAME ZONE_NAME [--encryption-key ENCRYPTION_KEY] [--capacity CAPACITY] [--iops IOPS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON]
```
{: pre}

The following example shows a volume with custom IOPS and capacity that is created with customer-managed encryption.

```sh
$ ibmcloud is volume-create demo-cli-volume custom us-east-1 --capacity 300 --iops 1500 --encryption-key crn:v1:bluemix:public:kms:us-east:a/a1234567:3b05b403-8f51-4dac-9114-c777d0a760d4:key:7a8a2761-08e3-455f-a348-144ed604bba9
Creating volume demo-cli-volume under account Test Account as user test.user@ibm.com...
                                          
ID                                     r014-3984600c-6f4d-4940-82de-519a867fa3c0   
Name                                   demo-cli-volume   
CRN                                    crn:v1:bluemix:public:is:us-east-1:a/a1234567::volume:r014-3984600c-6f4d-4940-82de-519a867fa3c0   
Status                                 pending   
Attachment state                       unattached   
Capacity                               300   
IOPS                                   1500   
Bandwidth(Mbps)                        3145   
Profile                                custom   
Encryption key                         crn:v1:bluemix:public:kms:us-east:a/a1234567:3b05b403-8f51-4dac-9114-c777d0a760d4:key:7a8a2761-08e3-455f-a348-144ed604bba9   
Encryption                             user_managed   
Resource group                         defaults   
Created                                2023-06-29T20:10:52+00:00   
Zone                                   us-east-1   
Health State                           inapplicable   
Volume Attachment Instance Reference   -   
Active                                 false
Busy                                   false   
Tags                                   - 
```
{: screen}

### Creating a stand-alone {{site.data.keyword.block_storage_is_short}} volume from a snapshot from the CLI
{: #create-vol-from-snapshot-cli}

Run the `ibmcloud is volume-create` command and specify the `--snapshot` option with the name or ID of the snapshot you're using to create the new volume. The volume is unattached, as indicated by the attachment state in the response.

This example creates the new volume from a snapshot that is specified by name.

```sh
$ ibmcloud is volume-create volume-4 general-purpose us-east-1 --snapshot snapshot-3
Creating volume volume-4 under account Test Account as user test.user@ibm.com...

ID                                     r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac
Name                                   volume-4
CRN                                    crn:v1:bluemix:public:is:us-east-1:a/a1234567::volume:r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac
Status                                 pending
Attachment state                       unattached
Capacity                               100
IOPS                                   3000
Bandwidth(Mbps)                        393
Profile                                general-purpose
Encryption key                         -
Encryption                             provider_managed
Resource group                         defaults
Created                                2023-06-29T16:14:59+00:00
Zone                                   us-east-1
Health State                           inapplicable
Volume Attachment Instance Reference   -
Active                                 false
Busy                                   false
Tags                                   -
```
{: screen}

For more information, see [ibmcloud is volume-create](/docs/vpc?topic=vpc-vpc-reference&interface=cli#volume-create) in the CLI reference.

### Creating a data volume as an attachment to an existing virtual server instance
{: #create-vol-as-attachment-cli}
{: cli}

You can use the `instance-volume-attachment-add` command to create a volume and attach it to an instance. The following example creates a volume with the `custom` profile, 100 GB capacity, and 100 IOPS.

```sh
ibmcloud is instance-volume-attachment-add acd-vol-attach1 my-instance-1 --profile custom --new-volume-name acd-vol-2 --iops 100 --capacity 100
Creating volume attachment acd-vol-attach1 for instance my-instance-1 under account Test Account as user test.user@ibm.com...
                     
ID                730f-5f63eb4d-2683-4dd6-a20a-5ab06b4061c6   
Name              acd-vol-attach1   
Volume            -   
Status            attaching   
Bandwidth(Mbps)   14   
Type              data   
Device            -   
Auto delete       false   
Created           2023-05-05T07:42:33+05:30   
```
{: codeblock}

For more information, see [ibmcloud is instance-volume-attachment-add](/docs/vpc?topic=vpc-vpc-reference&interface=cli#instance-volume-attachment-add) in the CLI reference.

### Creating an instance and adding user tags to volumes from the CLI
{: #create-instance-vol-cli}
{: cli}

You can specify user tags for boot and data volumes when you create a virtual server instance. Run the `instance-create` command and specify user tags in the `user_tags` option. You can also edit the tags later or add more tags to the specified volume. For more information, see the [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference).

In the example command,

* The boot volume is defined by the `--boot-volume` option that specifies the user tags `env:test4` and `env:dev4`.
* The data volume attachment is defined by the `--volume-attach` option and specifies the same user tags.

You can see the user tags when you look at the details of the boot or data volume after the instance is created.

```sh
ibmcloud is instance-create my-vsi-4 my-vpc-2 us-south-1 bx2-2x8 cli-subnet-1 --image ibm-centos-7-9-minimal-amd64-6 --keys my-keys4 --boot-volume '{"name": "boot-vol-4", "volume": {"name": "my-boot-vol-4", "profile": {"name": "general-purpose"},"user_tags": ["env:test4", "env:dev4"]}}' --volume-attach  '[{"name": "my-vol-att1", "volume": {"name":"my-vol-4", "profile": {"name": "general-purpose"}, "capacity": 10 ,"user_tags": ["env:test4", "env:dev4"]}}]'

Creating instance my-vsi-4 under account vpcdemo1 as user myuser@mycompany.com...

ID                                    efd06503-f514-4ea2-ba6c-2b9c715e6269
Name                                  my-vsi-4
CRN                                   crn:v1:bluemix:public:is:us-south-1:a/a1234567::instance:efd06503-f514-4ea2-ba6c-2b9c715e6269
Status                                pending
Availability policy on host failure   restart
Startable                             true
Profile                               bx2-2x8
Architecture                          amd64
vCPUs                                 2
Memory(GiB)                           8
Bandwidth(Mbps)                       4000
Volume bandwidth(Mbps)                1000
Network bandwidth(Mbps)               3000
Metadata service                      Enabled
                                      false

Image                                 ID                                          Name
                                      r006-c5471ef3-15f5-4468-964b-f9b4d3ca8120   ibm-centos-7-9-minimal-amd64-6

VPC                                   ID                                          Name
                                      r006-dd1c6831-cc4d-4c40-aab2-6b0c17f41424   my-vpc-2

Zone                                  us-south-1
Resource group                        ID                                 Name
                                      bdd96715c2a44f2bb60df4ff14a543f5   Default

Created                               2022-08-09T09:25:06+05:30
Boot volume                           ID   Name   Attachment ID                               Attachment name
                                      -    -      r006-1bcf5342-c12f-4908-ad0f-99218cd603b9   boot-vol-4

Data volumes                          ID   Name   Attachment ID                               Attachment name
                                      -    -      r006-c034f0f9-2269-44e4-a76a-4f4ad8c47b65   my-vol-att1
```
{: screen}

### Creating an instance template and adding user tags to volumes from the CLI
{: #create-instance-template-vol-cli}
{: cli}

You can specify user tags for boot and data volumes when you create an instance. Run the `instance-template-create` command and specify user tags in the `user_tags` parameter.

In the following example, the command adds two user tags to the instance template that are to be applied to the volumes.

```sh
ibmcloud is instance-template-create my-tpl-1 my-vpc-2 us-south-1 bx2-2x8 cli-subnet-1 --image ibm-centos-7-9-minimal-amd64-6 --keys my-keys4 --boot-volume '{"name": "boot-vol-1", "volume": {"name": "my-boot-vol-1", "profile": {"name": "general-purpose"},"user_tags": ["env:test1", "env:dev1"]}}' --volume-attach  '[{"name": "my-vol-att", "volume": {"name":"my-vol-1", "profile": {"name": "general-purpose"}, "capacity": 10 ,"user_tags": ["env:test1", "env:dev1"] }}]'

Creating instance template my-tpl-1 under account vpcdemo as user myuser@mycompany.com...

ID                          38ef87e9-3a4d-4d72-8450-86c16b76ae0d
Name                        my-tpl-1
CRN                         crn:v1:bluemix:public:is:us-south-1:a/a1234567::instance-template:38ef87e9-3a4d-4d72-8450-86c16b76ae0d
Resource group              Default
Image                       c5471ef3-15f5-4468-964b-f9b4d3ca8120
VPC                         dd1c6831-cc4d-4c40-aab2-6b0c17f41424
Zone                        us-south-1
Profile                     bx2-2x8
Metadata service            Enabled
                            false

Keys                        Key IDs
                            e7d18039-0e0e-4207-8598-de7cca949030

Boot volume                 Name            Capacity   Profile           IOPS   Attachment name   Auto delete   Tags
                            my-boot-vol-1   100        general-purpose   0      boot-vol-1     true          env:test1,env:dev1

Data volumes                ID   Name       Capacity   Profile           IOPS   Attachment name   Auto delete   Tags
                            -    my-vol-1   10         general-purpose   0      my-vol-att        false         env:test1,env:dev1

Primary Network Interface   Name      Subnet ID                                   Security Groups                             Allow source IP spoofing   Reserved IP Address   Reserved IP ID   Reserved IP Name   Reserved IP Auto Delete
                            primary   0617-c1f1e468-3283-4f44-9d08-3d57b77817f1   7c2db5ac-19e0-4d83-8226-e6178fedfe56   false                      -                     -                -                  -

Created                     2022-08-22T09:10:34+05:30

```
{: screen}

Use the instance template to create an instance and volumes with user tags. See the following example.

```sh
ibmcloud is instance-template-create --template my-tpl-1 --name my-vsi-3 --boot-volume '{"name": "boot-vol-3", "volume": {"name": "my-boot-vol-3", "profile": {"name": "general-purpose"},"user_tags": ["env:test3", "env:dev3"]}}' --volume-attach  '[{"name": "my-vol-att1", "volume": {"name":"my-vol-3", "profile": {"name": "general-purpose"}, "capacity": 10 ,"user_tags": ["env:test3", "env:dev3"] }}]'
```
{: pre}

## Creating {{site.data.keyword.block_storage_is_short}} volumes with the API
{: #creating-block-storage-api}
{: api}

You can create {{site.data.keyword.block_storage_is_short}} volumes by directly calling the [VPC REST APIs](/apidocs/vpc){: external}. For more information about the file shares VPC API, see the [VPC API reference](/apidocs/vpc).

### Before you begin
{: #block-storage-api-prereqs}

Define variables for the IAM token, API endpoint, and API version. For instructions, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

### Creating a data volume as part of instance provisioning with the API
{: #block-storage-create-instance-api}

Make a `POST /instances` request to create an instance, and define the volume by using the `volume_attachments` parameter. Specify a volume name, capacity, and profile. Also, specify `generation=2` in the request.

Valid volume names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure. For example, if you create two volumes with the same name in the same account and region, a `volume name duplicate` error is triggered.
{: important}

This example specifies customer-managed encryption and user tags for the boot and data volumes.

```sh
curl -X POST "$vpc_api_endpoint/v1/instances?version=2022-06-14&generation=2" -H "Authorization: $iam_token" -d '{
  "boot_volume_attachment": {
    "volume": {
      "encryption_key": {"crn": "crn:[...]"},
      "name": "my-boot-volume",
      "profile": {"name": "general-purpose"},
      "user_tags": {"env:test","env:prod"}}},
  "image": {"id": "9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366"},
  "keys": [{"id": "363f6d70-0000-0001-0000-00000013b96c"}],
  "name": "my-instance",
  "placement_target": {"id": "0787-8c2a09be-ee18-4af2-8ef4-6a6060732221"},
  "primary_network_interface": {
    "name": "my-network-interface",
    "subnet": {"id": "bea6a632-5e13-42a4-b4b8-31dc877abfe4"}},
  "profile": {"name": "bx2-2x8"},
  "volume_attachments": [
    {"volume": {
        "capacity": 1000,
        "encryption_key": {"crn": "crn:[...]"},
        "name": "my-data-volume",
        "profile": {"name": "5iops-tier"},
        "user_tags": {"env:test","env:prod"}}
    }],
  "vpc": {"id": "f0aae929-7047-46d1-92e1-9102b07a7f6f"},
  "zone": {"name": "us-south-1"}
}'
```
{: pre}

A successful response looks like this:

```json
{
  "bandwidth": 4000,
  "boot_volume_attachment": {
    "device": {
      "id": "a8a15363-a6f7-4f01-af60-715e85b28141"
    },
    "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/eb1b7391-2ca2-4ab5-84a8-b92157a633b0/volume_attachments/7389-a8a15363-a6f7-4f01-af60-715e85b28141",
    "id": "a8a15363-a6f7-4f01-af60-715e85b28141",
    "name": "my-boot-volume-attachment",
    "volume": {
      "crn": "crn:[...]",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/49c5d61b-41e7-4c01-9b7a-1a97366c6916",
      "id": "49c5d61b-41e7-4c01-9b7a-1a97366c6916",
      "name": "my-boot-volume"
    }
  },
  "created_at": "2022-06-15T16:11:57Z",
  "crn": "crn:[...]",
  "dedicated_host": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/dedicated_hosts/0787-8c2a09be-ee18-4af2-8ef4-6a6060732221",
    "id": "0787-8c2a09be-ee18-4af2-8ef4-6a6060732221",
    "name": "test-new",
    "resource_type": "dedicated_host"
  },
  "disks": [],
  "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/eb1b7391-2ca2-4ab5-84a8-b92157a633b0",
  "id": "eb1b7391-2ca2-4ab5-84a8-b92157a633b0",
  "image": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/images/9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366",
    "id": "9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366",
    "name": "my-image"
  },
  "memory": 8,
  "name": "my-instance",
  "network_interfaces": [
    {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-4aa2-a8d7-703aac843651/network_interfaces/7ca88dfb-8962-469d-b1de-1dd56f4c3275",
      "id": "7ca88dfb-8962-469d-b1de-1dd56f4c3275",
      "name": "my-network-interface",
      "primary_ipv4_address": "10.0.0.32",
      "resource_type": "network_interface",
      "subnet": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/7389-bea6a632-5e13-42a4-b4b8-31dc877abfe4",
        "id": "bea6a632-5e13-42a4-b4b8-31dc877abfe4",
        "name": "my-subnet"
      }
    }
  ],
  "placement_target": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/dedicated_hosts/0787-8c2a09be-ee18-4af2-8ef4-6a6060732221",
    "id": "0787-8c2a09be-ee18-4af2-8ef4-6a6060732221",
    "name": "test-new",
    "resource_type": "dedicated_host"
  },
  "primary_network_interface": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-4aa2-a8d7-703aac843651/network_interfaces/7ca88dfb-8962-469d-b1de-1dd56f4c3275",
    "id": "7ca88dfb-8962-469d-b1de-1dd56f4c3275",
    "name": "my-network-interface",
    "primary_ipv4_address": "10.0.0.32",
    "resource_type": "network_interface",
    "subnet": {
      "crn": "crn:[...]",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/bea6a632-5e13-42a4-b4b8-31dc877abfe4",
      "id": "bea6a632-5e13-42a4-b4b8-31dc877abfe4",
      "name": "my-subnet"
    }
  },
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/instance/profiles/bx2-2x8",
    "name": "bx2-2x8"
  },
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/4bbce614c13444cd8fc5e7e878ef8e21",
    "id": "4bbce614c13444cd8fc5e7e878ef8e21",
    "name": "Default"
  },
  "status": "running",
  "status_reasons": [],
  "vcpu": {
    "architecture": "amd64",
    "count": 2
  },
  "volume_attachments": [
    {
      "device": {
        "id": "a8a15363-a6f7-4f01-af60-715e85b28141"
      },
      "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-4aa2-a8d7-703aac843651/volume_attachments/7389-a8a15363-a6f7-4f01-af60-715e85b28141",
      "id": "a8a15363-a6f7-4f01-af60-715e85b28141",
      "name": "my-boot-volume-attachment",
      "volume": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/49c5d61b-41e7-4c01-9b7a-1a97366c6916",
        "id": "49c5d61b-41e7-4c01-9b7a-1a97366c6916",
        "name": "my-boot-volume"
      }
    },
    {
      "device": {
        "id": "e77125cb-4df0-4988-a878-531ae0ae0b70"
      },
      "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-4aa2-a8d7-703aac843651/volume_attachments/7389-e77125cb-4df0-4988-a878-531ae0ae0b70",
      "id": "e77125cb-4df0-4988-a878-531ae0ae0b70",
      "name": "my-volume-attachment-1",
      "volume": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/2cc091f5-4d46-48f3-99b7-3527ae3f4392",
        "id": "2cc091f5-4d46-48f3-99b7-3527ae3f4392",
        "name": "my-data-volume"
      }
    }
  ],
  "vpc": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/f0aae929-7047-46d1-92e1-9102b07a7f6f",
    "id": "f0aae929-7047-46d1-92e1-9102b07a7f6f",
    "name": "my-vpc"
  },
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: screen}

### Creating a stand-alone {{site.data.keyword.block_storage_is_short}} volume with the API
{: #block-storage-create-vol-api}

Make a `POST /volumes` request to create a volume. Specify a name, IOPS, capacity, the profile, and zone. Also, specify `generation=2` in the request.

The following example created a `custom` volume with 50 MB capacity and 100 IOPS in the `us-south` region. The request also specifies a customer root key for customer-managed encryption and a resource group.

```sh
curl -X POST "$vpc_api_endpoint/v1/volumes?version=2022-06-14&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "name": "my-volume-4",
      "iops": 100,
      "capacity": 50,
      "zone": {"name": "us-south-2"},
      "profile": {"name": "custom"},
      "encryption_key":{"crn":"crn:[...]"},
      "resource_group": {"id": "2d1bb5a8-40a8-447a-acf7-0eadc8aeb054"}
    }'
```
{: pre}

A successful response looks like the following example.

```json
{
  "capacity": 50,
  "created_at": "2022-06-14T23:16:53.000Z",
  "crn": "crn:[...]",
  "encryption": "user_managed",
  "encryption_key": {
    "crn": "crn:[...]"
  },
  "health_reasons": [],
  "health_state": "ok",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/2d1bb5a8-40a8-447a-acf7-0eadc8aeb054",
  "id": "2d1bb5a8-40a8-447a-acf7-0eadc8aeb054",
  "iops": 100,
  "name": "my-volume-4",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/custom",
    "name": "custom"
  },
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/4bbce614c13444cd8fc5e7e878ef8e21",
    "id": "4bbce614c13444cd8fc5e7e878ef8e21",
    "name": "Default"
  },
  "status": "available",
  "status_reasons": [],
  "volume_attachments": [],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
    "name": "us-south-2"
  }
}
```
{: screen}

For more information about volume creation with the API, see [Creating Block Storage volumes](/docs/vpc?topic=vpc-creating-block-storage&interface=api#creating-block-storage-api) and the API reference for [creating a volume](/apidocs/vpc/latest#create-volume).

### Creating a data volume from a snapshot of an unattached volume with the API
{: #block-storage-create-vol-snapshot-api}

You can specify a snapshot ID in a `POST /volumes` call to create a stand-alone data volume.

Data is fully restored when you attach the data volume to a virtual server instance. For more information, see [About restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-restore-concepts). For an example API call, see [Restoring a data volume from a snapshot of an unattached volume](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-restore-unattached-api).

## Creating {{site.data.keyword.block_storage_is_short}} volumes with Terraform
{: #creating-vol-terraform}
{: terraform}
{: help}
{: support}

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

VPC infrastructure services use a specific regional endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the appropriate region in the provider block in the `provider.tf` file.

See the following example of targeting a region other than the default `us-south`.

```terraform
provider "ibm" {
   region = "eu-de"
}
```
{: screen}

Valid volume names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure. For example, if you create two volumes with the same name in the same account and region, a `volume name duplicate` error is triggered.
{: important}

### Creating stand-alone {{site.data.keyword.block_storage_is_short}} volumes with Terraform
{: #creating-standalone-vol-terraform}

To create a {{site.data.keyword.block_storage_is_short}} volume, use the `ibm_is_volume` resource. The following example creates a volume with 4 TB capacity and the `10iops-tier` performance profile.

```terraform
resource "ibm_is_volume" "example" {
  name     = "example-volume"
  profile  = "10iops-tier"
  zone     = "us-south-1"
  capacity = 4000
}
```
{: codeblock}

The following example creates a volume with a `custom` profile. The volume that is created has 200 MB capacity and can perform 1000 IOPS.

```terraform
resource "ibm_is_volume" "example" {
  name           = "example-volume"
  profile        = "custom"
  zone           = "us-south-1"
  iops           = 1000
  capacity       = 200
  encryption_key = "crn:v1:bluemix:public:kms:us-south:a/a1234567:e4a29d1a-2ef0-42a6-8fd2-350deb1c647e:key:5437653b-c4b1-447f-9646-b2a2a4cd6179"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume){: external}.

### Creating a stand-alone {{site.data.keyword.block_storage_is_short}} volume from a snapshot with Terraform
{: #create-vol-from-snapshot-terraform}

To create a {{site.data.keyword.block_storage_is_short}} volume from a snapshot, use the `ibm_is_volume` resource. The following example creates a volume based on a snapshot that is identified by its ID.

```terraform
resource "ibm_is_volume" "storage" {
  name            = "example-volume"
  profile         = "general-purpose"
  zone            = "us-south-1"
  source_snapshot = ibm_is_snapshot.example.id
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume){: external}.

### Creating a boot volume from a snapshot as part of instance provisioning with Terraform
{: #block-storage-create-instance-terraform}

To create a data volume from a snapshot when you create a virtual server instance, use the `ibm_is_instance` resource. The following example creates a virtual server instance with the name "example-vsi-restore". The boot volume is defined in the `boot_volume` argument with its name, its source snapshot, and user tags.

```terraform
resource "ibm_is_snapshot" "example" {
  name          = "example-snapshot"
  source_volume = ibm_is_instance.example.volume_attachments[0].volume_id
}

resource "ibm_is_instance" "example" {
  name    = "example-vsi-restore"
  profile = "cx2-2x4"
  boot_volume {
    name     = "boot-restore"
    snapshot = ibm_is_snapshot.example.id
    tags     = ["tags-0"]
  }
  primary_network_interface {
    subnet = ibm_is_subnet.example.id
  }
  vpc  = ibm_is_vpc.example.id
  zone = "us-south-1"
  keys = [ibm_is_ssh_key.example.id]
  network_interfaces {
    subnet = ibm_is_subnet.example.id
    name   = "eth1"
  }
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_instance](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance){: external}.

### Creating a data volume to add to an existing instance with Terraform
{: #create-vol-for-instance-terraform}

To create a data volume and attach it to a virtual server instance, use the `ibm_is_instance_volume_attachment` resource.
The following example creates a 20 MB data volume with a `general-purpose` IOPS profile and attaches it to the `ibm_is_instance.example.id` instance.

```terraform
rresource "ibm_is_instance_volume_attachment" "example" {
  instance = ibm_is_instance.example.id

  name                               = "example-vol-att-1"
  profile                            = "general-purpose"
  capacity                           = "20"
  delete_volume_on_attachment_delete = true
  delete_volume_on_instance_delete   = true
  volume_name                        = "example-vol-1"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_instance_volume_attachment](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance_volume_attachment){: external}.

## Next steps
{: #next-step-creating-block-storage-vpc}

When you refresh the {{site.data.keyword.block_storage_is_short}} volumes page, the new volume appears at the beginning of the list of volumes. If the volume was created successfully, it shows a status of Available. For stand-alone volumes, the Attachment Type column is blank (-). The **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions") at the end of a table row provides a link for [attaching a {{site.data.keyword.block_storage_is_short}} volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage&interface=ui#attach).

You can continue creating more {{site.data.keyword.block_storage_is_short}} volumes or manage existing volumes.

* [View details about a {{site.data.keyword.block_storage_is_short}} volume.](/docs/vpc?topic=vpc-viewing-block-storage)
* [Detach a volume from a virtual server instance.](/docs/vpc?topic=vpc-managing-block-storage#detach)
* [Delete a {{site.data.keyword.block_storage_is_short}} volume.](/docs/vpc?topic=vpc-managing-block-storage#delete)
