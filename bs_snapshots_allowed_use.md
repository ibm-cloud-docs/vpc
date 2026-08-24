---

copyright:
  years: 2025, 2026
lastupdated: "2026-08-24"

keywords: snapshots, Block Storage snapshots, update snapshots, allowed use

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating allowed-use properties of {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-vpc-allowed-use}

You can update the Allowed Use properties of a snapshot to control how the snapshot can be used for provisioning virtual server instances or bare metal servers.
{: shortdesc}

The `Allowed Use` properties are inherited from the parent volume. The properties comprise a Boolean [Common Expression Language](https://github.com/cel-expr/cel-spec/blob/master/doc/langdef.md){: external} expression. When the expression is evaluated to be `true`, then the provisioning of a virtual server instance or a bare metal server is allowed with the snapshot. When the expression is evaluated to be `false`, the provisioning is blocked.

You must have the `is.volume.volume.manage-allowed-use` IAM role to change these properties of a snapshot.
{: requirement}

## Updating the Allowed Use property of a snapshot in the console
{: #snapshot-update-allowed-use-ui}
{: ui}

Updating the Allowed Use properties of a snapshot is not supported in the console. To update the Allowed Use properties, switch to the CLI, API, or Terraform instructions.

## Updating the Allowed Use property of a snapshot from the CLI
{: #snapshot-update-allowed-use-cli}
{: cli}

### Before you begin
{: #snapshot-update-allowed-use-cli-prereqs}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

### Updating the allowed-use properties
{: #snapshot-update-allowed-use-cli-steps}

You can use the `ibmcloud is snapshot-update` command to change the `Allowed Use` values of a snapshot.

```sh
ibmcloud is snapshot-update r006-b6b5e2ad-e60a-40c9-bbc2-356dad292fe3 --allowed-use-api-version "2025-03-03" --allowed-use-bare-metal-server "enable_secure_boot==true" --allowed-use-instance true
```
{: pre}

```sh
Updating snapshot r006-b6b5e2ad-e60a-40c9-bbc2-356dad292fe3 under account Test Account as user test.user@ibm.com...

ID                              r006-b6b5e2ad-e60a-40c9-bbc2-356dad292fe3
Name                            my-bootable-snapshot
CRN                             crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-b6b5e2ad-e60a-40c9-bbc2-356dad292fe3
Status                          stable
Clones                          Zone   Available   Created

Source volume                   ID                                          Name                         Remote Region   CRN                                                                                                                        Resource type
                                r006-9a40241d-d116-4fc3-83ea-6134d30ee33c   fence-denture-mosaic-royal   -               crn:v1:bluemix:public:is:us-south-1:a/a1234567::volume:r006-9a40241d-d116-4fc3-83ea-6134d30ee33c   volume

Backup policy plan              -
Snapshot Copies                 -
Bootable                        true
Encryption                      provider_managed
Encryption key                  -
Source Snapshot                 -
Minimum capacity(GB)            100
Size(GB)                        1
Source Image                    ID                                          Name                                 Remote Region   CRN                                                                                                                     Resource type
                                r006-d734b459-b5a0-4777-8600-9fa3254d2cea   ibm-ubuntu-24-04-6-minimal-amd64-2   -               crn:v1:bluemix:public:is:us-south:a/a1234567::image:r006-d734b459-b5a0-4777-8600-9fa3254d2cea   image

Operating system                Name                 Vendor      Version                                  Family         Architecture   Display name
                                ubuntu-24-04-amd64   Canonical   24.04 LTS Noble Numbat Minimal Install   Ubuntu Linux   amd64          Ubuntu Linux 24.04 LTS Noble Numbat Minimal Install (amd64)

Resource group                  ID                                 Name
                                6edefe513d934fdd872e78ee6a8e73ef   defaults

Created                         2025-02-17T17:08:15+00:00
Captured at                     2025-02-17T17:08:15+00:00
Tags                            -
Service Tags                    -
Storage Generation              1
Allowed Use API Version         2025-03-03
Allowed Use Bare Metal Server   enable_secure_boot==true
Allowed Use Instance            true
```
{: screen}

## Updating the Allowed Use property of a snapshot with the API
{: #snapshot-update-allowed-use-api}
{: api}

You can update the `Allowed Use` properties of a snapshot by using the API.

Make a `PATCH /snapshots/{id}` call and specify the snapshot ID and the allowed use properties you want to update.

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/snapshots/r006-b6b5e2ad-e60a-40c9-bbc2-356dad292fe3?version=2025-03-03&generation=2" \
   -H "Authorization: Bearer ${API_TOKEN}" \
   -d '{
     "allowed_use": {
       "api_version": "2025-03-03",
       "bare_metal_server": "enable_secure_boot==true",
       "instance": true
     }
   }'
```
{: pre}

## Updating the Allowed Use property of a snapshot with Terraform
{: #snapshot-update-allowed-use-terraform}
{: terraform}

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

To update a snapshot's allowed use properties, use the `ibm_is_snapshot` resource and specify the `allowed_use` argument.

```terraform
resource "ibm_is_snapshot" "example" {
  name          = "my-snapshot"
  source_volume = ibm_is_volume.example.id
  allowed_use = {
    api_version        = "2025-03-03"
    bare_metal_server  = "enable_secure_boot==true"
    instance           = true
  }
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_snapshot](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_snapshot){: external}.

## Next steps
{: #snapshots-vpc-allowed-use-next-steps}

* [Share snapshots with another account](/docs/vpc?topic=vpc-snapshots-vpc-share)
* [Manage snapshot tags](/docs/vpc?topic=vpc-snapshots-vpc-tags)
* [Manage fast restore clones](/docs/vpc?topic=vpc-snapshots-vpc-fast-restore)
