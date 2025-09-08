---

copyright:
  years: 2021, 2025
lastupdated: "2025-09-08"

keywords: Block Storage for VPC, boot volume, data volume, volume, data storage, virtual server instance, instance, adjustable volume, iops

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Adjusting IOPS of a {{site.data.keyword.block_storage_is_short}} volume
{: #adjusting-volume-iops}

For {{site.data.keyword.block_storage_is_full}} volumes that are attached to a running virtual server instance, you can increase or decrease IOPS to meet your performance needs. Adjust IOPS by specifying a different profile from the tiered family or a different IOPS value within a custom IOPS band. You can use the UI, the CLI, the API, or Terraform to adjust IOPS. You can adjust the volume's IOPS multiple times up to its maximum limit or reduce IOPS to its minimum limit. The IOPS adjustment causes no outage or lack of access to the storage.
{: shortdesc}

For example, you might find that an application scaled such that a lower-tier storage profile is now a performance bottleneck. Instead of ordering a new volume and migrating your data, you can change the performance characteristics of the existing volume by increasing IOPS in the next performance tier.

In another scenario, you might want to initially set your storage at a higher 10 IOPS/GB performance level to expedite a data upload. After the upload completes, you can reset storage to 5 IOPS/GB for normal operations. Alternatively, you might want to increase your volume's IOPS during peak times for your application and decrease IOPS during off-peak hours.

With this feature, you can:

* Adjust IOPS up or down by selecting a different [IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles&interface=api#tiers).
* Adjust IOPS within a [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles&interface=api#custom) band.
* Adjust IOPS without having to detach the volume from the virtual server instance.

The degree to which IOPS can be increased is determined by the maximum that is allowed by the volume's profile.

- [SSD defined performance profile](/docs/vpc?topic=vpc-block-storage-profiles#defined-performance-profile): [Select availability]{: tag-green} Customers with special access to volume profiles within the defined performance family can modify the performance level of their `sdp` volumes even if the volumes are not attached to a running virtual server instance. You can specify volume performance in the range of 100 - 64,000 IOPS without capacity-based restrictions. The steps for modifying IOPS are the same as for the custom profile.

- [Tiered profiles](/docs/vpc?topic=vpc-block-storage-profiles#tiers): you can adjust IOPS for an IOPS tier based on volume size or select the next profile that allows for increased performance. The volume must be attached to a running virtual server instance.

- [Custom volume profile](/docs/vpc?topic=vpc-block-storage-profiles#custom): you can increase IOPS by specifying the next IOPS custom band. For example, for a volume 10 - 39 GB, you can increase custom IOPS to 1,000 IOPS. The IOPS range within a custom band is based on the volume size. If you later [increase the size of a volume](/docs/vpc?topic=vpc-expanding-block-storage-volumes), you can increase the IOPS again. The volume must be attached to a running virtual server instance.

You can monitor the progress of your volume's IOPS change from the UI or CLI. You can also check the [Activity tracking events](/docs/vpc?topic=vpc-at_events) to verify that the IOPS were adjusted.

Billing for an updated volume is automatically updated. The prorated difference of the new price is added to the current billing cycle. The new full amount is then billed in the next billing cycle.

## Limitations
{: #adjustable-iops-limitations-block}

The following limitations apply to this release.

### General limitations
{: #exp-iops-general-limitations}

* Primary boot volume IOPS cannot be adjusted.
* To adjust IOPS, the volume must be attached to a virtual server instance that is powered on and _running_.
* To adjust IOPS, the volume must be in an _available_ state.
* When a volume is in transition, the volume remains in an _updating_ state until you reattach it to a running instance. For example, if the volume is detached while IOPS adjustment is in progress, the IOPS expansion resumes and completes only after the volume is reattached.
* When you delete an instance, volumes that are marked for auto-deletion are not deleted while the IOPS adjustment is underway.

### Limitations that are related to volume profiles
{: #exp-vol-IOPS-limitations}

* For a volume that was created with a [profile from the tiered family](/docs/vpc?topic=vpc-block-storage-profiles#tiers), select a different profile for the volume size to increase or decrease IOPS. If the volume size exceeds the maximum of the new tiered profile, you can't change the profile.
* A [custom volume profile](/docs/vpc?topic=vpc-block-storage-profiles#custom) can expand to the highest value that is allowed by the custom band. You can't switch custom bands unless you increase the volume size and move to a higher band.
* Custom IOPS can be adjusted multiple times until the maximum or minimum limit is reached.
* The maximum IOPS for a volume with the `sdp` profile is 64,000. To achieve more than 48,000 IOPS, the volume must be attached to a virtual server instance with a [3rd generation instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles).
* The maximum IOPS for a volume with a profile within the _tiered_ or _custom_ volume profile families is [48,000 IOPS](/docs/vpc?topic=vpc-block-storage-profiles#tiers).

## Adjust IOPS in the console
{: #adjust-vpc-iops-ui-block}
{: ui}

Follow these steps to adjust IOPS by selecting a new IOPS tier or custom IOPS band:

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**. By default, {{site.data.keyword.block_storage_is_short}} volumes display for all resource groups in your region.
1. In the list of all **{{site.data.keyword.block_storage_is_short}} volumes**, click the name of the volume to see the volume details.

   The volume that you select must be attached to a virtual server instance. In the list of volumes, its attachment type is _data_.
   {: note}

   Alternatively, go to the virtual server instance's details page and select the data volume that you want to adjust from the list of attached volumes.
   {: tip}

1. On the volume details page, locate **Profile** and click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") or use the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), and select **Edit IOPS profile**. Volumes must be attached to a virtual server instance for these actions.
1. In the side panel, adjust IOPS as follows:
   * For customers with special access to preview the defined performance profile, you can specify volume performance in the range of 100 - 64,000 IOPS without capacity-based restrictions.
   * For an IOPS tier, select a different tier from the menu. For example, you might have a 3 IOPS/GB general-purpose profile you're increasing to a 5 IOPS/GB profile.
   * For a Custom IOPS, the current IOPS value is shown and volume size. Enter a new IOPS value in the range specified for that custom band.

1. Review the estimated monthly order summary for your geography and new pricing.
1. If you're satisfied, click **Save and continue**.

Your new IOPS allocation is realized when you restart the instance.

## Adjust IOPS from the CLI
{: #adjust-vpc-iops-cli-block}
{: cli}

### Before you begin
{: #adjust-vpc-iops-cli-block-prereq}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

### Adjust IOPS for a custom profile
{: #adjust-iops-cli-block}

From the CLI, use the `ibmcloud is volume-update` command with the `--iops` option to indicate the new IOPS size for a custom profile. The IOPS that you choose must be within the range for the size of the volume. For more information, see the [custom volume profile](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#custom).

```sh
ibmcloud is volume-update VOLUME_ID --iops IOPS
```
{: pre}

This example shows an increase of IOPS from 100 IOPS to 3,000 IOPS for a 100 GB volume based on a 100 - 499 custom profile. The IOPS range for this custom band is 100 - 6,000 IOPS.

```sh
ibmcloud is volume-update 933c8781-f7f5-4a8f-8a2d-3bfc711788ee --iops 3000
```
{: pre}

```sh
Updating volume 933c8781-f7f5-4a8f-8a2d-3bfc711788ee under account Test Account as user test.user@ibm.com...
ID                                      0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    demo-volume-update
Capacity                                100
IOPS                                    3000
Profile                                 custom
Encryption Key                          -
Encryption                              provider managed
Status                                  available
Created                                 2021-08-23 10:09:28
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Zone                                    us-south-2
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Volume Attachment Instance Reference    Vdisk Name    Vdisk ID                                    Vdisk Type   Auto Delete
                                        Vdisk-data1   0738-fd146b1f-e1bb-4eab-ba78-3109e6bc3a2d   data         true
    									Instance Name   Instance ID
       									vsi-test1       0738-8b56da93-7990-4ccf-9dc5-5aee6a5f08f9
```
{: codeblock}

You can use the same command for adjusting the IOPS of a volume that was created with a `sdp` profile. The new IOPS value can be anywhere between 100-64,000.

### Adjust IOPS by specifying a different IOPS tier profile
{: #adjust-profile-cli}

From the CLI, use the `volume-update` command with the `--profile` parameter and indicate the name or href of the IOPS tier profile.

This example changes a 3 IOPS/GB profile to a 5 IOPS/GB profile. In this case, the volume can't exceed 9,600 GB to move to the higher profile.

```sh
ibmcloud is volume-update {volume-id} --profile 5iops-tier
```
{: pre}

```sh
ibmcloud is volume-update demo-volume-update --profile 5iops-tier
```
{: pre}

```sh
Updating volume demo-volume-update under account Test Account as user test.user@ibm.com...
                                          
ID                                     r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac   
Name                                   demo-volume-update   
CRN                                    crn:v1:bluemix:public:is:us-east-1:a/a1234567::volume:r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac    
Status                                 available   
Attachment state                       attached   
Capacity                               100   
IOPS                                   3000   
Bandwidth(Mbps)                        393   
Profile                                5iops-tier   
Encryption key                         -   
Encryption                             provider_managed   
Resource group                         defaults   
Created                                2023-06-29T16:14:59+00:00   
Zone                                   us-east-1   
Health State                           ok   
Volume Attachment Instance Reference   Attachment type   Instance ID                                 Instance name   Auto delete   Attachment ID                               Attachment name      
                                       data              0757_11f5db7f-35a1-4678-bcbd-c85204e09507   kj-test-ro      false         0757-4dfc4384-c4b5-497e-bab3-6415f9c4d44b   otp      
                                          
Active                                 true 
Busy                                   false   
Tags                                   -
```
{: screen}

For more information about available command options, see [`ibmcloud is volume-update`](/docs/cli?topic=cli-vpc-reference#volume-update).

## Adjust IOPS with the API
{: #adjust-vpc-iops-api-block}
{: api}

You can adjust IOPS for existing data volumes by calling the Virtual Private Cloud (VPC) API. 

### Adjust IOPS for a volume with the Custom or Defined performance profile
{: #adjust-iops-api-block}

Make a `PATCH /volumes` request and specify the `iops` parameter to adjust the IOPS within a custom volume profile band. 

You can't update the name of the volume and adjust IOPS in the same `PATCH /volumes` request. Make two `PATCH /volumes` requests.
{: note}

This example shows an increase of 100 IOPS to 3,000 IOPS for a 100 GB volume based on a 100 - 499 custom profile. The IOPS range for this custom band is 100 - 6,000 IOPS.

```sh
curl -X PATCH \
 "$vpc_api_endpoint/v1/volumes/$volume_id?version=2022-01-11&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "iops": 3000
    }'
```
{: codeblock}

The volume status shows `updating` while the IOPS is being adjusted. The current IOPS is shown until you restart the instance. In this example, IOPS is increased from 100 to 3000 for the 3 IOPS/GB profile.

```json
{
	"iops": 100,
	"created_at": "2022-01-11T09:46:43.000Z",
	"crn": "crn:v1:bluemix:public:is:us-south-1:a/<Acc id>::volume:<Volume ID>",
    .
    .
    .
	"status": "updating",
    .
    .
    .
}
```
{: screen}

When the IOPS expansion completes, restart the instance. The new value displays, and the volume status is `available`.

```json
{
	"capacity": 250,
	"created_at": "2022-01-11T09:46:43.000Z",
	"crn": "crn:[...]",
	"encryption": "provider_managed",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/2d1bb5a8-40a8-447a-acf7-0eadc8aeb054",
    "id": "2d1bb5a8-40a8-447a-acf7-0eadc8aeb054",
	"iops": 3000,
	"name": "my-volume-1",
    "profile": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/custom",
		"name": "custom"
	},
    "resource_group": {
        "href": "https://resource-manager.bluemix.net/v1/resource_groups/83daf012-5920-4ba9-9689-cc0d2d2281fb",
        "id": "83daf012-5920-4ba9-9689-cc0d2d2281fb",
		"name": "Default"
	},
	"status": "available",
	"volume_attachments": [{
		"delete_volume_on_instance_delete": true,
		"device": {
			"id": "4cbb38bc-57d5-4121-a796-d5b10cf0810a"
		},
		"href": "https://us-south.iaas.cloud.ibm.com/v1/instances/8f06378c-ed0e-481e-b98c-9a6dfbee1ed5/volume_attachments/4cbb38bc-57d5-4121-a796-d5b10cf0810a",
		"id": "<4cbb38bc-57d5-4121-a796-d5b10cf0810aAttachment ID>",
		"instance": {
			"crn": "crn:[...]",
			"href": "https://us-south.iaas.cloud.ibm.com/v1/instances/8f06378c-ed0e-481e-b98c-9a6dfbee1ed5",
			"id": "8f06378c-ed0e-481e-b98c-9a6dfbee1ed5",
			"name": "my-instance-1"
		},
		"name": "my-volume-attachment-1",
		"type": "data"
	}],
    "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
        "name": "us-south-2"
    }
}
```
{: screen}

You can use the same API request for adjusting the IOPS of a volume that was created with the `sdp` profile. The new IOPS value can be anywhere between 100-64,000. To achieve more than 48,000 IOPS, the volume must be attached to a virtual server instance with a [3rd generation instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles).

### Adjust IOPS by specifying a different IOPS tier profile
{: #adjust-profile-api-block}

Make a `PATCH /volumes` request and specify the `profile` parameter and indicate the name or href of the IOPS tier profile.

This example changes a 3 IOPS/GB profile to a 5 IOPS/GB profile. In this case, the volume can't exceed 9,600 GB to move to the higher profile.

```sh
curl -X PATCH \
 "$vpc_api_endpoint/v1/volumes/$volume_id?version=2022-01-11&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "profile": "5iops-tier"
    }'
```
{: codeblock}

## Adjusting IOPS with Terraform
{: #adjust-vpc-iops-terraform-block}
{: terraform}

### Adjust IOPS for a Custom profile
{: #adjust-iops-custom-terraform}

To modify the IOPS of a volume, use the `ibm_is_volume` resource. When applied, the following example updates the volume IOPS to 24,000.

```terraform
resource "ibm_is_volume" "storage" {
  name    = "demo-volume-update"
  size    = 8000
  iops    = 24000
  zone    = "us-south-2"
}
```
{: codeblock}

### Adjust IOPS for a tiered profile
{: #adjust-iops-tier-terraform}

To modify the IOPS of a volume that was created by using a performance tier, use the `ibm_is_volume` resource and specify a different tier. When applied, the following example updates the volume profile to `5iops-tier`.

```terraform
resource "ibm_is_volume" "storage" {
  name    = "demo-volume-update"
  size    = 8000
  profile = "5iops-tier"
  zone    = "us-south-2"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume){: external}.

## Next steps
{: #next-step-adjustable-iops-block}

Create more volumes or manage your existing {{site.data.keyword.block_storage_is_short}} volumes:

* [Creating {{site.data.keyword.block_storage_is_short}} volumes in the console](/docs/vpc?topic=vpc-creating-block-storage)
* [Managing {{site.data.keyword.block_storage_is_short}} volumes in the console](/docs/vpc?topic=vpc-managing-block-storage)
