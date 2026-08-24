---

copyright:
  years: 2021, 2026
lastupdated: "2026-08-24"

keywords: snapshots, Block Storage snapshots, rename snapshots, update snapshots

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Renaming {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-vpc-rename}

You can rename existing snapshots to make them simpler to identify.
{: shortdesc}

For information about snapshot naming conventions, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-planning#snapshots-vpc-naming).

## Renaming a snapshot in the console
{: #snapshots-vpc-rename-ui}
{: ui}

Use the following steps to rename a snapshot in the console.

1. Go to the list of snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Snapshots**.
2. Click the name of a snapshot from the list.
3. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit").
4. Provide a [new name](/docs/vpc?topic=vpc-snapshots-vpc-planning#snapshots-vpc-naming) for the snapshot, save, and confirm your changes.

## Renaming a snapshot from the CLI
{: #snapshots-vpc-rename-cli}
{: cli}

### Prerequisites for issuing commands from the CLI
{: #snapshots-vpc-rename-cli-requirement}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

Log in to {{site.data.keyword.cloud}}.

```sh
ibmcloud login --sso -a cloud.ibm.com
```
{: pre}

This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

### Renaming the snapshot
{: #vpc-rename-snapshot-cli}

To rename a snapshot, issue the `ibmcloud is snapshot-update` command and provide the snapshot ID and new name.

```sh
ibmcloud is snapshot-update SNAPSHOT_ID --name SNAPSHOT_NAME
```
{: pre}

See the following example.

```sh
ibmcloud is snapshot-update r138-e6664842-b370-496a-9ae7-da3fb647707c --name snappy-snap-snap
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
Tags                   -
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-update`](/docs/vpc?topic=vpc-vpc-reference#snapshot-update).

## Renaming a snapshot with the API
{: #snapshots-vpc-rename-api}
{: api}

You can rename a snapshot by using the API.

Make a `PATCH /snapshots` call and specify the snapshot ID and new name of the snapshot.

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2022-01-12&generation=2" \
   -H "Authorization: Bearer ${API_TOKEN}" \
   -d '{
     "name": "my-snapshot-renamed"
   }'
```
{: pre}

You can use the same call to rename a cross-regional copy. The cross-regional copy is independent from the source snapshot and source volume, and can be managed like any other snapshot.

## Updating a snapshot with Terraform
{: #snapshots-vpc-rename-terraform}
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
{: codeblock}

To update a snapshot, use the `ibm_is_snapshot` resource. You can change the name of the snapshot, the fast restore zones, and tags. However, changing the `resource_group` and `source_volume` values forces Terraform to delete the snapshot and create a different snapshot.

```terraform
resource "ibm_is_snapshot" "example" {
  name          = "my-snapshot"
  source_volume = ibm_is_volume.example.id
  }
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_snapshot](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_snapshot){: external}.

## Next steps
{: #snapshots-vpc-rename-next-steps}

* [Share snapshots with another account](/docs/vpc?topic=vpc-snapshots-vpc-share)
* [Manage snapshot tags](/docs/vpc?topic=vpc-snapshots-vpc-tags)
* [Manage fast restore clones](/docs/vpc?topic=vpc-snapshots-vpc-fast-restore)
