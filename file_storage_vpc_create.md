---

copyright:
  years: 2021, 2023
lastupdated: "2023-03-31"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating file shares and mount targets
{: #file-storage-create}

Create file shares and mount targets in the UI, CLI, or API.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Frankfurt, London, Dallas, Toronto, Washington, Sao Paulo, Sydney, Osaka, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

Before you get started, to create mount targets for file shares, make sure that you created a [VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{: important}

You can create file shares and mount targets either of the following ways:

* Create a file share and mount target together,
* Create a file share and add mount target later.

## Create a file share in the UI
{: #file-storage-create-ui}
{: ui}

In the {{site.data.keyword.cloud_notm}} console, you can create a file share and mount targets.

### Create a file share and mount target in the UI
{: #fs-create-share-target-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File Shares**. A list of file shares displays.

1. On the File shares for VPC page, click **Create**. The **Create** tab is selected by default.

1. Enter the information described in the Table 1. When you create a file share, specify the information for the mount target as well.

1. When finished, click **Create file share**. You're returned to the {{site.data.keyword.filestorage_vpc_short}} page, where a message indicates that the file share is provisioning. When completed, the status changes to **Active**.

| Field | Value |
|-------|-------|
| **Location** | Choose the geography, region, and zone where you want to create the file share. Location information is inherited from the VPC, for example, North America, Dallas, Dallas 2. |
| **Details** | |
| Name  | Specify a meaningful name for your file share. The file share name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want.
| Resource Group | Use the default resource group or specify a [resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups). Resource groups help organize your account resources for access control and billing purposes. |
| Tags | Enter user tags to apply to this file share. As you type, existing tags appear that you can select. For more information about tags, see [Add user tags to a file share](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#fs-add-user-tags). |
| Access Management Tags | Enter access management tags that you created in IAM to apply them to this file share. For more information about access management tags, see [Access management tags for file shares](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-about-mgt-tags). |
| Mount targets (Optional) | Click **Create** to create a new [mount target](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets) for the file share. You can create one mount target per VPC per file share. Provide a name for the mount target and select a VPC in that zone. You can add as many mount targets as you have VPCs. If you don't have one, first [create a VPC](/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc). For more information about creating mount targets as a separate operation, see [Create a mount target](#fs-create-mount-target-ui). |
| Profile | Select a profile for the file share. The profile that you select determines the input/output performance of the file share. For more information about the dp2, IOPS tier, or Custom profiles, see [File storage profiles](/docs/vpc?topic=vpc-file-storage-profiles). |
| Size | Specify the size for the file share. You can later [increase this size](/docs/vpc?topic=vpc-file-storage-expand-capacity), depending on the file share profile. |
| Encryption | Encryption with IBM-managed keys is enabled by default when you create a new file share. You can also choose **Customer Managed** and use your own encryption key. For more information about creating encrypted file shares, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption). If you create a replica share when you provision a new file share, the encryption is inherited. You can't encrypt a replica with a different key. If you change the encryption on the source share, the replica is updated. |
| Asynchronous replica | Informational field that says that you can create a replica after the file share is created. |
{: caption="Table 1. Values for creating a file share and mount target" caption-side="top"}

To see the REST API call, click the **Create with REST API </>** link. Viewing the API calls is a good way to learn about the API and understand actions and their dependencies.
{: tip}

### Create a mount target in the UI
{: #fs-create-mount-target-ui}

You can create one or several mount targets for an existing file share.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File shares**.

2. Select a file share from the list.

3. On the File shares details page, under Mount targets, click **Create**.

   You must have at least one VPC to create a mount target. If you don't have one, first [create a VPC](/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc).
   {: note}

4. In the **Create mount target** window, provide a name for the mount target.

5. Select a VPC from the list and then click **Create**.

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

### Gathering information to create file storage from the CLI
{: #fs-vpc-getinfo-cli}

Before you run the `ibmcloud is share-create` command, you can view information about other file shares, mount targets, and file storage profiles.

Review the following information:

| Details  |  Listing options  | What it provides  |
| -------- | ------------------|-------------------|
| File shares | `ibmcloud is shares` | List all shares in a region. |
| File share details | `ibmcloud is share SHARE_ID`  | Review details of a share. |
| Mount targets  | `ibmcloud is share-targets SHARE_ID` | List all mount targets for a file share. |
| File share profiles   | `ibmcloud is share-profiles` | List all file share profiles in a region. |
| File share profile details | `ibmcloud is share-profile PROFILE_NAME` | List details of a file share profile. Profile names are `tier-3iops`, `tier-5iops`, `tier-10iops`, `custom`, and `dp2`. |
{: caption="Table 1. Details for creating file shares." caption-side="top"}

### Create a file share and mount target from the CLI
{: #fs-create-share-target-cli}

Run the following command to create a file share and mount target. Indicate the zone in which to create the file share, [file share profile](/docs/vpc?topic=vpc-file-storage-profiles), file share size, and optional name and user tags. Provide a mount target JSON or JSON file to create the mount target in this command.

To create a file share with replication, see [Create a new file share with replication from the CLI](/docs/vpc?topic=vpc-file-storage-create-replication&interface=cli#fs-create-new-share-replica-cli).
{: note}

```sh
ibmcloud is share-create
  --zone ZONE_NAME
  --profile PROFILE
  --size SIZE
  [--name NAME]
  [--user-tags USER_TAGS]
  [--mount-targets TARGETS_JSON | @TARGETS_JSON_FILE]
  [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
  [--output JSON] [-q, --quiet]
```
{: pre}

This example creates a file share with two mount targets.

```text
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
Created           2022-09-17T15:26:21+05:30
```
{: screen}

### Create a mount target for an existing file share from the CLI
{: #fs-create-mount-target-cli}

Run the `share-target-create` command with the file share and VPC ID or name to create a mount target on the file share. The VPC must be unique to each mount target.

```sh
ibmcloud is share-target-create SHARE_ID [SHARE_NAME] --vpc VPC_ID [NAME] [--output JSON] [-q, --quiet]
```
{: pre}

This example command creates a mount target for a file share and VPC specified by name.

```text
ibmcloud is share-target-create my-fileshare-1 --vpc test-vpc-1
Mounting target 55251a2e-d6d4-4233-97b2-b5f8e8d1f479 under account test-vpcp1 as user user@mycompany.com...

ID                78ff9c4c97d013fb2a95b21abcde7758
Name              mytarget-1
VPC               ID                                          Name
                  55251a2e-d6d4-4233-97b2-b5f8e8d1f479   test-vpc-1

Lifecycle State   pending
Mount path        dal1051b-fz.adn.networklayer.com:/nxg_s_voll_mz0717_fde90e26_8796_4a5e_8147_dd14976d6e9f
Created           2022-09-17T18:05:18+05:30
```
{: screen}

## Create a file share with the API
{: #file-storage-create-api}
{: api}

You can create file shares and mount targets by directly calling the REST APIs. For more information about the file shares VPC API, see the [VPC API reference](/apidocs/vpc-beta).

{{site.data.keyword.filestorage_vpc_short}} regional API is released as beta for customers with special approval to preview this feature.
{: note}

### Before you begin – Set up your API environment
{: #fs-api-prereqs}

Define variables for the IAM token, API endpoint, and API version. For instructions, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

A good way to learn more about the API is to click **Get sample API call** on the provisioning pages in {{site.data.keyword.cloud_notm}} console. You can view the correct sequence of API requests and better understand actions and their dependencies.
{: tip}

### Create a file share with the API
{: #fs-create-file-share-api}

Make a `POST/shares` request to create a file share. Specify the size of the file share, a name, the IOPS profile, and zone.

You must provide `generation` parameter with the API request and specify `generation=2`. Do not use `generation=1` for the file storage service. For more information, see **Generation** in the [Virtual Private Cloud API reference](/apidocs/vpc#api-generation-parameter).

To create a new file share and a replica file share, see [Create a file share with replication with the API](/docs/vpc?topic=vpc-file-storage-create-replication&interface=api#fs-create-new-share-replica-api).
{: note}

The following example shows a request to create a 4800 GB file share with a 10 IOPS/GB profile.

```curl
curl -X POST \
"$rias_endpoint/v1/shares?version=2023-03-28&generation=2"\
-H "Authorization: $iam_token" \
-d '{
    "size": 4800,
    "name": "myshare-1",
    "profile": {
      "name": "tier-10iops"
    },
    "zone": {
      "name": "us-south-1"
    }
  }'
```
{: pre}

A successful response looks like the following example.

```json
{
  "created_at": "2023-03-28T22:31:50Z",
  "crn": "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "https://us-south-1.cloud.ibm.com/v1/shares/acd96d70-b8d3-4b56-ad7f-9c1035df93b2",
  "id": "acd96d70-b8d3-4b56-ad7f-9c1035df93b2",
  "initial_owner": {
    "gid": 0,
    "uid": 0
  },
  "iops": 3000,
  "lifecycle_state": "pending",
  "name": "myshare-1",
  "profile": {
    "href": "https://us-south-1.cloud.ibm.com/v1/share/profiles/tier-10iops",
    "name": "tier-10iops",
    "resource_type": "share_profile"
  },
  "replication_role": "none",
  "replication_status": "none",
  "replication_status_reasons": [],
  "resource_group": {
    "crn": "crn:v1:staging:public:resource-controller::a/e2f80b84-bc75-4f53-8737-8193ef1d1a7b::resource-group:e96d1fa9-76f2-4c87-a737-dbab3a947b24",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/e96d1fa9-76f2-4c87-a737-dbab3a947b24",
    "id": "e96d1fa9-76f2-4c87-a737-dbab3a947b24",
    "name": "Default"
  },
  "resource_type": "share",
  "size": 4800,
  "targets": [],
  "zone": {
    "href": "https://us-south-1.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: codeblock}

### Create a file share and mount target together with the API
{: #fs-create-share-target-api}

This request creates a file share and a mount target, and adds [user tags](/docs/vpc?topic=vpc-file-storage-managing&interface=api#fs-add-user-tags) to the share.

```curl
curl -X POST \
"$rias_endpoint/v1/shares?version=2023-03-28&generation=2"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json' \
-d '{
    "size": 4800,
    "targets": [
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
  "created_at": "2023-03-28T23:31:59Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "$vpc_api_endpoint/v1/shares/ff859972-8c39-4528-91df-eb9160eae918",
  "id": "ff859972-8c39-4528-91df-eb9160eae918",
  "iops": 48000,
  "lifecycle_state": "stable",
  "name": "share-name1",
  "profile": {
    "href": "$vpc_api_endpoint/v1/share/profiles/tier-10iops",
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
  "targets": [
    {
      "href": "$vpc_api_endpoint/v1/shares/ff859972-8c39-4528-91df-eb9160eae918/targets/9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
      "id": "9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
      "name": "mount-target-name1",
      "resource_type": "share_target",
      "vpc": {
        "crn": "crn:[...]",
        "href": "$vpc_api_endpoint/v1/vpcs/e6ff7b61-feb4-4c87-94aa-277d6f93e164",
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
    "href": "$vpc_api_endpoint/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: codeblock}

### Create a mount target with the API
{: #fs-create-mount-target-api}

This request creates or adds a mount target to an existing file share.

```curl
curl -X POST \
"$rias_endpoint/v1/shares/$share_id/targets?version=2023-03-28&generation=2"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{
    "name": "mount-target-name1",
    "vpc": {
      "id": "6e01bc24-4a6e-4a0c-a1bd-4caa0c8159e7"
    }
  }'
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "created_at": "2023-03-28T23:31:59Z",
  "href": "$vpc_api_endpoint/v1/shares/ff859972-8c39-4528-91df-eb9160eae918/targets/9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "id": "9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "lifecycle_state": "pending",
  "mount_path": "domain.com:/vol_xyz_2891fd0a_63aa_4deb_9ed5_1159e37cb5aa",
  "name": "mount-target-name1",
  "resource_type": "share_target",
  "vpc": {
    "crn": "crn:[...]",
    "href": "$vpc_api_endpoint/v1/vpcs/e6ff7b61-feb4-4c87-94aa-277d6f93e164",
    "id": "e6ff7b61-feb4-4c87-94aa-277d6f93e164",
    "name": "vpc-name1",
    "resource_type": "vpc"
  }
}
```
{: codeblock}

### Add supplemental IDs when you create a file share with the API
{: #fs-add-supplemental-id-api}

With the API, you can set `UID` and `GID` values for the `initial_owner` property to control access to your file shares. Wherever you mount the file share, the root folder uses that user ID and group ID owner. You set the `UID` or `GID`, or both when you create a share in a `POST /shares` call.

If you change the supplemental IDs (UID or GID) from the virtual server instance, there is no way to determine that it was changed. As a result, `initial_owner` does not change in the API database but changes only in the file storage system.
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

```curl
curl -X POST \
"$rias_endpoint/v1/shares?version=2023-03-28&generation=2\
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

## Next steps
{: #fs-create-next-steps}

Mount and use your file shares:

* [Viewing file shares and mount targets](/docs/vpc?topic=vpc-file-storage-view).
* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-vpc-mount-RHEL).
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos).
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-vpc-mount-ubuntu).
* [Manage your file shares](/docs/vpc?topic=vpc-file-storage-managing).
