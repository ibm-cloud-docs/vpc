---

copyright:
  years: 2021
lastupdated: "2021-05-03"

keywords: file storage, virtual private cloud, file share, mount target

subcollection: vpc

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:external: target="_blank" .external}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Creating file shares and mount targets
{: #file-storage-create}

Create file shares and mount targets by using the UI, CLI, or API. 
{:shortdesc}

File Storage for VPC is available to customers with special approval to preview this service in the Washington, Dallas, and Frankfurt regions. Contact your IBM Sales representative if you are interested in getting access.
{:note}

Before you get started, to create mount targets for file shares, make sure that you created a [VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{:important}

You can create file shares and mount targets either of the following ways:

* Create a file share and mount target together
* Create a file share and add mount target later

## Create a file share using the UI
{: #file-storage-create-ui}
{: ui}

In the {{site.data.keyword.cloud_notm}} console, you can create a file shares and mount targets.

### Create a file share and mount target using the UI
{: #fs-create-share-target-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File Shares**.

1. Click **Create**.

1. Enter the information described in the Table 1.

1. While creating a share, specify the information for the mount target as well.

1. When finished, click **Create file share**. You're returned to the File Shares for VPC page, where a message indicates that the file share is provisioning. When completed, the status changes to **Active**.

| Field | Value |
|-------|-------|
| Name  | Specify a meaningful name for your file share. The share name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want. |
| Resource Group | Specify a [resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups). Resource groups help organize your account resources for access control and billing purposes. |
| Location | Choose the zone where you want to create the file share. The zones inherited from the VPC, for example, _US South 3_. |
| Mount targets (Optional) | Click **Create** to create a new [mount target](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets) for the file share. You can create one mount target per VPC per file share. Provide a name for the mount target and select a VPC in that zone. You can add as many mount targets are you have VPCs. If you don't have one, first [create a VPC](/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc). (To use the API, see [Creating a VPC using the REST APIs](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis).) For information about creating mount targets as a separate operation, see [Create a mount target](#fs-create-mount-target-ui). |
| Profile | Select an IOPS tier for file share. The tier you select determines the input/output performance of a file share. For more information about file storage IOPS tier profiles, see [IOPS tier profiles](/docs/vpc?topic=vpc-file-storage-profiles). |
| Size | Specify the size for the file share. |
{: caption="Table 1. Values for creating a file share and mount target" caption-side="top"}

When you create file share, you can click the **</>** icon to view the sequence of API requests. Viewing the API calls is a good way to learn about the API and understand actions and their dependencies.
{: tip}

### Create a mount target using the UI
{: #fs-create-mount-target-ui}

You can create one or several mount targets for an existing file share.

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File shares**.

2. Select a file share from the list.

3. On the File shares details page, under Mount targets, click **Create**.

   You must have at least one VPC to create a mount target. If you don't have one, first [create a VPC](/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc).
   {:note}

4. In the **Create mount target** window, provide a name for the mount target.

5. Select a VPC from the list and then click **Create**.

## Create a file share using the CLI
{: #file-storage-create-cli}
{: cli}

### Before you begin
{: #before-creating-file-storage-cli}

1. Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

2. To use the CLI, set the `IBMCLOUD_IS_FEATURE_FILESHARE` environment variable to `true`. Copy the following code:

   ```
   export IBMCLOUD_IS_FEATURE_FILESHARE=true
   ```
   {:pre}

3. After you install the VPC CLI plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.
   
4. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli).

### Gathering information to create file storage with the CLI
{: #fs-vpc-getinfo-cli}

Before you run the `ibmcloud is share-create` command, you can view information about other file shares, mount targets, and file storage profiles.

Review the following information:

|     Details   |  Listing options  | What it provides  |
| --------------------- | --------------------------------|---------------------|
| File shares           | `ibmcloud is shares`            | List all shares in a region. |
| Share details           | `ibmcloud is share SHARE_ID`        | Review details of a share. |
| Mount targets             | `ibmcloud is share-targets SHARE_ID` | List all mount targets for a file share. |
| Share profiles         | `ibmcloud is share-profiles`     | List all file share profiles in a region. |
| Share profile details | `ibmcloud is share-profile PROFILE_NAME`     | List details of a file share profile. Profile names are `tier-3iops`, `tier-5iops`, and `tier-10iops`. |
{: caption="Table 1. Details for creating file shares" caption-side="top"}

### Create a file share and add a mount target using the CLI
{: #fs-create-share-target-cli}

Run the following command to create a file share and mount target. Indicate the zone in which to create the share, [share profile](/docs/vpc?topic=vpc-file-storage-profiles), share size, and name (optional). Provide a mount target JSON or JSON file to create the mount target in this command.

```
ibmcloud is share-create --zone ZONE_NAME --profile PROFILE --size SIZE [--name NAME] [--targets TARGETS_JSON | @TARGETS_JSON_FILE] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
```
{:pre}

Example mount target JSON file:

```json
{
  "name": "target-name1",
  "vpc": {
    "id": "6e01bc24-4a6e-4a0c-a1bd-4caa0c8159e7"
  }
}
```
{:codeblock}

### Create a mount target for an existing share using the CLI
{: #fs-create-mount-target-cli}

Run the `share-target-create` command with the file share ID and VPC ID to create a mount target on the file share. The VPC must be unique to each mount target.

```
ibmcloud is share-target-create SHARE_ID --vpc VPC_ID [--name NAME] [--output JSON] [-q, --quiet]
```
{:pre}

This example command creates a mount target and outputs share and target information to JSON.

```
ibmcloud is share-target-create 78ff9c4c97d013fb2a95b21abcde7758 --vpc 55251a2e-d6d4-4233-97b2-b5f8e8d1f479 --name target-name --output JSON
```
{:pre}


## Create a file share from the API
{: #file-storage-create-api}
{: api}

You can create file shares and mount targets by directly calling the REST APIs. For more information about the File Storage VPC API methods to create a file share, see the [VPC API reference](/apidocs/vpc-beta#create-share).

File Storage for VPC regional API is a beta-level release for customers with special approval to preview this feature. 
{:note}

### Before you begin – Set up your API environment
{: #fs-api-prereqs}

Define variables for the IAM token, API endpoint, and API version. For instructions, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

A good way to learn more about the API is to click **Get sample API call** on the provisioning pages in {{site.data.keyword.cloud_notm}} console. You can view the correct sequence of API requests and better understand actions and their dependencies.
{: tip}

### Create a file share from the API
{: #fs-create-file-share-api}

Use the `POST/shares` request to create a file share. Specify the size of the share, a name, the IOPS profile, and zone. 

You must provide `generation` parameter with the API request and specify `generation=2`. Do not use `generation=1` for the  file storage service. For more information, see **Generation** in the [Virtual Private Cloud API reference](https://{DomainName}/apidocs/vpc#api-generation-parameter).
{: note}

For example:

```curl
curl -X POST \
"$rias_endpoint/v1/shares?version=2021-02-23&generation=2\
-H "Authorization: $iam_token" \
-d $'{
  "size": 4800,
  "name": "share-name",
  "profile": {
    "name": "tier-10iops"
  },
  "zone": {
    "name": "us-south-1"
  }
}'
```
{: pre}

A successful response will look like this:

```json
{
  "created_at": "2021-03-31T23:31:59Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "$vpc_api_endpoint/v1/shares/ff859972-8c39-4528-91df-eb9160eae918",
  "id": "ff859972-8c39-4528-91df-eb9160eae918",
  "iops": 48000,
  "lifecycle_state": "pending",
  "name": "share-name1",
  "profile": {
    "href": "$vpc_api_endpoint/v1/share/profiles/tier-10iops",
    "name": "tier-10iops",
    "resource_type": "share_profile"
  },
  "resource_group": {
    "crn": "crn:[...]",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/6b45d0aa-e0a6-478b-a5d9-bb45b106676d",
    "id": "6b45d0aa-e0a6-478b-a5d9-bb45b106676d",
    "name": "Default"
  },
  "resource_type": "share",
  "size": 4800,
  "targets": [],
  "zone": {
    "href": "$vpc_api_endpoint/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: pre}

### Create a file share and mount target together from the API
{: #fs-create-share-target-api}

This request creates a file shares and a mount target for the file share.

```curl
curl -X POST \
"$rias_endpoint/v1/shares?version=2021-02-23&generation=2\
-H "Authorization: $iam_token" \
-H 'Content-Type: application/json' \
-d $'{
  "size": 4800,
  "targets": [
    {
      "name": "target-name1",
      "vpc": {
        "id": "a1fb6c4f-6a63-4d34-8bf6-55fab89e932a"
      }
    }
  ],
  "name": "share-name1",
  "profile": {
    "name": "tier-10iops"
  },
  "zone": {
    "name": "us-south-1"
  }
}'
```
{: pre}

A successful response will look like this:

```json
{
  "created_at": "2021-03-31T23:31:59Z",
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
      "name": "target-name1",
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
  "zone": {
    "href": "$vpc_api_endpoint/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{:codeblock}

### Create a mount target from the API
{: #fs-create-mount-target-api}

This request creates or adds a mount target to an already existing file share.

```curl
curl -X POST \
"$rias_endpoint/v1/shares/$share_id/targets?version=2021-03-15&generation=2\
-H "Authorization: $iam_token" \
-H 'Content-Type: application/json' \
-d $'{
  "name": "target-name1",
  "vpc": {
    "id": "6e01bc24-4a6e-4a0c-a1bd-4caa0c8159e7"
  }
}
```
{: pre}

A successful response will look like this:

```json
{
  "created_at": "2021-03-31T23:31:59Z",
  "href": "$vpc_api_endpoint/v1/shares/ff859972-8c39-4528-91df-eb9160eae918/targets/9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "id": "9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "lifecycle_state": "pending",
  "mount_path": "domain.com:/vol_xyz_2891fd0a_63aa_4deb_9ed5_1159e37cb5aa",
  "name": "target-name1",
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
{:codeblock}

## Next steps
{: #fs-create-next-steps}

Mount and use your file shares:

* [Viewing file shares and mount targets](/docs/vpc?topic=vpc-file-storage-view)
* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-vpc-mount-RHEL)
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos)
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-vpc-mount-ubuntu)
* [Manage your file shares](/docs/vpc?topic=vpc-file-storage-managing)
