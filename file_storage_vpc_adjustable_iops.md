---

copyright:
  years: 2022, 2023
lastupdated: "2023-07-11"

keywords: file share, file storage, IOPS, performance needs, adjust IOPS

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Adjusting file share IOPS
{: #adjusting-share-iops}

For {{site.data.keyword.filestorage_vpc_short}} file shares, you can increase or decrease IOPS to meet your performance needs. Adjust IOPS by specifying a different IOPS tier profile (IOPS are adjusted within the selected profile), custom profile, or dp2 profile. The process of adjusting the IOPS causes no outage or lack of access to the storage.
{: shortdesc}

Billing for an updated share is automatically updated. The prorated difference of the new price is added to the current billing cycle. The new full amount is then billed in the next billing cycle.

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Frankfurt, London, Madrid, Dallas, Toronto, Washington, Sao Paulo, Sydney, Osaka, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Adjustable IOPS concepts
{: #fs-adj-iops-concepts}

You can adjust IOPS for your file shares to tailor performance to meet your requirements.

For example, you might find that an application scaled such that a lower-tier storage profile is now a performance bottleneck. You can change the performance characteristics of the existing file share by increasing IOPS by selecting a higher IOPS tier (for example, 3 IOPS/GB tier to 5 IOPS/GB tier).

In another scenario, you might want to initially set your file storage at a higher 10 IOPS/GB performance level to expedite a data upload. After the upload completes, you can reset storage to 5 IOPS/GB for normal operations. Also, you might want to increase your file share's IOPS during peak times for your application and decrease IOPS during off-peak hours.

By using this feature, you can:

* Adjust IOPS up or down by selecting a different [IOPS tier](/docs/vpc?topic=vpc-file-storage-profiles#fs-tiers). IOPS is adjusted within the tier. The degree to which IOPS can be increased is determined by the maximum that is allowed by the file share's [IOPS tier profile](/docs/vpc?topic=vpc-file-storage-profiles). You can adjust IOPS for an IOPS tier based on share size or select the next profile that allows for increased performance.

    Maximum IOPS for a file share for a 3 IOPS/GB tier profile is 96,000 IOPS. All other profiles are capped at 48,000 IOPS. For 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share that is accessed by one instance is limited to 48,000 IOPS.
    {: note}

* Adjust IOPS when you're using a [custom IOPS](/docs/vpc?topic=vpc-file-storage-profiles#custom) or [dp2](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile) profile. The IOPS range is based on the file share size. For example, for a file share that's 25 GB, you can increase IOPS anywhere in the range 100-1,000 IOPS with a custom profile. If you later [increase the size of a file share](/docs/vpc?topic=vpc-file-storage-expand-capacity) to the next highest band, you can increase the IOPS again.

* Adjust IOPS from an IOPS tier profile to a custom or dp2 profile. In this case, the file share size restricts the custom or dp2 band that you select. For example, if you're adjusting IOPS for file share with a 3 IOPS/GB profile and it's 12,000 GB, you can adjust the IOPS by using a custom or dp2 profile. It's possible because both support a share size of 12,000 GB. If the file share size was greater than 16,000 GB, the custom profile would not support adjusting IOPS, but the dp2 profile would because it supports share sizes to 32,000 GB. The IOPS tier maximum sizes are 32,000 GB for 3 IOPS/GB, 9,600 GB for 5 IOPS/GB, and 4,800 GB for 10 IOPS/GB. When you move from a tiered profile to a custom or dp2 profile, billing is update based on the specified IOPS.

* Adjust IOPS from a custom or dp2 profile to an IOPS tier profile. Again, you're restricted to which IOPS tier profile you can select by the file share size. When you move from a custom or dp2 profile to any tiered profile, billing is updated, and the IOPS is adjusted according to the profile.

To adjust a file share's IOPS, the file share must be in a _stable_ state. Your user authorization is verified before IOPS is adjusted.

You can use the [UI](#fs-adjust-vpc-iops-ui-file), [CLI](#fs-adjust-vpc-iops-cli-file), or [API](#fs-adjust-vpc-iops-api-file) to adjust IOPS. You can adjust the file share's IOPS multiple times up to its maximum limit or reduce IOPS to its minimum limit.

You can monitor the progress of your file share's IOPS change from the UI or CLI. You can also use the [Activity Tracker](/docs/vpc?topic=vpc-at-events) to verify that the IOPS were adjusted.

## Limitations
{: #adjustable-iops-limitations-file}

The following limitations apply.

* To adjust IOPS, the file share must be in a _stable_ state.
* For a file share created with an [IOPS tier profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#fs-tiers) to increase or decrease IOPS, select a different profile for the file share size. If the file share size exceeds that of the new IOPS tier profile, you can't change the profile.
* A [custom IOPS profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#custom) can expand to the maximum allowed by the custom band. You can't switch custom bands unless you increase file share size and then move to a higher band.
* A [dp2 profile](/docs/vpc?topic=vpc-file-storage-profiles&nterface=ui#dp2-profile) can expand to the maximum allowed by the dp2 band. You can't switch dp2 bands unless you increase file share size and then move to a higher band.
* Adjusting IOPS by moving between profiles is restricted by the file share size.
* When you use a custom or dp2 profile, IOPS can be adjusted multiple time until the maximum or minimum limit is reached.
* Maximum IOPS for a file share for all profiles is [96,000 IOPS](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#fs-tiers). For 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share in an instance is limited to 48,000 IOPS.

## Adjust IOPS in the UI
{: #adjust-vpc-iops-ui-file}
{: ui}

Follow these steps to adjust IOPS by selecting a new IOPS tier or custom IOPS band:

1. Go to the list of file shares. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File shares**. By default, file shares display for all resource groups in your region.

2. In the **File shares for VPC** list page, click the name of a file share to see its details.

3. On the file share details page, locate **Profile** and click the pencil icon or use the **Actions** menu and select **Edit IOPS profile**. Volumes must be attached to a virtual server instance for these actions.

4. In the side panel, adjust IOPS as follows:

   * For an IOPS tier, select a different tier from the menu. For example, you might have a 3 IOPS/GB general-purpose profile that you want to increase to a 5 IOPS/GB profile.

   * For a custom IOPS or dp2 profile, the current IOPS value is shown and file share size. Enter a new IOPS value within the allowable range for that size.

5. Review the estimated monthly order summary for your geography and new pricing.

6. If you're satisfied, click **Save and continue**.

Your new IOPS allocation is realized when you restart the instance.

## Adjust IOPS from the CLI
{: #adjust-vpc-iops-cli-file}
{: cli}

### Adjust IOPS for a Custom or dp2 profile
{: #adjust-iops-cli-file}

From the CLI, use the `share-update` command with the `--iops` property to indicate the new IOPS size for a custom or dp2 profile. The IOPS that you indicate must be within the range for the size of the file share (see [Custom IOPS profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#custom) or [dp2](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile)).

```sh
ibmcloud is share-update SHARE_NAME --iops IOPS
```
{: pre}

The following example shows an increase of IOPS from 100 IOPS to 1,200 IOPS for a 100 GB file share based on a 100 - 499 custom profile. (The IOPS range for this custom band is 100 - 6,000 IOPS.)

```bash
ibmcloud is share-update my-file-share --profile custom-iops --iops 1200
Updating file share my-file-share under account VPC1 as user user1@mycompany.com...

ID                e1180bae-6799-4156-8bb7-e4d24013cda2
Name              my-file-share
CRN               crn:v1:bluemix:public:is:us-south-1:a/0ef3f509-04f4-465e-88c6-2d60b164d805::share:e1180bae-6799-4156-8bb7-e4d24013cda2
Lifecycle state   updating
Zone              us-south-1
Profile           custom-iops
Size(GB)          100
IOPS              1200
Encryption        provider_managed
Mount targets     ID                          Name   VPC ID   VPC Name
                  No mounted targets found.

Resource group    ID                                     Name
                  7f1645c5-8afa-4a7e-860d-3df563e0aa8d   Default

Created           2023-03-26T20:01:18+05:30
Last sync at      2023-03-26T05:53:28+05:53
```
{: screen}

### Adjust IOPS by specifying a different IOPS tier profile
{: #adjust-profile-cli-file}

From the CLI, use the `share-update` command with the `--profile` property and indicate the name or href of the IOPS tier profile.

The following example changes a 3 IOPS/GB profile to 5 IOPS/GB profile. In this case, the file share can't exceed 9,600 GB to move to the higher profile.

```sh
ibmcloud is share-update SHARE_NAME --profile 5iops-tier
```
{: pre}

```bash
ibmcloud is share-update my-file-share --profile tier-5iops
Updating file share my-file-share under account VPC1 as user user@mycompany.com...

ID                ba7c7c8a-c111-4f54-a7fe-bb6d3d66eb2a
Name              my-file-share
CRN               crn:v1:bluemix:public:is:us-south-1:a/0ef3f509-04f4-465e-88c6-2d60b164d805::share:ba7c7c8a-c111-4f54-a7fe-bb6d3d66eb2a
Lifecycle state   updating
Zone              us-south-1
Profile           tier-5iops
Size(GB)          100
IOPS              3000
Encryption        provider_managed
Mount targets     ID                          Name   VPC ID   VPC Name
                  No mounted targets found.

Resource group    ID                                     Name
                  7f1645c5-8afa-4a7e-860d-3df563e0aa8d   Default

Created           2023-02-26T20:01:18+05:30
Last sync at      2023-02-26T05:53:28+05:53
Latest job        Job status   Job status reasons
                  succeeded    -
```
{: codeblock}

## Adjust IOPS with the API
{: #adjust-vpc-iops-api-file}
{: api}

You can adjust IOPS for existing data file shares by calling the Virtual Private Cloud (VPC) API.

### Adjust IOPS for a Custom or dp2 profile
{: #adjust-iops-api-file}

Make a `PATCH /shares` request and specify the `iops` property to adjust the IOPS within the allowable range for a custom pr dp2 profile.

You can't update the name of the file share and adjust IOPS in the same `PATCH /shares` request. Make two `PATCH /shares` requests.
{: note}

The following example shows an increase of 100 IOPS to 3,000 IOPS for a 100 GB file share based on a 100 - 499 custom profile. The IOPS range for this custom band is 100 - 6,000 IOPS.

```sh
curl -X PATCH \
 "$vpc_api_endpoint/v1/shares/$share_id?version=2023-07-11&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "iops": 3000
    }'
```
{: codeblock}

The file share status shows `updating` while the IOPS is being adjusted. The current IOPS is shown until you restart the instance. In the following example, IOPS is adjusted 100 - 3000 for the 3 IOPS/GB profile.

```json
{
   "created_at": "2023-03-06T22:58:49.000Z",
   "crn": "crn:[...]",
   "encryption": "provider_managed",
   "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r134-a0c07083-f411-446c-9316-7b08d6448c86",
   "id": "r134-a0c07083-f411-446c-9316-7b08d6448c86",
   "iops": 100,
    .
    .
    .
	"status": "updating",
    .
    .
    .
{
```
{: codeblock}

When the IOPS expansion completes, restart the instance. The new value is displayed, and the file share status is `stable`.

```json
{
  "created_at": "2023-03-06T22:58:49.000Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/a0c07083-f411-446c-9316-7b08d6448c86",
  "id": "a0c07083-f411-446c-9316-7b08d6448c86",
  "iops": 3000,
  "lifecycle_state": "stable",
  "name": "my-share-updated",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/tier-3iops",
    "name": "custom",
    "resource_type": "share_profile"
  },
  "replication_role": "none",
  "replication_status": "none",
  "replication_status_reasons": [],
  "resource_group": {
    "crn": "crn:[...]",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
    "id": "678523bcbe2b4eada913d32640909956",
    "name": "Default"
  },
  "resource_type": "share",
  "size": 4800,
  "mount_targets": [
    {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/a0c07083-f411-446c-9316-7b08d6448c86/mount_targets/1b5571cb-536d-48d0-8452-81c05c6f7b80",
      "id": "r134-1b5571cb-536d-48d0-8452-81c05c6f7b80",
      "name": "my-mount-target",
      "resource_type": "share_target",
      "vpc": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/r134-12bb28fc-856d-4902-813b-dc065d1ed084",
        "id": "12bb28fc-856d-4902-813b-dc065d1ed084",
        "name": "my-vpc",
        "resource_type": "vpc"
      }
    }
  ],
  "user_tags": [],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1",
    "resource_type": "zone"
  }
}
```
{: codeblock}

### Adjust IOPS by specifying a different IOPS tier profile
{: #adjust-profile-api-file}

Make a `PATCH /shares` request and specify the `profile` property and indicate the name or href of the IOPS tier profile.

The following example changes a 3 IOPS/GB profile to 5 IOPS/GB profile. In this case, the file share can't exceed 9,600 GB to move to the higher profile.

```sh
curl -X PATCH \
 "$vpc_api_endpoint/v1/shares/$share_id?version=2023-07-11&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "profile": "5iops-tier"
    }'
```
{: codeblock}

## Adjust IOPS with Terraform
{: #adjust-vpc-iops-terrafom-file}
{: terraform}

You can adjust IOPS for existing data file shares in Terraform.

### Adjust IOPS for a Custom or dp2 profile
{: #adjust-iops-terraform-file}

To modify the performance level of a file share, use the `ibm_is_share` resource and provide the IOPS value that you want. When you specify the IOPS value, make sure it is within the performance range for the size of the file share. For more information, see [dp2 file storage profile](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile).

When it is applied, the following example updates the share performance to 5000 IOPS.

```terraform
resource "ibm_is_share" "example" {
  name    = "my-new-share"
  size    = 200
  iops    = 5000
  profile = "dp2"
  zone    = "us-south-2"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.

### Adjust IOPS by specifying a different IOPS tier profile
{: #adjust-profile-terraform-file}

To modify the performance level of a file share with an IOPS tier profile, use the `ibm_is_share` resource and specify a different IOPS tier. When it is applied, the following example updates the share performance to 5000 IOPS.

```terraform
resource "ibm_is_share" "example" {
  name    = "my-share"
  size    = 220
  profile = "tier-5iops"
  zone    = "us-south-2"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.


## Next steps
{: #next-step-adjustable-iops-file}

Create more file shares or manage your existing shares:

* [Create file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create).
* [Manage your file shares](/docs/vpc?topic=vpc-file-storage-managing).
