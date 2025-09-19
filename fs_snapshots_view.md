---

copyright:
  years: 2024, 2025
lastupdated: "2025-09-19"

keywords: view snapshots, view snapshot, viewing snapshots, see snapshots, File storage snapshots

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing {{site.data.keyword.filestorage_vpc_short}} snapshots
{: #fs-snapshots-view}

You can view a list of all snapshots, and drill down to see information about a particular snapshot. Choose the UI, CLI, API, or Terraform to retrieve this information.
{: shortdesc}

## Listing snapshots in the console
{: #fs-snapshots-view-ui}
{: ui}

In the console, you can view a list of all snapshots that belong to a file share on the Details page of the share.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.

1. The file shares are listed for a specific region. If you want to see resources in another region, click the arrow to expand the list and select a different region. By default, the newest shares are displayed at the beginning of the list.

1. Select the file share that you want to view, and click the **Snapshots** tab. 

   The Snapshot summary section contains the following information:

   | Field | Description |
   |-------|-------------|
   | Snapshot count  | The name that you provided when you created the snapshot. Click the name of the snapshot to see its [details](#fs-snapshots-view-snapshot-ui). |
   | Size of changed data in all snapshots | The first snapshot is a full copy of your file share. Subsequent snapshots capture the changes that occurred since the last snapshot was taken. This field shows the combined total of capacity that is used by the snapshots.|
   | Replication role | This role matches the replication role of the source share. |
   {: caption="Summary of the Snapshots tab." caption-side="bottom"}

   The Snapshots section contains the list of all the snapshots that were taken of this source share.

   | Field | Description |
   |-------|-------------|
   | Name  | The unique name of the snapshot that is linked to the snapshot details panel.  |
   | Size of the source share | The size of the source share when the snapshot was taken. If you restore a file share by using this snapshot, the size in this field indicates the size of file share that is going to be created. |
   | Status | For example, `stable`.  |
    {: caption="Summary of the Snapshots tab." caption-side="bottom"}

   By clicking the Actions icon ![Actions icon](../icons/action-menu-icon.svg "Actions"), you can display a menu of context-specific actions.
   - [Delete](/docs/vpc?topic=vpc-fs-snapshots-manage#fs-snapshots-delete-snapshot-ui). 
   - [Restore](/docs/vpc?topic=vpc-fs-snapshots-restore).

   The Actions menu is dynamic. If the share is a replica share, only the Restore action is displayed. If the share is a source share or has no replica, all 3 actions are available.
   {: note}

### Viewing snapshot details in the console
{: #fs-snapshots-view-snapshot-ui}

To see detailed information about a snapshot, locate the snapshot on the File share's details page. Then, click the name of a snapshot. The Snapshot details side panel displays information such as the name of the snapshot, its CRN and ID, and attached user and access management tags.

Although you can't create a snapshot on a replica share, the snapshots of the source share are auto-generated on the replica at the next scheduled replication sync. These replicated snapshots are created by the file service. They do not inherit the names or the tags from the original snapshots. However, they have the same fingerprint values with the source snapshots.
{: note}

The following table describes the information that can be viewed on the Snapshot details panel.

| Field | Description |
|-------|-------------|
| Name  | The name of the snapshot, which you can change by clicking the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit"). For more information, see [Naming snapshots](/docs/vpc?topic=vpc-fs-snapshots-planning&interface=ui#fs-snapshots-naming). |
| ID | Copiable UUID of the snapshot. |
| CRN | Copiable CRN of the snapshot. |
| Resource group | Resource group defined when you set up your VPC. |
| Status | For example, `stable`. | 
| Location | Same as the source file share's location.  |
| Created date | Date and time that the snapshot resource creation process started. |
| Size of source file share | The size of the share when the snapshot was taken in GBs. |
| Source file share | Source share from which the first snapshot was taken. Click the link for share details. |
| Encryption | Provider-managed or customer-managed encryption. For customer-managed encryption, the KMS instance, root key name, and root key ID are shown. |
| Fingerprint | The reference ID that you can use to locate the snapshot in the `.snapshot` folder. | 
| User tags | The tags that you added. This field is empty when you look at snapshots on a replica share. |
| Created by | It shows whether the snapshot was created by the user or a [backup policy](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-concepts). The field shows _replication_ when snapshots are viewed on a replica file share.|
{: caption="Snapshot details" caption-side="bottom"}

By clicking the Actions icon ![Actions icon](../icons/action-menu-icon.svg "Actions"), you can display a menu of context-specific actions.
   - [Rename](/docs/vpc?topic=vpc-fs-snapshots-manage&interface=ui#fs-snapshots-delete-snapshot-ui).
   - [Delete](/docs/vpc?topic=vpc-fs-snapshots-manage#fs-snapshots-delete-snapshot-ui). 
   - [Restore](/docs/vpc?topic=vpc-fs-snapshots-restore).

   The Actions menu is dynamic. If the share is a replica share, only the Restore action is displayed. If the share is a source share or has no replica, all 3 actions are available.
   {: note}
  
## Viewing snapshots from the CLI
{: #fs-snapshots-view-cli}
{: cli}

You can use the CLI to list all snapshots, all snapshots for a share, and details about a particular snapshot.

Although you can't create a snapshot on a replica share, the snapshots of the source share are auto-generated on the replica at the next scheduled replication sync. These replicated snapshots are created by the file service. They do not inherit the names or the tags from the original snapshots. However, they have the same fingerprint values with the source snapshots.
{: note}

### Before you begin
{: #fs-snapshots-view-cli-requirement}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

Log in to {{site.data.keyword.cloud}}.

```sh
ibmcloud login --sso -a cloud.ibm.com
```
{: pre}

This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

### Viewing all snapshots in the region from the CLI
{: #fs-snapshots-view-all-snapshots-cli}

Run the `ibmcloud is share-snapshots` command to see all the snapshots that are available in the zone.

```sh
ibmcloud is share-snapshots [--share SHARE] [--backup-policy-plan BACKUP_POLICY_PLAN [--backup-policy BACKUP_POLICY]] [--output JSON] [-q, --quiet]
```
{: pre}

```sh
ibmcloud is share-snapshots
```
{: pre}

```sh
Listing share snapshots in all resource groups and region au-syd under account Test Account as user test.user@ibm.com...
ID                                          Name                                Created at                  Fingerprint                            Minimum Size   Lifecycle state   Zone       Status      User Tags   
r026-c8fd81f8-3437-424e-8fc0-f9dd2f4766d3   demo-bkp-plan-2-02ae167865c0-44e8   2024-12-09T16:55:13+05:30   15b9509d-b6f8-461b-987a-2333be203e0e   40             stable            au-syd-1   available   dev:tags   
r026-9997158e-daa6-409d-8e51-b2bbbc23c5d2   demo-bkp-plan-2-29e97584db07-4e86   2024-12-09T16:55:12+05:30   0b9c1e17-d872-4858-b478-a4d46cfd3dfe   40             stable            au-syd-1   available   dev:tags   
r026-d04eb5f7-4159-4fa0-bfdd-83deca3b73b0   demo-bkp-plan-2-2b8c5b1bcadc-496f   2024-12-09T18:55:14+05:30   622a0e1f-7a48-4dce-adf0-c54adb4605c3   40             stable            au-syd-1   available   dev:tags   
r026-9289ba15-a922-4b75-9650-7afc9471e9d9   demo-bkp-plan-2-47a13671bf22-47e0   2024-12-09T17:55:11+05:30   9a433610-0134-4162-8af3-d9358af832b5   40             stable            au-syd-1   available   dev:tags   
r026-bee1b285-adc3-43e6-b8dd-5645efbe5732   demo-bkp-plan-2-4a90deb0778a-4f1a   2024-12-09T17:55:11+05:30   f8ec608d-9fbc-48cf-9018-01b1a8711449   40             stable            au-syd-1   available   dev:tags   
```
{: screen}

For more information about available command options, see [`ibmcloud is share-snapshots`](/docs/vpc?topic=vpc-vpc-reference#share-snapshots-list).

#### Viewing all snapshots of a share from the CLI
{: #fs-snapshots-view-all-share-snapshots-cli}

Run the `ibmcloud is share-snapshots` command and specify the share name or ID to filter the snapshot results. The following example shows the snapshots that were taken of the share `r026-734c173e-044f-4d09-a729-950364ea9900`.

```sh
ibmcloud is share-snapshots r026-734c173e-044f-4d09-a729-950364ea9900
```
{: pre}

```sh
Listing share snapshots in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
ID                                         Name                    Created at               Fingerprint                          Minimum size Lifecycle state Zone      Status    User Tags
r138-4463eb2c-4913-43b1-b9bf-62a94f74c146  my-first-share-snapshot 024-12-18T20:15:43+00:00 7abc3aef-c2bc-4f65-a296-2928e534d498 40           stable          us-south-1 Available env:test
```
{: screen}

For more information about available command options, see [`ibmcloud is share-snapshots`](/docs/vpc?topic=vpc-vpc-reference#share-snapshots-list).

#### Viewing all snapshots that were created by a backup policy from the CLI
{: #fs-snapshots-view-all-backup-policy-snapshots-cli}

Run the `ibmcloud is share-snapshots` command and specify the backup policy plan. You can specify either the ID or the name of the backup policy plan. If you want to use the name of the backup policy plan, you must also specify the backup policy with its name or ID.

```sh
ibmcloud is share-snapshots [--backup-policy-plan BACKUP_POLICY_PLAN] [--backup-policy BACKUP_POLICY]
```
{: pre}

The following example shows the snapshots that were taken by the backup policy plan with the ID `r006-158e0d66-338e-4501-b756-be0732677da8`.

```sh
ibmcloud is share-snapshots --backup-policy-plan r006-158e0d66-338e-4501-b756-be0732677da8 --share my-file-share
```
{: pre}

```sh
Listing share snapshots of share my-file-share under account Test Account as user test.user@ibm.com...
ID                                          Name                                Created at                  Fingerprint                            Minimum Size   LifeCycle State   Zone         Status      User Tags   
r006-b4d12d66-18de-4660-9fe7-12e667d5ed5b   demo-bkp-plan-1-d619b2fb4a68-4602   2024-11-19T11:33:27+05:30   643ce8da-bbbd-4515-b988-701aea2667df   40             stable            us-south-1   available   dev:tags   
r006-59d25cac-92cd-4076-b988-e5d6746a848f   demo-bkp-plan-1-e4406ae16e8f-4cb0   2024-11-19T12:32:23+05:30   202a5bd4-a1e0-4d89-914c-3574f45fc725   40             stable            us-south-1   available   dev:tags   
r006-41785c6b-5d48-4dc1-89b7-21a5b2ada21e   demo-bkp-plan-1-efc6f81b8505-4782   2024-11-19T10:32:19+05:30   acd57e05-ffe2-4eed-8e78-40b5000c53d3   40             stable            us-south-1   available   dev:tags 
```
{: screen}

For more information about available command options, see [`ibmcloud is share-snapshots`](/docs/vpc?topic=vpc-vpc-reference#share-snapshots-list).

### Viewing details of a snapshot from the CLI
{: #fs-snapshots-view-details-cli}

Run the `ibmcloud is share-snapshot` command with the snapshot ID, or the snapshot name and the share name or ID. Snapshot IDs are unique across the account. However, snapshot names are only unique at the snapshot level.

```sh
ibmcloud is share-snapshot SHARE_SNAPSHOT [--output JSON] [-q, --quiet]
```
{: pre}

The following example shows the details of a snapshot.

```sh
ibmcloud is share-snapshot my-file-share r006-6c760e3e-33fc-41a4-b896-8a2c229ddccd
```
{: pre}

```sh
Getting share snapshot ID r006-6c760e3e-33fc-41a4-b896-8a2c229ddccd for share ID my-file-share under account Test Account as user test.user@ibm.com...

ID                   r006-6c760e3e-33fc-41a4-b896-8a2c229ddccd   
Name                 my-first-share-snapshot
Fingerprint          7f29bdd5-bc67-4e67-9b24-638a73be4742   
Backup Policy Plan   -   
Status               available   
Created at           2025-03-10T19:17:55+00:00   
Captured At          2025-03-10T19:17:58+00:00   
CRN                  crn:v1:bluemix:public:is:us-south-2:a/a1234567::share-snapshot:r006-d14e4b29-cb73-4886-a267-b2cd58d67641/r006-6c760e3e-33fc-41a4-b896-8a2c229ddccd   
LifeCycle Reasons    Code   Message   More Info      
                     -      -               
                        
LifeCycle State      stable   
Href                 https://us-south.iaas.cloud.ibm.com/v1/shares/r006-d14e4b29-cb73-4886-a267-b2cd58d67641/snapshots/r006-6c760e3e-33fc-41a4-b896-8a2c229ddccd   
Minimum Size         10   
Zone                 ID   Name      
                          us-south-2      
                        
Resource group       ID                                 Name      
                     6edefe513d934fdd872e78ee6a8e73ef   defaults      
                        
Status reasons       Status code   Status message      
                     -             -      
                        
Resource type        share_snapshot

```
{: screen}

For more information about available command options, see [`ibmcloud is share-snapshot`](/docs/vpc?topic=vpc-vpc-reference#share-snapshot-view).

## Listing snapshots with the API
{: #fs-snapshots-view-all-api}
{: api}

You can list your snapshots that belong to the same share programmatically by using the VPC API. 

Although you can't create a snapshot on a replica share, the snapshots of the source share are auto-generated on the replica at the next scheduled replication sync. These replicated snapshots are created by the file service. They do not inherit the names or the tags from the original snapshots. However, they have the same fingerprint values with the source snapshots.
{: note}

### Listing all snapshots with the API
{: #fs-snapshots-list-all-api}

You can programmatically list all snapshots of your shares by calling the `/shares/{share-id}/snapshots` method in the [VPC API](/apidocs/vpc/latest#list-snapshots){: external} as shown in the following sample request. By default, the list shows the most recent snapshots first, followed by older snapshots in descending order.

```sh
curl -X GET \
  "$vpc_api_endpoint/v1/shares/{share-id}/snapshots?version=2024-12-10&generation=2" \
  -H "Authorization: $iam_token"
```
{: pre}

You can filter the list by using the backup policy plan ID to display the snapshots that were taken by a backup policy. For example, the following call filters the list to show snapshots that were created by a specific backup plan and limits the results to five per page.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/shares/r006-0fe9e5d8-0a4d-4818-96ec-e99708644a58/snapshots?version=2024-12-10&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "limit": 5,
      "backup_policy_plan":
        "id": "r006-b470ae68-8325-4a9c-9051-1ade8c3806c3"
      }'
   ```
   {: pre}

A successful response looks like the following example.

```json
{
  "limit": 5,
  "snapshots": [
    {
      "captured_at": "2024-12-10T01:21:12.000Z",
      "created_at": "2024-12-10T01:59:46.000Z",
      "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::share-snapshot:r006-0fe9e5d8-0a4d-4818-96ec-e99708644a58/r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4",
      "fingerprint": "7abc3aef-c2bc-4f65-a296-2928e534d498",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-0fe9e5d8-0a4d-4818-96ec-e99708644a58/snapshots/r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4",
      "id": "r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4",
      "lifecycle_state": "stable",
      "minimum_size": 10,
      "name": "my-first-share-snapshot",
      "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/fee82deba12e4c0fb69c3b09d1f12345",
        "id": "fee82deba12e4c0fb69c3b09d1f12345",
        "name": "Default"
      },
      "resource_type": "share_snapshot",
      "status": "available",
      "status_reasons": [],
      "user_tags": [],
      "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
        "name": "us-south-1"
      }
    },
    {
      "captured_at": "2024-12-10T02:21:12.000Z",
      "created_at": "2024-12-10T02:59:46.000Z",
      "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::share-snapshot:r006-1fe9e5d8-0a4d-4818-96ec-e99708644a58/r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4",
      "fingerprint": "9bbc3aef-c2bc-4f65-a296-2928e534d498",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-1fe9e5d8-0a4d-4818-96ec-e99708644a58/snapshots/r006-d13ee54f-baa4-40d3-b35c-b9ec163972b4",
      "id": "r006-d13ee54f-baa4-40d3-b35c-b9ec163972b4",
      "lifecycle_state": "stable",
      "minimum_size": 10,
      "name": "my-second-share-snapshot",
      "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/fee82deba12e4c0fb69c3b09d1f12345",
        "id": "fee82deba12e4c0fb69c3b09d1f12345",
        "name": "Default"
      },
      "resource_type": "share_snapshot",
      "status": "available",
      "status_reasons": [],
      "user_tags": [],
      "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
        "name": "us-south-1"
      }
    }
  ],
  "total_count": 1
}
```
{: codeblock}

### Listing details of a snapshot with the API
{: #fs-snapshots-view-api}

You can programmatically retrieve the details of a single snapshot by calling the `/shares/{share-id}/snapshots/{snapshot-id}` method in the [VPC API](/apidocs/vpc/latest#get-snapshot){: external} and specifying the share ID and the snapshot ID as shown in the following sample request.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/shares/r006-1fe9e5d8-0a4d-4818-96ec-e99708644a58/snapshots/r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4?version=2024-12-10&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

A successful response looks like the following example.

```json
    {
      "captured_at": "2024-12-10T01:21:12.000Z",
      "created_at": "2024-12-10T01:59:46.000Z",
      "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::share-snapshot:r006-0fe9e5d8-0a4d-4818-96ec-e99708644a58/r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4",
      "fingerprint": "7abc3aef-c2bc-4f65-a296-2928e534d498",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-0fe9e5d8-0a4d-4818-96ec-e99708644a58/snapshots/r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4",
      "id": "r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4",
      "lifecycle_state": "stable",
      "minimum_size": 10,
      "name": "my-share-snapshot",
      "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/fee82deba12e4c0fb69c3b09d1f12345",
        "id": "fee82deba12e4c0fb69c3b09d1f12345",
        "name": "Default"
      },
      "resource_type": "share_snapshot",
      "status": "available",
      "status_reasons": [],
      "user_tags": [],
      "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
        "name": "us-south-1"
      }
    },
```
{: codeblock}

## Viewing snapshots with Terraform
{: #fs-snapshots-view-terraform}
{: terraform}

You can use Terraform to view your snapshots. To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

VPC infrastructure services use a specific regional endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the appropriate region in the provider block in the `provider.tf` file.

See the following example of targeting a region other than the default `us-south`.

```terraform
provider "ibm" {
  region = "eu-de"
}
```
{: screen}

### Listing all snapshots of a file share with Terraform
{: #fs-snapshots-view-all-terraform}

Import the details of a collection of snapshots as a read-only data source. You can filter the collection of snapshots by `source_share`, `resource_group`, `name`, and so on.

```terraform
data "ibm_is_snapshots" "example" {
}
```
{: codeblock}

For more information, see [ibm_is_snapshots](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_snapshots){: external}.

### Listing details of a snapshot with Terraform
{: #fs-snapshots-view-snap-terraform}

Import the details of a snapshot as a read-only data source. You can specify either the snapshot ID or the snapshot name.

```terraform
resource "ibm_is_share_snapshot" "example" {
  name = "my-example-share-snapshot"
  share = ibm_is_share.example.id
  tags = ["my-example-share-snapshot-tag"]
}
data "ibm_is_share_snapshot" "example" {
    share_snapshot = ibm_is_share_snapshot.is_share_snapshot_instance.is_share_snapshot_id
    share = ibm_is_share.example.id
}
```
{: codeblock}

For more information, see [ibm_is_snapshot](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_snapshot){: external}.

## Next steps
{: #fs-snapshots-view-next-steps}

You can modify or delete snapshots and restore a share from a snapshot.

* [Modify or delete snapshots](/docs/vpc?topic=vpc-fs-snapshots-manage).
* [Restore a share from a snapshot](/docs/vpc?topic=vpc-fs-snapshots-restore).
