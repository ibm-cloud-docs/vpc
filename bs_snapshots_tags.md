---

copyright:
  years: 2021, 2026
lastupdated: "2026-08-24"

keywords: snapshots, Block Storage snapshots, tags, user tags, access management tags, backup policy

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing {{site.data.keyword.block_storage_is_short}} snapshot tags
{: #snapshots-vpc-tags}

You can add user tags and access management tags to your {{site.data.keyword.block_storage_is_short}} snapshots. User tags can be used by backup policies to create backups of the snapshot.
{: shortdesc}

For more information about creating access tags, see [Working with tags](/docs/account?topic=account-tag). You must have the access listed in [Granting users access to tag resources](/docs/account?topic=account-access) for `access_tags`.
{: tip}

## Adding tags to a snapshot in the console
{: #snapshots-vpc-add-tags-ui}
{: ui}

You can add [user tags](/docs/vpc?topic=vpc-block-storage-about#storage-about-user-tags) and [access management tags](/docs/vpc?topic=vpc-block-storage-about#storage-about-mgt-tags) to your {{site.data.keyword.block_storage_is_short}} snapshots. You can add user tags to an existing snapshot or when you create a snapshot. The tags can be used by a backup policy to create backups of the snapshot.

### Adding user tags from the snapshots list screen
{: #snapshots-vpc-add-tags-list-ui}

Use the following steps to add user tags from the snapshots list screen.

1. Go to the [list of snapshots](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-list-ui).
2. Locate an _available_ snapshot created by the _user_. (Snapshots that were created by a backup policy are identified as created by the _Backup policy_.)
3. In the **Tags** column, snapshots with tags show a number that indicates the tags that are already applied. Snapshots without tags have an **Add tags** link. Click **Add tags**.
4. In the new window, type a tag in the User tags text box.
5. Click **Save**.

### Adding tags from the snapshot details page
{: #snapshots-vpc-add-tags-details-ui}

use the following steps to add tags from the snapshot details page.

1. Go to the [list of snapshots](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-list-ui).
2. Click the name of a snapshot in the list.
3. On the snapshot details page, click the **Add tags** link.
4. In the new window, enter a user tag in the User tags field or an access management tag in the Access management tags field.
5. Click **Save**.

When the user tags are matched with a backup policy, a backup is triggered based on the backup plan schedule. For more information, see [Creating a backup policy](/docs/vpc?topic=vpc-create-backup-policy-and-plan).

## Adding tags to a snapshot from the CLI
{: #snapshots-vpc-add-tags-cli}
{: cli}

You can add tags to a snapshot from the CLI.

Specify a `snapshot-update` command with the `--tags` option to add user tags to a volume.

Use the same option to add tags to a volume when you create a snapshot by using `ibmcloud is snapshot-create`.
{: tip}

The following example adds user tags `env:test` and `env:prod` to a volume that is identified by its ID.

```sh
ibmcloud is snapshot-update r138-e6664842-b370-496a-9ae7-da3fb647707c --name snappy-snap-snap --tags env:test,env:prod
```
{: pre}

```sh
Updating snapshot r138-e6664842-b370-496a-9ae7-da3fb647707c under account Test Account as user test.user@ibm.com...

ID                     r138-e6664842-b370-496a-9ae7-da3fb647707c
Name                   snappy-snap-snap
CRN                    crn:v1:bluemix:public:is:eu-de:a/a1234567::snapshot:r138-e6664842-b370-496a-9ae7-da3fb647707c
Status                 stable
Clones                 Zone      Available   Created
                       eu-de-3   true        2023-02-17T20:28:53+00:00
                       eu-de-1   true        2023-02-17T18:53:57+00:00

Source volume          ID                                          Name
                       r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   vicky-block-test1

Bootable               false
Encryption             provider_managed
Encryption key         -
Minimum capacity(GB)   20
Size(GB)               1
Resource group         ID                                 Name
                       a0eb5d9062af485fa5bb2c6999c74eac   test-snap

Created                2023-02-17T18:53:57+00:00
Captured at            2023-02-17T18:53:57+00:00
Tags                   env:test,env:prod
```
{: screen}

When the user tags are matched with a backup policy, a backup is triggered based on the backup plan schedule. For more information, see [Creating a backup policy](/docs/vpc?topic=vpc-create-backup-policy-and-plan).

## Adding user tags to a snapshot with the API
{: #snapshots-vpc-add-tags-api}
{: api}

You can add user tags to a snapshot by using the API.

Make a `PATCH /snapshots` call and specify the snapshot ID and user tags. The following example adds user tags `env:test` and `env:prod` to the snapshot.

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2022-01-12&generation=2" \
    -H "Authorization: Bearer ${API_TOKEN}" \
    -d `{
       "user_tags": [
         "env:test",
         "env:prod"
       ]
    }'
```
{: codeblock}

When the user tags are matched with a backup policy, a backup is triggered based on the backup plan schedule. For more information, see [Creating a backup policy](/docs/vpc?topic=vpc-create-backup-policy-and-plan).

## Adding tags to a snapshot with Terraform
{: #snapshots-vpc-add-tags-terraform}
{: terraform}

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

VPC infrastructure services use a specific regional endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the appropriate region in the provider block in the `provider.tf` file.

See the following example of targeting a region other than the default `us-south`.

```terraform
provider "ibm" {
  region = "eu-de"
}
```
{: screen}

You can add user tags and access management tags to a snapshot by using the `ibm_is_snapshot` resource. The following example adds user tags `env:test` and `env:prod` to a snapshot.

```terraform
resource "ibm_is_snapshot" "example" {
  name          = "example-snapshot"
  source_volume = ibm_is_instance.example.volume_attachments[0].volume_id
  tags          = ["env:test", "env:prod"]
}
```
{: codeblock}

You can also add access management tags by using the `access_tags` argument. Access management tags must be in the format `key:value`, and you can attach only those access tags that already exist.

```terraform
resource "ibm_is_snapshot" "example" {
  name          = "example-snapshot"
  source_volume = ibm_is_instance.example.volume_attachments[0].volume_id
  tags          = ["env:test", "env:prod"]
  access_tags   = ["project:myproject"]
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_snapshot](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_snapshot){: external}.

When the user tags are matched with a backup policy, a backup is triggered based on the backup plan schedule. For more information, see [Creating a backup policy](/docs/vpc?topic=vpc-create-backup-policy-and-plan).

## Next steps
{: #snapshots-vpc-tags-next-steps}

* [Manage fast restore clones](/docs/vpc?topic=vpc-snapshots-vpc-fast-restore)
* [Create remote region copies](/docs/vpc?topic=vpc-snapshots-vpc-remote-copy)
* [Delete snapshots](/docs/vpc?topic=vpc-snapshots-vpc-delete)
