---

copyright:
  years: 2021, 2026
lastupdated: "2026-08-24"

keywords: snapshots, Block Storage snapshots, fast restore, snapshot clones, zonal copies

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing fast restore snapshot clones
{: #snapshots-vpc-fast-restore}

Create and manage fast restore clones for {{site.data.keyword.block_storage_is_short}} snapshots to reduce recovery time. Fast restore clones are zonal copies that enable quicker volume restoration.
{: shortdesc}

## Editing fast restore zones in the console
{: #snapshots-edit-fast-restore}
{: ui}

Use the following steps to edit the zones where fast restore clones are stored in the console. You can add or remove zones as needed.

1. Select a snapshot from the [list of snapshots](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-list-ui).
2. From the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Edit fast restore**.
3. From the side panel, select or deselect the zones for fast restore in your region. Review the billing update based on your selection.
4. Click **Save**. You're returned to the [snapshots details page](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-snapshot-ui). The Fast restore section shows the new zone initially with a _pending_ status.

Fast restore information is updated when you refresh. Zone information is updated to show enabled or disabled, depending on your changes.

## Creating a snapshot clone for fast restore from the CLI
{: #snapshots-fast-restore-cli}
{: cli}

To create a zonal copy of a snapshot, issue the `ibmcloud is snapshot-clone-create` command with the snapshot ID and the zone or zones where you want to create copies. The following command example creates the fast restore clone of `r138-4463eb2c-4913-43b1-b9bf-62a94f74c146` in the `eu-de-3` zone.

```sh
ibmcloud is snapshot-clone-create r138-4463eb2c-4913-43b1-b9bf-62a94f74c146  --zone eu-de-3
```
{: pre}

```sh
Creating zonal clone of snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...

Zone        eu-de-3
Available   false
Created     2023-02-17T20:29:21+00:00
Href        https://eu-de.iaas.cloud.ibm.com/v1/regions/eu-de/zones/eu-de-3
```
{: screen}

The snapshot clone appears to be unavailable while the snapshot clone is created. It takes only a few seconds. Issue the `ibmcloud is snapshot-cl` command with the snapshot ID and the clone target zone to see the new snapshot clone as available.

```sh
ibmcloud is snapshot-cl r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 eu-de-3
```
{: pre}

```sh
Getting zonal clone eu-de-3 of snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...

Zone        eu-de-3
Available   true
Created     2023-02-17T20:29:21+00:00
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-clone-create`](/docs/vpc?topic=vpc-vpc-reference#snapshots-cli).

## Deleting a snapshot clone from the CLI
{: #snapshots-delete-clone-cli}
{: cli}

To delete a snapshot clone, issue the `ibmcloud is snapshot-clone-delete` command with the Snapshot ID and the zone where you want the snapshot copy removed. The following command example deletes the fast restore copy of the snapshot `r138-4463eb2c-4913-43b1-b9bf-62a94f74c146` in the `eu-de-3` zone.

```sh
@$ ibmcloud is snapshot-clone-delete r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 eu-de-3
This will delete zonal clone eu-de-3 for snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 and cannot be undone. Continue [y/N] ?> y
Deleting zonal clone eu-de-3 for snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...
OK
Deletion request for zonal snapshot clone eu-de-3 has been accepted.
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-clone-delete`](/docs/vpc?topic=vpc-vpc-reference#snapshots-cli).

## Creating a snapshot clone for fast restore with the API
{: #snapshots-fast-restore-api}
{: api}

To create [fast restore snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=api#snapshots_vpc_fast_restore), you create a clone of the snapshot in a different zone. You then [restore a volume](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-restore-API) from that clone.

In the API, you create a clone for an existing snapshot by making a `PUT /snapshots/{id}/clones/{zone_name}` call to clone the snapshot to the specified zone. See the following example.

```sh
curl -X PUT \
"$vpc_api_endpoint/v1/snapshots/5e160469-0837-48a7-8973-e44c8d5fd85a/clones/us-south-1&version=2025-02-18&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "available": true,
  "created_at": "2025-02-18T20:35:38.600Z",
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  },
  "storage_generation": 1,
}
```
{: codeblock}

You can also specify the `clone` property when you create a snapshot of a volume. See the following example.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/snapshots?version=2025-02-18&generation=2" \
-H "Authorization: Bearer $iam_token" \
-d '{
    "clones": [{"zone": {"name": "us-south-1"}}],
    "name": "my-snapshot2",
    "resource_group": {"id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4"},
    "source_volume": {"id": "8948ad59-bc0f-7510-812f-5dc64f59fab8"},
    "user_tags": ["env:test","env:prod"]
   }'
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "bootable": false,
  "clones": [
      "available": true,
      "created_at": "2025-02-18T20:18:38.600Z",
      "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
        "name": "us-south-1"
     }
  ],
  "created_at": "2025-02-18T20:18:18Z",
  "crn": "crn:[...]",
  "deletable": false,
  "encryption": "user_managed",
  "encryption_key": {
    "crn": "crn:[...]"
  },
  "storage_generation": 1
}
```
{: codeblock}

## Deleting a snapshot clone with the API
{: #snapshots-delete-clone-api}
{: api}

Make a `DELETE /v1/snapshots/{id}/clones/{zone-name}` call to delete a snapshot clone in the specified zone. You can't undo the delete when the snapshot clone is deleted.

See the following example.

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots/fde0b8d5-2d75-4c28-af7d-12ffc3ae2a55/clones/us-south-1&version=2025-02-18&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

## Creating a snapshot clone for fast restore with Terraform
{: #snapshots-fast-restore-terraform}
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

You can create fast restore clones when you create a snapshot by using the `ibm_is_snapshot` resource with the `clones` argument. Specify the zones where you want to create fast restore clones. The following example creates a snapshot with fast restore clones in the `us-south-1` and `us-south-2` zones.

```terraform
resource "ibm_is_snapshot" "example_clones" {
  name          = "example-snapshot"
  source_volume = ibm_is_instance.example.volume_attachments[0].volume_id
  clones        = ["us-south-1", "us-south-2"]
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_snapshot](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_snapshot){: external}.

## Deleting snapshot clones with Terraform
{: #snapshots-delete-clone-terraform}
{: terraform}

To delete fast restore clones with Terraform, remove the zone entries from the `clones` argument in your `ibm_is_snapshot` resource. When you apply the changes, Terraform deletes the clones in the removed zones.

For example, to delete the clone in `us-south-2` while keeping the clone in `us-south-1`:

```terraform
resource "ibm_is_snapshot" "example_clones" {
  name          = "example-snapshot"
  source_volume = ibm_is_instance.example.volume_attachments[0].volume_id
  clones        = ["us-south-1"]  # Removed us-south-2
}
```
{: codeblock}

To delete all fast restore clones, remove the `clones` argument entirely.

After you update your Terraform configuration, run `terraform apply` to delete the clones. The deletion cannot be undone.

## Next steps
{: #snapshots-vpc-fast-restore-next-steps}

* [Create remote region copies](/docs/vpc?topic=vpc-snapshots-vpc-remote-copy)
* [Delete snapshots](/docs/vpc?topic=vpc-snapshots-vpc-delete)
* [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore)
