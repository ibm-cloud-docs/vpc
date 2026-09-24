---

copyright:
  years: 2026
lastupdated: "2026-09-24"

keywords: snapshots, Block Storage snapshots, software attachment, snapshot software attachment, update software attachment, rename software attachment, vendor-managed licensing, catalog image, boot volume

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating {{site.data.keyword.block_storage_is_short}} snapshot software attachments
{: #snapshots-vpc-software-attachments}

You can update the name of the software attachments of a snapshot.
{: shortdesc}

Provisioning a virtual server instance from a catalog image with a software billing plan creates a boot volume with a software attachment. The software attachment contains the software plan and licensable software details. When you create a snapshot of that boot volume, the snapshot inherits the software attachment. Snapshots that were created before this feature was available and that reference a software billing plan are also updated with a software attachment. You can update the software attachment of the snapshot from the CLI, API, or Terraform.

## Updating a snapshot software attachment in the console
{: #snapshots-vpc-software-attachments-ui}
{: ui}

Updating a snapshot software attachment is not supported in the console. To update a snapshot software attachment, use the CLI, API, or Terraform instructions.

## Updating a snapshot software attachment with the CLI
{: #updating-snapshots-vpc-software-attachments-cli}
{: cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

To update a snapshot software attachment, use the [`ibmcloud is snapshot-software-attachment-update`](/docs/vpc?topic=vpc-vpc-reference#snapshot-software-attachment-update) command. The `SNAPSHOT` variable is the ID or name of the snapshot. The `SWAC` variable is the ID or name of the snapshot software attachment. The `--name` value is the new name for the snapshot software attachment.

```sh
ibmcloud is snapshot-software-attachment-update SNAPSHOT SWAC --name NEW_NAME [--output JSON] [-q, --quiet]
```
{: pre}

The following example renames the software attachment `my-snapshot-software-attachment` to `my-renamed-snapshot-software-attachment` for the snapshot `my-snapshot`.

```sh
ibmcloud is snapshot-software-attachment-update my-snapshot my-snapshot-software-attachment --name my-renamed-snapshot-software-attachment
```
{: pre}

For more information about available command options, see [`ibmcloud is snapshot-software-attachment-update`](/docs/vpc?topic=vpc-vpc-reference#snapshot-software-attachment-update).

## Retrieving a snapshot software attachment with the CLI
{: #snapshots-vpc-retrieve-software-attachment-cli}
{: cli}

You can retrieve a specific software attachment of a snapshot by ID or name from the CLI. Use [`ibmcloud is snapshot-software-attachment`](/docs/vpc?topic=vpc-vpc-reference#snapshot-software-attachment). The `SNAPSHOT` variable is the ID or name of the snapshot. The `SWAC` variable is the snapshot software attachment ID or name.

```sh
ibmcloud is snapshot-software-attachment SNAPSHOT SWAC [--output JSON] [-q, --quiet]
```
{: pre}

- To retrieve a snapshot software attachment by ID, specify the software attachment ID as the `SWAC` variable.

   The following example retrieves the software attachment with ID `r134-fbb5d014-5cf1-4bf9-af23-86fc238c3c0a` for the snapshot `cli-snap`.

   ```sh
   ibmcloud is snapshot-software-attachment cli-snap r134-fbb5d014-5cf1-4bf9-af23-86fc238c3c0a
   ```
   {: pre}

- To retrieve a snapshot software attachment by name, specify the software attachment name as the `SWAC` variable.

   The following example retrieves the software attachment named `twiddling-thud-stash-reliant` for the snapshot `cli-snap`.

   ```sh
   ibmcloud is snapshot-software-attachment cli-snap twiddling-thud-stash-reliant
   ```
   {: pre}

For more information about available command options, see [`ibmcloud is snapshot-software-attachment`](/docs/vpc?topic=vpc-vpc-reference#snapshot-software-attachment).

## Updating a snapshot software attachment with the API
{: #snapshots-vpc-software-attachments-api}
{: api}

Make a `PATCH /snapshots/{snapshot_id}/software_attachments/{id}` request and specify the snapshot ID and the software attachment ID to update the software attachment with the information that is provided in a snapshot software attachment patch object.

The following example updates a software attachment with an ID of `$software_attachment_id` for a snapshot with an ID of `$snapshot_id`.

```sh
curl -X PATCH "https://us-south.iaas.cloud.ibm.com/v1/snapshots/$snapshot_id/software_attachments/$software_attachment_id?version=2026-06-23&generation=2" \
  -H "accept: application/json" \
  -H "Content-Type: application/merge-patch+json" \
  -H "Authorization: Bearer $iam_token" \
  -d '{"name":"my-renamed-snapshot-software-attachment"}'
```
{: pre}

For more information, see [Update a snapshot software attachment](/docs/apis/vpc/latest#update-snapshot-software-attachment) in the VPC API reference.

## Retrieving a snapshot software attachment with the API
{: #get-snapshot-software-attachment-api}
{: api}

Make a `GET /snapshots/{snapshot_id}/software_attachments/{id}` request and specify the software attachment ID and snapshot ID. This request retrieves a single snapshot software attachment that is specified by an identifier in the URL.

The following example retrieves a specific software attachment with an ID of `$software_attachment_id` for a snapshot with a snapshot ID of `$snapshot_id`.

```sh
curl -X GET "https://us-south.iaas.cloud.ibm.com/v1/snapshot/$snapshot_id/software_attachments/$software_attachment_id?version=2026-06-23&generation=2" -H "accept: application/json" -H "Authorization: Bearer $iam_token"
```
{: pre}

For more information, see [Retrieve a snapshot software attachment](/docs/apis/vpc/latest#get-snapshot-software-attachment) in the VPC API.

## Next steps
{: #snapshots-vpc-software-attachments-next-steps}

* [View {{site.data.keyword.block_storage_is_short}} snapshot software attachments](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-software-attachments-cli)
* [Rename snapshots](/docs/vpc?topic=vpc-snapshots-vpc-rename)
* [Share snapshots with another account](/docs/vpc?topic=vpc-snapshots-vpc-share)
* [Manage {{site.data.keyword.block_storage_is_short}} snapshot tags](/docs/vpc?topic=vpc-snapshots-vpc-tags)
