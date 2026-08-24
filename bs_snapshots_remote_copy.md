---

copyright:
  years: 2021, 2026
lastupdated: "2026-08-24"

keywords: snapshots, Block Storage snapshots, remote copy, cross-regional copy, regional copy

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating and managing remote region copies of {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-vpc-remote-copy}

You can create cross-regional copies of your snapshots to store them in different regions for disaster recovery or data migration purposes. Remote region copies are independent from the source snapshot and can be managed like any other snapshot.
{: shortdesc}

## Creating a remote copy in the console
{: #napshots-remote-copy-create-ui}
{: ui}

Use the following steps to create cross-regional copies of snapshots from the Snapshots for VPC list or from the snapshot details page.

1. In the console, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage snapshots**.
2. In the list of snapshots, find the snapshot that you want to duplicate in another region. Make sure that the snapshot is in Stable status.
3. Click the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Copy snapshot**.
4. Select the region where you want to create the copy.

   You can have only one copy per region. If no regions are available for copies, the option Copy Snapshot is disabled.
   {: restriction}

5. Click **Create**.

Alternatively, click the snapshot's name to view its details. You can either access the **Copy Snapshot** option from the **Actions** menu or you can scroll to the remote copies card and click **Create copy**. The same provisioning panel opens where you can make the region selection.

## Deleting remote region copy in the console
{: #napshots-remote-copy-delete-ui}
{: ui}

Snapshot copies in a remote region are independent from the parent snapshot and the parent volume. You can delete them anytime by using the Snapshots for VPC list.

Use the following steps to delete a remote region copy in the console.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Snapshots**.
2. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") in the row of the snapshot that you want to delete.
3. Select **Delete**.
4. Confirm the deletion and click **Delete**.

## Creating a remote region copy from the CLI
{: #snapshots-remote-copy-create-cli}
{: cli}

You can create a cross-regional copy of a snapshot by using the `snapshot-create` command with the `--source-snapshot-crn` option and the source snapshot CRN, which creates a snapshot in the target region by using the CRN of a snapshot from the source region. The created snapshot uses the customer-defined encryption key if the CRN of an encryption key was also specified. The source snapshot must in Stable status for the copy to be created successfully.

```sh
ibmcloud is snapshot-create --name my-cli-snapshot-crc --source-snapshot-crn crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-b9590a48-63a3-445e-b819-3f2c0b82daf8
```
{: pre}

```sh
Creating snapshot my-cli-snapshot-crc under account Test Account as user test.user@ibm.com...

ID                     r142-bd4532c0-e73c-44f9-a017-89e5368c521a
Name                   my-cli-snapshot-crc
CRN                    crn:v1:bluemix:public:is:us-east:a/a1234567::snapshot:r142-bd4532c0-e73c-44f9-a017-89e5368c521a
Status                 pending
Clones                 Zone   Available   Created

Source volume          ID                                          Name                   Remote Region
                       r006-be21061a-4dc6-4c9f-b17d-421838fde399   -remote-421838fde399   us-south

Snapshot Copies        ID   Name   Remote Region   CRN   Resource type

Bootable               true
Encryption             provider_managed
Encryption key         -
Source Snapshot        ID                                          Name                   Remote Region   CRN                                                                                                                        Resource type
                       r006-b9590a48-63a3-445e-b819-3f2c0b82daf8   cli-snap-crc-test-sn   us-south        crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-b9590a48-63a3-445e-b819-3f2c0b82daf8   snapshot

Minimum capacity(GB)   100
Size(GB)               1
Source Image           ID                                          Name                   Remote Region
                       r006-24d856e2-6aec-41c2-8f36-5a8a3766f0d6   -remote-5a8a3766f0d6   us-south

Operating system       Name             Vendor   Version                 Family   Architecture   Display name
                       centos-7-amd64   CentOS   7.x - Minimal Install   CentOS   amd64          CentOS 7.x - Minimal Install (amd64)

Resource group         ID                                 Name
                       cdc21b72d4e647b195de988b175e3d82   Default

Created                2023-04-24T18:54:29+05:30
Captured at            2023-04-24T09:48:03+05:30
Tags                   -
Service Tags           -
```
{: codeblock}

For more information about available command options, see [`ibmcloud is snapshot-create`](/docs/cli?topic=cli-vpc-reference#snapshot-create)

## Deleting a remote region copy from the CLI
{: #snapshots-remote-copy-delete-cli}
{: cli}

You can delete a cross-regional copy of a snapshot by using the `ibmcloud is snapshot-delete` command with the snapshot ID.

```sh
ibmcloud is snapshot-delete r142-bd4532c0-e73c-44f9-a017-89e5368c521a
```
{: pre}

```sh
This will delete snapshot r142-bd4532c0-e73c-44f9-a017-89e5368c521a and cannot be undone. Continue [y/N] ?> y
Deleting snapshot r142-bd4532c0-e73c-44f9-a017-89e5368c521a under account Test Account as user test.user@ibm.com...
    OK
Snapshot r142-bd4532c0-e73c-44f9-a017-89e5368c521a is deleted.
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-delete`](/docs/cli?topic=cli-vpc-reference#snapshot-delete).

## Creating a remote region copy with the API
{: #snapshots-remote-copy-create-api}
{: api}

You can create a cross-regional copy of a snapshot by making an API call in the target region. Specify the CRN of the source snapshot to create a copy in the target region. The created snapshot uses the customer-defined encryption key if the CRN of an encryption key was also specified. The source snapshot must in Stable status for the copy to be created successfully. See the following example, where the target region is us-east and the original snapshot is in us-south.

```json
POST https://us-east.iaas.cloud.ibm.com/v1/snapshots
{
     "name": "my-snapshot",    // required
     "source_snapshot": {      // required
      	"crn": "crn:[...]"
     },
     "resource_group": {       // optional
       "id": "2d1bb5a8-40a8-447a-acf7-0eadc8aeb054"
     },
     "encryption_key": "crn:[...]"     // optional
}
```
{: screen}

A successful response looks like the following example:

```json
{
  "created_at": "2023-05-18T20:18:18Z",
  "deletable": false,
  "encryption": "user_managed",
  "encryption_key": {
    "crn": "crn:[...]"
  },
  "href": "https://us-east.iaas.cloud.ibm.com/v1/snapshots/r139-f6bfa329-0e36-433f-a3bb-0df632e79263",
  "id": "r139-f6bfa329-0e36-433f-a3bb-0df632e79263",
  "lifecycle_state": "pending",
  "minimum_capacity": 100,
  "name": "my-snapshot",
  "operating_system": {
    "architecture": "amd64",
    "dedicated_host_only": false,
    "display_name": "Ubuntu Linux 20.04 LTS Focal Fossa Minimal Install (amd64)",
    "family": "Ubuntu Linux",
    "gpu_supported": [],
    "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/ubuntu-20-04-amd64",
    "name": "ubuntu-20-04-amd64",
    "vendor": "Canonical",
    "version": "20.04 LTS Focal Fossa Minimal Install"
  },
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
    "id": "678523bcbe2b4eada913d32640909956",
    "name": "Default"
  },
  "resource_type": "snapshot",
  "service_tags": [],
  "size": 1,
  "source_image": {
    "crn": "crn:[...]",
    "remote": {
    	"region": {
    	   "name": "us-south",
    	   "hfef": "https://us-east.iaas.cloud.ibm.com/v1/regions/us-south"
    	}
    },
    "href": "https://us-south.iaas.cloud.ibm.com/v1/images/r006-32045dc2-b463-4cda-b424-bc3dcf51dfbb",
    "id": "r006-32045dc2-b463-4cda-b424-bc3dcf51dfbb",
    "name": "ibm-ubuntu-20-04-minimal-amd64-1"
  },
  "source_snapshot": {
    "crn": "crn:[...]",
    "remote": {
    	"region": {
    	   "name": "us-south",
    	   "hfef": "https://us-east.iaas.cloud.ibm.com/v1/regions/us-south"
    	}
    },
    "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/r006-511a798c-5816-4082-8ecb-554a440f83de",
    "id": "r006-511a798c-5816-4082-8ecb-554a440f83de",
    "name": "my-snapshot-data"
  },
  "source_volume": {
    "crn": "crn:[...]",
    "remote": {
    	"region": {
    	   "name": "us-south",
    	   "hfef": "https://us-east.iaas.cloud.ibm.com/v1/regions/us-south"
    	}
    },
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-411a798c-5816-4082-8ecb-554a440f83de",
    "id": "r006-411a798c-5816-4082-8ecb-554a440f83de",
    "name": "my-instance-data"
  },
  "user_tags": []
}
```
{: screen}

## Deleting a remote-region copy of a snapshot with the API
{: #snapshots-remote-copy-delete-api}
{: api}

Make a `DELETE /snapshots/{id}`in the target region where the remote copy is located.

```sh
curl -X DELETE https://us-east.iaas.cloud.ibm.com/v1/snapshots/{id}
```
{: pre}

## Creating a cross-regional copy with Terraform
{: #snapshots-remote-copy-create-terraform}
{: terraform}

To create a copy of the snapshot in a remote region, use the `ibm_is_snapshot` resource. The source snapshot must in Stable status for the copy to be created successfully. The created snapshot uses the customer-defined encryption key if the CRN of an encryption key was also specified. The following example creates a copy in the target region by using the ID of the source snapshot. The copy is going to be encrypted by the encryption key that is specified by its CRN.

```terraform
resource "ibm_is_snapshot" "snapshot" {
  name 		        = "my-cross-regional-snapshot"
  source_snapshot = "r138-4463eb2c-4913-43b1-b9bf-62a94f74c146"
  encryption_key  = "crn:bluemix:public:kms:us-south:a/df0564dd126042ebb03e0224728ce939:4957299d-0ba0-487f-a1a0-c724a729b8b4:key:0cb88b98-9261-4d07-8329-8f594b6641b5"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_snapshot](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_snapshot){: external}.

## Deleting a remote region copy with Terraform
{: #snapshots-remote-copy-delete-terraform}
{: terraform}

Use the `terraform destroy` command to conveniently delete a remote object such as a cross-regional copy of a snapshot. The following example shows the syntax for deleting a snapshot. Substitute the actual ID of the snapshot in for `ibm_is_snapshot.example.id`.

```terraform
terraform destroy --target ibm_is_snapshot.example.id
```
{: codeblock}

For more information, see [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.

## Next steps
{: #snapshots-vpc-remote-copy-next-steps}

* [Delete snapshots](/docs/vpc?topic=vpc-snapshots-vpc-delete)
* [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore)
