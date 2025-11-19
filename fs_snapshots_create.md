---

copyright:
  years: 2024, 2025
lastupdated: "2025-11-19"

keywords: snapshots, File Storage, File Storage snapshot

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating {{site.data.keyword.filestorage_vpc_short}} snapshots
{: #fs-snapshots-create}

You can create a snapshot of a {{site.data.keyword.filestorage_vpc_short}} share in the console, from the CLI, with the API, or Terraform. Snapshots capture the data on the share at a specific data and time. Snapshots can be used later to retrieve old versions of files or to create new shares with the data of the snapshot.
{: shortdesc}

Although you can't create a snapshot on a replica share, the snapshots of the origin share are copied from the source to the replica at the next scheduled sync. These replica snapshots are created by the file service. They do not inherit the tags or the name from the original snapshots. However, they have the same fingerprint value as the source snapshot. They can't be manually deleted from the replica share, but are removed from the replica share at the next replication sync if the source snapshot is deleted on the source share.
{: note}

The creation of a share snapshot is quick. You can expect to have the snapshot stable and available within seconds. However, you can take only 1 snapshot in a minute. If you take too many snapshots too quickly, you can encounter errors.

[Select availability]{: tag-green} During the select availability release of regional file shares, the zone value of regional file share snapshots is blank in both the CLI and API responses.

## Creating a snapshot in the console
{: #fs-snapshots-create-ui}
{: ui}

In the console, you can create a snapshot of a {{site.data.keyword.filestorage_vpc_short}} share.

1. You can access the File Storage snapshot for VPC provisioning panel in the [{{site.data.keyword.cloud}} console](/login){: external} in multiple ways.

   - From the **[File storage share for VPC](/infrastructure/storage/fileShares)** list:
      1. Click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.
      1. From the list of shares, locate the share that you want to snapshot.
      1. Click the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Create snapshot**.

   - From the **File storage share details** screen:
      1. Click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**. From the list of File storage shares, select the share that you want to make a snapshot of.
      1. On the share details page, select **Create snapshot** from the **Actions** menu.

2. Enter a unique name for the snapshot, and any user tags that you want to use to identify this resource. For suggestions about how to name your snapshots, see [Naming snapshots](/docs/vpc?topic=vpc-fs-snapshots-planning#fs-snapshots-naming). Consider keyvalue pairs for your tags (for example, `dev:test` or `costctr:124`) and do not include sensitive information because tags are visible account-wide.

3. Click **Create**. You're returned to the screen that you started from. Messages are displayed while the snapshot is being created and when it's ready, the snapshot is displayed in the list of snapshots on the file share details page. For more information, see [View snapshot details in the console](/docs/vpc?topic=vpc-fs-snapshots-view&interface=ui#fs-snapshots-view-snapshot-ui).

## Creating a snapshot from the CLI
{: #fs-snapshots-create-cli}
{: cli}

### Before you begin
{: #fs-snapshots-create-procedure-cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

Log in to {{site.data.keyword.cloud}}.

```sh
ibmcloud login --sso -a cloud.ibm.com
```
{: pre}

This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

Before you start, gather the following information:

- A unique name for the snapshot. Share snapshot name must be unique at the share level. For suggestions about how to name your snapshots, see [Naming snapshots](/docs/vpc?topic=vpc-fs-snapshots-planning#fs-snapshots-naming). 
- The name or ID of the source share.
- Any tags that you want to attach to the snapshot.

Use the following CLI commands to collect the information that you need.

- `ibmcloud is shares` - This command lists all available shares in the region that you selected. Locate the share in the list, verify the status (`available`).
- `ibmcloud is share SHARE_ID` - Use this command with the share ID from the output of the previous command to review the details of the share. If the output shows that the share is available, attached to an instance and not busy, you can create a snapshot.

### Creating a snapshot from the CLI
{: #fs-snapshot-create-cli}
{: cli}

To create a snapshot, run the `ibmcloud is share-snapshot-create` command.

```sh
ibmcloud is share-snapshot-create SHARE [--name NAME] [--user-tags USER_TAGS] [--output JSON] [-q, --quiet]
```
{: pre}

The following example creates a snapshot with the name `my-first-share-snapshot` of the zonal share `my-file-share`. The snapshot is tagged with `env:test`.

```sh
ibmcloud is share-snapshot-create my-file-share --name my-first-share-snapshot --user-tags env:test
```
{: pre}

```sh
Creating snapshot my-first-share-snapshot under account Test Account as user test.user@ibm.com...

ID                     r138-4463eb2c-4913-43b1-b9bf-62a94f74c146
Name                   my-first-share-snapshot
Fingerprint            7abc3aef-c2bc-4f65-a296-2928e534d498
Backup Policy Plan     -
Lifecycle state        pending
LifeCycle Reasons      Code   Message   More Info      
                       -      -         
Status                 pending
Created at             2024-12-18T20:15:43+00:00
Captured at            -
CRN                    crn:v1:bluemix:public:is:us-south:a/a1234567::share-snapshot:r006-0fe9e5d8-0a4d-4818-96ec-e99708644a58/r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4
Href                   https://us-south.iaas.cloud.ibm.com/v1/shares/r006-0fe9e5d8-0a4d-4818-96ec-e99708644a58/snapshots/r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4
Minimum Size           40
User Tags              env:test
Zone                   ID  Name
                           us-south-1
Resource group         ID                                 Name
                       6edefe513d934fdd872e78ee6a8e73ef   defaults

Status reasons         Status code   Status message
                       -             -

Resource type          share_snapshot
```
{: screen}

The status shows `pending` while the snapshot is created. If you want to, you can issue a second `ibmcloud is share-snapshot` command with the snapshot ID to see the new snapshot in `stable` status.

For snapshots of regional file shares, the zone value is blank in both the CLI and API responses. 
{: preview}

For more information about available command options, see [`ibmcloud is share-snapshot-create`](/docs/cli?topic=cli-vpc-reference#share-snapshot-create).

## Creating a snapshot with the API
{: #fs-snapshots-create-api}
{: api}

You can create a snapshot by using the API.

### Prerequisites for creating a snapshot with the API
{: #fs-snapshots-create-procedure-api}

You can create a snapshot by calling the [VPC API](/apidocs/vpc). Before you start, gather the following information:

- A unique name for the snapshot. Share snapshot name must be unique at the share level. For suggestions about how to name your snapshots, see [Naming snapshots](/docs/vpc?topic=vpc-fs-snapshots-planning#fs-snapshots-naming). 
- The ID of the source share.
- Any tags that you want to attach to the snapshot.

### Creating a snapshot with the API
{: #fs-snapshots-create-snapshot-api}

You can programmatically create a snapshot of a file share by calling the `shares/{share-id}/snapshots` API method. The following example creates a snapshot of a share by using the share ID, and specifies user tags that can be associated with a [backup policy](/docs/vpc?topic=vpc-backup-service-about).

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/{share-id}/snapshots?version=2024-12-10&generation=2" \
-H "Authorization: $iam_token"
-d '{
  "name": "my-first-share-snapshot",
  "user_tags": [
    "env:test"
  ]
}'
```
{: codeblock}
{: curl}

A successful response looks like the following example. The snapshot lifecycle state is `pending` while the snapshot is created. When it is successfully created, the status changes to `stable`.

```json
{
  "captured_at": "2024-12-10T01:21:12.000Z",
  "created_at": "2024-12-10T01:59:46.000Z",
  "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::share-snapshot:r006-0fe9e5d8-0a4d-4818-96ec-e99708644a58/r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4",
  "fingerprint": "7abc3aef-c2bc-4f65-a296-2928e534d498",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-0fe9e5d8-0a4d-4818-96ec-e99708644a58/snapshots/r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4",
  "id": "r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4",
  "lifecycle_state": "pending",
  "minimum_size": 10,
  "name": "my-first-share-snapshot",
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/fee82deba12e4c0fb69c3b09d1f12345",
    "id": "fee82deba12e4c0fb69c3b09d1f12345",
    "name": "Default"
  },
  "resource_type": "share_snapshot",
  "status": "available",
  "status_reasons": [],
  "user_tags": [],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: codeblock}

For snapshots of regional file shares, the zone value is blank in both the CLI and API responses.
{: preview}

## Creating a snapshot with Terraform
{: #fs-snapshots-create-terraform}
{: terraform}

You can create a snapshot by using Terraform.

Before you start, gather the following information:

- A unique name for the snapshot.
- The ID of the source share.
- Any tags that you want to attach to the snapshot.

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

To create a snapshot, use the `ibm_is_share_snapshot` resource. The following example creates a snapshot of the share with the ID `r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa`. The snapshot is named `my-first-share-snapshot`.

```terraform
resource "ibm_is_share_snapshot" "example" {
   name          = "my-first-share-snapshot"
   source_share = "r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share_snapshot](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share_snapshot){: external}.

## Next steps
{: #fs_snapshots_create_next_steps}

After you create a snapshot, you can view more details about it or restore a share from the snapshot.

- [View](/docs/vpc?topic=vpc-fs-snapshots-view) all the snapshots that you created, or details about a single snapshot.
- [Restore a share](/docs/vpc?topic=vpc-fs-snapshots-restore) from a snapshot.
