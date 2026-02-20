---

copyright:
  years: 2024, 2026
lastupdated: "2026-02-20"

keywords: file share, file storage, accessor share, cross-account share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# Sharing and mounting a file share from another account
{: #file-storage-accessor-create}

As a storage administrator who manages multiple accounts, you can share an NFS file system across accounts, so the data that your applications depend on is available across the different systems. You can also share your {{site.data.keyword.filestorage_vpc_short}} with the [IBM watsonx](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/welcome-main.html?context=wx){: external} service.
{: shortdesc}

[Cross-account service-to-service authorization](/docs/vpc?topic=vpc-file-s2s-auth) is used to establish trust between share owner and accessor accounts. Also, the appropriate IAM platform and services roles must be assigned to the users so they can perform their tasks. To create an accessor share within the same account as the origin share, the user must have *Share Broker* and *Editor* roles. To create an accessor share within a different account from the origin share, the user must have *Share Remote Account Accessor* and *Editor* roles. For more information, see [IAM Roles and actions](/docs/account?topic=account-iam-service-roles-actions#is.share-roles).
{: requirement}

After the authorization is set in place and roles are assigned, you can create an accessor share that is bound to the origin share. The accessor share inherits the profile, size, encryption type both at rest and in-transit from the origin share. The origin share's account can see the IDs of the accounts who can mount the shared NFS share.

When the accessor share is created, it is linked to the origin share by an accessor binding. The accessor bindings are sub-resources that are owned by the origin share, and only the administrator of the origin share can retrieve or delete them.

As the accessor, you can't edit the properties of the origin share, and you can't delete the origin share. The accessors can mount the share by creating an accessor share and a mount target to the accessor share. Then, you can access and use the data of the origin share, including the snapshots that might be present.

Sharing a file share with other accounts or services is not supported for file shares with VPC-wide access mode. 
{: note}

## Transit encryption policy
{: #file-storage-transit-encryption-policy}

The share owner can control how data is encrypted during transit by setting allowed transit encryption modes. If this field is not provided, the system uses the default allowed encryption modes defined in the selected share profile. When an accessor creates a mount target, they must choose an encryption mode that matches one of the share’s allowed modes. For example, if ipsec is selected, all mount targets for that share must use ipsec for consistency.

For zonal file shares, the share owner can choose from the following transit encryption modes: ipsec, none, or both.
- If ipsec is enforced, all accessor mount targets must use ipsec.
- If none is enforced, encryption-in-transit is not allowed.
- If both are allowed, accessor accounts can choose which one to use.

[Select availability]{: tag-green} For regional file shares, the share owner can choose from the following transit encryption modes: stunnel, none, or both.
- If stunnel is enforced, all accessor mount targets must use stunnel.
- If none is enforced, encryption-in-transit is not allowed.
- If both are allowed, accessor accounts can choose which one to use.

File shares that were created before the release of the cross-account access feature (18 June 2024) have an allowed transit encryption type that is based on their existing mount targets. After that date, you must specify the allowed transit encryption type when you create file shares. All mount targets that are created for one file share must have the same transit encryption type.

## Creating an accessor share in the console
{: #file-storage-create-accessor-ui}
{: ui}

In the {{site.data.keyword.cloud_notm}} console, you can create an accessor share with or without a mount target. However, you need to create a mount target when you want to mount the share on a virtual server instance.

### Creating an accessor share in the console
{: #fs-create-accessor-target-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**. A list of file shares displays.

1. On the File storage shares for VPC page, click **Create** >  **Create accessor share**.

1. Enter the following information.

   | Field | Value |
   |-------|-------|
   | Accessor share name  | Specify a meaningful name for your accessor share. The file share name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want.
   | Remote share CRN | Enter the CRN of the origin file share.|
   {: caption="Values for creating a file share" caption-side="top"}

1. You return to the {{site.data.keyword.filestorage_vpc_short}} page, where a message indicates that the file share is provisioning. When the transaction completes, the share status changes to **Active**.

## Creating an accessor share from the CLI
{: #file-storage-create-accessor-cli}
{: cli}

### Before you begin
{: #before-creating-accessor-share-cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

### Creating an accessor share without a mount target from the CLI
{: #fs-create-accessor-share-cli}

You can use the `ibmcloud is share-create` command to provision a file share in your selected zone with the CRN of the origin share.

```sh
ibmcloud is share-create --name my-accessor-share --origin-share CRN
```
{: pre}

```sh
ibmcloud is share-create --name my-accessor-share --origin-share crn:v1:bluemix:public:is:us-south-2:a/7654321::share:r006-d73v40a6-e08f-4d07-99e1-d28cbf2188ed
```
{: pre}

```sh
Creating file share my-file-share under account Test Account as user test.user@ibm.com...

ID                               r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6
Name                             my-accessor-share
CRN                              crn:v1:bluemix:public:is:us-south-2:a/1234567::share:r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6
Lifecycle state                  stable
Access control mode              security_group
Accessor binding role            accessor
Allowed transit encryption modes ipsec,none
Origin share                     CRN                                                                                             Name            Remote account  Remote region
                                 crn:v1:bluemix:public:is:us-south-2:a/7654321::share:r006-d73v40a6-e08f-4d07-99e1-d28cbf2188ed  my-origin-share a7654321        -
Zone                             us-south-2
Profile                          dp2
Size(GB)                         1000
IOPS                             1000
User Tags                        -
Encryption                       provider_managed
Mount Targets                    ID                          Name
                                 No mounted targets found.

Resource group                   ID                                 Name
                                 db8e8d865a83e0aae03f25a492c5b39e   Default

Created                          2024-06-25T22:15:15+00:00
Replication role                 none
Replication status               none
Replication status reasons       Status code   Status message
                                 -             -
Snapshot count                   0
Snapshot size                    0
Source snapshot                  -
Allowed Access Protocols         nfs4    
Availability Mode                zonal   
Bandwidth(Mbps)                  1    
Storage Generation               1  
```
{: screen}

The accessor share inherits the following characteristics from its origin share: profile, size, encryption type both at rest and in-transit. If you try to use this command with the property `--origin-share` with other properties such as `--iops`, `--profile`, `--bandwidth` and `--replica-share`, the request fails.
{: important}

For more information about the command options, see [`ibmcloud is share-create`](/docs/vpc?topic=vpc-vpc-reference#share-create).

### Creating a mount target for an accessor share from the CLI
{: #fs-create-accessor-mount-target-cli}

To create a mount target for the file share, run the `share-mount-target-create` command. Before you begin, gather some necessary information.

When you create a mount target, you must specify the file share that it is for. You can use the file share's name or ID. You must specify the VPC, too, either with its ID or name. The VPC must be unique to each mount target. You must also specify the security access group that's going to be used to manage access to the share. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all servers where you want to mount the share.

Lastly, you must specify values for the options that are needed to create a [virtual network interface](/docs/vpc?topic=vpc-vni-about) for the mount target. Use the appropriate CLI commands to list the available [subnets](/docs/vpc?topic=vpc-vpc-reference#subnets-list), [reserved IP addresses in a subnet](/docs/vpc?topic=vpc-vpc-reference#subnet-reserved-ips-list), [security groups](/docs/vpc?topic=vpc-vpc-reference#security-groups-list) to get the information that you need.

The following example creates a mount target with a virtual network interface for a file share that has security group access mode.

```sh
ibmcloud is share-mount-target-create my-accessor-share --subnet my-subnet --name my-cli-share-mount-target-1 --vni-name my-share-vni-1  --vni-sgs my-sg --resource-group-name Default --vpc my-vpc
```
{: pre}

```sh
Mounting target for share r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6 under account Test Account as user test.user@ibm.com...

ID                          r006-dd497561-c7c9-4dfb-af0a-c84eeee78b61
Name                        my-cli-share-mount-target-1  
VPC                         ID                                          Name      
                            r006-912b5ac5-4ed1-4b2a-8f09-5fd8d79f6042   my-vpc      
                               
Access control mode         security_group   
Resource type               share_mount_target   
Virtual network interface   ID                                          Name             Protocol State Filtering Mode      
                            0727-709ccc60-0c43-45cb-87b6-7ea1e37ef423   my-share-vni-1   auto      
                               
Lifecycle state             stable   
Mount path                  10.240.64.5:/2be3efe6_6dff_46ea_9cec_b397f3141faf   
Transit Encryption          none   
Created                     2024-09-16T21:27:09+00:00   
```
{: screen}

For more information about the command options, see [`ibmcloud is share-mount-target-create`](/docs/vpc?topic=vpc-vpc-reference#share-mount-target-create).

### Creating an accessor share with a mount target from the CLI
{: #fs-create-accessor-share-target-cli}

You can create an accessor share with one or more mount targets in one step by using the `ibmcloud is share-create` command. You need to provide the zone name, and the CRN of the origin share. The accessor share inherits the profile, size, encryption type both at rest and in-transit from the origin share. You can also specify a name, user tags, and even the initial owner UID. To create the mount target, you need to provide the mount target information in JSON format.

The following example shows how to create an accessor share with mount target.

```sh
ibmcloud is share-create --name my-new-accessor-share --origin-share crn:v1:bluemix:public:is:us-south-2:a/7654321::share:r006-d73v40a6-e08f-4d07-99e1-d28cbf2188ed --mount-targets '
>[{"name":"my-new-mount-target","virtual_network_interface": {"name":"my-vni","subnet": {"id":"r006-298acd6c-e71e-4204-a04f-fe4a4dd89805"}}}]'
```
{: pre}

```sh
Creating file share my-new-file-share under account Test Account as user test.user@ibm.com...

ID                               r006-925214bc-ded5-4626-9d8e-bc4e2e579232
Name                             my-new-accessor-share
CRN                              crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-925214bc-ded5-4626-9d8e-bc4e2e579232
Lifecycle state                  pending
Access control mode              security_group
Accessor binding role            accessor
Allowed transit encryption modes ipsec,none
Origin share                     CRN                                                                                             Name            Remote account  Remote region
                                 crn:v1:bluemix:public:is:us-south-2:a/7654321::share:r006-d73v40a6-e08f-4d07-99e1-d28cbf2188ed  my-origin-share a7654321        -

Zone                             us-south-2
Profile                          dp2
Size(GB)                         500
IOPS                             2000
User Tags                        env:dev
Encryption                       provider_managed
Mount Targets                    ID                                          Name
                                 r006-ad313f6b-ccb5-4941-a5f7-0c953f1043df   my-new-mount-target

Resource group                   ID                                 Name
                                 db8e8d865a83e0aae03f25a492c5b39e   Default

Created                          2024-06-25T00:30:11+00:00
Replication role                 none
Replication status               none
Replication status reasons       Status code   Status message
                                 -             -
Snapshot count                   0
Snapshot size                    0
Source snapshot                  - 
Allowed Access Protocols         nfs4    
Availability Mode                zonal   
Bandwidth(Mbps)                  1    
Storage Generation               1  
```
{: screen}

## Creating an accessor share with the API
{: #file-storage-create-accessor-api}
{: api}

You can create file shares and mount targets by directly calling the REST APIs.

### Before you begin
{: #fs-accessor-api-prereqs}

Set up your API environment. Define variables for the IAM token, API endpoint, and API version. For instructions, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

### Creating an accessor share with the API
{: #fs-create-accessor-file-share-api}

Make a `POST /shares` request to create a file share.

The following example shows a request to create an accessor share by using the CRN of the origin share. The accessor share inherits the following characteristics from its origin share: profile, zone, size, IOPS, encryption type both at rest and in-transit. The values for `initial_owner`,`access_control_mode`, and `encryption_key` are also inherited from origin_share.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2024-06-25&generation=2"\
  -H "Authorization: Bearer $iam_token" \
  -d '{
  "name": "my-accessor-share",
  "user_tags": ["string"],
  "origin_share": {"crn": "crn:v1:bluemix:public:is:us-south-1:a/a1234567::share:0fe9e5d8-0a4d-4818-96ec-e99708644a58"}
}
```
{: pre}

A successful response looks like the following example.

```json
  {
  "access_control_mode": "security-group",
  "allowed_transit_encryption_modes": ["none", "ipsec"],
  "created_at": "2024-05-06T23:31:59Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/ff859972-8c39-4528-91df-eb9160eae918",
  "id": "ff859972-8c39-4528-91df-eb9160eae918",
  "iops": 48000,
  "lifecycle_state": "stable",
  "name": "my-accessor-share",
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
  "snapshot_count": 10,
  "snapshot_size": 10,
  "user_tags": ["string"],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"}
  }
```
{: screen}

### Creating a mount target for a file share with the API
{: #fs-create-accessor-mount-target-api}

If the share's access control mode is `security_group`, then the mount target must be created with a [virtual network interface](/docs/vpc?topic=vpc-vni-about). When the share's allowed transit encryption mode is `ipsec`, the mount target's `transit_encryption` value must be `ipsec`, too.

Make a `POST /shares/{share_id}/mount_targets` request and specify a subnet and security group for the mount target network interface. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all servers where you want to mount the share.

This example adds a mount target to an existing accessor share, which is identified by ID, and provides a subnet and security group to create the network interface.

```json
 curl -X POST "$vpc_api_endpoint/v1/shares/r006-201487a2-cf35-4b2a-a4fe-f480803e1e80/mount_targets/?version=2023-07-18&generation=2"\
 -d '{
     "virtual_network_interface": {
        "subnet": {"id": "1a0b3d75-8a62-4c78-9263-f9bcd25a8759"},
        "security_groups": [{"id": "b2599112-7027-480e-ad1b-fd917d2fcb84"}]
     },
     "transit_encryption": "user_managed"
}'
```
{: codeblock}



## Creating a file share and mount target with Terraform
{: #file-storage-create-accessor-terraform}
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

### Creating an accessor share with Terraform
{: #file-accessor-share-create-terraform}

To create an accessor share, use the `ibm_is_share` resource. The accessor share inherits the following characteristics from its origin share: profile, size, encryption type both at rest and in-transit.

```terraform
resource "ibm_is_share" "example-origin" {
  allowed_transit_encryption_modes = ["ipsec", "none"]
  access_control_mode = "security_group"
  name    = "my-share"
  size    = 200
  profile = "dp2"
  zone    = "au-syd-2"
}
resource "ibm_is_share" "example-accessor" {
  origin_share {
    crn   = ibm_is_share.example-origin.crn
  }
  name  = "my-accessor-share-1"
}
```
{: codeblock}

### Creating a mount target with Terraform
{: #file-accessor-share-mount-create-terraform}

To create a mount target for a file share, use the `ibm_is_share_mount_target` resource. The following example creates a mount target with `security_group` access control mode. First, specify the share for which the mount target is created. Then, you specify the name of the mount target and define the new virtual network interface by providing an IP address and a name. You must also specify the security group that you want to use to manage access to the file share that the mount target is associated to. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all servers where you want to mount the share. The attribute `auto_delete = true` means that the virtual network interface is to be deleted if the mount target is deleted.

```terraform
resource "ibm_is_share_mount_target" "zonal-mount-target-with-vni" {
     access_protocol    = "nfs4"
     name               = "my-zonal-mount-target"
     share              = ibm_is_share.is_share.ID
     security_groups    = [<security_group_ids>]
     transit_encryption = "ipsec"
     virtual_network_interface {
       name             = "my-example-vni"
       primary_ip {
          address       = “10.240.64.5”
          auto_delete   = true
          name          = "my-example-pip"
         }
     }    
   }
```
{: codeblock}

The previous example enables encryption in transit for a zonal file share. When you want to protect your data with transit encryption for a reginal share, specify the `transit_encryption` argument as `stunnel`.

For more information about the arguments and attributes, see [ibm_is_share_mount_target](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share_mount_target){: external}.

### Creating an accessor share with a mount target with security group access mode
{: #file-accessor-share-create-with-target-sg-terraform}

You don't need to create the file share and the mount target separately. To create a file share with a mount target that allows for security group-based authentication within a VPC, use the `ibm_is_share` resource. The accessor share inherits the following characteristics from its origin share: profile, size, encryption type both at rest and in-transit. Specify the access control mode as `security_group`, and define the mount target by providing a name for it, details of the virtual network interface such as name, subnet, or IP address. Also, specify the security groups that you want to use to control access to the file share. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all servers where you want to mount the share.

```terraform
resource "ibm_is_share" "share4" {
   zone                   = "us-south-2"
   name                   = "my-share4"
   origin_share {
       id                 = ibm_is_share.example-origin.id
       }
   access_control_mode    = "security_group"
   mount_target {
       access_protocol    = "nfs4"
       name               = "my-mount-target"
       security_groups    = [<security_group_ids>]
       transit_encryption = "ipsec"
       virtual_network_interface {
           primary_ip {
              address.    = "10.240.64.5"
              auto_delete = true
              name        = "my-example-pip"
           }
    }
   }
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.

## Next steps
{: #fs-accessor-create-next-steps}

Mount your file shares. Mounting is a process by which a server's operating system makes files and directories on the storage device available for users to access through the server's file system. For more information, see the following topics:
* [IBM Cloud File Share Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility)
* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-mount-RHEL).
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos).
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-mount-ubuntu).
* [Mounting file shares on z/OS](/docs/vpc?topic=vpc-file-storage-mount-zos)

Creating snapshots of an Accessor share is not supported. However, if the origin share has snapshots, you can access those snapshots in the `.snapshot` directory. For more information, see [Restoring files from a snapshot](/docs/vpc?topic=vpc-fs-snapshots-restore&interface=ui#fs-snapshots-restore-single-file).
{: note}
