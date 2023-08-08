---

copyright:
  years: 2022, 2023
lastupdated: "2023-08-08"

keywords: file share, file storage, increase capacity, expand capacity, expand share size, file share size

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Expanding file share capacity
{: #file-storage-expand-capacity}

For {{site.data.keyword.filestorage_vpc_short}} file shares, you can increase the file share size from its original capacity in GB increments up to 32,000 GB capacity, depending on your file share profile. This process doesn't require you to perform manual steps. For example, you don't need to migrate your data to a larger file share. The expansion operation causes no outage or lack of access to your storage.
{: shortdesc}

Billing for the file share is automatically updated to add the pro-rated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

## Expandable file share concepts
{: #expandable-share-concepts}

You can increase the capacity of the file share. The file share size cannot be less than the current file share size.

Capacity can be increased for file shares that are in a `stable` state. Your user authorization is verified before the file share is expanded. You can use the [UI](#expand-vpc-share-ui), [CLI](#expand-vpc-share-cli), or [API](#expand-vpc-share-api) to increase file share capacity. You can expand a file share multiple times up to its [maximum capacity limit](#exp-share-capacity-IOPS-limitations). After you expanded the file share, you can't reduce the capacity.

Expanded capacity is determined by the maximum that is allowed by the file share profile. File shares that are created from a [Custom](/docs/vpc?topic=vpc-file-storage-profiles#custom) profile or a [dp2](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile) profile can be expanded within the allowable IOPS range for that file share size.

File shares that are created from an [IOPS tier profile](/docs/vpc?topic=vpc-file-storage-profiles#fs-tiers) can be expanded to the maximum size for its IOPS tier:

* A general-purpose, 3 IOPS/GB profile can be expanded up to 32,000 GB.
* A 5 IOPS/GB profile can be expanded up to 9,600 GB.
* A 10 IOPS/GB profile can be expanded up to 4,800 GB.

IOPS is automatically adjusted for tiered file share profiles, based on the size of the file share. For example, if you expand a share with a 5 IOPS/GB profile from 250 GB to 1,000 GB, it has a maximum IOPS of 5,000 IOPS (1,000 GB capacity _x_ 5 IOPS). Because a 5 IOPS/GB file share can potentially expand to 9,600 GB, the max IOPS is adjusted to 48,000 IOPS. The capacity and the IOPS are immediately changed and you don't to restart the instance.

You can monitor the progress of your file share expansion from the UI or CLI. You can also use the [Activity Tracker](/docs/vpc?topic=vpc-at-events) to verify that the file share was expanded. After a file share is expanded, you can't reduce capacity.

## Requirements
{: #expandable-share-prereqs}

The file share must be in a `stable` state before you can request that the capacity is increased.

## Limitations
{: #expandable-share-limitations}

The following limitations apply to this release.

* File shares can expand, with the following restrictions:
    * If the file share was created with a [Tiered IOPS profile](/docs/vpc?topic=vpc-file-storage-profiles#fs-tiers) that limits capacity to less than 32,000 GB, it can expand only to the allowed capacity for that tier.
    * If the file share was created with a [Custom IOPS profile](/docs/vpc?topic=vpc-file-storage-profiles#custom) in an IOPS range that doesn't allow expanding to 16,000 GB (custom GB max), it can expand only to its maximum capacity for the specified custom range.
    * If the file share was created with a [dp2 profile](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile) in an IOPS range that doesn't allow expanding to 32,000 GB (dp2 GB max), it can expand only to its maximum capacity for the specified dp2 range.
    * File shares can expand multiple times until maximum capacity is reached.
* IOPS increase to the maximum allowed by the profile.
* You can't independently modify IOPS for a file share that was created from an IOPS tier profile. IOPS is adjusted when you expand capacity.
* When you expand a file share that was created from a custom or dp2 profile, the capacity is increased, but the IOPS remains the same unless you choose to [adjust IOPS](/docs/vpc?topic=vpc-adjusting-share-iops).
* Maximum IOPS for a file share is capped at 48,000 IOPS if it is accessed by a single host. For a file share that is accessed by multiple hosts, IOPS can reach up to 96,000 IOPS.
* After a file share is expanded, you can't reduce its size.

## Expanding file share capacity in the UI
{: #expand-vpc-shares-ui}
{: ui}

Follow these steps for expanding file share capacity in the UI:

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File Shares**.

2. The File Shares for VPC list page shows all file shares that were created in that zone. Click the name of the file share to get to the details page.

3. On the file shares details page, expand the **Actions** menu.

4. Select **Expand file share**. The Expand file share size panel shows the current size of the file share (in GBs), and the profile to which it is associated. Alternatively, you can also click the edit icon next to the file share size information.

5. Enter a larger file share size, based on the maximum allowed for the [file share profile](/docs/vpc?topic=vpc-file-storage-profiles). The default is the current size + 1 GB.

6. Review the estimated monthly order summary for your geography and new pricing.

7. If you're satisfied, click **Save and continue**.

Your new file storage allocation is available in a few minutes. If your requirements change, you can increase capacity again after the file share size is increased and when it's in `stable` state.

**Note**: You can't change the file share to a smaller size after you expand its capacity.

## Expanding file shares from the CLI
{: #expand-vpc-shares-cli}
{: cli}

From the CLI, use the `share-update` command with the `--size` option to indicate the new size of the file share in GBs.

```sh
ibmcloud is share-update SHARE_ID --size SIZE_GB
```
{: pre}

This example expands the capacity of a file share that is created from a 5 IOPS/GB tier profile to the maximum size for that profile. To get the maximum IOPS of 48,000, you would need to restart the instance.

```sh
$ ibmcloud is share-update 933c8781-f7f5-4a8f-8a2d-3bfc711788ee --size 9600
Updating share 933c8781-f7f5-4a8f-8a2d-3bfc711788ee under account MyAccount 01 as user user1@mycompany.com...

ID                0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name              demo-share-update
CRN               crn:v1:bluemix:public:is:us-south-3:a/c16d1edde3fd4a71a0130aed371405a0::share:0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Lifecycle State   stable
Zone              us-south-2
Profile           tier-5iops
Size              9600
IOPS              48000
Encryption        provider_managed
Mount targets     ID                                          Name                 VPC ID                                      VPC Name
                  04165941-44c5-4e20-acde-a11c922e3415   demo-share-target    13b34e2f-5038-4a95-8fe0-966910b41b4d   -
                  fea02493-5536-40b8-8597-e0c14e1daf4f   demo-share-target2   2a5e858a-3549-4bf2-bd2a-c302bc768759   -

Resource Group    ID                                 Name
                  875623bcde2b4ebda924d32640908845   Default

Created           2023-08-08T02:15:52-06:00
```
{: screen}

## Expanding file share capacity with the API
{: #expand-vpc-share-api}
{: api}

You can expand existing file shares by calling the VPC API. Make a `PATCH /shares/{id}` request and specify the ID of the file share for which you want to increase the size.

This request example expands a file share with a capacity of 50 GB to 2500 GB for a 5 IOPS/GB profile.

```sh
curl -X PATCH \
 "$vpc_api_endpoint/v1/shares/$share_id?version=2023-08-08&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "size": 2500
    }'
```
{: codeblock}

The file share status shows `updating` while the capacity is increased. The current capacity is shown while it's updating.

```json
{
  "created_at": "2023-08-08T23:31:59Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "$vpc_api_endpoint/v1/shares/199d78ec-b971-4a5c-a904-8f37ae710c63",
  "id": "199d78ec-b971-4a5c-a904-8f37ae710c63",
  "iops": 12500,
  "lifecycle_state": "updating",
  "name": "share-name1",
    .
    .
	"size": 2500,
    .
    .
    .
}
```
{: codeblock}

When the file share expansion completes, the new value displays, and the status is `stable`.

```json
{
  "created_at": "2023-08-08T23:31:59Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "$vpc_api_endpoint/v1/shares/199d78ec-b971-4a5c-a904-8f37ae710c63",
  "id": "199d78ec-b971-4a5c-a904-8f37ae710c63",
  "iops": 12500,
  "lifecycle_state": "stable",
  "name": "share-name1",
  "profile": {
    "href": "$vpc_api_endpoint/v1/share/profiles/tier5iops",
    "name": "tier-5iops",
    "resource_type": "share_profile"
  },
  "resource_group": {
    "crn": "crn:[...]",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/875623bcde2b4ebda924d32640908845",
    "id": "875623bcde2b4ebda924d32640908845",
    "name": "Default"
  },
  "resource_type": "share",
  "size": 2500,
  .
  .
  .
}
```
{: codeblock}

## Expanding file shares with Terraform
{: #expand-vpc-shares-terraform}
{: terraform}

To increase the capacity of a file share, use the `ibm_is_share` resource. When applied, the following example updates the share capacity to 300 GB.

```terraform
resource "ibm_is_share" "example" {
  name    = "my-new-share"
  size    = 300
  iops    = 5000
  profile = "dp2"
  zone    = "us-south-2"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.

## Next steps
{: #next-step-expandable-shares}

Mount and use your file shares:

* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-vpc-mount-RHEL)
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos)
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-vpc-mount-ubuntu)
