---

copyright:
  years: 2022
lastupdated: "2022-10-24"

keywords:

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:preview: .preview}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Adjusting file share IOPS
{: #adjusting-share-iops}

For File Storage for VPC file shares, you can increase or decrease IOPS to meet your performance needs. Adjust IOPs by specifying a different IOPS tier profile (IOPS are adjusted within the selected profile). Or, specify new IOPS within a custom IOPS band. There's no outage or lack of access to the storage while adjusting IOPS.
{: shortdesc}

Billing for an updated share is automatically updated. The prorated difference of the new price is added to the current billing cycle. The new full amount is then billed in the next billing cycle.

File Storage for VPC is available for customers with special approval to preview this service in the Washington, Dallas, Frankfurt, London, Sydney, Sao Paulo, Tokyo, and Toronto regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Adjustable IOPS concepts
{: #fs-adj-iops-concepts}

You can adjust IOPS for your file shares to tailor performance to meet your requirements. 

For example, you might find that an application has scaled such that a lower-tier storage profile is now a performance bottleneck. You can change the performance characteristics of the existing file share by increasing IOPS by selecting a higher IOPS tier (for example, 3 IOPS/GB tier to 5 IOPS/GB tier).

In another scenario, you might want to initially set your file storage at a higher 10 IOPS/GB performance level to expedite a data upload. After the upload completes, you can reset storage to a lower 5 IOPS/GB for normal operations. Also, you might want to increase your file share's IOPS during peak times for your application and decrease IOPS during off-peak hours.

Using this feature, you can:

* Adjust IOPS up or down by selecting a different [IOPS tier](/docs/vpc?topic=vpc-file-storage-profiles#fs-tiers). IOPS are adjusted within the tier. The degree to which IOPS can be increased is determined by the maximum allowed by the file share's[ IOPS tier profile](/docs/vpc?topic=vpc-file-storage-profiles). You can adjust IOPS for an IOPS tier based on share size or select the next profile that allows for increased performance. 

    Maximum IOPS for a file share for a 3 IOPS/GB tier profile is 96,000 IOPs. All other profiles are capped at 48,000 IOPS. For 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share accessed by one instance is limited to 48,000 IOPS.
    {: note}

* Adjust IOPS within a [custom IOPS profile](/docs/vpc?topic=vpc-file-storage-profiles#custom) band. The IOPS range within a custom band is based on the file share size. For example, for a file share that's 25 GB, you can increase IOPS anywhere from 100-1,000 IOPS. If you later [increase the size of a file share](/docs/vpc?topic=vpc-file-storage-expand-capacity) to the next highest band, you can increase the IOPS again.

* Adjust IOPS from an IOPS tier profile to a customer profile. In this case, the file share size restricts the custom band you select. For example, if you're adjusting IOPS for file share based on a 3 IOPS/GB profile and it's 12,000 GB, you can adjust the IOPS because a custom profile allows file shares up to 16,000 GB. If the file share size is more than 16,000 GB, you can't adjust the IOPS. IOPS tier maximum sizes are 32,000 GB for 3 IOPS/GB, 9,600 GB for 5 IOPS/GB, and 4,800 GB for 10 IOPS/GB. When moving from a tiered profile to a custom profile, billing is update based on the specified IOPS.

* Adjust IOPS from a custom profile to an IOPS tier proflie. Again, you're restriced to which IOPS tier profile you can specify by the file share size. When moving from a custom profile to any tiered profile, billing is updated and the IOPS is adjusted as per the profile.

To adjust a file share's IOPS, the file share must be in a _stable_ state. Your user authorization is verified before adjusting IOPS. 
 
You can use the [UI](#fs-adjust-vpc-iops-ui), [CLI](#fs-adjust-vpc-iops-cli), or [API](#fs-adjust-vpc-iops-api) to adjust IOPS. You can adjust the file share's IOPS multiple times up to its maximum limit or reduce IOPS to its lower limit.
 
You can monitor the progress of your file share's IOPS change from the UI or CLI. You can also use the [Activity Tracker](/docs/vpc?topic=vpc-at-events) to verify that the IOPS were adjusted.

## Limitations
{: #adjustable-iops-limitations}

The following limitations apply to this release.

* To adjust IOPS, the file share must be in a _stable_ state.
* For a file share created using an [IOPS tier profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#fs-tiers), to increase or decrease IOPS, select a different profile for the file share size. If the file share size exceeds that of the new IOPS tier profile, you can't change the profile.
* A [custom IOPS profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#custom) can expand to the highest allowed by the custom band. You can't switch custom bands unless you increase file share size and move to a higher band.
* Adjusting IOPS by moving from a tiered profile to a custom profile, or from a custom profile to a tiered profile, is restricted by the file share size.
* Custom IOPS can adjusted multiple times until the maximum or minimum limit is reached.
* Maximum IOPS for a file share for all profiles is [96,000 IOPs](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#fs-tiers). Note that 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share/instance is limited to 48,000 IOPS.

## Adjust IOPS using the UI
{: #adjust-vpc-iops-ui}
{: ui}

Follow these steps to adjust IOPS by selecting a new IOPS tier or custom IOPS band:

1. Navigate to the list of file shares. In [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File shares**. By default, file shares display for all resource groups in your region.

2. In the **File shares for VPC** list page, click the name of a file share to see its details.

3. On the file share details page, locate **Profile** and click the pencil icon or use the **Actions** menu and select **Edit IOPS profile**. Volumes must be attached to a virtual server instance for these actions.

4. In the side panel, adjust IOPS as follows:

   * For an IOPS tier, select a different tier from the dropdown menu. For example, you might have a 3 IOPS/GB general purpose profile you're increasing to a 5 IOPS/GB profile.

   * For a Custom IOPS, the current IOPS value is shown and file share size. Enter a new IOPS value in the range specified for that custom band.

5. Review the estimated monthly order summary for your geography and new pricing.

6. If you're satisfied, click **Save and continue**.

Your new IOPS allocation will be realized when you restart the instance.

## Adjust IOPS using the CLI
{: #adjust-vpc-iops-cli}
{: cli}

### Adjust IOPS for a Custom profile
{: #adjust-iops-cli}

From the CLI, use the `share-update` command with the `--iops` property to indicate the new IOPS size for a custom profile. The IOPS you indicate must be within the range for the size of the file share (see [Custom IOPS profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#custom)).

```
ibmcloud is share-update SHARE_NAME --iops IOPS
```
{: pre}

This example shows an increase of IOPS from 100 IOPS to 1,200 IOPS for a 100 GB file share based on a 100 - 499 custom profile. (The IOPS range for this custom band is 100 - 6,000 IOPS.)

```bash
ibmcloud is share-update my-file-share --profile custom-iops --iops 1200
Updating file share my-file-share under account VPC1 as user user1@mycompany.com...
                     
ID                e1180bae-6799-4156-8bb7-e4d24013cda2
Name              my-file-share
CRN               crn:v1:staging:public:is:us-south-1:a/0ef3f509-04f4-465e-88c6-2d60b164d805::share:e1180bae-6799-4156-8bb7-e4d24013cda2
Lifecycle state   updating
Zone              us-south-1
Profile           custom-iops
Size(GB)          100
IOPS              1200
Encryption        provider_managed   
Targets           ID                          Name   VPC ID   VPC Name
                  No mounted targets found.

Resource group    ID                                     Name
                  7f1645c5-8afa-4a7e-860d-3df563e0aa8d   Default
                     
Created           2022-06-26T20:01:18+05:30
Last sync at      2022-06-26T05:53:28+05:53
```
{: screen}

### Adjust IOPS by specifying a higher or lower IOPS tier profile
{: #adjust-profile-cli}

From the CLI, use the `share-update` command with the `--profile` property and indicate the name or href of the IOPS tier profile.

This example changes a 3 IOPS/GB profile to 5 IOPS/GB profile. In this case, the file share can't exceed 9,600 GB to move to the higher profile.

```
ibmcloud is share-update SHARE_NAME --profile 5iops-tier
```
{: pre}

```bash
ibmcloud is share-update my-file-share --profile tier-5iops 
Updating file share my-file-share under account VPC1 as user user@mycompany.com...
                     
ID                ba7c7c8a-c111-4f54-a7fe-bb6d3d66eb2a
Name              my-file-share
CRN               crn:v1:staging:public:is:us-south-1:a/0ef3f509-04f4-465e-88c6-2d60b164d805::share:ba7c7c8a-c111-4f54-a7fe-bb6d3d66eb2a
Lifecycle state   updating
Zone              us-south-1
Profile           tier-5iops
Size(GB)          100
IOPS              3000
Encryption        provider_managed   
Targets           ID                          Name   VPC ID   VPC Name 
                  No mounted targets found.

Resource group    ID                                     Name
                  7f1645c5-8afa-4a7e-860d-3df563e0aa8d   Default
                     
Created           2022-06-26T20:01:18+05:30   
Last sync at      2022-06-26T05:53:28+05:53   
Latest job        Job status   Job status reasons
                  succeeded    -
```
{: codeblock}

## Adjust IOPS with the API
{: #adjust-vpc-iops-api}
{: api}

You can adjust IOPS for existing data file shares by calling the Virtual Private Cloud (VPC) API.

### Adjust IOPS for a Custom profile
{: #adjust-iops-api}

Make a `PATCH /shares` request and specify the `iops` property to adjust the IOPS within a custom IOPS profile band. 

You can't update the name of the file share and adjust IOPS in the same `PATCH /shares` request. Make two `PATCH /shares` requests.
{: note}

This example shows an increase of 100 IOPS to 3,000 IOPS for a 100 GB file share based on a 100 - 499 custom profile. The IOPS range for this custom band is 100 - 6,000 IOPS. 

```
curl -X PATCH \
 "$vpc_api_endpoint/v1/shares/$share_id?version=2022-05-24&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "iops": 3000
    }'
```
{: codeblock}

The file share status shows `updating` while the IOPS are being adjusted. The current IOPS is shown until you restart the instance. In this example, IOPS are adjusted from 100 to 3000 for the 3 IOPS/GB profile.

```
{
   "created_at": "2022-05-24T22:58:49.000Z",
   "crn": "crn:[...]",
   "encryption": "provider_managed",
   "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r134-a0c07083-f411-446c-9316-7b08d6448c86",
   "id": "r134-a0c07083-f411-446c-9316-7b08d6448c86",
   "iops": 100,
   "created_at": "2022-05-24T09:46:43.000Z",
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

When the IOPS expansion completes, restart the instance. The new value displays and the file share status is `stable`.

```
{
  "created_at": "2022-05-24T22:58:49.000Z",
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
  "targets": [
    {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/a0c07083-f411-446c-9316-7b08d6448c86/targets/1b5571cb-536d-48d0-8452-81c05c6f7b80",
      "id": "r134-1b5571cb-536d-48d0-8452-81c05c6f7b80",
      "name": "my-target",
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

### Adjust IOPS by specifying a higher or lower IOPS tier profile
{: #adjust-profile-api}

Make a `PATCH /shares` request and specify the `profile` property and indicate the name or href of the IOPS tier profile.

This example changes a 3 IOPS/GB profile to 5 IOPS/GB profile. In this case, the file share can't exceed 9,600 GB to move to the higher profile.

```
curl -X PATCH \
 "$vpc_api_endpoint/v1/shares/$share_id?version=2022-05-24&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "profile": "5iops-tier"
    }'
```
{: codeblock}

## Next steps
{: #next-step-adjustable-iops}

Create more file shares or manage your existing shares:

* [Create file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create)
* [Manage your file shares](/docs/vpc?topic=vpc-file-storage-managing)
