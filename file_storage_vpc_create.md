---

copyright:
  years: 2021, 2025
lastupdated: "2025-07-23"

keywords: file share, file storage, virtual network interface, encryption in transit, profiles, 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating file shares and mount targets
{: #file-storage-create}

Create file shares and mount targets in the console, CLI, API, or Terraform.
{: shortdesc}

Before you get started, and try to create mount targets for file shares, make sure that you created a [VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{: important}

You can create file shares and mount targets either of the following ways:

* Create a file share and mount target together,
* Create a file share and add mount target later.

When you create a mount target, its transit encryption type must reflect the share's allowed transit encryption modes. You can create multiple mount targets for the share if it's to be used by resources in different VPCs. You can create one mount target per VPC for the file share.

## Creating a file share in the console
{: #file-storage-create-ui}
{: ui}

In the {{site.data.keyword.cloud_notm}} console, you can create a file share with or without a mount target. However, you need to create a mount target when you want to mount the share on a virtual server instance.

### Creating a file share in the console
{: #fs-create-share-target-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**. A list of file shares displays.

1. On the File storage shares for VPC page, click **Create**. Select **Create file share**.

1. Enter the following information. 

   | Field | Value |
   |-------|-------|
   | **Location** | Choose the geography, region, and zone where you want to create the file share. Location information can be inherited from the VPC, for example, North America, Dallas (us-south), us-south-2. |
   | **Details** | |
   | Name  | Specify a meaningful name for your file share. The file share name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want.
   | Resource Group | Use the default resource group or specify a [Resource group](/docs/vpc?topic=vpc-iam-getting-started&interface=ui#iam-resource-groups). Resource groups help organize your account resources for access control and billing purposes. |
   | Tags (optional) | Enter any user tags to apply to this file share. As you type, existing tags appear that you can select. For more information about tags, see [Add user tags to a file share](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#fs-add-user-tags). |
   | Access Management Tags (optional) | Enter access management tags that you created in IAM to apply them to this file share. For more information about access management tags, see [Access management tags for file shares](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-about-mgt-tags). |
   | Profile | All file shares are created with the dp2 profile. For more information, see [file Storage profiles](/docs/vpc?topic=vpc-file-storage-profiles). \n Select the size and IOPS for your file share. You can increase the capacity later, and you can also adjust the IOPS as needed. |
   | Mount target access mode  | Select how you want to manage access to this file share: |
   |  | Security group: Access to the file share is based on [security group](/docs/vpc?topic=vpc-using-security-groups#sg-getting-started) rules. This option can be used to restrict access to specific virtual server instances. You can also use this option if you want to mount the file share to a virtual server instance in another zone. This option is recommended as you have more control over who can access the data that is stored on the file share. When you choose this type of access, you can also specify the allowed transit encryption modes. |
   |  | Virtual private cloud: Access to the file share is granted to any virtual server instance in the same region. Cross-zone mounting, encryption in transit, cross-zone mounting, and snapshots are not supported when this access mode is selected.  |
   | Allowed transit encryption modes| As the share owner, you can specify how you want clients within your account and authorized accounts to connect to your file share. You can select *none* if you do not want them to use encryption in transit, and *user-managed* if you want them to use encryption in transit. If you select both, then the transit encryption type of the first mount target decides the transit encryption types of all future mount targets within the account. |
   {: caption="Values for creating a file share" caption-side="top"}

1. The creation of mount targets is optional. You can skip this step if you do not want to create a mount target now. Otherwise, click **Create**. You can create one mount target per VPC per file share. 

   - If you selected security group as the access mode, enter the information as described in the Table 2. This action creates and attaches a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to your mount target that identifies the file share with a reserved IP address and applies the rules of the selected Security group. This mount target supports encryption-in-transit and cross-zone mounting.
      1. Provide a mount target name. The name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want.
      2. Select an available VPC. The list includes only those VPCs with a subnet in the selected location. The location selection is inherited from the file share (for example, us-south-2).
      3. A default virtual network interface is generated. You can customize it by clicking the Edit icon ![Edit icon](/images/edit.png). You can change the name or subnet if you have multiple subnets in the zone.
      4. Click **Next**.
      5. **Encryption in transit** is disabled by default. Click the toggle to change the preset value. For more information about this feature, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit). 
      6. Then, click **Next**. 
      7. Review your selection, and either click **Back** to return and update your choices or click **Create**.

   - If you selected VPC as the access mode, provide a name for the mount target and select a VPC from the list. This mount target can be used to mount the file share on any virtual server instance of the selected VPC in the same zone as the file share. Cross-zone mounting is not supported.

1. Encryption at rest. By default, all file shares are encrypted by IBM-managed keys. You can also choose to create an envelop-encryption for your shares with your own keys. If you want to use your own keys, select one of the [key management services](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok).

   | Field | Value |
   |------|------|
   | Encryption | To use customer-managed encryption, select either {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}. The key management service (KMS) instance includes the root key that is imported to or created in that KMS instance. |
   | Encryption service instance | If you provisioned multiple KMS instances in your account, select the one that includes the root key that you want to use for customer-managed encryption. |
   | Key name | Select the root key within the KMS instance that you want to use for encrypting the share. |
   | Key ID | The field shows the key ID that is associated with the data encryption key that you selected. |
   {: caption="Values for customer-managed encryption for file shares." caption-side="bottom"}

1. When all the required information is entered, click **Create file share**. You return to the {{site.data.keyword.filestorage_vpc_short}} page, where a message indicates that the file share is provisioning. When the transaction completes, the share status changes to **Active**.
   
If you're not ready to order yet or just looking for pricing information, you can add the information that you see in the side panel to an Estimate. For more information about how this feature works, see [Estimating your costs](/docs/account?topic=account-cost).    
{: tip}

### Creating a mount target in the console
{: #fs-create-mount-target-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.

2. Select a file share from the list.

3. On the File shares details page, under Mount targets, click **Create**.

   You must have at least one VPC to create a mount target. If you don't have one, first [create a VPC](/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc).
   {: requirement}

4. Depending on the [mount target access mode](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-mount-access-mode) of the share, the **Create mount target** form looks different.

   - If the share has security group access mode, enter the following information. This action creates and attaches a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to your mount target that identifies the file share with a reserved IP address and applies the rules of the selected [security group](/docs/vpc?topic=vpc-using-security-groups#sg-getting-started). This mount target supports encryption-in-transit and cross-zone mounting.
     1. Provide a mount target name. The name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want.
     2. Select an available VPC. The list includes only those VPCs with a subnet in the selected location. The location selection is inherited from the file share (for example, us-south-2).
     3. A default virtual network interface is generated. You can customize it by clicking the Edit icon ![Edit icon](/images/edit.png). You can change the name or subnet if you have multiple subnets in the location.
     4. Click **Next**.
     5. **Encryption in transit** is disabled by default. Click the toggle to change the preset value. For more information about this feature, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit).
     6. Then, click **Next**. 

   - If the share has VPC as the access mode, provide a name for the mount target and select a VPC from the list. This mount target can be used to mount the file share on any virtual server instance of the selected VPC in the same zone as the file share. Cross-zone mounting is not supported.

5. Click **Create**.

## Creating a file share from the CLI
{: #file-storage-create-cli}
{: cli}

### Before you begin
{: #before-creating-file-storage-cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

### Gathering information from the CLI
{: #fs-vpc-getinfo-cli}

Before you run the `ibmcloud is share-create` command, you can gather information that you need for provisioning a share by viewing information about other file shares, mount targets, and file storage profiles.

| Details             |  Listing options                           | What it provides                         |
|---------------------|--------------------------------------------|------------------------------------------|
| File shares         | `ibmcloud is shares`                       | List all shares in a region.             |
| File share details  | `ibmcloud is share SHARE_ID`               | Review details of a share.               |
| File share snapshots | `ibmcloud is share-snapshots` | List available snapshots in the zone. |
| File share profiles | `ibmcloud is share-profiles` | List all file share profiles in a region. Only `dp2` can be used to create file shares.|
| Mount targets       | `ibmcloud is share-mount-targets SHARE_ID` | List all mount targets for a file share. |
| Subnets             | `ibmcloud is subnets`                      | List all subnets.                        |
| Reserved IP addresses | `ibmcloud is subnet-reserved-ips`   | List all reserved IP addresses in the subnet. |
| Security Groups     | `ibmcloud is security-groups`              | List all security groups.                | 
{: caption="Details for creating file shares." caption-side="top"}

### Creating a file share without a mount target from the CLI
{: #fs-create-share-cli}

You can use the `ibmcloud is share-create` command to provision a file share in your selected zone with the `dp2` profile, with your specific capacity and IOPS values. The following example shows how to create 1000-GB file share with 1000 IOPS in the us-south-2 zone. This file share is created with the default security group access mode and with provider-managed encryption.

```sh
$ ibmcloud is share-create --name my-file-share --zone us-south-1 --profile dp2 --size 40 --iops 100
Creating file share my-file-share under account Test Account as user test.user@ibm.com...
                                      
ID                                 r134-4c4bd25f-4ecf-4636-a99a-c80dff3e8910    
Name                               my-file-share   
CRN                                crn:v1:staging:public:is:ussouth1:a/efe5afc483594adaa8325e2b4d1290df::share:r1344c4bd25f-4ecf-4636-a99a-c80dff3e8910    
Lifecycle state                    pending   
Access control mode                security_group   
Accessor binding role              none   
Allowed transit encryption modes   ipsec,none
Zone                               us-south-1   
Profile                            dp2   
Size(GB)                           40   
IOPS                               100   
Encryption                         provider_managed   
Mount Targets                      ID                          Name      
                                   No mounted targets found.      
                                      
Resource group                     ID                                 Name      
                                   8302a7a06e7440d6927383cd7c06e7d2   Default      
                                      
Created                            2025-07-23T12:21:30+05:30   
Replication role                   none   
Replication status                 none   
Replication status reasons         Status code   Status message      
                                   -             -      
                                      
Snapshot count                     0   
Snapshot size                      0   
Source snapshot                    -    
```
{: screen}

Security group access mode is the default and recommended setting. However, you can choose to create a file share with the VPC access mode that allows every Compute host in the VPC to mount the file share. See the following example.

```sh
$ ibmcloud is share-create --name my-vpc-file-share --zone us-south-2 --profile dp2 --size 1000 --iops 500 --access-control-mode vpc
Creating file share my-vpc-file-share under account Test Account as user test.user@ibm.com...
                                
ID                                 r134-83100dcb-24d8-45a6-91f3-256e5c17233f       
Name                               my-vpc-file-share   
CRN                                crn:v1:staging:public:is:us-south-2:a/efe5afc483594adaa8325e2b4d1290df::share:r134-83100dcb-24d8-45a6-91f3-256e5c17233f    
Lifecycle state                    pending   
Access control mode                vpc   
Accessor binding role              none
Allowed transit encryption modes   ipsec,none
Zone                               us-south-2   
Profile                            dp2   
Size(GB)                           1000   
IOPS                               500  
Encryption                         provider_managed   
Mount Targets                      ID                                 Name      
                                   No mounted targets found.      
                                
Resource group                     ID                                 Name      
                                   11caaa983d9c4beb82690daab08717e9  Default      
                                
Created                            2025-04-18T19:22:41+00:00   
Replication role                   none   
Replication status                 none   
Replication status reasons         Status code   Status message      
                                   -             -      
                                
Snapshot count                     0   
Snapshot size                      0   
Source snapshot                    -    
```
{: screen}

For more information about the command options, see [`ibmcloud is share-create`](/docs/vpc?topic=vpc-vpc-reference#share-create).

### Creating a mount target for a file share from the CLI
{: #fs-create-mount-target-cli}

To create a mount target for the file share, run the `share-mount-target-create` command. Before you begin, gather some necessary information.

When you create a mount target, you must specify the file share that it is for. You can use the file share's name or ID. You must specify the VPC, too, either with its ID or name. The VPC must be unique to each mount target. You must also specify the security access group that's going to be used to manage access to the share. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all servers where you want to mount the share.

Lastly, you must specify values for the options that are needed to create a [virtual network interface](/docs/vpc?topic=vpc-vni-about) for the mount target. Use the appropriate CLI commands to list the available [subnets](/docs/vpc?topic=vpc-vpc-reference#subnets-list), [reserved IP addresses in a subnet](/docs/vpc?topic=vpc-vpc-reference#subnet-reserved-ips-list), [security groups](/docs/vpc?topic=vpc-vpc-reference#security-groups-list) to get the information that you need.

The following example creates a mount target with a virtual network interface for a file share that has security group access mode.

```sh
$ ibmcloud is share-mount-target-create my-file-share --subnet my-subnet --name my-cli-share-mount-target-1 --vni-name my-share-vni-1 --vni-sgs my-sg --resource-group-name Default --vpc my-vpc --transit-encryption none
Mounting target for share r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6 under account Test Account as user test.user@ibm.com...
                               
ID                          r006-dd497561-c7c9-4dfb-af0a-c84eeee78b61   
Name                        my-cli-share-mount-target-1   
VPC                         ID                                          Name      
                            r006-6e8fb140-5668-45b8-b98a-d5cb0e0bf39b   my-vpc      
                               
Access control mode         security_group   
Resource type               share_mount_target   
Virtual network interface   ID                                          Name             Protocol State Filtering Mode
                            r006-13c070d8-d038-49c6-95f5-e8503c5595e3   my-share-vni-1   auto   
                               
Lifecycle state             pending   
Created                     2025-07-22T22:15:15+00:00
Mount path                  -    
Transit Encryption          none     
   
```
{: screen}

The following example creates a mount target for a file share that has VPC access mode.

```sh
$ ibmcloud is share-mount-target-create my-vpc-file-share --vpc cli-vpc-3 --name my-vpc-mount-targetnfs4 --transit-encryption none
Mounting target for share r134-10e82e16-ff7f-4ca4-b543-d24084fc03cf under account Test Account as user test.user@ibm.com...
                         
ID                        r134-71fd953c-8e49-48e8-ab49-5977c324a365   
Name                      my-vpc-mount-target   
VPC                       ID                                          Name      
                          r134-060fbfe6-4d0f-47b0-b9c5-94da8e719e22   cli-vpc-3      
                         
Access control mode       vpc   
Resource type             share_mount_target
Virtual network interface   
Lifecycle state           pending   
Created                   2025-07-22T22:15:15+00:00
Mount path                -    
Transit Encryption        none   
```
{: screen}

For more information about the command options, see [`ibmcloud is share-mount-target-create`](/docs/vpc?topic=vpc-vpc-reference#share-mount-target-create).

### Creating a file share with a mount target from the CLI
{: #fs-create-share-target-cli}

You can create a file share with one or more mount targets in one step by using the `ibmcloud is share-create` command. You need to provide the zone name, the [file share profile](/docs/vpc?topic=vpc-file-storage-profiles), the file share size, and the IOPS. You can also specify a name, user tags, and even the initial owner UID. To create the mount target, you need to provide the mount target information in JSON format.

The following example shows how to create a file share with 40 GB capacity and 100 IOPS in the `us-south-1` zone. The file share is tagged with `env:dev` and has security group access control mode. The file share can be mounted on authorized virtual servers by using the mount target `my-target1`.

```sh
$ ibmcloud is share-create --name my-fs-cli-1 --profile dp2 --zone us-south-1 --size 40 --mount-targets '[{ "name":"my-target1", "virtual_network_interface":{"name":"my-fs-cli-vni", "primary_ip":{"address":"12345","auto-delete":true,"name":"vni-target-1"},"security_groups":[{"id":"r134-d1fef8e0-8e16-41a7-bca5-84e2029deb05"}],"subnet":{"id":"0726-3d6502c8-baa1-4632-a5d9-d4b015aa93c1"}}}]'
Creating file share my-fs-cli-1 under account Test Account as user test.user@ibm.com...
                                
ID                               r134-5a544f6d-5fb9-4f31-acac-1fd13a5531bf   
Name                             my-fs-cli-1   
CRN                              crn:v1:staging:public:is:us-south-1:a/efe5afc483594adaa8325e2b4d1290df::share:r134-5a544f6d-5fb9-4f31-acac-1fd13a5531bf   
Lifecycle state                  pending   
Access control mode              security_group
Accessor binding role            none
Allowed transit encryption modes ipsec,none
Zone                             us-south-1   
Profile                          dp2   
Size(GB)                         40   
IOPS                             100      
Encryption                       provider_managed   
Mount Targets                    ID                                          Name      
                                 r134-68c20e6b-58d6-48c2-8c9c-564c8eed72db   my-target1      
                                
Resource group                   ID                                          Name      
                                 11caaa983d9c4beb82690daab08717e9            Default      
                                
Created                          2025-07-23T15:53:17+05:30                                   
Replication role                 none 
Replication status               none   
Replication status reasons       Status code   Status message      
                                 -             -      

Snapshot count                   0   
Snapshot size                    0   
Source snapshot                  -                                   
```
{: screen}

The following example creates a file share with VPC access mode and a mount target that can be used by any virtual server instance within the VPC.

```sh
$ ibmcloud is share-create --name my-file-share-8 --zone us-south-1 --profile dp2 --size 40 --iops 2000 --user-tags env:dev --mount-targets '[{ "name": "my-new-mount-target","vpc": {"name": "my-vpc"}}]'
Creating file share my-file-share-8 under account Test Account as user test.user@ibm.com...

ID                           r006-95ec87ba-c5fd-4178-a114-2a55c4d907d4   
Name                         my-file-share-8   
CRN                          crn:v1:bluemix:public:is:us-south-1:a/a1234567::share:r006-95ec87ba-c5fd-4178-a114-2a55c4d907d4   
Lifecycle state              pending   
Access control mode          vpc   
Accessor binding role        none   
Zone                         us-south-1   
Profile                      dp2   
Size(GB)                     40   
IOPS                         2000   
User Tags                    env:dev   
Encryption                   provider_managed   
Mount Targets                ID                                          Name      
                             r006-8b917757-ad19-4bec-8417-83157b047cea   my-new-mount-target      
                                
Resource group               ID                                 Name      
                             6edefe513d934fdd872e78ee6a8e73ef   Default    
                                
Created                      2025-04-18T19:36:35+00:00 

Replication role             none   
Replication status           none   
Replication status reasons   Status code   Status message      
                             -             -      
                                
Snapshot count               0   
Snapshot size                0   
Source snapshot              -       
```
{: screen}

### Creating a file share with customer-managed encryption from the CLI
{: #fs-create-encrypted-share-cli}

By default, {{site.data.keyword.filestorage_vpc_short}} shares are encrypted with IBM-managed encryption. However, you can also create an envelop-encryption for your file shares by using one of the supported key management services to create or import your own root keys. For more information, see [Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption)

For more information about how to create a file share with customer-managed encryption, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-byok-encryption&interface=cli#fs-byok-cli).

### Creating a file share with a replica in another zone from the CLI
{: #fs-create-share-with-replica-cli}

For more information about how to create a file share with a replica simultaneously, see [Create a file share with replication from the CLI](/docs/vpc?topic=vpc-file-storage-create-replication&interface=cli#fs-create-new-share-replica-cli).

### Creating a file share from a snapshot from the CLI
{: #fs-create-share-from-snapshot-cli}

You can use a snapshot of a file share to create another file share in the same zone. To create a file share based on a snapshot, run the `ibmcloud is share-create` command.

You can specify the name, the ID, or the CRN of the snapshot. If you choose to specify the name of the snapshot, you need to also specify the name or the ID of the file share that the snapshot belongs to. Snapshot names must be unique at the file share level, but another file share can have a snapshot with the same name.

```sh
ibmcloud is share-create --snapshot r026-7647ba64-9728-4bb4-be2f-d958f738fdae 
```
{: pre}

For more information about how to create a file share from a snapshot with other examples, see [Restoring data from a file share snapshot](/docs/vpc?topic=vpc-fs-snapshots-restore&interface=cli#fs-snapshots-restore-CLI).

### Adding supplemental IDs when you create a file share with the CLI
{: #fs-add-supplemental-id-cli}

With the CLI, you can set `UID` and `GID` values for the `--initial-owner-uid` and `--initial-owner-gid` property to control access to your file shares. Wherever you mount the file share, the root folder uses that user ID and group ID owner. You can set the `UID` or `GID`, or both when you create a share.

If you change the supplemental IDs (UID or GID) from the virtual server instance, it is not possible to determine that it was changed. As a result, `initial_owner` changes only in the file storage system, but the change isn't reflected in the CLI or API responses.
{: note}

Table 1 shows UID and GID values that you can set and values that are reserved.

| ID value | Description |
|----------|-------------|
| **UID** | |
| UID 0 | Reserved for root. |
| UID 1–99 | Reserved for predefined accounts. |
| UID 100–999 | Reserved by the system for administrative system accounts and groups. |
| UID 1000–10000 | Used by applications account. |
| UID 10000+ | Available for user accounts. |
| **GID** | |
| GID 0 | Reserved for root. |
| GID 1–99 | Reserved for the system and application use. |
| GID 100+ | Allocated for the user’s group. |
{: caption="Unix/Linux&reg; supplemental ID values." caption-side="top"}

To set supplemental IDs when you create a share, run the `ibmcloud is share-create` command and specify the `--initial-owner-gid` and `--initial-owner-gid` properties with the supplemental IDs. See the following example.

```sh
$ ibmcloud is share-create --name my-file-share --zone us-south-2 --profile dp2 --size 1000 --iops 1000 --initial-owner-gid 101 --initial-owner-uid 10001
Creating file share my-file-share under account Test Account as user test.user@ibm.com...
                                
ID                                 r134-346811e7-cf29-4baa-af77-909fb549d1e6   
Name                               my-file-share   
CRN                                crn:v1:staging:public:is:us-south-2:a/efe5afc483594adaa8325e2b4d1290df::share:r134-346811e7-cf29-4baa-af77-909fb549d1e6   
Lifecycle state                    pending   
Access control mode                security_group   
Accessor binding role              none   
Allowed transit encryption modes   ipsec,none   
Zone                               us-south-2   
Profile                            dp2   
Size(GB)                           1000   
IOPS                               1000   
Encryption                         provider_managed   
Mount Targets                      ID                          Name      
                                   No mounted targets found.      
                                      
Resource group                     ID                                 Name      
                                   11caaa983d9c4beb82690daab08717e9   Default      
                                      
Created                            2025-07-23T12:28:50+05:30   
Replication role                   none   
Replication status                 none   
Replication status reasons         Status code   Status message      
                                   -             -      
Snapshot count                     0
Snapshot size                      0    
Source snapshot                    -    
```
{: screen}

## Creating a file share with the API
{: #file-storage-create-api}
{: api}

You can create file shares and mount targets by directly calling the REST APIs.

### Before you begin 
{: #fs-api-prereqs}

Set up your API environment. Define variables for the IAM token, API endpoint, and API version. For instructions, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

You must provide the `generation` parameter and specify `generation=2`. For more information, see **Generation** in the [Virtual Private Cloud API reference](/apidocs/vpc/latest#api-generation-parameter).
{: requirement}

A good way to learn more about the API is to click **Get sample API call** on the provisioning pages in {{site.data.keyword.cloud_notm}} console. You can view the correct sequence of API requests and better understand actions and their dependencies.
{: tip}

### Creating a file share with the API
{: #fs-create-file-share-api}

Make a `POST /shares` request to create a file share. Specify the size of the file share, a name, the IOPS profile, and zone. If you want to be able to create a file share with granular access authorization, specify `security_group` as the access mode. Shares with security group access mode can be configured to support encryption in transit, cross-zone mounts, snapshots, and backups, too. See the following example.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2024-05-06&generation=2"\
-H "Authorization: $iam_token" \
-d '{
    "access_control_mode": "security-group",
    "allowed_transit_encryption_modes": ["none", "user-managed"],
    "size": 4800,
    "iops": 3000,
    "name": "myshare-1",
    "profile": {"name": "dp2"},
    "zone": {"name": "us-south-1"}
}
```
{: codeblock}

Make sure that when you create the mount target, you also specify a virtual network interface that is a member of the security group that your virtual server instance belongs to.
{: important}

The following example shows a request to create a 4800 GB file share. It specifies the access control mode `vpc`, which enables all clients in each mount target's VPC to have access to this file share. This option is less secure, and does not support newer features.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2023-08-08&generation=2"\
-H "Authorization: $iam_token" \
-d '{
    "size": 4800,
    "iops": 3000,
    "name": "myshare-1",
    "profile": {"name": "dp2"},
    "access_control_mode": "vpc",
    "zone": {"name": "us-south-1"}
  }'
```
{: pre}

A successful response looks like the following example.

```json
{
  "access_control_mode": "vpc",
  "created_at": "2023-08-08T22:31:50Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/acd96d70-b8d3-4b56-ad7f-9c1035df93b2",
  "id": "acd96d70-b8d3-4b56-ad7f-9c1035df93b2",
  "initial_owner": {
    "gid": 0,
    "uid": 0
  },
  "iops": 3000,
  "lifecycle_state": "pending",
  "name": "myshare-1",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/dp2",
    "name": "dp2",
    "resource_type": "share_profile"
  },
  "replication_role": "none",
  "replication_status": "none",
  "replication_status_reasons": [],
  "resource_group": {
    "crn": "crn:v1:public:resource-controller::a/e2f80b84-bc75-4f53-8737-8193ef1d1a7b::resource-group:e96d1fa9-76f2-4c87-a737-dbab3a947b24",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/e96d1fa9-76f2-4c87-a737-dbab3a947b24",
    "id": "e96d1fa9-76f2-4c87-a737-dbab3a947b24",
    "name": "Default"
  },
  "resource_type": "share",
  "size": 4800,
  "mount_targets": [],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: codeblock}

### Creating a mount target for a file share with the API
{: #fs-create-mount-target-api}

This request creates or adds a mount target to an existing file share. In this example, the `vpc` property is specified because the file share's access control mode is `vpc`. Data encryption in transit cannot be enabled.

Access control modes of the mount target and the share must match. Both must be either `vpc` or `security_group`. When you create a mount target with `security_group` access mode, pay attention to the share's `allowed_transit_encryption_modes`. The `transit_encryption` value must reflect what is allowed for the share.
{: important}

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets?version=2023-08-08&generation=2"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{
    "access_control_mode": "vpc"
    "name": "mount-target-name1",
    "vpc": {"id": "6e01bc24-4a6e-4a0c-a1bd-4caa0c8159e7"},
    "transit_encryption": "none"
  }'
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "access_control_mode": "vpc",
  "created_at": "2023-08-08T23:31:59Z",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/ff859972-8c39-4528-91df-eb9160eae918/mount_targets/9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "id": "9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "lifecycle_state": "pending",
  "mount_path": "domain.com:/vol_xyz_2891fd0a_63aa_4deb_9ed5_1159e37cb5aa",
  "name": "mount-target-name1",
  "resource_type": "share_target",
  "transit_encryption": "none",
  "vpc": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/e6ff7b61-feb4-4c87-94aa-277d6f93e164",
    "id": "e6ff7b61-feb4-4c87-94aa-277d6f93e164",
    "name": "vpc-name1",
    "resource_type": "vpc"
  }
}
```
{: codeblock}

### Adding a mount target to an existing file share by specifying a subnet and security group
{: #fs-create-file-share-subnet-sg-api}

Make a `POST /shares/{share_id}/mount_targets` request and specify a subnet and security group for the mount target network interface. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all servers where you want to mount the share.

This example adds a mount target to an existing file share, which is identified by ID, and provides a subnet and security group to define the network interface. Encryption in transit is enabled.

```json
 curl -X POST "$vpc_api_endpoint/v1/shares/f1ab81ef-dd30-459a-85e0-9094164978b1/mount_targets/?version=2023-07-18&generation=2"\
 -d '{
     "virtual_network_interface": {
        "subnet": {"id": "1a0b3d75-8a62-4c78-9263-f9bcd25a8759"},
        "security_groups": [{"id": "b2599112-7027-480e-ad1b-fd917d2fcb84"}]
     },
     "transit_encryption": "user_managed"
}'
```
{: codeblock}

### Creating a file share and mount target together with the API
{: #fs-create-share-target-api}

The following example request creates a file share that has the VPC-wide access mode and a mount target that can be used by every virtual server instance in the specified VPC. It also adds [user tags](/docs/vpc?topic=vpc-file-storage-managing&interface=api#fs-add-user-tags) to the share. 

Access to the mount target is VPC wide; all instances in the VPC have access to this file share. Newer features such as cross-zone mounting and data encryption in transit are not supported.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2025-04-08&generation=2\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{
    "access_control_mode": "vpc",
    "size": 4800,
    "iops": 48000,
    "mount_targets": [
      {
        "name": "mount-target-name1",
        "vpc": {"id": "a1fb6c4f-6a63-4d34-8bf6-55fab89e932a"}
      }
    ],
    "name": "share-name1",
    "profile": {"name": "dp2"},
    "user_tags": [
      "env:test",
      "env:prod"
    ],
    "resource_group": {},
    "zone": {"name": "us-south-1"}
  }'
```
{: pre}

A successful response looks like the following example.

```json
{
  "access_control_mode": "vpc",
  "allowed_transit_encryption_modes": "none",
  "created_at": "2025-04-08T23:31:59Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/ff859972-8c39-4528-91df-eb9160eae918",
  "id": "ff859972-8c39-4528-91df-eb9160eae918",
  "iops": 48000,
  "lifecycle_state": "stable",
  "name": "share-name1",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/dp2",
    "name": "dp2",
    "resource_type": "share_profile"
    },
  "replication_role": "none",
  "replication_status": "none",
  "replication_status_reasons": [],
  "resource_group": {
    "crn": "crn:[...]",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/6b45d0aa-e0a6-478b-a5d9-bb45b106676d",
    "id": "6b45d0aa-e0a6-478b-a5d9-bb45b106676d",
    "name": "Default"
    },
  "resource_type": "share",
  "size": 4800,
  "mount_targets": [
    {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/ff859972-8c39-4528-91df-eb9160eae918/mount_targets/9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
      "id": "9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
      "name": "mount-target-name1",
      "resource_type": "share_target",
      "vpc": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/e6ff7b61-feb4-4c87-94aa-277d6f93e164",
        "id": "e6ff7b61-feb4-4c87-94aa-277d6f93e164",
        "name": "vpc-name1",
        "resource_type": "vpc"
      }
    }
  ],
  "user_tags": ["env:test","env:prod"],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: codeblock}

### Creating a file share and mount target by specifying a subnet
{: #fs-create-file-share-subnet-vni-api}

The default access control mode for file shares is `security_group`. It's more secure than the vpc-wide options and supports newer features. To create the mount target with the network interface at the same time that the file share is created, make a `POST /shares` request and specify a subnet. Specifying the `subnet` property is required when you're not specifying a [virtual network interface](#fs-create-file-share-vni-api).

The following example creates and attaches a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to your mount target with a reserved IP address and applies the rules of the selected security group. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all servers where you want to mount the share. 

In this example, the mount target section specifies a subnet ID. The system picks a reserved IP from that subnet for the [virtual network interface](/docs/vpc?topic=vpc-vni-about) when the mount target is created.

```json
curl -X POST "$vpc_api_endpoint/v1/shares?version=2023-08-08&generation=2"\
-H "Authorization: $iam_token"\
-d '{
    "allowed_transit_encryption_modes": ["user-managed"],
    "size": 10,
    "name": "my-share-1",
    "profile": {"name": "dp2"},
     "zone": {"name": "us-south-1"},
     "mount_targets": [{
         "virtual_network_interface": {"subnet": {"id": "4e95744c-7e64-48c9-b5d2-3b6481b1dfde"}},
         "transit_encryption": {"user_managed"}}]
}'
```
{: codeblock}

When the `transit_encryption` property is set to `user_managed`, encryption in transit with an instance identity certificate is enabled. The default value for the `transit_encryption` property is `none`, which disables encryption in transit. However, if the `allowed_transit_encryption_modes` is specified as `user-managed`, then the mount target must have `user_managed` as the value of `transit_encryption`.

A successful response looks like the following example.

```json
 {
    "access_control_mode": "security_group",
    "allowed_transit_encryption_modes": ["user-managed"],
    "created_at": "2023-08-08T12:15:12Z",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/90c4bb62-1724-47bd-8c45-f7d37d7c3508/mount_targets/7e5bdb52-676d-43b2-991f-2053cf6855eb",
    "id": "7e5bdb52-676d-43b2-991f-2053cf6855eb",
    "lifecycle_state": "pending",
    "mount_path": "",
    "name": "myshare-1",
    "primary_ip": {"address": ""},
    "resource_type": "share_target",
    "size": 10,
    "snapshot_count": 0, 
    "snapshot_size": 0,
    "subnet": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/4e95744c-7e64-48c9-b5d2-3b6481b1dfde",
        "id": "4e95744c-7e64-48c9-b5d2-3b6481b1dfde",
        "name": "subnet-2",
        "resource_type": "subnet"
    },
    "transit_encryption": "user-managed",
    "virtual_network_interface": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/virtual_network_interface/710y-b8aa945c-7eac-4c15-bad6-a56db9d1e9bd",
        "id": "710y-b8aa945c-7eac-4c15-bad6-a56db9d1e9bd",
        "name": "enlace-traverse-oat-console",
        "resource_type": "VirtualNetworkInterface"
    },
    "vpc": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/82fa21ae-a645-4dd5-9136-d48a723bf00e",
        "id": "82fa21ae-a645-4dd5-9136-d48a723bf00e",
        "name": "my-vpc-2",
        "resource_type": "vpc"
    },
    "user_tags": []
}
```
{: codeblock}

### Creating a file share and mount target by specifying a subnet and security group
{: #fs-create-file-share-both-vni-api}

To create the mount target network interface, make a `POST /shares` request and specify a subnet and security group. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all servers where you want to mount the share.

In this example, the `mount_targets` property specifies a subnet ID and security group ID. When the `transit_encryption` property is set to `user_managed`, it enables encryption in transit by using an instance identity certificate. The default value is none, which disables encryption in transit.

```json
curl -X POST "$vpc_api_endpoint/v1/shares?version=2023-08-08&generation=2"\
-H "Authorization: $iam_token" \
-d '{
    "size": 20,
    "iops": 100,
    "name": "myshare-3",
    "profile": {"name": "dp2"},
    "zone": {"name": "us-south-1"},
    "allowed_transit_encryption_modes": ["user-managed"],
    "mount_targets": [{
        "virtual_network_interface": {
            "subnet": {"id": "4e95744c-7e64-48c9-b5d2-3b6481b1dfde"},
            "security_groups": [{"id": "34c09abb-37bf-4ef6-88bb-f63a0ef28915"}]
        },
        "transit_encryption": {"user_managed"}}
      ]
    }'
```
{: codeblock}

The following response shows that access control mode is `security_group`, which is the default value.

```json
{
    "access_control_mode": "security_group",
    "allowed_transit_encryption_modes": ["user-managed"],
    "created_at": "2023-08-08T12:55:40Z",
    "crn": "crn:[...]",
    "encryption": "provider_managed",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-56f91d4a-2801-470a-b368-176bde64e954",
    "id": "r006-56f91d4a-2801-470a-b368-176bde64e954",
    "initial_owner": {
        "gid": 0,
        "uid": 0
    },
    "iops": 100,
    "lifecycle_state": "pending",
    "name": "myshare-3",
    "mount_targets": [
        {
            "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-56f91d4a-2801-470a-b368-176bde64e954/mount_targets/r006-b8573e2c-60ee-4ecc-9eae-c52f890a8195",
            "id": "r006-b8573e2c-60ee-4ecc-9eae-c52f890a8195",
            "name": "sticky-idealist-spoiled-sloppily",
            "resource_type": "share_target",
            "transit_encryption": {"user_managed"}
        }
    ],
    "profile": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/dp2",
        "name": "dp2",
        "resource_type": "share_profile"
    },
    "replication_role": "none",
    "replication_status": "none",
    "replication_status_reasons": [],
    "resource_group": {
        "crn": "crn:[...]",
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups//678523bcbe2b4eada913d32640909956",
        "id": "678523bcbe2b4eada913d32640909956",
        "name": "Default"
    },
    "resource_type": "share",
    "size": 20,
    "snapshot_count": 10, 
    "snapshot_size": 10,
    "user_tags": [],
    "zone": {
        "href": "https://us-south.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
        "name": "us-south-1"
    }
}
```
{: codeblock}

### Creating a file share and mount target by specifying a virtual network interface
{: #fs-create-file-share-vni-api}

To perform this operation, you must already have a [virtual network interface](/docs/vpc?topic=vpc-vni-create&interface=api) and that virtual network interface must not be attached to another resource.
{: requirement}

Make a `POST /shares` request and create a mount target with a virtual network interface. Specify the ID of an unattached virtual network interface in the mount target's `virtual_network_interface` property.

```json
curl -X POST "$vpc_api_endpoint/v1/shares?version=2023-08-08&generation=2" \
-H "Authorization: $iam_token" \
-d '{
    "size": 10,
    "name": "my-share-sc-2",
    "profile": {"name": "dp2"},
    "zone": {"name": "us-south-3"},
    "allowed_transit_encryption_modes": ["none"],
    "mount_targets": [{
        "name": "mount-target-1",
        "transit_encryption": {"none"},
        "virtual_network_interface": {"id": "0767-fa41aecb-4f21-423d-8082-630bfba1e1d9"}
      }]
    }'
 ```
 {: codeblock}

### Creating a file share from a snapshot with the API
{: #fs-create-share-from-snapshot-api}

For more information about how to create a file share from a snapshot, see [Restoring data from a file share snapshot](/docs/vpc?topic=vpc-fs-snapshots-restore&interface=api#fs-snapshots-restore-API).

### Adding supplemental IDs when you create a file share with the API
{: #fs-add-supplemental-id-api}

With the API, you can set `UID` and `GID` values for the `initial_owner` property to control access to your file shares. Wherever you mount the file share, the root folder uses that user ID and group ID owner. You set the `UID` or `GID`, or both when you create a share in a `POST /shares` call.

If you change the supplemental IDs (UID or GID) from the virtual server instance, it is not possible to determine that it was changed. As a result, `initial_owner` does not change in the API database but changes only in the file storage system.
{: note}

Table 1 shows UID and GID values that you can set and values that are reserved.

| ID value | Description |
|----------|-------------|
| **UID** | |
| UID 0 | Reserved for root. |
| UID 1–99 | Reserved for predefined accounts. |
| UID 100–999 | Reserved by the system for administrative system accounts and groups. |
| UID 1000–10000 | Used by applications account. |
| UID 10000+ | Available for user accounts. |
| **GID** | |
| GID 0 | Reserved for root. |
| GID 1–99 | Reserved for the system and application use. |
| GID 100+ | Allocated for the user’s group. |
{: caption="Unix/Linux&reg; supplemental ID values." caption-side="top"}

To set supplemental IDs when you create a share, make a `POST /shares` call and specify the `initial_owner` property with the supplemental IDs. See the following example.

```json
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2023-08-08&generation=2"\
-H "Authorization: $iam_token" \
-d '{
    "initial_owner": {"gid": 101,"uid": 10001},
    "size": 4800,
    "name": "share-name",
    "profile": {"name": "dp2"},
    "zone": {"name": "us-south-1"}
     .
     .
     .
   }'
```
{: codeblock}

## Creating a file share and mount target with Terraform
{: #file-storage-create-terraform}
{: terraform}

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

### Creating a file share with Terraform
{: #file-share-create-terraform}

To create a file share, use the `ibm_is_share` resource. The following example creates a share with 200 GB capacity and the `dp2` performance profile.

```terraform
resource "ibm_is_share" "example" {
  allowed_transit_encryption_modes = "none"
  name                             = "my-new-share"
  size                             = "200"
  iops                             = "500"
  profile                          = "dp2"
  zone                             = "us-south-2"
}
```
{: codeblock}

### Creating a file share with a replica with Terraform
{: #file-share-create-with-replica-terraform}

To create a file share, use the `ibm_is_share` resource. To add a replica, define the replica share similarly to the following example. It creates a share with 220 GB capacity and the `dp2` performance profile. The `replica_share` argument defines the replica share's name, the frequency of replication (in cron spec), the performance profile, and the zone where the replica share is to be created.

```terraform
resource "ibm_is_share" "example-2" {
  allowed_transit_encryption_modes = "none"
  zone                             = "us-south-1"
  size                             = "220"
  name                             = "my-share"
  profile                          = "dp2"
  replica_share {
    name                           = "my-replica"
    replication_cron_spec          = "0 */5 * * *"
    profile                        = "dp2"
    zone                           = "us-south-3"
  }
}
```
{: codeblock}

### Creating a file share from a snapshot with Terraform
{: #fs-create-share-from-snapshot-terraform}

For more information about how to create a file share from a snapshot, see [Restoring data from a file share snapshot](/docs/vpc?topic=vpc-fs-snapshots-restore&interface=terraform#fs-snapshots-restore-terraform).

### Creating a mount target with Terraform
{: #file-share-mount-create-terraform}

To create a mount target for a file share that provides granular authentication with the use of Security groups, use the `is_share_mount_target` resource. The following example creates a mount target with `security_group` access control mode. First, specify the share for which the mount target is created. Then, you specify the name of the mount target and define the new virtual network interface by providing an IP address and a name. You must also specify the security group that you want to use to manage access to the file share that the mount target is associated to. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all servers where you want to mount the share. The attribute `auto_delete = true` means that the virtual network interface is to be deleted if the mount target is deleted.

```terraform
resource "ibm_is_share_mount_target" "target-with-vni" {
     share    = ibm_is_share.is_share.ID
     name     = <share_target_name>
     virtual_network_interface {
             name = <virtual_network_interface_name>
             primary_ip {
             address = “10.240.64.5”
             auto_delete = true
             name = <reserved_ip_name>
     }
     resource_group = <resource_group_id>
     security_groups = [<security_group_ids>]
     transit_encryption = none
   }
}
```
{: codeblock}

You can also create a mount target for a file share with the VPC access mode by using the `is_share_mount_target` resource. The following example creates a mount target with the name `my-share-target` for the file share that is specified by its ID. When the mount target is created, all virtual server instances in the VPC can mount the share by using it.

```terraform
resource "ibm_is_share_mount_target" "target-without-vni" {
     share=ibm_is_share.is_share.ID
     name = <share_target_name>
     vpc = <vpc_id>
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share_mount_target](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share_mount_target){: external}.

### Creating a file share with a mount target with security group access mode
{: #file-share-create-with-target-sg-terraform}

You don't need to create the file share and the mount target separately. To create a file share with a mount target that allows for security group-based authentication within a VPC, use the `ibm_is_share` resource. Specify the access control mode as `security_group`, and define the mount target by providing a name for it, details of the virtual network interface such as name, subnet, or IP address. Also, specify the security groups that you want to use to control access to the file share. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all servers where you want to mount the share.

```terraform
resource "ibm_is_share" "share4" {
   zone    = "us-south-2"
   size    = "800"
   name    = "my-share4"
   profile = "dp2"
   allowed_transit_encryption_modes = "user_managed"
   access_control_mode = "security_group"
   mount_target {
       name = "target"
       virtual_network_interface {
       primary_ip {
               address = 10.240.64.5
               auto_delete = true
               name = <reserved_ip_name>
       }
      resource_group = <resource_group_id>
      security_groups = [<security_group_ids>]
      transit_encryption = user_managed
      }
   }
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.

### Creating a file share with a mount target with VPC-wide access mode
{: #file-share-create-with-target-vpc-terraform}

To create a file share with a mount target that is accessible to all virtual server instances within a VPC, use the `ibm_is_share` resource. Specify the access control mode as `vpc`, and define the mount target by providing a name for it and the VPC where it's going to be used in.

```terraform
resource "ibm_is_share" "share3" {
    zone    = "us-south-2"
    size    = "700"
    name    = "my-share3"
    profile = "dp2"
    access_control_mode = "vpc" 
    mount_target {
          name = "target"
          vpc = <vpc_id>
    }
}
```
{: codeblock}

Cross-zone mounting and encryption in transit are not supported for this type of file share.

## Next steps
{: #fs-create-next-steps}

Mount your file shares. Mounting is a process by which a server's operating system makes files and directories on the storage device available for users to access through the server's file system. For more information, see the following topics:
* [IBM Cloud File Share Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility)
* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-mount-RHEL).
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos).
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-mount-ubuntu).
* [Mounting file shares on z/OS](/docs/vpc?topic=vpc-file-storage-mount-zos)

Manage your file shares and data. For more information, see the following topics:
* [Viewing file shares and mount targets](/docs/vpc?topic=vpc-file-storage-view).
* [Manage your file shares](/docs/vpc?topic=vpc-file-storage-managing).
* [Create a file share with replication](/docs/vpc?topic=vpc-file-storage-create-replication).
* [Sharing and mounting a file share from another account](/docs/vpc?topic=vpc-file-storage-accessor-create&interface=ui).
