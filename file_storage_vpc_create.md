---

copyright:
  years: 2021, 2023
lastupdated: "2023-07-09"

keywords: file share, file storage, virtual network interface, encryption in transit, profiles, 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating file shares and mount targets
{: #file-storage-create}

Create file shares and mount targets in the UI, CLI, API, or Terraform.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Frankfurt, London, Madrid, Dallas, Toronto, Washington, Sao Paulo, Sydney, Osaka, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

Before you get started, to create mount targets for file shares, make sure that you created a [VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{: important}

You can create file shares and mount targets either of the following ways:

* Create a file share and mount target together,
* Create a file share and add mount target later.

## Create a file share in the UI
{: #file-storage-create-ui}
{: ui}

In the {{site.data.keyword.cloud_notm}} console, you can create a file share with or without a mount target. However, you need to create a mount target when you want to mount the share on a virtual server instance. You can create multiple mount targets for the share if it's to be used by hosts in different VPCs.

### Create a file share in the UI
{: #fs-create-share-target-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File Shares**. A list of file shares displays.

1. On the File shares for VPC page, click **Create**.

1. Enter the following information. 

   | Field | Value |
   |-------|-------|
   | **Location** | Choose the geography, region, and zone where you want to create the file share. Location information is inherited from the VPC, for example, North America, Dallas, Dallas 2. |
   | **Details** | |
   | Name  | Specify a meaningful name for your file share. The file share name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want.
   | Resource Group | Use the default resource group or specify a [resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups). Resource groups help organize your account resources for access control and billing purposes. |
   | Tags | Enter any user tags to apply to this file share. As you type, existing tags appear that you can select. For more information about tags, see [Add user tags to a file share](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#fs-add-user-tags). |
   | Access Management Tags | Enter access management tags that you created in IAM to apply them to this file share. For more information about access management tags, see [Access management tags for file shares](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-about-mgt-tags). |
   {: caption="Table 1. Values for creating a file share" caption-side="top"}

1. The creation of mount targets is optional. You can skip this step if you do not want to create a mount target now. For more information about creating mount targets as a separate operation, see [Create a mount target](#fs-create-mount-target-ui). Otherwise, click **Create**. You can create one mount target per VPC per file share. Provide a name for the mount target and select the VPC where the file share is to be used in.

1. Select the right profile for your workload. The profile that you select determines the input/output performance of a file share. The `dp2` profile provides the most flexibility. For more information about file storage IOPS tier and Custom profiles, see [File storage profiles](/docs/vpc?topic=vpc-file-storage-profiles).

1. Specify the size for the file share. You can later [increase this size](/docs/vpc?topic=vpc-file-storage-expand-capacity). The maximum capacity of the share depends on the file share profile.

1. Encryption at rest. By default, all file shares are encrypted by IBM-managed keys. You can also choose to encrypt your shares with your own keys. If you want to use your own keys, select one of the [key management services](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok).

   | Field | Value |
   | ----- | ----- |
   | Encryption | To use customer-managed encryption, select either {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}. The key management service (KMS) instance includes the root key that is imported to or created in that KMS instance. |
   | Encryption service instance | If you provisioned multiple KMS instances in your account, select the one that includes the root key that you want to use for customer-managed encryption. |
   | Key name | Select the root key within the KMS instance that you want to use for encrypting the share. |
   | Key ID | Shows the key ID that is associated with the data encryption key that you selected. |
   {: caption="Table 3. Values for customer-managed encryption for file shares." caption-side="bottom"}

1. When all the required information is entered, click **Create file share**. You return to the {{site.data.keyword.filestorage_vpc_short}} page, where a message indicates that the file share is provisioning. When the transaction completes, the share status changes to **Active**.

### Create a mount target in the UI
{: #fs-create-mount-target-ui}

You can create several mount targets for an existing file share if the share is to be used by resources in multiple VPCs. You can create one mount target per VPC per file share. 

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File shares**.

1. Select a file share from the list.

1. On the File shares details page, under Mount targets, click **Create**. Provide a name for the mount target and Select a VPC from the list.

   You must have at least one VPC to create a mount target. If you don't have one, first [create a VPC](/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc).
   {: requirement}

1. Click **Create**.

## Create a file share from the CLI
{: #file-storage-create-cli}
{: cli}

### Before you begin
{: #before-creating-file-storage-cli}

1. Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

2. To use the CLI, set the `IBMCLOUD_IS_FEATURE_FILESHARE` environment variable to `true`. Copy the following code:

   ```text
   export IBMCLOUD_IS_FEATURE_FILESHARE=true
   ```
   {: pre}

3. After you install the VPC CLI plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.

4. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli).

As of 30 May 2023, you can use `--mount-targets` instead of `-targets` option. To see and use the updated option, set the feature environment variable `IBMCLOUD_IS_FEATURE_FILESHARE_CHANGE_TO_MOUNT_TARGETS` to true.
{: beta}

   ```text
   export IBMCLOUD_IS_FEATURE_FILESHARE_CHANGE_TO_MOUNT_TARGETS=true
   ```
   {: pre}

### Gathering information to create file storage from the CLI
{: #fs-vpc-getinfo-cli}

Before you run the `ibmcloud is share-create` command, you can view information about other file shares, mount targets, and file storage profiles.

Review the following information:

| Details  |  Listing options  | What it provides  |
| -------- | ------------------|-------------------|
| File shares | `ibmcloud is shares` | List all shares in a region. |
| File share details | `ibmcloud is share SHARE_ID`  | Review details of a share. |
| Mount targets  | `ibmcloud is share-mount-targets SHARE_ID` | List all mount targets for a file share. |
| File share profiles   | `ibmcloud is share-profiles` | List all file share profiles in a region. `dp2` is recommended. |
| File share profile details | `ibmcloud is share-profile PROFILE_NAME` | List details of a file share profile. Profile names are `tier-3iops`, `tier-5iops`, `tier-10iops`, `custom`, and `dp2`. |
{: caption="Table 1. Details for creating file shares." caption-side="top"}

### Create a file share and mount target from the CLI
{: #fs-create-share-target-cli}

Run the following command to create a file share and mount target. Indicate the zone in which to create the file share, [file share profile](/docs/vpc?topic=vpc-file-storage-profiles), file share size, and optional name and user tags. Provide a mount target JSON or JSON file to create the mount target in this command.

```sh
ibmcloud is share-create
  --zone ZONE_NAME
  --profile PROFILE
  --size SIZE
  --mount-targets value
  [--name NAME]
  [--initial-owner-gid INITIAL_OWNER_GID ]
  [--initial-owner-uid INITIAL_OWNER_UID]
  [--user-tags USER_TAGS]
  [--mount-targets TARGETS_JSON | @TARGETS_JSON_FILE]
  [--output JSON] [-q, --quiet]
```
{: pre}

This example creates a file share with two mount targets.

```sh
ibmcloud is share-create --name my-file-share-8 --zone us-south-1 --profile tier-3iops --size 40  --user-tags env:dev,env:prod --mount-targets '[{"name": "my-target121","vpc": {"name": "vpcl"}},{"name": "my-target122","vpc": {"name": "vpc2"}}]'
Creating file share my-file-share-8 under account vpc1 as user user@mycompany.com...

ID                0e04ca62-e02a-4319-9551-380c7f7cd87f
Name              my-file-share-8
CRN               crn:[...]
Lifecycle State   pending
Zone              us-south-1
Profile           tier-3iops
Size(GB)          40
IOPS              3000
User Tags         env:dev,env:prod
Encryption        provider_managed
Mount Targets     ID                                       Name           VPC ID                                   VPC Name
                  87171615-f8b4-4ccd-a216-bab12eb1a973     my-target121   81ee2425-7c2f-4042-9d8f-d02b0ee8886c     vpc1
                  b041fc54-f0b9-4397-b68e-2ab032cd1206     my-target122   c338ca08-1256-4420-9932-a9960a0f1cfd     vpc2

Resource Group    ID                                 Name
                  bdd96715c2a44f2bb60df4ff14a543f5   Default
Created           2023-06-20T15:26:21+05:30
```
{: screen}


```bash
ibmcloud is share-create --zone us-south-2 --vpc {vpc_id} --name my-encrypted-share --profile tier-5iops --size 100 --encryption key {crn}
```
{: pre}

The following example shows a new file share that is created with customer-managed encryption.

```bash
$ ibmcloud is share-create --zone us-south-2 --vpc {vpc_id} --name my-encrypted-share --profile tier-5iops --encryption key abccorp-kp-vpc-2 fd57250e-908c-4785-a8a5-1f53176bcd2f
Creating volume demovolume in resource group Default under account VPC 01 as user rtuser1@mycompany.com...
ID                                      339c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    my-encrypted-share
Size                                    100
IOPS                                    1000
Profile                                 tier-5iops
Encryption Key                          crn:v1:bluemix:public:kms:us-south:a/8d65fb1cf5e99e86dd7229ddef9e5b7b:b1abf7c5-381d-4f34-836e-5db7193250bc:key:fd57250e-908c-4785-a8a5-1f53176bcd2f
Encryption                              customer_managed
Status                                  pending
Resource Group                          Default(dbb12715c2a22f2bb60df4ffd4a543f2)
Created                                 2023-02-07 10:09:28
Zone                                    us-south-2
Mount targets                           none
```
{: screen}

To create a file share with replication, see [Create a file share with replication from the CLI](/docs/vpc?topic=vpc-file-storage-create-replication&interface=cli#fs-create-new-share-replica-cli).
{: note}

### Create a mount target for an existing file share from the CLI
{: #fs-create-mount-target-cli}

To create a mount target for the file share, run the `share-mount-target-create` command with the file share and VPC ID or name. The VPC must be unique to each mount target. Specify the values for the `vni` options to create a virtual network interface for the mount target.

```sh
ibmcloud is share-mount-target-create SHARE [--name NAME] [--transit-encryption user_managed | none] [--subnet SUBNET] [([--vni-name VNI_NAME] [[--vni-rip VNI_RIP] | [[--vni-rip-address VNI_RIP_ADDRESS] [--vni-rip-auto-delete VNI_RIP_AUTO_DELETE] [--vni-rip-name VNI_RIP_NAME]]] [--vni-sgs VNI_SGS] --resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME)]  [--output JSON] [-q, --quiet]
```
{: pre}

The following example creates a mount target with VPC-wide access mode for a file share and the VPC is specified by name.

```sh
ibmcloud is share-mount-target-create my-fileshare-1 --vpc test-vpc-1
Mounting target 55251a2e-d6d4-4233-97b2-b5f8e8d1f479 under account test-vpcp1 as user user@mycompany.com...

ID                78ff9c4c97d013fb2a95b21abcde7758
Name              mytarget-1
VPC               ID                                          Name
                  55251a2e-d6d4-4233-97b2-b5f8e8d1f479   test-vpc-1

Lifecycle State   pending
Mount path        dal1051b-fz.adn.networklayer.com:/nxg_s_voll_mz0717_fde90e26_8796_4a5e_8147_dd14976d6e9f
Created           2023-06-20T18:05:18+05:30
```
{: screen}

The following example creates a mount target with security group access mode for a file share. This command creates and attaches a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to your mount target that identifies the file share with a reserved IP address and applies the rules of the selected Security group.The virtual network interface is defined by specifying the reserved IP address, and the rules of `SECURITY_GROUP1` determine which resources can access the mount target `my-target-3`.
[New]{: tag-new}

```sh
ibmcloud is share-mount-target-create my-new-share --subnet cli-subnet-1 --name my-mount-target-3 --vni-name cli-share-vni-1 --vni-rip my-reserved-ip --vni-sgs concrete-proudly-coastal-obvious,sg-vni --resource-group-name Default
```
{: codeblock}

## Create a file share with the API
{: #file-storage-create-api}
{: api}

You can create file shares and mount targets by directly calling the REST APIs. 

{{site.data.keyword.filestorage_vpc_short}} regional API is released as beta for customers with special approval to preview this feature. For more information about the file shares VPC API, see the [VPC API Beta reference](/apidocs/vpc-beta).
{: beta}

### Before you begin 
{: #fs-api-prereqs}

Set up your API environment. Define variables for the IAM token, API endpoint, and API version. For instructions, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

As described in the [Beta VPC API](/apidocs/vpc-beta) reference [versioning](/apidocs/vpc-beta#api-versioning-beta) policy, support for older versions of the beta API is limited to 45 days. Therefore, beta API requests must specify a `version` query parameter date value within the last 45 days. You must also provide `generation` parameter and specify `generation=2`. For more information, see **Generation** in the [Virtual Private Cloud API reference](/apidocs/vpc#api-generation-parameter).
{: requirement}

A good way to learn more about the API is to click **Get sample API call** on the provisioning pages in {{site.data.keyword.cloud_notm}} console. You can view the correct sequence of API requests and better understand actions and their dependencies.
{: tip}

### Create a file share with the API
{: #fs-create-file-share-api}

Make a `POST /shares` request to create a file share. Specify the size of the file share, a name, the IOPS profile, and zone.

The following example shows a request to create a 4800 GB file share with a 10 IOPS/GB profile. It specifies the access control mode `vpc`, which enables all clients in each mount target's VPC to have access to this file share.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2023-06-20&generation=2&maturity=beta"\
-H "Authorization: $iam_token" \
-d '{
    "size": 4800,
    "name": "myshare-1",
    "profile": {
      "name": "tier-10iops"
    },
    "access_control_mode": "vpc",
    "zone": {
      "name": "us-south-1"
    }
  }'
```
{: pre}

A successful response looks like the following example.

```json
{
  "access_control_mode": "vpc",
  "created_at": "2023-06-20T22:31:50Z",
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
    "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/tier-10iops",
    "name": "tier-10iops",
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

### Create a file share and mount target together with the API
{: #fs-create-share-target-api}

The following example request creates a file share that has VPC-wide access mode and a mount target that can be used by every virtual server instance in the specified VPC. It also adds [user tags](/docs/vpc?topic=vpc-file-storage-managing&interface=api#fs-add-user-tags) to the share. 

Access to the mount target is VPC wide; all instances in the VPC have access to this file share. You can also restrict access to a specific virtual server instance in the VPC by specifying a virtual network interface in the mount target definition.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2023-06-20&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json' \
-d '{
    "size": 4800,
    "mount_targets": [
      {
        "name": "mount-target-name1",
        "vpc": {
          "id": "a1fb6c4f-6a63-4d34-8bf6-55fab89e932a"
        }
      }
    ],
    "name": "share-name1",
    "profile": {
      "name": "tier-10iops"
    },
    "user_tags": [
      "env:test",
      "env:prod"
    ],
    "zone": {
      "name": "us-south-1"
    }
  }'
```
{: pre}

A successful response looks like the following example.

```json
{
  "access_control_mode": "vpc",
  "created_at": "2023-06-20T23:31:59Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/ff859972-8c39-4528-91df-eb9160eae918",
  "id": "ff859972-8c39-4528-91df-eb9160eae918",
  "iops": 48000,
  "lifecycle_state": "stable",
  "name": "share-name1",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/tier-10iops",
    "name": "tier-10iops",
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
  "user_tags": [
    "env:test",
    "env:prod"
   ],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: codeblock}

### Create a file share and mount target by specifying a subnet
{: #fs-create-file-share-subnet-vni-api}

[New]{: tag-new}

The following examople creates and attaches a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to your mount target that identifies the file share with a reserved IP address and applies the rules of the selected Security group. To create the mount target with the network interface, make a `POST /shares` request and specify a subnet. Specifying the `subnet` property is required when you're not using the [`primary_ip` property](#fs-create-file-share-pni-api) and specifying `address`for a reserved IP.

In this example, the mount target specifies a subnet ID. The default access control mode is `security_group`, which is shown in the response.

```json
curl -X POST "$vpc_api_endpoint/v1/shares?version=2023-05-11&generation=2&maturity=beta" \
-H "Authorization: $iam_token" \
-d '{
    "size": 10,
    "name": "myshare-1",
    "profile": {
        "name": "dp2"
     },
     "zone": {
        "name": "us-south-1"
     },
     "mount_targets": [
        "virtual_network_interface": {
            "subnet": {
                "id": "4e95744c-7e64-48c9-b5d2-3b6481b1dfde"
                },
        },
         "transit_encryption": {
            "none"
        }
    ]
}'
```
{: codeblock}

A successful response looks like the following example.

```json
 {
    "access_control_mode": "security_group",
    "created_at": "2023-05-11T12:15:12Z",
    "href": "$vpc_api_endpoint/v1/shares/90c4bb62-1724-47bd-8c45-f7d37d7c3508/mount_targets/7e5bdb52-676d-43b2-991f-2053cf6855eb",
    "id": "7e5bdb52-676d-43b2-991f-2053cf6855eb",
    "lifecycle_state": "pending",
    "mount_path": "",
    "name": "myshare-1",
    "primary_ip": {
        "address": ""
    },
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

[New]{: tag-new}

To create the mount target network interface, make a `POST /shares` request and specify a subnet and security group.

In this example, the `mount_targets` property specifies a subnet ID and security group ID.

```json
curl -X POST "$vpc_api_endpoint/v1/shares?version=2023-06-11&generation=2&maturity=beta" \
-H "Authorization: $iam_token" \
-d '{
    "size": 20,
    "name": "myshare-3",
    "profile": {
        "name": "dp2"
     },
     "zone": {
        "name": "us-south-1"
     },
     "mount_targets": [
        "virtual_network_interface": {
            "subnet": {
                "id": "4e95744c-7e64-48c9-b5d2-3b6481b1dfde"
                },
            "security_groups": [
                {
                "id": "34c09abb-37bf-4ef6-88bb-f63a0ef28915"
                }
            ]
        },
         "transit_encryption": {
            "none"
        }
    ]
}'
```
{: codeblock}

The following response shows that access control mode is `security_group`, which is the default value.

```json
{
    "access_control_mode": "security_group",
    "created_at": "2023-06-11T12:55:40Z",
    "crn": "crn:[...]",
    "encryption": "provider_managed",
    "href": "$vpc_api_endpoint/v1/shares/r134-56f91d4a-2801-470a-b368-176bde64e954",
    "id": "r134-56f91d4a-2801-470a-b368-176bde64e954",
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
            "href": "$vpc_api_endpoint/v1/shares/r134-56f91d4a-2801-470a-b368-176bde64e954/mount_targets/r134-b8573e2c-60ee-4ecc-9eae-c52f890a8195",
            "id": "r134-b8573e2c-60ee-4ecc-9eae-c52f890a8195",
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

### Creating a file share and mount target by specifying a primary network interface
{: #fs-create-file-share-pni-api}

[New]{: tag-new}

Make a `POST /shares` request and create a mount target with a primary network interface. Specify the `primary_ip` property and optional `address` subproperty, which reserves the IP address that you indicate. This address must not already be reserved on the subnet.

If you don't specify an address, an available address on the subnet is automatically selected. The default access control mode is `security_group`, which is displayed in the response.

By default, `auto_delete` for the primary IP is set to `true`. When you create a mount target with this option and later delete the target, the primary IP is also deleted. To release the primary IP when the mount target is deleted, set `auto_delete` to `false`. This example releases the primary IP.

```json
curl -X POST "$vpc_api_endpoint/v1/shares?version=2023-05-11&generation=2&maturity=beta" \
-H "Authorization: $iam_token" \
-d '{
    "size": 10,
    "name": "share-sc-2",
    "profile": {
        "name": "dp2"
    },
    "zone": {
        "name": "us-south-3"
    },
    "mount_targets": [
        "virtual_network_interface": {
            "primary_ip": {
                "name": "primary-ip1",
                "address": "192.0.2.0",
                "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/aea5fe79f-52c3-4f05-86ae-9540a10489f5/reserved_ips/6fd4925d-7774-4e87-829e-7e5765d454ad",
                "id": "6fd4925d-7774-4e87-829e-7e5765d454ad",
                "name": "my-reserved-ip",
                "auto_delete": "false"
                }
            },
        "transit_encryption": {
            "none"
          }
       ]
    }'
 ```
 {: codeblock}

### Create a mount target for an existing file share with the API
{: #fs-create-mount-target-api}

This request creates or adds a mount target to an existing file share. In this example, the `vpc` property is specified because the file share's access control mode is `vpc`. Data encryption in transit is not enabled.

Access control modes must match when a mount target is created for an existing share. Both must be either `vpc` or `security_group`.
{: important}

```curl
curl -X POST \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets?version=2023-06-20&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{
    "name": "mount-target-name1",
    "vpc": {
      "id": "6e01bc24-4a6e-4a0c-a1bd-4caa0c8159e7"
    },
    "transit_encryption": "none"
  }'
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "access_control_mode": "vpc",
  "created_at": "2023-06-20T23:31:59Z",
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

[New]{: tag-new}

Make a `POST /shares/{share_id}/mount_targets` request and specify a subnet and security group for the mount target network interface.

This example adds a mount target to an existing file share, which is identified by ID, and provides a subnet and security group to define the network interface. 

```json
 curl -X POST "$vpc_api_endpoint/v1/shares/f1ab81ef-dd30-459a-85e0-9094164978b1/mount_targets/?version=2023-04-04&generation=2&maturity=beta"\
 -d '{
     "virtual_network_interface": {
        "subnet": {
            "id": "1a0b3d75-8a62-4c78-9263-f9bcd25a8759"
            },
        "security_groups": [
            {
            "id": "b2599112-7027-480e-ad1b-fd917d2fcb84"
            }
        ]
     },
     "transit_encryption": "user_managed"
}'
```
{: codeblock}

### Add supplemental IDs when you create a file share with the API
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
"$vpc_api_endpoint/v1/shares?version=2023-05-06&generation=2&maturity=beta"\
-H "Authorization: $iam_token" \
-d '{
    "initial_owner": {
       "gid": 101,
       "uid": 10001
    },
    "size": 4800,
    "name": "share-name",
    "profile": {
    "name": "tier-10iops"
     },
    "zone": {
       "name": "us-south-1"
     }
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

VPC infrastructure services use a regional specific based endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the right region in the provider block in the `provider.tf` file.

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

To create a file share, use the `ibm_is_share` resource. To add a replica, define the replica share similarly to the following example. It creates a share with 220 GB capacity and the `tier-3iops` performance profile. The `replica_share` argument defines the replica share's name, the frequency of replication (in cron spec), the performance profile, and the zone where the replica share is to be created.

```terraform
resource "ibm_is_share" "example-2" {
  zone    = "us-south-1"
  size    = "220"
  name    = "my-share"
  profile = "tier-3iops"
  replica_share {
    name                  = "my-replica"
    replication_cron_spec = "0 */5 * * *"
    profile               = "tier-3iops"
    zone                  = "us-south-3"
  }
}
```
{: codeblock}

### Creating a file share with a mount target with VPC-wide access mode
{: #file-share-create-with-target-vpc-terraform}

To create a file share with a mount target that is accessible to all virtual server instances within a VPC, use the `ibm_is_share` resource. Specify the access control mode as `vpc`, and define the mount target by providing a name for it and the VPC where it's going to be used in.

```terraform
resource "ibm_is_share" "share3" {
    zone    = "us-south-2"
    size    = "700"
    name    = "my-share3"
    profile = "tier-3iops"
    access_control_mode = "vpc" 
    mount_target {
          name = "target"
          vpc = <vpc_id>
    }
}
```
{: codeblock}

### Creating a file share with a mount target with security group access mode
{: #file-share-create-with-target-sg-terraform}

To create a file share with a mount target that allows for security group-based authentication within a VPC, use the `ibm_is_share` resource. Specify the access control mode as `security_group`, and define the mount target by providing a name for it, details of the virtual network interface such as name, subnet, or IP address. Also, specify the security groups that you want to use to control access to the file share.

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

### Creating a mount target with Terraform
{: #file-share-mount-create-terraform}

To create a mount target for a file share, use the `is_share_mount_target` resource. The following example creates a mount target with the name `my-share-target` with the VPC access control mode. All virtual server instances in the VPC can mount the share by using the mount target `my-share-target`.

```terraform
resource "ibm_is_share_mount_target" "target" {
     name = <share_target_name>
     vpc = <vpc_id>
}
```
{: codeblock}

You can also create mount targets that provide granular authentication with the use of Security groups. The following example creates a mount target with `security_group` access control mode. You specify the name of the mount target and define the new virtual network interface by providing an IP address and a name. You must also specify the security group that you want to use to manage access to the file share that the mount target is associated to. The attribute `auto_delete = true` means that the virtual network interface is to be deleted if the mount target is deleted.

```terraform
resource "ibm_is_share_mount_target" "target" {
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

For more information about the arguments and attributes, see [ibm_is_share_mount_target](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share_mount_target){: external}.

## Next steps
{: #fs-create-next-steps}

Mount and use your file shares:

* [Viewing file shares and mount targets](/docs/vpc?topic=vpc-file-storage-view).
* [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit).
* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-vpc-mount-RHEL).
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos).
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-vpc-mount-ubuntu).
* [Manage your file shares](/docs/vpc?topic=vpc-file-storage-managing).
* [Create a file share with replication](/docs/vpc?topic=vpc-file-storage-create-replication).
