---

copyright:
  years: 2021, 2026
lastupdated: "2026-02-18"

keywords: view snapshots, view snapshot, viewing snapshots, see snapshots, Block Storage snapshots

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing {{site.data.keyword.block_storage_is_short}} snapshots and consistency groups
{: #snapshots-vpc-view}

You can view a list of all snapshots and consistency groups, and drill down to see information about a particular snapshot. Choose the UI, CLI, API, or Terraform to retrieve this information.
{: shortdesc}

Consistency groups are not supported for second-generation storage volumes in the current release.
{: note}

## Listing snapshots in the console
{: #snapshots-vpc-view-ui}
{: ui}

You can use the UI to list your snapshots and consistency groups.

### Listing all snapshots in the console
{: #snapshots-vpc-view-list-ui}

In the console, you can view a list of all snapshots that you created, with the most recent one at the beginning of the list. You can filter the list to view specific snapshots.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage snapshots for VPC**. The page has two main tabs: Snapshot consistency group and Snapshots.

1. Select the **Snapshots** tab. The snapshots are listed for a specific region. If you want to see snapshots in another region, click the arrow to expand the list and select a different region. By default, the newest snapshots are displayed at the beginning of the list.

1. As your list of snapshots grows, use the filter to indicate the number of snapshots to display per page. Use the page navigation arrows to move forward and back through the list.

Table 1 describes the information for all snapshots that is shown on the list of snapshots.

| Field | Description |
|-------|-------------|
| Name  | The name that you provided when you created the snapshot. Click the name of the snapshot to see its [details](#snapshots-vpc-view-snapshot-ui). |
| Status | Status of the snapshot, depending on whether it's usable (_active_ status), unusable, or being created. For more information, see [Snapshot statuses](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-status). |
| Size | Size of the snapshot in GBs. The size is inherited from the source volume. |
| Source volume | It shows the boot or data volume from which the snapshot was created. Click the name of the volume to see its [details](/docs/vpc?topic=vpc-viewing-block-storage). |
| Snapshot copies | Number of copies that a snapshot has in other regions. |
| Created date | The local date and time when the snapshot was created. |
| Consistency group | This field shows the name of the consistency group if the snapshot is a member of a snapshot set.|
{: caption="List of all snapshots" caption-side="bottom"}

Click the settings icon ![Settings icon](../icons/settings.svg "Settings") to display optional fields that you can add to the table.

| Field | Description |
|-------|-------------|
| Encryption | It shows [IBM-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-provider-managed-encryption) or [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-customer-managed-encryption). The encryption is inherited from the source volume. |
| Fast restore status | Status of restore as enabled, pending, or disabled. |
| Resource group |The resource group that the snapshot belongs to.|
| Bootable | Yes or No. It shows whether the snapshot was taken of a boot volume or a data volume. |
| Created by | It shows whether the snapshot was created by the user or a [backup policy](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-concepts). |
| Generation | Gen 1 or Gen 2, this value is inherited from the parent volume based on its volume profile. |
{: caption="List of optional informational fields for all snapshots" caption-side="bottom"}

By clicking the Actions icon ![Actions icon](../icons/action-menu-icon.svg "Actions"), you can display a menu of context-specific actions.
   - [Rename](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-rename-ui)
   - Copy UUID.
   - Copy CRN.
   - [Create volume](/docs/vpc?topic=vpc-snapshots-vpc-restore#snapshots-vpc-restore-ui).
   - [Create snapshot copy](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=ui#crsnapshots-vpc-create-ui).
   - [Share snapshot](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-s2s-ui).
   - [Manage share permissions](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-s2s-update-ui).
   - [Edit fast restore](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-edit-fast-restore).
   - [Delete](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-delete-snapshot-ui).
   - [Delete all snapshots for a volume](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-delete-all-ui).

You can also list all snapshots that were created from a {{site.data.keyword.block_storage_is_short}} volume from the volume details page. For more information, see [View all snapshots that were created from the {{site.data.keyword.block_storage_is_short}} volume](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#view-snapshots-for-volume).
{: tip}

### Viewing snapshot details in the console
{: #snapshots-vpc-view-snapshot-ui}

To see detailed information about a snapshot, locate the snapshot on the Block Storage snapshots for VPC list. Then, click the name of a snapshot. The details page displays the name of the snapshot, its status, and attached user tags in the title.

The snapshot details panel shows the information that is described in the following table.

| Field | Description |
|-------|-------------|
| Name  | The name of the snapshot, which you can change by clicking the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit"). For more information, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-naming). |
| Snapshot ID | Copiable GUID of the snapshot. |
| Bootable | Yes or No. |
| CRN | Copiable CRN of the snapshot. |
| Resource group | Resource group defined when you set up your VPC. |
| Created date | Date and time that the snapshot resource creation process started. |
| Captured date | The date and time that this snapshot was captured. |
| Created by | This field shows either `User` or `Backup policy`. If the snapshot is created by a backup policy, the UI also displays the name of the backup plan that created the backup snapshot. |
| Size| Size in GBs of the snapshot, inherited from the source volume. |
| Source volume | Source volume from which the first snapshot was taken. Click the link for volume details. If the volume was deleted, the name appears without a link. |
| Encryption | Provider-managed or customer-managed encryption. For customer-managed encryption, the KMS instance, root key name, and root key ID are shown. |
| Generation  | This value is either Gen 1 or Gen 2 based on the volume profile of the source volume. |
| Virtual server expression | The allowed-use expression defines and restricts which virtual server profiles the bootable snapshot is compatible with. When you provision a virtual server instance by using the snapshot, the expression is matched against the requested instance properties. For more information, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui). |
| Fast restore panel | Use [fast restore](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots_vpc_fast_restore) to create a volume from this snapshot that is fully provisioned. Click **Edit** to [enable or disable fast restore](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-edit-fast-restore) in a zone. |
| Consistency group panel | It displays information about the consistency group that the snapshot is a member of. Click **Create virtual server** to restore volumes from the consistency group. |
{: caption="Snapshot details" caption-side="bottom"}

By clicking the Actions icon ![Actions icon](../icons/action-menu-icon.svg "Actions"), you can display a menu of context-specific actions.
   - [Create a volume from the snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui)
   - [Edit fast restore](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-edit-fast-restore), or delete a snapshot.

### Listing all consistency groups of snapshots in the console
{: #consistency-group-vpc-view-list-ui}

In the console, you can view a list of all consistency groups that you created, with the most recent one at the beginning of the list. You can filter the list to view specific consistency groups.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage snapshots for VPC**. The page has two main tabs: Snapshot consistency group and Snapshots.

1. Select the **Snapshot consistency group** tab. The groups are listed for a specific region. If you want to see consistency groups in another region, click the arrow to expand the list and select a different region. By default, the newest snapshot groups are displayed at the beginning of the list.

1. As your list of consistency groups grows, use the filter to indicate the number of items to display per page. Use the page navigation arrows to move forward and back through the list.

The following table describes the information for all consistency groups in the list.

| Field  | Description |
|--------|-------------|
| Name   | The name that you provided when you created the consistency group. Click the name of the group to see its [details](#snapshot-vpc-view-consistency-group-ui). |
| Status | Status of the consistency group, depending on whether it's usable (_active_ status), unusable, or being created. |
| Source VSI | It shows the virtual server instance that contains the source volumes of the snapshots in the consistency group. Click the name of the virtual server instance to see its details. |
| Snapshot members | Number of snapshots that are members of the group. |
| Created date | The local date and time when the snapshot was created. |
{: caption="List of all consistency groups" caption-side="bottom"}

Click the settings icon ![Settings icon](../icons/settings.svg "Settings") to display optional fields that you can add to the table.

| Field      | Description |
|------------|-------------|
| Created by | It shows whether the snapshot set was created by the user or a [backup policy](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-concepts). |
| Resource group | The resource group that the consistency group belongs to.|
| Created by | It shows whether the snapshot was created by the user or a [backup policy](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-concepts). |
{: caption="List of optional informational fields for all snapshots" caption-side="bottom"}

By clicking the Actions icon ![Actions icon](../icons/action-menu-icon.svg "Actions"), you can display a menu of context-specific actions.
   - Rename
   - Copy UUID
   - Copy the Cloud Resource Name (CRN).
   - [Create virtual server](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-restore-cr-details-ui)
   - Delete

### Viewing details of a consistency group in the console
{: #snapshot-vpc-view-consistency-group-ui}

To see detailed information about a consistency group, locate the group on the Block Storage snapshots for VPC list. Then, click the name of a consistency group.

The overview section provides details about the consistency group and the virtual server instance that the consistency group snapshots were taken of. The first panel contains information about the consistency group.

| Field | Description |
|-------|-------------|
| Name  | The unique name of the consistency group. If you want to update it, click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit"). |
| Consistency group ID | This field shows the ID string of the consistency group.   |
| CRN  | Cloud Resource Name of the consistency group.  |
| Created by  | The field shows either `user` for a manually created snapshot, or a [backup policy](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-concepts) for automated snapshots. |
| Resource group  | The resource group that the consistency group belongs to. |
| Created date | It shows the date when the consistency group was created.  |
| Delete snapshot members | It shows whether the snapshots are deleted or kept when the consistency group is deleted. Click the toggle to change the status. |
| Region| The VPC region that the consistency group was created in.|
{: caption="Consistency group overview." caption-side="bottom"}

For more information, see [Managing snapshot consistency groups](/docs/vpc?topic=vpc-snapshots-vpc-manage-consistency-groups).

The second panel contains information about the virtual server instance that the source volumes of the snapshot set are attached to.

| Field | Description |
|-------|-------------|
| Name  | Name of the virtual server instance. Click the link to go to the details page of the instance.  |
| Virtual server instance ID  | You can copy the ID to use in other interfaces such as CLI, API, or Terraform. |
| Virtual private cloud  | The name of the VPC that the consistency group is created in. Click the link to go to the details page of the VPC. |
| Image  | Information about the operating system image. |
| Resource group  | The resource group that the virtual server instance belongs to.  |
| Tags  | The user-defined tags that are used for the virtual server instance.  |
{: caption="Virtual server instance overview." caption-side="bottom"}

The Snapshots list displays the snapshots within the consistency group and provides the same information and actions as described in [Table 1](#snapshots-vpc-view-list-ui).

## Viewing snapshots from the CLI
{: #snapshots-vpc-view-cli}
{: cli}

You can use the CLI to list all snapshots, all snapshots for a volume, and details about a particular snapshot.

### Before you begin
{: #snapshots-vpc-view-cli-requirement}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

Log in to {{site.data.keyword.cloud}}.

```sh
ibmcloud login --sso -a cloud.ibm.com
```
{: pre}

This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

### Viewing all snapshots of an account in a region from the CLI
{: #snapshots-vpc-view-all-account-cli}

Run the `snapshots` command to list all snapshots. The following example provides a list of all the snapshots in all the resource groups in the `us-south` region.

```sh
ibmcloud is snapshots [--json]
```
{: pre}

```sh
Listing snapshots in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
ID                                          Name             Status   Source volume                               Bootable   Resource group   Created                     Catalog Offering Version   Catalog Offering Plan   Storage Generation   
r006-22db5609-69a1-4fe2-bd02-f64fb11230b3   my-test-snapshot stable   r006-ccbc5bc6-cd88-48e0-9a1d-de0f4d024324   true       defaults         2025-07-31T00:04:24+00:00   -                          -                       1   
r006-e799fad8-aefa-4df5-81bd-dac6d13200a6   my-test-snap-1   stable   r006-2c5f577c-8cd0-47ef-8aec-cd8d1423249f   true       defaults         2025-08-06T22:38:01+00:00   -                          -                       1   
```
{: screen}

For more information about available snapshot command options, see [`ibmcloud is snapshots`](/docs/vpc?topic=vpc-vpc-reference#snapshots-list).

### Viewing all snapshots of a volume from the CLI
{: #snapshots-vpc-view-all-snapshots-cli}

Run the `snapshots` command and specify the volume ID.

```sh
ibmcloud is snapshots --volume VOLUME-ID [--json]
```
{: pre}

The following example shows all three snapshots that were taken of the volume `r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa`.

```sh
ibmcloud is snapshots --volume  r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa
```
{: pre}

```sh
Listing snapshots in all resource groups and region eu-de under account Test Account as user test.user@ibm.com...
ID                                          Name                             Status   Source volume                               Bootable   Resource group   Created
r138-7cac80af-63bb-4a1b-83dd-5f6d550a5db7   bear-peroxide-viewable-oxidant   stable   r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      test-snap        2023-02-17T18:49:48+00:00
r138-4463eb2c-4913-43b1-b9bf-62a94f74c146   cli-snapshot-test                stable   r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      defaults         2023-02-17T20:15:43+00:00
r138-e6664842-b370-496a-9ae7-da3fb647707c   snappy-snap-snap                 stable   r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      test-snap        2023-02-17T18:53:57+00:00
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshots`](/docs/vpc?topic=vpc-vpc-reference#snapshots-list).

### Viewing all snapshots in a consistency group from the CLI
{: #snapshots-cr-vpc-view-all-cli}

Run the `snapshots` command and specify the consistency group ID, name, or CRN.

```sh
ibmcloud is snapshots --volume CONSISTENCY_GROUP_ID
```
{: pre}

The following example uses the ID of the consistency group to list the snapshots that are members of the set.

```sh
ibmcloud is snapshots --snapshot-consistency-group r174-7c8762e2-c1b9-424e-b773-23240d1780dd
```
{: pre}

```sh
Listing snapshots in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
ID                                          Name            Status   Source volume                               Bootable   Resource group   Created
r174-b8cac978-a990-4824-a15c-604856982efe   snapshot-no-2   stable   r174-0641e516-09a1-4291-96ca-af254017123e   true       Default          2023-09-05T23:14:40+05:30
r174-7311f226-8259-46be-9bfa-5b2cd08bdf2f   snapshot-no-1   stable   r174-bf595773-9922-4dd4-9a3c-998b10022e0c   false      Default          2023-09-05T23:14:40+05:30
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshots`](/docs/vpc?topic=vpc-vpc-reference#snapshots-list).

### Viewing details of a snapshot from the CLI
{: #snapshots-vpc-view-details-cli}

Run the `snapshots` command and specify the snapshot ID.

```sh
ibmcloud is snapshots SNAPSHOT_ID [--json]
```
{: pre}

The following example shows the details of a bootable snapshot in the `us-south` region.

```sh
ibmcloud is snapshot r006-e799fad8-aefa-4df5-81bd-dac6d13200a6
```
{: pre}

```sh
Getting snapshot r006-e799fad8-aefa-4df5-81bd-dac6d13200a6 under account Test Account as user test.user@ibm.com...
                                   
ID                              r006-e799fad8-aefa-4df5-81bd-dac6d13200a6   
Name                            my-test-snap-1   
CRN                             crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-e799fad8-aefa-4df5-81bd-dac6d13200a6   
Status                          stable   
Clones                          Zone   Available   Created      
                                   
Source volume                   ID                                          Name               Remote Region   CRN                                                                                                                        Resource type      
                                r006-2c5f577c-8cd0-47ef-8aec-cd8d1423249f   test-volume-fast   -               crn:v1:bluemix:public:is:us-south-1:a/a1234567::volume:r006-2c5f577c-8cd0-47ef-8aec-cd8d1423249f   volume      
                                   
Backup policy plan              -   
Snapshot Copies                 -   
Bootable                        true   
Encryption                      provider_managed   
Encryption key                  -   
Source Snapshot                 -   
Minimum capacity(GB)            100   
Size(GB)                        10   
Source Image                    ID                                          Name                                 Remote Region   CRN                                                                                                                     Resource type      
                                r006-cec640a3-615e-4364-bae7-d3642d9f9e34   ibm-ubuntu-22-04-5-minimal-amd64-4   -               crn:v1:bluemix:public:is:us-south:a/a1234567::image:r006-cec640a3-615e-4364-bae7-d3642d9f9e34   image      
                                   
Operating system                Name                 Vendor      Version                                     Family         Architecture   Display name      
                                ubuntu-22-04-amd64   Canonical   22.04 LTS Jammy Jellyfish Minimal Install   Ubuntu Linux   amd64          Ubuntu Linux 22.04 LTS Jammy Jellyfish Minimal Install (amd64)      
                                   
Resource group                  ID                                 Name      
                                6edefe513d934fdd872e78ee6a8e73ef   defaults      
                                   
Created                         2025-08-06T22:38:01+00:00   
Captured at                     2025-08-06T22:38:02+00:00   
Tags                            -   
Service Tags                    -   
Storage Generation              1   
Allowed Use API Version         2025-07-17   
Allowed Use Bare Metal Server   true   
Allowed Use Instance            true   
```
{: screen}

The `allowed-used` properties are inherited from the source volume or snapshot at creation time. You can change their value either at creation time or later if you are authorized to the `is.snapshot.snapshot.manage-allowed-use` IAM role. The properties comprise a Boolean [Common Expression Language](https://github.com/google/cel-spec/blob/master/doc/langdef.md){: external} expression. When the instance expression is evaluated to be `true`, then the provisioning of a virtual server instance is allowed with the snapshot. When the expression is evaluated to be `false`, the provisioning is blocked. If the value is not specified at the time of creation, then the constraint expressions are taken from the source resource.

The following example shows the details of a nonbootable snapshot in the `eu-de` region, which also has a fast restore clone in the `eu-de-1` zone.

```sh
ibmcloud is snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146
```
{: pre}

```sh
Getting snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...

ID                         r138-4463eb2c-4913-43b1-b9bf-62a94f74c146
Name                       cli-snapshot-test
CRN                        crn:v1:bluemix:public:is:eu-de:a/a1234567::snapshot:r138-4463eb2c-4913-43b1-b9bf-62a94f74c146
Status                     stable
Clones                     Zone      Available   Created
                           eu-de-1   true        2023-02-17T20:15:46+00:00

Source volume              ID                                          Name
                           r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   block-test1

Bootable                   false
Encryption                 provider_managed
Encryption key             -
Minimum capacity(GB)       20
Size(GB)                   1
Snapshot Consistency Group ID                                                    Name                    Snapshot Consistency Group ID                                         Name                                   CRN                                                                    Resource type
                           r174-4625d29b-3ac9-4bee-aca8-9366c4fd0c44 multiple-snapshots-consistency-group-1   crn:v1:bluemix:public:is:eu-de:a/a1234567:snapshot-consistency-group:r174-4625d29b-3ac9-4bee-aca8-9366c4fd0c44   snapshot_consistency_group

Resource group             ID                                 Name
                           6edefe513d934fdd872e78ee6a8e73ef   defaults

Created                    2023-12-05T20:15:43+00:00
Captured at                2023-12-05T20:15:44+00:00
Tags                       env:prod,env:test
Service Tags               -
Storage Generation         1
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshots`](/docs/vpc?topic=vpc-vpc-reference#snapshots-list).

### Viewing all fast restore snapshot clones from the CLI
{: #snapshots-view-zonal-clones-cli}

You can list all available fast restore clones of a snapshot by issuing the `ibmcloud is snapshot-clones` command. The following example lists the available fast restore snapshot clones of the snapshot that has the ID of `r138-4463eb2c-4913-43b1-b9bf-62a94f74c146`.

```sh
ibmcloud is snapshot-clones r138-4463eb2c-4913-43b1-b9bf-62a94f74c146
```
{: pre}

```sh
Listing zonal clones of snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...
Zone      Available   Created
eu-de-1   true        2023-02-17T20:15:46+00:00
eu-de-3   true        2023-02-17T20:29:21+00:00
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-clones`](/docs/vpc?topic=vpc-vpc-reference#snapshot-clones-list).

### Viewing details of a fast restore snapshot clone from the CLI
{: #snapshots-clone-details-cli}

You can run the `ibmcloud is snapshot-clone` command to list the details of a specific fast restore snapshot clone. The following example shows how to list the details of the fast restore snapshot clone of snapshot `r138-4463eb2c-4913-43b1-b9bf-62a94f74c146` that is in the `eu-de-3` zone.

```sh
ibmcloud is snapshot-clone r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 eu-de-3
```
{: pre}

```sh
Getting zonal clone eu-de-3 of snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...

Zone        eu-de-3
Available   true
Created     2023-02-17T20:29:21+00:00
Href        https://eu-de.iaas.cloud.ibm.com/v1/regions/eu-de/zones/eu-de-3
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-clone`](/docs/vpc?topic=vpc-vpc-reference#snapshot-clone-view).

### Viewing details of a remote region snapshot copy from the CLI
{: #snapshots-regional-copy-details-cli}

You can run the `ibmcloud is snapshot` command to list the details about a specific remote region snapshot copy. The following example shows how to list the details of the remote region snapshot (`my-cli-snapshot-crc`) that is in the `us-south` region. The response provides information about the parent snapshot (`cli-snap-crc-test`), the parent volume, and the backup policy that created the snapshot as well.

```sh
ibmcloud is snapshot my-cli-snapshot-crc
```
{: pre}

```sh
Getting snapshot my-cli-snapshot-crc-target under account Test Account as user test.user@ibm.com...

ID                     r006-8428038a-a399-4894-8c84-c8d7a4a75fae
Name                   my-cli-snapshot-crc
CRN                    crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-8428038a-a399-4894-8c84-c8d7a4a75fae
Status                 stable
Clones                 Zone   Available   Created

Source volume          ID                                          Name                   Remote Region   CRN                                                                                                                       Resource type
                       r014-77ecae08-4a1e-4d15-94f0-814bfc1e108b   -remote-814bfc1e108b   us-east         crn:v1:bluemix:public:is:us-east-a/a1234567::volume:r014-77ecae08-4a1e-4d15-94f0-814bfc1e108b   volume

Backup policy plan     ID                                          Name                   Resource type
                       r014-d659eb15-ff07-4d54-8dd8-808a037e1b61   -remote-808a037e1b61   backup_policy_plan

Snapshot Copies        -
Bootable               false
Encryption             provider_managed
Encryption key         -
Source Snapshot        ID                                          Name                Remote Region   CRN                                                                                                                       Resource type
                       r014-1b960f89-e5f8-4323-ab81-ceaf15ea2a1c   cli-snap-crc-test   us-east         crn:v1:bluemix:public:is:us-east:a/a1234567::snapshot:r014-1b960f89-e5f8-4323-ab81-ceaf15ea2a1c   snapshot

Minimum capacity(GB)   100
Size(GB)               1
Source Image           -
Resource group         ID                                 Name
                       6edefe513d934fdd872e78ee6a8e73ef   defaults

Created                2024-06-18T17:21:24+00:00
Captured at            2024-06-18T17:21:20+00:00
Tags                   -
Service Tags           -
Storage Generation     1
```
{: screen}

### Viewing all snapshot consistency groups from the CLI
{: #snapshots-vpc-view-all-consistency-groups-cli}

Run the `snapshot-consistency-groups` command to list all consistency groups in a region.

```sh
ibmcloud is snapshot-consistency-groups
```
{: pre}

For more information about available command options, see [`ibmcloud is snapshot-consistency-groups`](/docs/vpc?topic=vpc-vpc-reference#snapshots-cli).

### Viewing details of a snapshot consistency group from the CLI
{: #snapshots-vpc-view-a-consistency-group-cli}

Run the `snapshot-consistency-group` command to list the details of a specific consistency group in a region.

```sh
ibmcloud is snapshot-consistency-group CONSISTENCY_GROUP_ID
```
{: pre}


For more information about available command options, see  [`ibmcloud is snapshot-consistency-group`](/docs/vpc?topic=vpc-vpc-reference#snapshots-cli).

## Listing snapshots with the API
{: #snapshots-vpc-view-all-api}
{: api}

You can list your snapshots by using the API.

### Listing all snapshots with the API
{: #snapshots-vpc-list-all-api}

You can programmatically list all snapshots of your volumes by calling the `/snapshots` method in the [VPC API](/apidocs/vpc/latest#list-snapshots){: external} as shown in the following sample request. By default, the list shows the most recent snapshots first, followed by older snapshots in descending order.

```sh
curl -X GET \
  "$vpc_api_endpoint/v1/snapshots?version=2022-12-16&generation=2" \
  -H "Authorization: $iam_token"
```
{: pre}

You can filter the list by using the resource group ID, source volume ID, or source volume CRN, and further filter the results by using the following options:

* Limit - To control the number of snapshots displayed on a page.
* Start - To specify which snapshot (by ID) to start the list.
* Sort - To control the order in which you sort the page, such as by date created or alphabetically by snapshot name.

For example, the following call filters the list to show snapshots that were created for a single volume and limits the results to five per page.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/snapshots?version=2022-08-16&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "limit": 5,
      "source_volume":
        "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8"
      }'
   ```
   {: pre}

A successful response looks like the following example.

```json
{
    "first": {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots?limit=50"
    },
    "limit": 5,
    "snapshots": [
      {
        "allowed_use": {
          "api_version": "2025-03-31",
          "bare_metal_server": "false",
          "instance": "gpu.count > 0 && gpu.manufacturer == 'nvidia'"
        },
        "bootable": true,
        "clones": [],
        "created_at": "2022-12-18T20:18:18Z",
        "crn": "crn:[...]",
        "deletable": true,
        "encryption": "user_managed",
        "encryption_key": {
          "crn": "crn:[...]"
        },
        "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/f945d446-0d46-478a-a298-acba907d45d1",
        "id": " f945d446-0d46-478a-a298-acba907d45d1",
        "lifecycle_state": "stable",
        "minimum_capacity": 100,
        "name": "my-snapshot",
        "operating_system": {
          "architecture": "amd64",
          "dedicated_host_only": false,
          "display_name": "Ubuntu Linux 20.04 LTS Focal Fossa Minimal Install (amd64)",
          "family": "Ubuntu Linux",
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
          "href": "https://us-south.iaas.cloud.ibm.com/v1/images/de066f7d-b95f-4ca2-9aa0-b6362889e933",
          "id": "de066f7d-b95f-4ca2-9aa0-b6362889e933",
          "name": "ubuntu-20-04-minimal-amd64-1"
        },
        "source_volume": {
          "crn": "crn:[...]",
          "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/8948ad59-bc0f-7510-812f-5dc64f59fab8",
          "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8",
          "name": "instance-vol1"
        },
        "storage_generation": 1,
        "user_tags": []
      },
      {
        "allowed_use": {
          "api_version": "2025-03-31",
          "bare_metal_server": "false",
          "instance": "gpu.count > 0 && gpu.manufacturer == 'nvidia'"
        },
        "bootable": true,
        "clones": [],
        "created_at": "2022-12-17T20:11:28Z",
        "crn": "crn:[...]",
        "deletable": true,
        "encryption": "user_managed",
        "encryption_key": {
          "crn": "crn:[...]"
        },
        "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/e5bfa329-0e36-433f-a3bb-0df632e79263",
        "id": "e5bfa329-0e36-433f-a3bb-0df632e79263",
        "lifecycle_state": "stable",
        "minimum_capacity": 100,
        "name": "my-snapshot-2",
        "operating_system": {
          "architecture": "amd64",
          "dedicated_host_only": false,
          "display_name": "Ubuntu Linux 20.04 LTS Focal Fossa Minimal Install (amd64)",
          "family": "Ubuntu Linux",
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
          "href": "https://us-south.iaas.cloud.ibm.com/v1/images/23045dc2-b463-4cda-b424-bc3dcf51dfbb",
          "id": "23045dc2-b463-4cda-b424-bc3dcf51dfbb",
          "name": "ubuntu-20-04-minimal-amd64-1"
        },
        "source_volume": {
          "crn": "crn:[...]",
          "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/8948ad59-bc0f-7510-812f-5dc64f59fab8",
          "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8",
          "name": "instance-vol1"
        },
        "storage_generation": 1,
        "user_tags": []
      }
    ],
    "total_count": 2
  }
```
{: codeblock}

### Listing details of a snapshot with the API
{: #snapshots-vpc-view-api}

You can programmatically retrieve the details of a single snapshot by calling the `/snapshots` method in the [VPC API](/apidocs/vpc/latest#get-snapshot){: external} and specifying the snapshot ID as shown in the following sample request.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2022-12-16&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

A successful response looks like the following example.

```json
{
  "allowed_use": {
    "api_version": "2025-03-31",
    "instance": "gpu.count >= 2 && gpu.model in ['h100', 'a100'] && gpu.memory > 16 && enable_secure_boot == true",
    "bare_metal_server": "false"
  },
  "bootable": true,
  "clones": [],
  "captured_at": "2022-12-16T20:19:12Z",
  "created_at": "2021-12-16T20:18:18Z",
  "crn": "crn:[...]",
  "deletable": true,
  "encryption": "user_managed",
  "encryption_key": {
    "crn": "crn:[...]"
  },
  "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d",
  "id": "7528eb61-bc01-4763-a67a-a414a103f96d",
  "lifecycle_state": "stable",
  "minimum_capacity": 100,
  "name": "my-snapshot",
  "operating_system": {
    "architecture": "amd64",
    "dedicated_host_only": false,
    "display_name": "Ubuntu Linux 20.04 LTS Focal Fossa Minimal Install (amd64)",
    "family": "Ubuntu Linux",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/ubuntu-20-04-amd64",
    "name": "ubuntu-20-04-amd64",
    "vendor": "Canonical",
    "version": "20.04 LTS Focal Fossa Minimal Install"
  },
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/59ff2d74-b0e5-4b40-a553-b812e50c72e9",
    "id": "59ff2d74-b0e5-4b40-a553-b812e50c72e9",
    "name": "Default"
  },
  "resource_type": "snapshot",
  "service_tags": [],
  "size": 1,
  "source_image": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/images/dc021ec6-759d-4b87-8e0a-04106b8aa635",
    "id": "dc021ec6-759d-4b87-8e0a-04106b8aa635",
    "name": "ubuntu-20-04-minimal-amd64-1"
  },
  "source_volume": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/c90453a6-7b5e-47ce-95fc-f6fff9e5b79c",
    "id": "c90453a6-7b5e-47ce-95fc-f6fff9e5b79c",
    "name": "my-instance-data"
  },
  "storage_generation": 1,
  "user_tags": []
}
```
{: codeblock}

* The `allowed-used` properties are inherited from the source volume or snapshot at creation time. You can change their value either at creation time or later if you are authorized to the `is.snapshot.snapshot.manage-allowed-use` IAM role. The properties comprise a Boolean [Common Expression Language](https://github.com/google/cel-spec/blob/master/doc/langdef.md){: external} expression. When the instance expression is evaluated to be `true`, then the provisioning of a virtual server instance is allowed with the snapshot. When the expression is evaluated to be `false`, the provisioning is blocked. If the value is not specified at the time of creation, then the constraint expressions are taken from the source resource.

### Viewing all fast restore snapshot clones with the API
{: #snapshots-view-zonal-clones-api}

You can programmatically list all fast restore clones by calling the `/snapshots/clones` method in the [VPC API](/apidocs/vpc/latest#list-snapshot-clones){: external} and specifying the snapshot ID as shown in the following sample request.

Make a `GET /v1/snapshots/{id}/clones` request to list all fast restore snapshot clones.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d/clones?version=2022-12-16&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

A successful response provides information about the fast restore snapshot clones.

```json
{
  "clones": [
    {
      "available": true,
      "created_at": "2022-12-16T19:55:28.537Z",
      "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
        "name": "us-south-2"
      }
    },
    {
      "available": true,
      "created_at": "2022-12-16T19:58:22.337Z",
      "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-3",
        "name": "us-south-3"
      }
    }
  ]
}
```
{: codeblock}

### Viewing details of a fast restore snapshot clone with the API
{: #snapshots-clone-details-api}

You can programmatically retrieve the details of a single fast restore clone by calling the `/snapshots/clones` method in the [VPC API](/apidocs/vpc/latest#get-snapshot-clone){: external} and specifying the snapshot ID and zone as shown in the following sample request.

Make a `GET /v1/snapshots/{id}/clones/{zone-name}` request to retrieve a single fast restore snapshot clone specified by ID and zone name.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d/clones/us-south-1?version=2022-12-16&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

A successful response shows the following information about the fast restore snapshot clone.

```json
{
  "available": true,
  "created_at": "2022-12-16T20:15:35.164Z",
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: codeblock}

### Viewing details of a remote region snapshot copy with the API
{: #snapshots-regional-copy-details-api}

You can programmatically retrieve the details of a single snapshot by calling the `/snapshots` method in the [VPC API](/apidocs/vpc/latest#get-snapshot){: external} and specifying the snapshot ID as shown in the following sample request.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2022-12-16&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

A successful response shows information that is similar to the following example.

```json
{
  "allowed_use": {
    "api_version": "2025-03-31",
    "instance": "gpu.count >= 2 && gpu.model in ['h100', 'a100'] && gpu.memory > 16 && enable_secure_boot == true",
    "bare_metal_server": "false"
  },
  "bootable": true,
  "created_at": "2023-05-18T20:18:18Z",
  "crn": "crn:[...]",
  "deletable": false,
  "encryption": "user_managed",
  "encryption_key": {
    "crn": "crn:[...]"
  },
  "href": "https://us-east.iaas.cloud.ibm.com/v1/snapshots/r139-f6bfa329-0e36-433f-a3bb-0df632e79263",
  "id": "r139-f6bfa329-0e36-433f-a3bb-0df632e79263",
  "lifecycle_state": "stable",
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
  "region": "us-east",
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
    	   "href": "https://us-east.iaas.cloud.ibm.com/v1/regions/us-south"
    	}
    },
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-411a798c-5816-4082-8ecb-554a440f83de",
    "id": "r006-411a798c-5816-4082-8ecb-554a440f83de",
    "name": "my-instance-data"
  },
  "storage_generation": 1,
  "user_tags": []
}
```
{: codeblock}

### Viewing all snapshot consistency groups with the API
{: #snapshots-vpc-view-all-consistency-groups-apip}

You can programmatically list all consistency groups by calling the `/snapshot_consistency_groups` method in the [VPC API](/apidocs/vpc/latest#list-snapshot-consistency-groups){: external} as shown in the following sample request.

```sh
curl -X GET "$vpc_api_endpoint/v1/snapshot_consistency_groups?version=2023-12-05&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

### Viewing details of a snapshot consistency group with the API
{: #snapshots-vpc-view-a-consistency-group-api}

You can programmatically retrieve details of a consistency group by calling the `/snapshot_consistency_groups` method in the [VPC API](/apidocs/vpc/latest#get-snapshot-consistency-group){: external} as shown in the following sample request.

```sh
curl -X GET "$vpc_api_endpoint/v1/snapshot_consistency_groups/006-e8707243-96b3-4c27-be1f-57eff0196207?version=2023-12-05&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

A successful response looks like the following example.

```json
{
    "created_at": "2023-12-05T22:40:42Z",
    "id": "r006-e8707243-96b3-4c27-be1f-57eff0196207",
    "delete_snapshots_on_delete": true,
    "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot-consistency-group:r006-e8707243-96b3-4c27-be1f-57eff0196207",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshot_consistency_groups/r006-e8707243-96b3-4c27-be1f-57eff0196207",
    "name": "my-consistency-group-snapshots",
    "resource_group": {
        "id": "a41ca701e1c041c1bb224564ff645770",
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/a41ca701e1c041c1bb224564ff645770",
        "name": "Default"
    },
    "lifecycle_state": "stable",
    "resource_type": "snapshot_consistency_group",
    "service_tags": [
        "is.instance:0727_44fda9fe-eeef-4e71-8fa4-4c3f7300e645"
    ],
    "snapshots": [
        {
            "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-540d8d17-5334-4d04-b845-270dbb30233f",
            "id": "r006-540d8d17-5334-4d04-b845-270dbb30233f",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/r006-540d8d17-5334-4d04-b845-270dbb30233f",
            "name": "my-test-scg-1",
            "resource_type": "snapshot"
        },
        {
            "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-77cc526a-b21c-4bdc-a2b3-442a1d4e78c0",
            "id": "r006-77cc526a-b21c-4bdc-a2b3-442a1d4e78c0",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/r006-77cc526a-b21c-4bdc-a2b3-442a1d4e78c0",
            "name": "my-test-scg-2",
            "resource_type": "snapshot"
        }
    ]
}
```
{: screen}

## Viewing snapshots with Terraform
{: #snapshots-vpc-view-terraform}
{: terraform}

You can use Terraform to view your snapshots.

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

### Listing all snapshots with Terraform
{: #snapshots-vpc-view-all-terraform}

Import the details of a collection of snapshots as a read-only data source. You can filter the collection of snapshots by `source_volume`, `resource_group`, `name`, and so on.

```terraform
data "ibm_is_snapshots" "example" {
}
```
{: codeblock}

For more information, see [ibm_is_snapshots](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_snapshots){: external}.

### Listing details of a snapshot with Terraform
{: #snapshots-vpc-view-snap-terraform}

Import the details of a snapshot as a read-only data source. You can specify either the snapshot ID or the snapshot name.

```terraform
data "ibm_is_snapshot" "example" {
  identifier = "r138-e6664842-b370-496a-9ae7-da3fb647707c"
}
```
{: codeblock}

```terraform
data "ibm_is_snapshot" "example" {
  name = "snappy-snap-snap"
}
```
{: codeblock}

* The `allowed-used` properties are inherited from the source volume or snapshot at creation time. You can change their value either at creation time or later if you are authorized to the `is.snapshot.snapshot.manage-allowed-use` IAM role. The properties comprise a Boolean [Common Expression Language](https://github.com/google/cel-spec/blob/master/doc/langdef.md){: external} expression. When the instance expression is evaluated to be `true`, then the provisioning of a virtual server instance is allowed with the snapshot. When the expression is evaluated to be `false`, the provisioning is blocked. If the value is not specified at the time of creation, then the constraint expressions are taken from the source resource.

For more information, see [ibm_is_snapshot](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_snapshot){: external}.

### Listing all fast restore snapshot clones
{: #snapshots-clone-view-terraform}

Import the details of all the fast restore clones of a snapshot as a read-only data source.

```terraform
data "ibm_is_snapshot_clones" "ds_snapshotclones" {
  snapshot = "r138-e6664842-b370-496a-9ae7-da3fb647707c"
}
```
{: codenblock}

For more information, see [ibm_is_snapshot_clones](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_snapshot_clones){: external}.

### Listing details of a fast restore clone with Terraform
{: #snapshots-view-zonal-clones-terraform}

Import the details of a snapshot's fast restore clone in a zone as a read-only data source.

```terraform
data "ibm_is_snapshot_clone" "ds_snapshotclone" {
  snapshot = "r138-e6664842-b370-496a-9ae7-da3fb647707c"
  zone     = "eu-de-1"
}
```
{: codenblock}

For more information, see [ibm_is_snapshot_clone](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_snapshot_clone){: external}.

### Listing all consistency groups with Terraform
{: #snapshots-vpc-view-consistency-groups-terraform}

Import the details of a collection of consistency groups as a read-only data source.

```terraform
data "ibm_is_snapshot_consistency_groups" "example" {
}
```
{: codeblock}

For more information, see [ibm_is_consistency_groups](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_consistency_groups){: external}.


### Listing details of a consistency group with Terraform
{: #snapshots-vpc-view-consistency-group_details-terraform}

Import the details of a snapshot consistency group as a read-only data source. You can specify the consistency group by either the ID or the name of the consistency group.

```terraform
data "ibm_is_snapshot_consistency_group" "example" {
  identifier = ibm_is_snapshot_consistency_group.is_snapshot_consistency_group.id
}
```
{: codeblock}

```terraform
data "ibm_is_snapshot_consistency_group" "example" {
  name = "my-data-consistency-group"
}
```
{: codeblock}

For more information, see [ibm_is_consistency_group](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_consistency_group){: external}.

## Next steps
{: #snapshots_vpc_view_next_steps}

You can modify or delete snapshots and restore a volume from a snapshot.

* [Modify or delete snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage).
* [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).
