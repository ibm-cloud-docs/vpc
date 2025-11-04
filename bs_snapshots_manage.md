---

copyright:
  years: 2021, 2025
lastupdated: "2025-11-04"

keywords: snapshots, Block Storage snapshots, manage snapshots, fast restore clone, backup snapshot, remote copy, cross-regional copy

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-vpc-manage}

You can manage existing snapshots in several ways. Rename existing snapshots to make them simpler to identify. Add user tags to snapshots for use by the VPC backup service. Enable of disable fast restore copies of a snapshot. Delete snapshots that you no longer need and free up space for new snapshots. Verify {{site.data.keyword.iamshort}} access. Verify snapshot statuses.
{: shortdesc}

## Naming snapshots
{: #snapshots-vpc-naming}

Consider naming the snapshot to indicate the volume that you copied. For example, _my-volume_ would be _my-volume-snapshot1_. Also, for quick identification, consider naming boot volumes by adding _boot_ as a prefix, such as _boot_my-volume-snapshot1_. As your list of snapshots grows, you can quickly identify the name and type of volume from which you created the snapshot.

Snapshot names adhere to the same requirements as volume names. Valid names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Snapshot names must begin with a lowercase letter and must be unique across the VPC. The UI provides name checking as a convenience. For example, if you end a snapshot name with a hyphen (-), the UI notifies you of the error. It also checks for duplicate names.

When you create a cross-regional copy of a snapshot, the new snapshot is named `[copy]-[source-snapshot-name]`. For example, a cross-regional copy of the snapshot-my-volume-snapshot1_ is automatically named _copy-my-volume-snapshot1_ when it is placed in the target region. Cross-regional copies of snapshots are independent from the source snapshot and the source volume, and the copies can be managed like any other normal snapshot.

## Renaming a snapshot in the console
{: #snapshots-vpc-rename-ui}
{: ui}

Use the following steps to rename a snapshot in the console.

1. Go to the list of snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Snapshots**.
2. Click the name of a snapshot from the list.
3. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit").
4. Provide a [new name](#snapshots-vpc-naming) for the snapshot, save, and confirm your changes.

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

### Renaming a snapshot from the CLI
{: #vpc-rename-snapshot-cli}

You can rename a snapshot from the CLI.

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

## Changing the Allowed Use property of a snapshot from the CLI
{: #snapshot-update-allowed-use-cli}
{: cli}

The `Allowed Use` properties are inherited from the parent volume. You can use the `ibmcloud is snapshot-update` command to change these values.

You must have the `is.volume.volume.manage-allowed-use` IAM role to change these properties of a snapshot.
{: requirement}

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

The properties comprise a Boolean [Common Expression Language](https://github.com/google/cel-spec/blob/master/doc/langdef.md){: external} expression. When the expression is evaluated to be `true`, then the provisioning of a virtual server instance or a bare metal server is allowed with the snapshot. When the expression is evaluated to be `false`, the provisioning is blocked.

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
{: codeblock}

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
{: screen}

To update a snapshot, use the `ibm_is_snapshot` resource. You can change the name of the snapshot, the fast restore zones, and tags. However, changing the `resource_group` and `source_volume` values forces Terraform to delete the snapshot and create a different snapshot.

```terraform
resource "ibm_is_snapshot" "example" {
  name          = "my-snapshot"
  source_volume = ibm_is_volume.example.id
  }
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_snapshot](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_snapshot){: external}.

## Sharing a snapshot with another account in the console
{: #snapshots-vpc-s2s-ui}
{: ui}

You can share a snapshot with another account in the console.

1. Go to the list of snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Snapshots**.
2. From the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Share snapshot**.
3. Enter the account ID of the account that you want to share the snapshot with.
4. Click **Create a custom IAM authorization**.

Alternatively, you can create a service-to-service authorization through the **Manage > Access (IAM) > Authorizations** menu. For more information, see [Creating service-to-service authorization for cross-account restore in the console](/docs/vpc?topic=vpc-block-s2s-auth&interface=ui#block-s2s-auth-xaccountrestore-ui).

## Managing sharing permissions for a snapshot in the console.
{: #snapshots-vpc-s2s-update-ui}
{: ui}

1. Go to the list of snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Snapshots**.
2. From the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Manage share permission**.
3. The side-panel displays the list of accounts that you shared your snapshot with.

   The list shows all the authorization that were set up for the snapshot. For example, if an account has an authorization for this specific snapshot and an authorization for all snapshots in your account, that account is listed twice.
   {: note}

4. Click **Manage IAM Authorization** to go to the Authorization page to modify or revoke the authorization.

Alternatively, you can manage a service-to-service authorization policy directly through the **Manage > Access (IAM) > Authorizations** menu. For more information, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth&interface=ui).

## Sharing a snapshot with another account from the CLI
{: #snapshots-vpc-s2s-cli}
{: cli}

You can create a service-to-service authorization for a specific snapshot from the CLI by using the `ibmcloud iam authorization-policy-create` command. For more information, see [Creating service-to-service authorization for cross-account restore from the CLI](/docs/vpc?topic=vpc-block-s2s-auth&interface=cli#block-s2s-auth-xaccountrestore-cli).

## Managing sharing permissions for a snapshot from the CLI
{: #snapshots-vpc-s2s-update-cli}
{: cli}

You can remove a service-to-service authorization for a specific snapshot from the CLI by using the `authorization-policy-delete` command. For more information, see [Removing an authorization by using the CLI](/docs/account?topic=account-serviceauth&interface=cli#remove-auth-cli).

## Sharing a snapshot with another account with the API
{: #snapshots-vpc-s2s-api}
{: api}

You can programmatically create a service-to-service authorization for a specific snapshot by calling the `policies` method in the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy). For more information, see [Creating service-to-service authorization for cross-account restore with the API](/docs/vpc?topic=vpc-block-s2s-auth&interface=api#block-s2s-auth-xaccountrestore-api).

## Managing sharing permissions for a snapshot with the API
{: #snapshots-vpc-s2s-update-api}
{: api}

You can programmatically revoke a service-to-service authorization for a specific snapshot by calling the `policies` method in the [IAM Policy Management API](/apidocs/iam-policy-management#delete-policy). For more information, see [Removing an authorization by using the API](/docs/account?topic=account-serviceauth&interface=api#remove-auth-api).

## Adding tags to a snapshot in the console
{: #snapshots-vpc-add-tags-ui}
{: ui}

You can add [user tags](/docs/vpc?topic=vpc-block-storage-about#storage-about-user-tags) and [access management tags](/docs/vpc?topic=vpc-block-storage-about#storage-about-user-tags) to your {{site.data.keyword.block_storage_is_short}} snapshots. You can add user tags to an existing snapshot or when you create a snapshot. The tags can be used by a backup policy to create backups of the snapshot.

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
4. In the new window, enter a user tag or access management tag in the respective fields.
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
-H "Authorization: $iam_token" \
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

## Creating a remote copy in the console
{: #napshots-remote-copy-create-ui}
{: ui}

Use the following steps to create cross-regional copies of snapshots from the Snapshots for VPC list or from the snapshot details page.

1. In the console, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage snapshots**.
2. In the list of snapshots, find the snapshot that you want to duplicate in another region. Make sure that the snapshot is in Stable status.
3. click the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions")and select **Copy snapshot**.
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

## Deleting snapshots in the console
{: #snapshots-vpc-delete-snapshot-ui}
{: ui}

You can delete any snapshot for a volume or all snapshots for a volume. To be able to delete a snapshot, it must meet to the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a volume.

A convenient way to determine whether you can delete a snapshot is to look in the [console](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-list-ui) for the list of snapshots and check its status.
You can delete all snapshots for a volume. Deleting all snapshots requires further confirmation in the console.

### Deleting a single snapshot in the console
{: #snapshots-vpc-delete-single-snapshot-ui}

You can delete a snapshot from the list of all snapshots by using the following steps.

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Snapshots**.
2. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") in the row of the snapshot that you want to delete.
3. Select **Delete**.
4. Confirm the deletion and click **Delete**.

You can also delete a snapshot from the details page of a {{site.data.keyword.block_storage_is_short}} volume.

1. Go to the list of all {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
2. Select a volume from the list, and click the volume name to go to the volume details page.
3. Click **Snapshots**. A list of snapshots that are taken of this volume is displayed, and you can take the following actions:
    * Click **Delete all** to delete all snapshots for this volume.
    * Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") to delete a specific snapshot.
4. Select **Delete**. If the snapshot is actively restoring a volume, the delete operation does not work.
5. Confirm the deletion.

### Deleting all snapshots for a volume in the console
{: #snapshots-vpc-delete-all-ui}

To delete all snapshots for a volume in the console, follow these steps.

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Snapshots**.
2. Click the row to select the snapshot that you want to delete.
3. From the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Delete all for volume**.
4. Confirm the deletion by typing _delete_ and then click **Delete**.

### Deleting snapshots from the {{site.data.keyword.block_storage_is_short}} details page in the console
{: #snapshots-vpc-delete-from-volume}

You can delete the most recently created snapshot from the list of snapshots from the {{site.data.keyword.block_storage_is_short}} volume details page. Optionally, you can delete all snapshots from this view.

1. Go to the list of all {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
2. Select a volume from the list and click the volume name to go to the volume details page.
3. Click **Snapshots** to see a list of snapshots taken of this volume.
4. Click **Delete all** to delete all snapshots for this volume.
5. Alternatively, select a single snapshot in the list for deletion and then:
   1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions").
   2. Select **Delete**.
   3. Confirm the deletion.

## Deleting snapshots from the CLI
{: #snapshots-vpc-delete-snapshot-cli}
{: cli}

You can delete any snapshot for a volume or all snapshots for a volume. To be able to delete a snapshot, it must meet to the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a volume.

### Deleting a single snapshot from the CLI
{: #snapshots-vpc-delete-single-snapshot-cli}

Use the following steps to delete a single snapshot by using the CLI.

1. List the snapshots that are available for a volume to confirm the ID of the snapshot that you want to delete.

    ```sh
    ibmcloud is snapshots --volume VOLUME [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
    ```
    {: pre}

    ```sh
    $ ibmcloud is snapshots --volume  r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa
    Listing snapshots in all resource groups and region eu-de under account Test Account as user test.user@ibm.com...
    ID                                          Name                             Status   Source volume                               Bootable   Resource group   Created
    r138-7cac80af-63bb-4a1b-83dd-5f6d550a5db7   bear-peroxide-viewable-oxidant   stable   r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      test-snap        2023-02-17T18:49:48+00:00
    r138-4463eb2c-4913-43b1-b9bf-62a94f74c146   cli-snapshot-test                stable   r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      defaults         2023-02-17T20:15:43+00:00
    r138-e6664842-b370-496a-9ae7-da3fb647707c   snappy-snap-snap                 stable   r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      test-snap        2023-02-17T18:53:57+00:00
    ```
    {: screen}

2. Run the `snapshot-delete` command and specify the ID of the snapshot. To delete multiple snapshots, you must specify all of their IDs in the same command.

    ```sh
    ibmcloud is snapshot-delete SNAPSHOT_ID
    ```
    {: pre}

3. Confirm the deletion of the snapshot. The response message indicates that the snapshot is deleted.

   ```sh
   $ ibmcloud is snapshot-delete r138-e6664842-b370-496a-9ae7-da3fb647707c
   This will delete snapshot r138-e6664842-b370-496a-9ae7-da3fb647707c and cannot be undone. Continue [y/N] ?> y
   Deleting snapshot r138-e6664842-b370-496a-9ae7-da3fb647707c under account Test Account as user test.user@ibm.com...
   OK
   Snapshot r138-e6664842-b370-496a-9ae7-da3fb647707c is deleted.
   ```
   {: screen}

For more information about available command options, [`ibmcloud is snapshot-delete`](/docs/vpc?topic=vpc-vpc-reference#snapshot-delete).

### Deleting all snapshots from the CLI
{: #snapshots-vpc-delete-all-snapshot-cli}

Use the following steps to delete all snapshots by using the CLI.

1. List all snapshots.

    ```sh
    ibmcloud is snapshots --volume VOLUME [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME | --all-resource-groups]
    ```
    {: pre}

2. Enter the `snapshots-delete` command and specify the volume ID.

    ```sh
    ibmcloud is snapshots-delete --volume VOLUME_ID
    ```
    {: pre}

3. Confirm the deletion of the snapshots. The response message indicates when the snapshot deletion request is accepted and the snapshots are being deleted.

   ```sh
   $ ibmcloud is snapshots-delete --volume  r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa
   This will delete snapshot by volume r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa and cannot be undone. Continue [y/N] ?> y
   Deleting snapshot by volume r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa under account Test Account as user test.user@ibm.com...
   OK
   Deletion request for snapshots by volume r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa has been accepted.
   ```
   {: screen}

For more information about available command options, [`ibmcloud is snapshot-delete`](/docs/vpc?topic=vpc-vpc-reference#snapshot-delete).

## Deleting snapshots with the API
{: #snapshots-vpc-delete-snapshot-api}
{: api}

You can delete any snapshot for a volume or all snapshots for a volume. To be able to delete a snapshot, it must meet to the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a volume.

### Deleting a single snapshot with the API
{: #snapshots-vpc-delete-single-snapshot-api}

Make a `DELETE /snapshots/{snapshot_ID}` call to delete a specific snapshot by ID.

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2022-12-22&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

### Deleting all snapshots for a volume with the API
{: #snapshots-vpc-delete-all-api}

Make a `DELETE/snapshots` call and specify the source volume ID for the `source_volume.id` parameter in the request.

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots?source_volume.id=_volume-id_&version=2022-12-22&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

## Deleting snapshots with Terraform
{: #snapshots-vpc-delete-snapshot-terraform}
{: terraform}

You can delete any snapshot for a volume or all snapshots for a volume. To be able to delete a snapshot, it must meet to the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a volume.

### Deleting a single snapshot with Terraform
{: #snapshots-vpc-delete-terraform}
{: terraform}

Use the `terraform destroy` command to conveniently delete a remote object such as a single snapshot. The following example deletes `my-snapshot`.

```terraform
terraform destroy --target ibm_is_snapshot.my-snapshot
```
{: codeblock}

For more information, see [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.

### Deleting all snapshots for a volume with Terraform
{: #snapshots-vpc-delete-all-snapshots-terraform}
{: terraform}

To delete all snapshots of a volume with terraform, use the `ibm_is_volume` resource.

```terraform
resource "ibm_is_volume" "storage" {
  name                 = "example-volume"
  profile              = "general-purpose"
  zone                 = "us-south-1"
  delete_all_snapshots = true
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume#delete_all_snapshots){: external}.

## Monitoring snapshot lifecycle states
{: #snapshots-vpc-status}

Table 2 describes the snapshot states in the snapshot lifecycle. You can see these states in the console, the command outputs of the CLI, in the API responses, and in Terraform data sources.

| Snapshot status | Explanation |
|-----------------|-------------|
| Stable | The snapshot is available for restoring a volume. |
| Waiting | Snapshot information is being retrieved. |
| Pending | While the snapshot is being created, the percentage completed displays. |
| Failed | The snapshot failed to be created, the volume can't be restored from a snapshot. |
| Suspended | Snapshot is temporarily unavailable. |
| Updating | You changed something about the snapshot and it is being updated. |
| Deleting | The snapshot is being deleted. |
| Deleted | The snapshot was deleted and is not available to restore volumes. |
{: caption="Snapshot lifecycle states" caption-side="bottom"}

## Next steps
{: #snapshots-vpc-manage-next-steps}

You can [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).
