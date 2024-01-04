---

copyright:
  years: 2021, 2024
lastupdated: "2024-01-04"

keywords: file share, file storage, virtual network interface, encryption in transit, profiles, 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating file shares and mount targets
{: #file-storage-create}

Create file shares and mount targets in the UI, CLI, API, or Terraform.
{: shortdesc}

Before you get started, to create mount targets for file shares, make sure that you created a [VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{: important}

You can create file shares and mount targets either of the following ways:

* Create a file share and mount target together,
* Create a file share and add mount target later.

## Creating a file share in the UI
{: #file-storage-create-ui}
{: ui}

In the {{site.data.keyword.cloud_notm}} console, you can create a file share with or without a mount target. However, you need to create a mount target when you want to mount the share on a virtual server instance. You can create multiple mount targets for the share if it's to be used by hosts in different VPCs.

### Creating a file share in the UI
{: #fs-create-share-target-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Storage > File Shares**. A list of file shares displays.

1. On the File shares for VPC page, click **Create**.

1. Enter the following information. 

   | Field | Value |
   |-------|-------|
   | **Location** | Choose the geography, region, and zone where you want to create the file share. Location information is inherited from the VPC, for example, North America, Dallas, Dallas 2. |
   | **Details** | |
   | Name  | Specify a meaningful name for your file share. The file share name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want.
   | Resource Group | Use the default resource group or specify a [Resource group](/docs/vpc?topic=vpc-iam-getting-started&interface=ui#iam-resource-groups). Resource groups help organize your account resources for access control and billing purposes. |
   | Tags | Enter any user tags to apply to this file share. As you type, existing tags appear that you can select. For more information about tags, see [Add user tags to a file share](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#fs-add-user-tags). |
   | Access Management Tags | Enter access management tags that you created in IAM to apply them to this file share. For more information about access management tags, see [Access management tags for file shares](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-about-mgt-tags). |
   | Profile | All file shares are created with the dp2 profile. For more information, see [file Storage profiles](/docs/vpc?topic=vpc-file-storage-profiles). \n Select the size and IOPS for your file share. You can increase the capacity later, and you can also adjust the IOPS as needed. |
   | Mount target access mode  | Select how you want to manage access to this file share: |
   |  | Security group: Access to the file share is based on [security group](/docs/vpc?topic=vpc-using-security-groups#sg-getting-started) rules. This option can be used to restrict access to specific virtual server instances. You can also use this option if you want to mount the file share to a virtual server instance in another zone. This option is recommended as you have more control over who can access the data that is stored on the file share. |
   |  | Virtual private cloud: Access to the file share is granted to any virtual server instance in the same region. Cross-zone mounting is not supported. |
   {: caption="Table 1. Values for creating a file share" caption-side="top"}

1. The creation of mount targets is optional. You can skip this step if you do not want to create a mount target now. Otherwise, click **Create**. You can create one mount target per VPC per file share. 

   - If you selected security group as the access mode, enter the information as described in the Table 2. This action creates and attaches a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to your mount target that identifies the file share with a reserved IP address and applies the rules of the selected Security group. This mount target supports encryption-in-transit and cross-zone mounting.

     | Field | Value |
     |-------|-------|
     | **Details** | |
     | Mount target name | Specify a mount target name. The name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want. |
     | Zone | Zone is inherited from the file share (for example, Dallas 2). |
     | VPC | Select an available VPC. The list includes only those VPCs with a subnet in the selected zone. |
     | Subnet | Select a subnet from the list. |
     | **Reserved IP address** | Required for the mount target. The IP address cannot be changed afterward. However, you can delete the mount target and create another one with a different IP address. |
     | Reserving method | You can have the file service select an IP address for you. The reserved IP becomes visible after the mount target is created. Or, specify your own IP. |
     | Auto-release | Releases the IP address when you delete the mount target. Enabled by default. |
     | **Security groups** | The [default security group](/docs/vpc?topic=vpc-updating-the-default-security-group) for the VPC is selected. You can use it or select another security group from the list. |
     | **Encryption in transit** | Disabled by default, click the toggle to enable. For more information about this feature, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit). |
     {: caption="Table 2. Values for creating a mount target." caption-side="top"}

   - If you selected VPC as the access mode, provide a name for the mount target and select a VPC from the list. This mount target can be used to mount the file share on any virtual server instance of the selected VPC in the same zone as the file share. Cross-zone mounting is not supported.

1. Encryption at rest. By default, all file shares are encrypted by IBM-managed keys. You can also choose to create an envelop-encryption for your shares with your own keys. If you want to use your own keys, select one of the [key management services](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok).

   | Field | Value |
   | ----- | ----- |
   | Encryption | To use customer-managed encryption, select either {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}. The key management service (KMS) instance includes the root key that is imported to or created in that KMS instance. |
   | Encryption service instance | If you provisioned multiple KMS instances in your account, select the one that includes the root key that you want to use for customer-managed encryption. |
   | Key name | Select the root key within the KMS instance that you want to use for encrypting the share. |
   | Key ID | The field shows the key ID that is associated with the data encryption key that you selected. |
   {: caption="Table 3. Values for customer-managed encryption for file shares." caption-side="bottom"}

1. When all the required information is entered, click **Create file share**. You return to the {{site.data.keyword.filestorage_vpc_short}} page, where a message indicates that the file share is provisioning. When the transaction completes, the share status changes to **Active**.
   
If you're not ready to order yet or just looking for pricing information, you can add the information that you see in the side panel to an Estimate. For more information about how this feature works, see [Estimating your costs](/docs/billing-usage?topic=billing-usage-cost).    
{: tip}

### Creating a mount target in the UI
{: #fs-create-mount-target-ui}

You can create several mount targets for an existing file share if the share is to be used by resources in multiple VPCs. You can create one mount target per VPC per file share. 

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Storage > File shares**.

2. Select a file share from the list.

3. On the File shares details page, under Mount targets, click **Create**.

   You must have at least one VPC to create a mount target. If you don't have one, first [create a VPC](/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc).
   {: requirement}

4. Depending on the [mount target access mode](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-mount-access-mode) of the share, the **Create mount target** form looks different.

   - If the share has security group access mode, enter the following information. This action creates and attaches a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to your mount target that identifies the file share with a reserved IP address and applies the rules of the selected [security group](/docs/vpc?topic=vpc-using-security-groups#sg-getting-started). This mount target supports encryption-in-transit and cross-zone mounting.

     | Field | Value |
     |-------|-------|
     | **Details** | |
     | Mount target name | Specify a mount target name. The name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want. |
     | Zone | Zone is inherited from the file share (for example, Dallas 2). |
     | VPC | Select an available VPC. The list includes only those VPCs with a subnet in the selected zone. |
     | Subnet | Select a subnet from the list. |
     | **Reserved IP address** | Required for the mount target. The IP address cannot be changed afterward. However, you can delete the mount target and create another one with a different IP address. |
     | Reserving method | You can have the file service select an IP address for you. The reserved IP becomes visible after the mount target is created. Or, specify your own IP. |
     | Auto-release | Releases the IP address when you delete the mount target. Enabled by default. |
     | **Security groups** | The [security group](/docs/vpc?topic=vpc-using-security-groups#sg-getting-started) for the VPC is selected by default, or select from the list. |
     | **Encryption in transit** | Disabled by default, click the toggle to enable. For more information about this feature, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit). |
     {: caption="Table 4. Values for creating a mount target." caption-side="top"}

   - If the share has VPC as the access mode, provide a name for the mount target and select a VPC from the list. This mount target can be used to mount the file share on any virtual server instance of the selected VPC in the same zone as the file share. Cross-zone mounting is not supported.

5. Click **Create**.

## Creating a file share from the CLI
{: #file-storage-create-cli}
{: cli}

### Before you begin
{: #before-creating-file-storage-cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

3. After you install the VPC CLI plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.

4. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli#create-a-vpc-cli).

### Gathering information from the CLI
{: #fs-vpc-getinfo-cli}

Before you run the `ibmcloud is share-create` command, you can gather information that you need for provisioning a share by viewing information about other file shares, mount targets, and file storage profiles.

| Details             |  Listing options                           | What it provides                         |
|---------------------|--------------------------------------------|------------------------------------------|
| File shares         | `ibmcloud is shares`                       | List all shares in a region.             |
| File share details  | `ibmcloud is share SHARE_ID`               | Review details of a share.               |
| File share profiles | `ibmcloud is share-profiles` | List all file share profiles in a region. Only `dp2` can be used to create file shares.|
| Mount targets       | `ibmcloud is share-mount-targets SHARE_ID` | List all mount targets for a file share. |
| Subnets             | `ibmcloud is subnets`                      | List all subnets.                        |
| Reserved IP addresses | `ibmcloud is subnet-reserved-ips`   | List all reserved IP addresses in the subnet. |
| Security Groups     | `ibmcloud is security-groups`              | List all security groups.                |     
{: caption="Table 1. Details for creating file shares." caption-side="top"}

### Creating a file share without a mount target from the CLI
{: #fs-create-share-cli}

You can use the `ibmcloud is share-create` command to provision a file share in your selected zone with the `dp2` profile, with your specific capacity and IOPS values. The following example shows how to create 1000-GB file share with 1000 IOPS in the us-south-2 zone. This file share is created with the default security group access mode and with provider-managed encryption.

```sh
$ ibmcloud is share-create --name my-file-share --zone us-south-2 --profile dp2 --size 1000 --iops 1000
Creating file share my-file-share under account Test Account as user test.user@ibm.com...
                                
ID                           r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   
Name                         my-file-share   
CRN                          crn:v1:bluemix:public:is:us-south-2:a/1234567::share:r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   
Lifecycle state              pending   
Access control mode          security_group   
Zone                         us-south-2   
Profile                      dp2   
Size(GB)                     1000   
IOPS                         1000   
Encryption                   provider_managed   
Mount Targets                ID                          Name      
                             No mounted targets found.      
                                
Resource group               ID                                 Name      
                             db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
Created                      2023-10-18T22:15:15+00:00   
Replication role             none   
Replication status           none   
Replication status reasons   Status code   Status message      
                             -             -                              
```
{: screen}

Security group access mode is the default and recommended setting. However, you can choose to create a file share with the VPC access mode that allows every Compute host in the VPC to mount the file share. See the following example.

```sh
$ ibmcloud is share-create --name my-vpc-file-share --zone us-south-2 --profile dp2 --size 1000 --iops 500 --access-control-mode vpc
Creating file share my-vpc-file-share under account Test Account as user test.user@ibm.com...
                                
ID                           r006-b1707390-3825-41eb-a5bb-1161f77f8a58   
Name                         my-vpc-file-share   
CRN                          crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-b1707390-3825-41eb-a5bb-1161f77f8a58   
Lifecycle state              pending   
Access control mode          vpc   
Zone                         us-south-2   
Profile                      dp2   
Size(GB)                     1000   
IOPS                         500   
Encryption                   provider_managed   
Mount Targets                ID                          Name      
                             No mounted targets found.      
                                
Resource group               ID                                 Name      
                             db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
Created                      2023-10-18T22:57:05+00:00   
Replication role             none   
Replication status           none   
Replication status reasons   Status code   Status message      
                             -             -  
```
{: screen}

For more information about the command options, see [`ibmcloud is share-create`](/docs/vpc?topic=vpc-vpc-reference#share-create).

### Creating a mount target for a file share from the CLI
{: #fs-create-mount-target-cli}

To create a mount target for the file share, run the `share-mount-target-create` command. Before you begin, gather some necessary information.

When you create a mount target, you must specify the file share that it is for. You can use the file share's name or ID. You must specify the VPC, too, either with its ID or name. The VPC must be unique to each mount target. You must also specify the security access group that's going to be used to manage access to the share. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all virtual server instances on which you want to mount the file share.

Lastly, you must specify values for the options that are needed to create a [virtual network interface](/docs/vpc?topic=vpc-vni-about) for the mount target. Use the appropriate CLI commands to list the available [subnets](/docs/vpc?topic=vpc-vpc-reference#subnets-list), [reserved IP addresses in a subnet](/docs/vpc?topic=vpc-vpc-reference#subnet-reserved-ips-list), [security groups](/docs/vpc?topic=vpc-vpc-reference#security-groups-list) to get the information that you need.

The following example creates a mount target with a virtual network interface for a file share that has security group access mode.

```sh
$ ibmcloud is share-mount-target-create my-file-share --subnet my-subnet --name my-cli-share-mount-target-1 --vni-name my-share-vni-1  --vni-sgs my-sg --resource-group-name Default --vpc my-vpc
Mounting target for share r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6 under account Test Account as user test.user@ibm.com...
                               
ID                          r006-dd497561-c7c9-4dfb-af0a-c84eeee78b61   
Name                        my-cli-share-mount-target-1   
VPC                         ID                                          Name      
                            r006-6e8fb140-5668-45b8-b98a-d5cb0e0bf39b   my-vpc      
                               
Access control mode         security_group   
Resource type               share_mount_target   
Virtual network interface   ID                                          Name      
                            0717-13c070d8-d038-49c6-95f5-e8503c5595e3   my-share-vni-1      
                               
Lifecycle state             pending   
Mount path                  -   
Transit Encryption          none   
```
{: screen}

The following example creates a mount target for a file share that has VPC access mode.

```sh
$ ibmcloud is share-mount-target-create my-vpc-file-share --vpc my-vpc --name my-vpc-mount-target
Mounting target for share r006-b1707390-3825-41eb-a5bb-1161f77f8a58 under account Test Account as user test.user@ibm.com...
                         
ID                    r006-5ed68506-860e-4dea-a1eb-9634704e3c4d   
Name                  my-vpc-mount-target   
VPC                   ID                                          Name      
                      r006-6e8fb140-5668-45b8-b98a-d5cb0e0bf39b   my-vpc      
                         
Access control mode   vpc   
Resource type         share_mount_target   
Lifecycle state       pending   
Mount path            -   
Transit Encryption    none   
Created               2023-10-18T23:09:43+00:00   
```
{: screen}

For more information about the command options, see [`ibmcloud is share-mount-target-create`](/docs/vpc?topic=vpc-vpc-reference#share-mount-target-create).

### Creating a file share with a mount target from the CLI
{: #fs-create-share-target-cli}

You can create a file with one or more mount targets in one step by using the `ibmcloud is share-create` command. You need to provide the zone name, the [file share profile](/docs/vpc?topic=vpc-file-storage-profiles), the file share size, and the IOPS. You can also specify a name, user tags, and even the initial owner UID. To create the mount target, you need to provide the mount target information in JSON format.

The following example shows how to create a new file share with 500 GB capacity and 2000 IOPS in the `us-south-2` zone. The file share is tagged with `env:dev` and has security group access control mode. The file share can be mounted on authorized virtual servers by using the mount target `my-new-mount-target`.

```sh
$ ibmcloud is share-create --name my-new-file-share --zone us-south-2 --profile dp2 --size 500 --iops 2000  --user-tags env:dev --mount-targets '
>[{"name":"my-new-mount-target","virtual_network_interface": {"name":"my-vni","subnet": {"id":"0717-298acd6c-e71e-4204-a04f-fe4a4dd89805"}}}]'
Creating file share my-new-file-share under account Test Account as user test.user@ibm.com...
                                
ID                           r006-925214bc-ded5-4626-9d8e-bc4e2e579232   
Name                         my-new-file-share   
CRN                          crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-925214bc-ded5-4626-9d8e-bc4e2e579232   
Lifecycle state              pending   
Access control mode          security_group   
Zone                         us-south-2   
Profile                      dp2   
Size(GB)                     500   
IOPS                         2000   
User Tags                    env:dev   
Encryption                   provider_managed   
Mount Targets                ID                                          Name      
                             r006-ad313f6b-ccb5-4941-a5f7-0c953f1043df   my-new-mount-target      
                                
Resource group               ID                                 Name      
                             db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
Created                      2023-10-19T00:30:11+00:00   
Replication role             none   
Replication status           none   
Replication status reasons   Status code   Status message      
                             -             -      
```
{: screen}

The following example creates a file share with VPC access mode and a mount target that can be used by any virtual server instance within the VPC.

```sh
$ ibmcloud is share-create --name my-file-share-8 --zone us-south-1 --profile dp2 --size 40 --iops 2000  --user-tags env:dev --mount-targets '
> [{"name": "my-new-mount-target","vpc": {"name": "my-vpc"}}]'
Creating file share my-file-share-8 under account Test Account as user test.user@ibm.com...
                                
ID                           r006-97733317-35c3-4726-9c28-1159de30012e   
Name                         my-file-share-8   
CRN                          crn:v1:bluemix:public:is:us-south-1:a/a1234567::share:r006-97733317-35c3-4726-9c28-1159de30012e   
Lifecycle state              pending   
Access control mode          vpc   
Zone                         us-south-1   
Profile                      dp2   
Size(GB)                     40   
IOPS                         2000   
User Tags                    env:dev  
Encryption                   provider_managed   
Mount Targets                ID                                          Name      
                             r006-36d67ada-ca83-44be-adad-dc58e7c38dc5   my-new-mount-target      
                                
Resource group               ID                                 Name      
                             db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
Created                      2023-10-18T23:52:45+00:00   
Replication role             none   
Replication status           none   
Replication status reasons   Status code   Status message      
                             -             -      
```
{: screen}

### Creating a file share with customer-managed encryption from the CLI
{: #fs-create-encrypted-share-cli}

By default, {{site.data.keyword.filestorage_vpc_short}} shares are encrypted with IBM-managed encryption. However, you can also create an envelop-encryption for your file shares by using one of the supported key management services to create or import your own root keys. For more information, see [Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption)

For more information about how to create a file share with customer-managed encryption, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption&interface=cli#fs-byok-cli).

### Creating a file share with a replica in another zone with the CLI
{: #fs-create-share-with-replica-cli}

For more information about how to create a file share with a replica simultanously, see [Create a file share with replication from the CLI](/docs/vpc?topic=vpc-file-storage-create-replication&interface=cli#fs-create-new-share-replica-cli).

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

Make a `POST /shares` request to create a file share. Specify the size of the file share, a name, the IOPS profile, and zone.

The following example shows a request to create a 4800 GB file share with a 10 IOPS/GB profile. It specifies the access control mode `vpc`, which enables all clients in each mount target's VPC to have access to this file share.

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
  "crn": "crn": "crn:[...]",
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

This request creates or adds a mount target to an existing file share. In this example, the `vpc` property is specified because the file share's access control mode is `vpc`. Data encryption in transit is not enabled.

Access control modes must match when a mount target is created for an existing share. Both must be either `vpc` or `security_group`.
{: important}

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets?version=2023-08-08&generation=2"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{
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

When you create a file share with the API, you specify `security_group` as the access mode. When you create the mount target, you specify a virtual network interface.

### Adding a mount target to an existing file share by specifying a subnet and security group
{: #fs-create-file-share-subnet-sg-api}

Make a `POST /shares/{share_id}/mount_targets` request and specify a subnet and security group for the mount target network interface. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all virtual server instances on which you want to mount the file share.

This example adds a mount target to an existing file share, which is identified by ID, and provides a subnet and security group to define the network interface. 

```json
 curl -X POST "$vpc_api_endpoint/v1/shares/f1ab81ef-dd30-459a-85e0-9094164978b1/mount_targets/?version=2023-07=18&generation=2"\
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

Access to the mount target is VPC wide; all instances in the VPC have access to this file share. You can also restrict access to a specific virtual server instance in the VPC by specifying a virtual network interface in the mount target definition.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2023-08-08&generation=2\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{
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
    "zone": {"name": "us-south-1"}
  }'
```
{: pre}

A successful response looks like the following example.

```json
{
  "access_control_mode": "vpc",
  "created_at": "2023-08-08T23:31:59Z",
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

The following example creates and attaches a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to your mount target with a reserved IP address and applies the rules of the selected security group. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all virtual server instances on which you want to mount the file share. To create the mount target with the network interface, make a `POST /shares` request and specify a subnet. Specifying the `subnet` property is required when you're not specifying a [virtual network interface](#fs-create-file-share-vni-api).

In this example, the mount target specifies a subnet ID. When the `transit_encryption` property is set to `user_managed`, encryption in transit with an instance identity certificate is enabled. The default is none, which disables encryption in transit. The default access control mode is `security_group`, which is shown in the response.

```json
curl -X POST "$vpc_api_endpoint/v1/shares?version=2023-08-08&generation=2"\
-H "Authorization: $iam_token"\
-d '{
    "size": 10,
    "name": "myshare-1",
    "profile": {"name": "dp2"},
     "zone": {"name": "us-south-1"},
     "mount_targets": [
         "virtual_network_interface": {"subnet": {"id": "4e95744c-7e64-48c9-b5d2-3b6481b1dfde"}},
         "transit_encryption": {"user_managed"}
    ]
}'
```
{: codeblock}

A successful response looks like the following example.

```json
 {
    "access_control_mode": "security_group",
    "created_at": "2023-08-08T12:15:12Z",
    "href": "$vpc_api_endpoint/v1/shares/90c4bb62-1724-47bd-8c45-f7d37d7c3508/mount_targets/7e5bdb52-676d-43b2-991f-2053cf6855eb",
    "id": "7e5bdb52-676d-43b2-991f-2053cf6855eb",
    "lifecycle_state": "pending",
    "mount_path": "",
    "name": "myshare-1",
    "primary_ip": {"address": ""},
    "resource_type": "share_target",
    "subnet": {
        "crn": "crn:[...]",
        "href": "$vpc_api_endpoint/v1/subnets/4e95744c-7e64-48c9-b5d2-3b6481b1dfde",
        "id": "4e95744c-7e64-48c9-b5d2-3b6481b1dfde",
        "name": "subnet-2",
        "resource_type": "subnet"
    },
    "transit_encryption": "none",
    "virtual_network_interface": {
        "crn": "crn:[...]",
        "href": "$vpc_api_endpoint/v1/virtual_network_interface/710y-b8aa945c-7eac-4c15-bad6-a56db9d1e9bd",
        "id": "710y-b8aa945c-7eac-4c15-bad6-a56db9d1e9bd",
        "name": "enlace-traverse-oat-console",
        "resource_type": "VirtualNetworkInterface"
    },
    "vpc": {
        "crn": "crn:[...]",
        "href": "$vpc_api_endpoint/v1/vpcs/82fa21ae-a645-4dd5-9136-d48a723bf00e",
        "id": "82fa21ae-a645-4dd5-9136-d48a723bf00e",
        "name": "my-vpc-2",
        "resource_type": "vpc"
    }
}
```
{: codeblock}

### Creating a file share and mount target by specifying a subnet and security group
{: #fs-create-file-share-both-vni-api}

To create the mount target network interface, make a `POST /shares` request and specify a subnet and security group. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all virtual server instances on which you want to mount the file share.

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
     "mount_targets": [
        "virtual_network_interface": {
            "subnet": {"id": "4e95744c-7e64-48c9-b5d2-3b6481b1dfde"},
            "security_groups": [{"id": "34c09abb-37bf-4ef6-88bb-f63a0ef28915"}]
        },
        "transit_encryption": {"user_managed"}
      ]
    }'
```
{: codeblock}

The following response shows that access control mode is `security_group`, which is the default value.

```json
{
    "access_control_mode": "security_group",
    "created_at": "2023-08-08T12:55:40Z",
    "crn": "crn:[...]",
    "encryption": "provider_managed",
    "href": "$vpc_api_endpoint/v1/shares/r006-56f91d4a-2801-470a-b368-176bde64e954",
    "id": "r006-56f91d4a-2801-470a-b368-176bde64e954",
    "initial_owner": {
        "gid": 0,
        "uid": 0
    },
    "iops": 100,
    "lifecycle_state": "pending",
    "name": "myshare-3",
    "profile": {
        "href": "$vpc_api_endpoint/v1/share/profiles/dp2",
        "name": "dp2",
        "resource_type": "share_profile"
    },
    "replication_role": "none",
    "replication_status": "none",
    "replication_status_reasons": [],
    "resource_group": {
        "crn": "crn:[...]",
        "href": "$vpc_api_endpoint/v2/resource_groups/678523bcbe2b4eada913d32640909956",
        "id": "678523bcbe2b4eada913d32640909956",
        "name": "Default"
    },
    "resource_type": "share",
    "size": 20,
    "mount_targets": [
        {
            "href": "$vpc_api_endpoint/v1/shares/r006-56f91d4a-2801-470a-b368-176bde64e954/mount_targets/r006-b8573e2c-60ee-4ecc-9eae-c52f890a8195",
            "id": "r006-b8573e2c-60ee-4ecc-9eae-c52f890a8195",
            "name": "sticky-idealist-spoiled-sloppily",
            "resource_type": "share_target"
        }
    ],
    "user_tags": [],
    "zone": {
        "href": "https://us-south-genesis-dal-dev13-etcd.iaasdev.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
        "name": "us-south-1"
    }
}
```
{: codeblock}

### Creating a file share and mount target by specifying a virtual network interface
{: #fs-create-file-share-vni-api}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

This operation requires that you have already [created a virtual network interface](/docs/vpc?topic=vpc-vni-create&interface=api) and that the virtual network interface is not currently attached to another resource.

Make a `POST /shares` request and create a mount target with a virtual network interface. Specify the identity of an unattached virtual network interface in the mount target's `virtual_network_interface` property.

```json
curl -X POST "$vpc_api_endpoint/v1/shares?version=2023-08-08&generation=2" \
-H "Authorization: $iam_token" \
-d '{
    "size": 10,
    "name": "share-sc-2",
    "profile": {"name": "dp2"},
    "zone": {"name": "us-south-3"},
    "mount_targets": [
        "name": "mount-target-1",
        "transit_encryption": {"user_managed"},
        "virtual_network_interface": {
            "id": "0767-fa41aecb-4f21-423d-8082-630bfba1e1d9"
          }
       ]
    }'
 ```
 {: codeblock}

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
{: caption="Table 1. Unix/Linux&reg; supplemental ID values." caption-side="top"}

To set supplemental IDs when you create a share, make a `POST /shares` call and specify the `initial_owner` property with the supplemental IDs. See the following example.

```json
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2023-08-08&generation=2"\
-H "Authorization: $iam_token" \
-d '{
    "initial_owner": {
       "gid": 101,
       "uid": 10001
    },
    "size": 4800,
    "name": "share-name",
    "profile": {
    "name": "dp2"
     },
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

VPC infrastructure services use a regional specific endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the appropriate region in the provider block in the `provider.tf` file.

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
  name    = "my-new-share"
  size    = "200"
  iops    = "500"
  profile = "dp2"
  zone    = "us-south-2"
}
```
{: codeblock}

### Creating a file share with a replica with Terraform
{: #file-share-create-with-replica-terraform}

To create a file share, use the `ibm_is_share` resource. To add a replica, define the replica share similarly to the following example. It creates a share with 220 GB capacity and the `dp2` performance profile. The `replica_share` argument defines the replica share's name, the frequency of replication (in cron spec), the performance profile, and the zone where the replica share is to be created.

```terraform
resource "ibm_is_share" "example-2" {
  zone    = "us-south-1"
  size    = "220"
  name    = "my-share"
  profile = "dp2"
  replica_share {
    name                  = "my-replica"
    replication_cron_spec = "0 */5 * * *"
    profile               = "dp2"
    zone                  = "us-south-3"
  }
}
```
{: codeblock}

### Creating a mount target with Terraform
{: #file-share-mount-create-terraform}

To create a mount target for a file share that provides granular authentication with the use of Security groups, use the `is_share_mount_target` resource. The following example creates a mount target with `security_group` access control mode. First, specify the share for which the mount target is created for. Then, you specify the name of the mount target and define the new virtual network interface by providing an IP address and a name. You must also specify the security group that you want to use to manage access to the file share that the mount target is associated to. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all virtual server instances on which you want to mount the file share. The attribute `auto_delete = true` means that the virtual network interface is to be deleted if the mount target is deleted.

```terraform
resource "ibm_is_share_mount_target" "target-with-vni" {
     share=ibm_is_share.is_share.ID
     name = <share_target_name>
     virtual_network_interface {
     name = <virtual_network_interface_name>
     primary_ip {
         address = “10.240.64.5”
         auto_delete = true
         name = <reserved_ip_name>
     }
     resource_group = <resource_group_id>
     security_groups = [<security_group_ids>]
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

You don't have to create the file share and the mount target separately. To create a file share with a mount target that allows for security group-based authentication within a VPC, use the `ibm_is_share` resource. Specify the access control mode as `security_group`, and define the mount target by providing a name for it, details of the virtual network interface such as name, subnet, or IP address. Also, specify the security groups that you want to use to control access to the file share. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all virtual server instances on which you want to mount the file share.

```terraform
resource "ibm_is_share" "share4" {
   zone    = "us-south-2"
   size    = "800"
   name    = "my-share4"
   profile = "dp2"
   access_control_mode = "security_group"
   mount_target {
       name = "target"
       virtual_network_interface {
       primary_ip {
               address = 10.240.64.5
               auto_delete = true
               name = "<reserved_ip_name>
       }
      resource_group = <resource_group_id>
      security_groups = [<security_group_ids>]
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

## Next steps
{: #fs-create-next-steps}

Mount your file shares. Mounting is a process by which a server's operating system makes files and directories on the storage device available for users to access through the server's file system. For more information, see the following topics:
* [IBM Cloud File Share Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility)
* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-vpc-mount-RHEL).
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos).
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-vpc-mount-ubuntu).
* [Mounting file shares on z/OS](/docs/vpc?topic=vpc-file-storage-vpc-mount-zos)

Manage your file shares and data. For more information, see the following topics:
* [Viewing file shares and mount targets](/docs/vpc?topic=vpc-file-storage-view).
* [Manage your file shares](/docs/vpc?topic=vpc-file-storage-managing).
* [Create a file share with replication](/docs/vpc?topic=vpc-file-storage-create-replication).
 