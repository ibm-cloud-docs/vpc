---

copyright:
  years: 2021, 2023
lastupdated: "2023-02-17"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# Viewing snapshots
{: #snapshots-vpc-view}

You can view a list of all snapshots and drill down to see information about a particular snapshot. Choose the UI, CLI, or API to retrieve this information.
{: shortdesc}

## List snapshots in the UI
{: #snapshots-vpc-view-ui}
{: ui}

### List all snapshots in the UI
{: #snapshots-vpc-view-list-ui}

View a list of all snapshots that you created, with the most recent one at the beginning of the list. You can filter the list to view specific snapshots. Follow these steps.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**. By default, the newest snapshots display at the beginning of the list.

   You can also view a list of snapshots for a volume. For more information, see [View all snapshots that were created from the {{site.data.keyword.block_storage_is_short}}e volume](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#view-snapshots-for-volume).
   {: tip}

1. View snapshots in your account region. If you created snapshots in a different region, expand the list and select the region.
1. As your list of snapshots grows, use the filter to indicate the number of snapshots to display per page. Use the page navigation arrows to move forward and back through the list.

Table 1 describes the information for all snapshots in the list of snapshots.

| Field | Description |
|-------|-------------|
| Name  | It shows the name that you provided when you created the snapshot. Click the name of the snapshot to see its [details](#snapshots-vpc-view-snapshot-ui). |
| Status | It shows the status of the snapshot, depending on whether it's usable (_active_ status), unusable, or being created. For more information, see [Snapshot statuses](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-status). |
| Encryption |It shows [IBM-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-provider-managed-encryption) or [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-customer-managed-encryption). The encryption is inherited from the source volume. |
| Size | It shows the size of the snapshot in GBs. The size is inherited from the source volume. |
| Source volume | It shows the boot or data volume from which the snapshot was created. Click the name of the volume to see its [details](/docs/vpc?topic=vpc-viewing-block-storage). |
| Fast restore status | It shows enabled, pending, or disabled. |
| Requested | It shows the date and time when the snapshot was requested. |
| Created by | It shows whether the snapshot was created by the user or a [backup policy](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-concepts). |
| Actions menu | Click the actions menu icon to display a menu of context-specific actions. |
| | Copy the snapshot ID. |
| | Copy the Cloud Resource Name (CRN). |
| | [Create volume](/docs/vpc?topic=vpc-snapshots-vpc-restore#snapshots-vpc-restore-ui). |
| | [Edit fast restore](https://test.cloud.ibm.com/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-edit-fast-restore) |
| | [Delete the snapshot](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-delete-snapshot-ui). |
| | [Delete all snapshots for a volume](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-delete-all-ui). |
{: caption="Table 1. List of all snapshots" caption-side="bottom"}

You can also list all snapshots that were created from a {{site.data.keyword.block_storage_is_short}} volume from the volume details page. For more information, see [List all snapshots for a volume](/docs/vpc?topic=vpc-view-snapshots-for-volume).

### View snapshot details in the UI
{: #snapshots-vpc-view-snapshot-ui}

To see details about a snapshot:

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**.

2. Click the name of a snapshot. The snapshot details side panel shows the information that is described in Table 2.

| Field | Description |
|-------|-------------|
| Status | The status of the snapshot, such as _Stable_. For a list of snapshot statuses, see [Snapshot statuses](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-status). |
| Tags | User tags are inherited from the source volume when the snapshot is created. The tags appear near the snapshot name. Click the link to add user or access management tags. When you restore a volume from this snapshot, the new volume also inherits the tags. When the user tags match tags for target resources in a backup policy, the new volume is automatically backed up based on a backup plan schedule. For more information about creating backups, see [Creating a backup policy](/docs/vpc?topic=vpc-backup-about). For more information about working with tags, see [Working with tags](/docs/account?topic=account-tag). |
| Name  | The name of the snapshot, which you can change by clicking the pencil icon. For more information, see [Change the snapshot name](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-rename). |
| Resource group | Resource group defined when you set up your VPC. |
| Snapshot ID | Copiable GUID of the snapshot. |
| CRN | Copiable CRN of the snapshot. |
| Created | Date and time that the snapshot resource creation process started. |
| Created by | By the user, a manually created snapshot, or automatically by a [backup policy](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-concepts). |
| Captured | The date and time that this snapshot was captured, that is, the volume data was snapshotted. If absent, the snapshot wasn't captured or the snapshot was created before this feature was introduced (January 2022). |
| Region | Region of the source volume and snapshot. |
| Size| Size in GBs of the snapshot, inherited from the source volume. |
| Source volume | Source volume from which the first snapshot was taken. Click the link for volume details. If the volume was deleted, the name appears without a link. |
| Encryption | Provider-managed or customer-managed encryption. For customer-managed encryption, the KMS instance, root key name, and root key ID are shown. |
| Fast restore panel | Use [fast restore](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots_vpc_fast_restore) to create a volume from this snapshot that is fully provisioned. Click **edit** to [enable fast restore](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-edit-fast-restore) in a zone. |
| Actions menu | [Create a volume from the snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore#snapshots-restore-create-vol-ui), [Edit fast restore](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-edit-fast-restore), or delete a snapshot. |
{: caption="Table 2. Snapshot details" caption-side="bottom"}

## View snapshots from the CLI
{: #snapshots-vpc-view-cli}
{: cli}

You can use the CLI to list all snapshots, all snapshots for a volume, and details about a particular snapshot.

### Before you begin
{: #snapshots-vpc-view-cli-requirement}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

2. Select the current generation of VPC. 
   ```sh
   ibmcloud is target --gen 2
   ```
   {: pre}

3. Set the `IBMCLOUD_IS_FEATURE_SNAPSHOT` environment variable to `true`.
   ```zsh
   export IBMCLOUD_IS_FEATURE_SNAPSHOT=true
   ```
   {: pre}

### View all snapshots of an account in a region from the CLI
{: #snapshots-vpc-view-all-cli}

Run the `snapshots` command to list all snapshots.

```zsh
ibmcloud is snapshots [--json]
```
{: pre}

The following example provides a list of all the snapshots in all the resource groups in the `eu-de` region that were created by the users of the Test Account.

```sh
cloudshell:~$ ibmcloud is snapshots [--json]
Listing snapshots in all resource groups and region eu-de under account Test Account as user test.user@ibm.com...
ID                                          Name                                           Status     Source volume                               Bootable   Resource group   Created   
r138-7cac80af-63bb-4a1b-83dd-5f6d550a5db7   bear-peroxide-viewable-oxidant                 stable     r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      test-snap        2023-02-17T18:49:48+00:00    
r138-f95ef79e-5452-4a2a-b674-923c8ab84923   csi-plan-6kraq5zm-x7ss0u3h-3dbc4108a6b6-4356   stable     r010-a32b3fc7-8cd1-4b6e-ab97-f9837024fc03   false      defaults         2023-01-18T15:35:19+00:00   
r138-9ad969ee-64b2-409a-b1e8-a3093446ad4e   csi-plan-nmplujx4-67413149ce61-4740            unusable   r010-63524a21-889d-4c44-95d1-4d2332394975   false      defaults         2023-01-18T13:40:28+00:00   
r138-0f005da7-026c-453a-aa9b-2bc6f5cc6961   demo2-8be140e9561c-4381                        stable     r010-5a558256-ca83-4534-973f-f7a3b80aebe7   true       defaults         2023-02-01T00:41:23+00:00   
r138-f7030231-bc5b-4672-8454-2f30271578f6   my-hpcs-snapshot                               stable     r010-921c3c6f-6a0a-4914-bdc5-3e8c9167ec30   false      defaults         2022-02-11T16:54:35+00:00   
r138-e6664842-b370-496a-9ae7-da3fb647707c   snappy-snap-snap                               stable     r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      test-snap        2023-02-17T18:53:57+00:00
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshots`](/docs/vpc?topic=vpc-vpc-reference#snapshots).

### View all snapshots of a volume from the CLI
{: #snapshots-vpc-view-cli}

Run the `snapshots` command and specify the volume ID.

```zsh
ibmcloud is snapshots --volume VOLUME-ID [--json]
```
{: pre}

The following example shows all three snapshots that were taken of the volume `r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa`.

```zsh
cloudshell:~$ ibmcloud is snapshots --volume  r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa
Listing snapshots in all resource groups and region eu-de under account Test Account as user test.user@ibm.com...
ID                                          Name                             Status   Source volume                               Bootable   Resource group   Created   
r138-7cac80af-63bb-4a1b-83dd-5f6d550a5db7   bear-peroxide-viewable-oxidant   stable   r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      test-snap        2023-02-17T18:49:48+00:00   
r138-4463eb2c-4913-43b1-b9bf-62a94f74c146   cli-snapshot-test                stable   r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      defaults         2023-02-17T20:15:43+00:00   
r138-e6664842-b370-496a-9ae7-da3fb647707c   snappy-snap-snap                 stable   r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      test-snap        2023-02-17T18:53:57+00:00 
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshots`](/docs/vpc?topic=vpc-vpc-reference#snapshots).

### View details of a snapshot from the CLI
{: #snapshots-vpc-view-details-cli}

Run the `snapshots` command and specify the snapshots ID.

```zsh
ibmcloud is snapshots SNAPSHOT_ID [--json]
```
{: pre}

The following example shows the details of a bootable snapshot in the `us-south` region.

```zsh
cloudshell:~$ ibmcloud is snapshot c2bc3194-0cab-40c4-9434-db9f26218700
Getting snapshot c2bc3194-0cab-40c4-9434-db9f26218700 under account vpc1 as user user@mycompany.com...

ID                     c2bc3194-0cab-40c4-9434-db9f26218700
Name                   my-snapshot
CRN                    crn:v1:bluemix:public:is:us-south:a/7f75c7b025e54bc5635f754b2f888665::snapshot:c2bc3194-0cab-40c4-9434-db9f26218700
Status                 stable
Source volume          ID                                          Name
                       fe027a90-19e7-4cb5-bda3-4c0e35d2bcdf        test5-vol

Progress               -
Bootable               true
Encryption             provider_managed
Encryption key         -
Minimum capacity(GB)   100
Size(GB)               1
Source image           ID                                          Name
                       fb4d7950-8ff4-4d9a-9d9f-3056cb8c36d9        centos-8-2-minimal-amd64-2

Operating system       Name             Vendor   Version                 Family   Architecture   Display name
                       centos-8-amd64   CentOS   8.x - Minimal Install   CentOS   amd64          CentOS 8.x - Minimal Install (amd64)

Resource group         ID                                 Name
                       ef2694fe-d6d1-4136-94c3-0ae315204e6b   Default

Created                2022-01-14T01:53:15+05:30
Captured               2022-01-14T01:53:34+05:30
```
{: screen}

The following example shows the details of a nonbootable snapshot in the `eu-de` region, which also has a fast restore clone in the `eu-de-1` zone.

```sh
cloudshell:~$ ibmcloud is snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146
Getting snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...
                          
ID                     r138-4463eb2c-4913-43b1-b9bf-62a94f74c146   
Name                   cli-snapshot-test   
CRN                    crn:v1:bluemix:public:is:eu-de:a/a10d63fa66daffc9b9b5286ce1533080::snapshot:r138-4463eb2c-4913-43b1-b9bf-62a94f74c146   
Status                 stable   
Clones                 Zone      Available   Created      
                       eu-de-1   true        2023-02-17T20:15:46+00:00      
                          
Source volume          ID                                          Name      
                       r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   block-test1      
                          
Bootable               false   
Encryption             provider_managed   
Encryption key         -   
Minimum capacity(GB)   20   
Size(GB)               1   
Resource group         ID                                 Name      
                       6edefe513d934fdd872e78ee6a8e73ef   defaults      
                          
Created                2023-02-17T20:15:43+00:00   
Captured at            2023-02-17T20:15:44+00:00   
Tags                   env:prod,env:test
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshots`](/docs/vpc?topic=vpc-vpc-reference#snapshots).

### View all fast restore snapshot clones from the CLI
{: #snapshots-view-zonal-clones-cli}

You can list all available fast restore clones of a snapshot by issuing the `ibmcloud is snapshot-cls` command. The following example lists the available fast restore snapshot clones of the snapshot that has the ID of `r138-4463eb2c-4913-43b1-b9bf-62a94f74c146`.

```sh
cloudshell:~$ ibmcloud is snapshot-cls r138-4463eb2c-4913-43b1-b9bf-62a94f74c146
Listing zonal clones of snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...
Zone      Available   Created   
eu-de-1   true        2023-02-17T20:15:46+00:00   
eu-de-3   true        2023-02-17T20:29:21+00:00   
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-cls`](/docs/vpc?topic=vpc-vpc-reference#snapshots-cli).

### View details of a fast restore snapshot clone from the CLI
{: #snapshots-clone-details-cli}

You can run the `ibmcloud is snapshot-cl` command to list the details of a specific fast restore snapshot clone. The following example shows how to list the details of the fast restore snapshot clone of snapshot `r138-4463eb2c-4913-43b1-b9bf-62a94f74c146` that resides in the `eu-de-3` zone. 

```sh
cloudshell:~$ ibmcloud is snapshot-cl r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 eu-de-3
Getting zonal clone eu-de-3 of snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...
               
Zone        eu-de-3   
Available   true   
Created     2023-02-17T20:29:21+00:00   
Href        https://eu-de.iaas.cloud.ibm.com/v1/regions/eu-de/zones/eu-de-3 
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-cl`](/docs/vpc?topic=vpc-vpc-reference#snapshots-cli).

## List snapshots with the API
{: #snapshots-vpc-view-all-api}
{: api}

### List all snapshots with the API
{: #snapshots-vpc-view-all-api}

With the [VPC API](/apidocs/vpc), make a `GET/snapshots` request to list all snapshots of your volumes. By default, the list shows the most recent snapshots first, followed by older snapshots in descending order.

   ```curl
   curl -X GET \
   "$vpc_api_endpoint/v1/snapshots?version=2022-12-16&generation=2" \
   -H "Authorization: $iam_token"
   ```
   {: codeblock}

You can filter the list by using the resource group ID, source volume ID, or source volume CRN, and further filter the results by using the following options:

* Limit - To control the number of snapshots displayed on a page.
* Start - To specify which snapshot (by ID) to start the list.
* Sort - To control the order in which you sort the page, such as by date created or alphabetically by snapshot name.

For more information, see the [VPC API reference](/apidocs/vpc).

For example, the following call filters the list to show snapshots that were created for a single volume and limits the results to five per page.

```curl
curl -X GET \
"$vpc_api_endpoint/v1/snapshots?version=2022-08-16&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "limit": 50,
      "source_volume":
        "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8"
      }
    }'
   ```
   {: pre}

A successful response looks like the following example.

```json
{
    "first": {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots?limit=50"
    },
    "limit": 50,
    "snapshots": [
      {
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
        "user_tags": []
      },
      {
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
        "user_tags": []
      }
    ],
    "total_count": 2
  }
```
{: codeblock}

### List details of a snapshot with the API
{: #snapshots-vpc-view-api}

For details about a single snapshot, use the VPC API to make a `GET/snapshots` call and specify the snapshot ID.

   ```curl
   curl -X GET \
   "$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2022-12-16&generation=2" \
   -H "Authorization: $iam_token"
   ```
   {: pre}

A successful response looks like the following example.

```json
{
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
  "user_tags": []
}
```
{: codeblock}

### View all fast restore snapshot clones with the API
{: #snapshots-view-zonal-clones-api}

Make a `GET /v1/snapshots/{id}/clones` call to list all fast restore snapshot clones.

```curl
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

### View details of a fast restore snapshot clone with the API
{: #snapshots-clone-details-api}

Make a `GET /v1/snapshots/{id}/clones/{zone-name}` call to retrieve a single fast restore snapshot clone specified by ID and zone name.

```curl
curl -X GET \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d/clones/us-south-1?version=2022-12-16&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

A successful response shows the following information about the fast restore snapshot clone:

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

## Next steps
{: #snapshots_vpc_view_next_steps}

* [Modify or delete snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage).
* [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).
