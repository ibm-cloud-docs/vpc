---

copyright:
  years: 2022, 2024
lastupdated: "2024-01-05"

keywords: consistency groups, Block Storage snapshots, multi-volume snapshot, instance snapshot,

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating snapshot consistency groups
{: #snapshots-vpc-create-consistency-groups}

A snapshot consistency group contains snapshots of multiple volumes that are attached to the same virtual server instance. You can include or exclude boot volumes. You can create a consistency group in the UI, CLI, API, and Terraform.
{: shortdesc}

When you request a snapshot of a consistency group, the system ensures that all write operations are complete before it takes the snapshots. Then, the system generates snapshots of all the tagged Block Storage volumes that are attached to the virtual server instance at the same time. Depending on the number and size of the attached volumes, plus the amount of data that is to be captured, you might observe a slight IO pause. This IO pause can range from a few milliseconds up to 4 seconds.

Before you start, gather the following information:

- A unique name for the consistency group. The name cannot exceed 64 characters. If you do not provide one, the system auto-generates a name for you. (The snapshots in the consistency group are named by using the first 16 characters of the group name and 3-4 auto-generated characters, so their names are unique, too.)
- The IDs of the source volumes.
- The resource group ID, which is optional. However, you can't change the resource group after the snapshot is created.
- Any tags that you want to attach to the snapshots.

## Creating a consistency group snapshot in the UI
{: #mvsnapshot-create-ui}
{: ui}

In the console, you can create a consistency group snapshot of {{site.data.keyword.block_storage_is_short}} volumes that are attached to a running virtual server instance.

1. Click the **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Storage > Block Storage snapshots**.
1. From the list of consistency groups that is initially empty, click **Create**.
1. Enter the required information to define your snapshot group and select the virtual server instance that has the volumes that you want to include in your snapshot consistency group.

   | Field          | Value |
   |----------------|-------|
   | Location       | Specify the geography and region for this snapshot set. |
   | Snapshot type  | Select Multiple volumes. |
   | Virtual Server instance | From the list, select the server instance. |
   | Consistency group name  | Provide a unique name for the consistency group. The name cannot exceed 64 characters. If you leave this field empty, the system auto-generates a name for you. The snapshots in the consistency group are named by using the first 16 characters of the group name and 3-4 auto-generated characters. |
   | Resource group | Select a [Resource group](/docs/vpc?topic=vpc-iam-getting-started&interface=ui#iam-resource-groups) for the snapshot, or use the default. You can't change the resource group after the snapshot is created. |
   | Tags           | Specify any user tags that you want to identify this resource. |
   | Access management tags | Specify any [access management tags](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#storage-add-access-mgt-tags) for this resource. |
   | Volumes        | Select the volumes that you want to include in the group from the list. |
   {: caption="Table 1. Selections for creating a snapshot" caption-side="bottom"}

1. Click **Create**. You're returned to the screen that you started from. Messages are displayed while the snapshots and the consistency group are being created and when they are ready. The snapshots and the consistency group are displayed on the Block Storage snapshots for VPC page, on their respective tabs. For more information, see [View snapshot details in the UI](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-snapshot-ui).

## Creating a consistency group snapshot from the CLI
{: #mvsnapshot-create-cli}
{: cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to {{site.data.keyword.cloud}}.

   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

1. Select the current generation of VPC.

   ```sh
   ibmcloud is target --gen 2
   ```
   {: pre}

1. List the volumes that are attached to the instance that you want to capture in a snapshot consistency group. Note the volume IDs in the output.

   ```sh
   ibmcloud is instance INSTANCE
   ```
   {: pre}

1. Use the `snapshot-consistency-group-create` command to create a consistency group. Provide the new snapshot's name and the ID of the source volume for each snapshot that is going to be part of the consistency group.

   If the source volume has `user_managed` encryption, you must provide the `--encryption_key`. The `--encryption_key` property must not be specified otherwise.
   {: requirement}

   ```sh
   ibmcloud is snapshot-consistency-group-create --snapshots '[{
        "name": "snapshot-no-1",
        "source_volume": {"id": "r106-bf595773-9922-4dd4-9a3c-998b10022e0c"},
        "user_tags": ["env:dev", "env:test"]
   }, {
        "name": "snapshot-no-2",
        "source_volume": {"id": "r106-aeb2bbe6-1d3c-4186-82ce-abddb03e243e"},
        "user_tags": ["env:dev", "env:test"]
   }]' --delete-snapshot-on-delete false --name multiple-snapshots-consistency-group-1 

   Creating snapshot consistency group under account Test Account as user test.user@ibm.com...
                               
   ID                          r006-4625d29b-3ac9-4bee-aca8-9366c4fd0c44   
   Name                        multiple-snapshots-consistency-group-1   
   CRN                         crn:v1:bluemix:public:is:us-south:a/a123456::snapshot-consistency-group:r006-4625d29b-3ac9-4bee-aca8-9366c4fd0c44   
   Href                        https://us-south.iaas.cloud.ibm.com/v1/snapshots/r006-4625d29b-3ac9-4bee-aca8-9366c4fd0c44   
   Status                      pending   
   Backup policy plan          -   
   Delete snapshot on delete   false   
   Source Snapshot             -   
   Resource group              ID                                 Name      
                               11caaa983d9c4beb82690daab08717e9   Default      
                               
   Created                     2023-12-05T17:32:53+05:30   
   Service Tags                - 
   ```
   {: screen}

For more information about available command options, see [`ibmcloud is snapshot-consistency-group-create`](/docs/cli?topic=cli-vpc-reference#snapshot-consistency-group-create).

## Creating a consistency group snapshot with the API
{: #mvsnapshot-create-api}
{: api}

Before you begin, [retrieve the instance details](/apidocs/vpc/latest#get-instance){: external} of the virtual server instance that you want to capture in the snapshot consistency group. Note the CRNs or IDs of the `volume_attachments` and the `boot_volume_attachment`.

You can programmatically create a consistency group by calling the `/snapshot_consistency_groups` method in the [VPC API](/apidocs/vpc/latest#create-snapshot-consistency-group){: external} as shown in the following sample request.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/snapshot_consistency_groups?version=2023-12-05&generation=2"\
 -H "Authorization: $iam_token" \
 -d '{
   "delete_snapshots_on_delete":true,
   "name": "my-snapshot-consistency-group",
   "resource_group":{"id": "2d1bb5a8-40a8-447a-acf7-0eadc8aeb054"},
   "snapshots": {
    {
     "name": "my-snapshot-1",
     "source_volume": {"crn": "crn:[...]"},
     "encryption_key"; "crn:[...]"
    },
    {
     "name": "my-snapshot-2",
     "source_volume": {"crn": "crn:[...]"},
     "encryption_key"; "crn:[...]"
    },
   }"
```
{: pre}

If the source volume has `user_managed` encryption, you must provide the `encryption_key`. The `encryption_key` property must not be specified otherwise.
{: requirement}

A successful response looks like the following example.

```json
{
  "created_at": "2023-12-05T20:18:18Z",
  "crn": "crn:[...]",
  "delete_snapshots_on_delete": true,
  "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshot_consistency_groups/r006-f6bfa329-0e36-433f-a3bb-0df632e79263",
  "id": "r006-f6bfa329-0e36-433f-a3bb-0df632e79263",
  "lifecycle_state": "pending",
  "name": "my-snapshot-consistency-group",
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
    "id": "2d1bb5a8-40a8-447a-acf7-0eadc8aeb054",
    "name": "Default"
  },
  "resource_type": "snapshot_consistency_group",
  "service_tags": [],
  "snapshots": [
    {
      "crn": "crn:[...]",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/r006-076191ba-49c2-4763-94fd-c70de73ee2e6",
      "id": "r006-076191ba-49c2-4763-94fd-c70de73ee2e6",
      "name": "my-snapshot-1",
      "resource_type": "snapshot"
    },
    {
      "crn": "crn:[...]",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/r006-07e34f34-49c2-4763-94fd-c70de73ae342",
      "id": "r006-07e34f34-49c2-4763-94fd-c70de73ae342",
      "name": "my-snapshot-2",
      "resource_type": "snapshot"
    }
  ]
}
```
{: screen}

## Creating a consistency group snapshot with Terraform
{: #mvsnapshot-create-terraform}
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

1. Import the details of the virtual server instance as a read-only data source. After the data source is created, you can access the attribute references for `boot_volume` and `volume_attachments`.

```terraform
data "ibm_is_instance" "example" {
  name        = ibm_is_instance.example.name
  private_key = file("~/.ssh/id_rsa")
  passphrase  = ""
}
```
{: codeblock}

1. To create a consistency group of snapshots, use the `ibm_is_snapshot_consistency_group` resource. The following example creates a consistency group of two volumes.

```terraform
resource "ibm_is_snapshot_consistency_group" "is_snapshot_consistency_group" {
  name = "my-data-consistency-group"
  snapshots {
    [ 
      name = "snapshot-1"
      source_volume = {id = "r016-0a74c6af-f9f5-4671-9025-c4efd41ac6da"}
      user_tags = ["my-tag"]
    ].
    [ 
      name = "snapshot-2"
      source_volume = {id = "r016-0a74c6af-f9f5-4671-9025-c4efd41ac6db"}
      user_tags = ["my-tag"]
    ]
  }
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_snapshot_consistency_group](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_snapshot_consistency_group){: external}.
