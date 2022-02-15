---

copyright:
  years: 2021, 2022
lastupdated: "2022-02-08"

keywords: block storage, boot volume, data volume, volume, data storage, virtual server instance, instance, adjustable volume, iops

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Adjusting block storage volume IOPS
{: #adjusting-volume-iops}

For {{site.data.keyword.block_storage_is_short}} volumes attached to a virtual server instance, you can increase or decrease IOPS to meet your performance needs. Adjust IOPs by specifying a different IOPS tier profile or different IOPS value withing a custom IOPS band. There's no outage or lack of access to the storage while adjusting IOPS.
{: shortdesc}

Billing for an updated volume is automatically updated. The prorated difference of the new price is added to the current billing cycle. The new full amount is then billed in the next billing cycle.

## Adjustable IOPS concepts
{: #adjustable-iops-concepts}

You can adjust IOPS for your block storage data volumes attached to a running virtual server instance to tailor performance to meet your requirements. 

For example, you might find that an application has scaled such that a lower-tier storage profile is now a performance bottleneck. Instead of ordering a new volume and migrating your data, you can change the performance characteristics of the existing volume by increasing IOPS in the next performance tier.

In another scenario, you might want to initially set your storage at a higher 10 IOPS/GB performance level to expedite a data upload. After the upload completes, you can reset storage to a lower 5 IOPS/GB for normal operations. Alternatively, you might want to increase your volume's IOPS during peak times for your application and decrease IOPS during off-peak hours.

Using this feature, you can:

* Adjust IOPS up or down by selecting a different [IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles&interface=api#tiers).
* Adjust IOPS within a [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles&interface=api#custom) band. 
* Adjust IOPS without having to detach the volume from the virtual server instance.

The degree to which IOPS can be increased is determined by the maximum allowed by the volume's [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers). You can adjust IOPS for an IOPS tier based on volume size or select the next profile that allows for increased performance. 

If you use a [Custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom), you can increase IOPS by specifying the next IOPS custom band. For example, for a volume 10-39 GB, you can increase custom IOPS to 1,000 IOPS by specifying a custom IOPS value. The IOPS range within a custom band is based on the volume size. If you later [increase the size of a volume](/docs/vpc?topic=vpc-expanding-block-storage-volumes), you can increase the IOPS again.

To adjust a volume's IOPS, the volume must be in an _available_ state and the instance must be running. Your user authorization is verified before adjusting IOPS.
 
You can use the [UI](#adjust-vpc-iops-ui), [CLI](#adjust-vpc-iops-cli), or [API](#adjust-vpc-iops-api) to adjust IOPS. You can adjust the volume's IOPS multiple times up to its maximum limit or reduce IOPS to its lower limit.
 
You can monitor the progress of your volume's IOPS change from the UI or CLI. You can also use the [Activity Tracker](/docs/vpc?topic=vpc-at-events) to verify that the IOPS were adjusted.

## Limitations
{: #adjustable-iops-limitations}

The following limitations apply to this release.

### General limitations
{: #exp-iops-general-limitations}

* Primary boot volume IOPS cannot be adjusted.
* To adjust IOPS, the volume must be attached to a running virtual server instance.
* The volume must be in an _available_ state with the instance powered on and in a _running_ state.

### Volume and IOPs limitations
{: #exp-vol-IOPs-limitations}

* For a volume created using an [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers), to increase or decrease IOPS, select a different profile for the volume size. If the volume size exceeds that of the new IOPS tier profile, you can't change the profile.
* A [custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom) can expand to the highest allowed by the custom band. You can't switch custom bands unless you increase volume size and move to a higher band.
* Custom IOPS can adjusted multiple times until the maximum or minimum limit is reached.
* Maximum IOPS for a volume for all profiles is [48,000 IOPs](/docs/vpc?topic=vpc-block-storage-profiles#tiers).

### Additional limitations
{: #exp-vols-additional-limitations}

* When a volume is in transition (for example, the volume is detached while IOPS adjustment is in progress), the volume remains in an _updating_ state until you reattach it to an instance. The IOPS expansion the resumes and completes.
* When you delete an instance, volumes marked for auto-deletion are not deleted when IOPS adjustment is underway.

## Adjust IOPS using the UI
{: #adjust-vpc-iops-ui}
{: ui}

Follow these steps to adjust IOPS by selecting a new IOPS tier or custom IOPS band:

1. Navigate to the list of block storage volumes. In [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**. By default, block storage volumes display for all resource groups in your region.

2. In the list of all **Block storage for VPC volumes**, click the name of the volume to see the volume details. 

   The volume that you select must be attached to a virtual server instance. In the list of volumes, its attachment type is _data_.
   {: note}

3. Alternatively, go to to a virtual server instance with an attached volume that you want to adjust IOPS and select it from the list of attached volumes.

4. On the volume details page, locate **Profile** and click the pencil icon or use the **Actions** menu and select **Edit IOPS profile**. Volumes must be attached to a virtual server instance for these actions.

5. In the side panel, adjust IOPS as follows:

   * For an IOPS tier, select a different tier from the dropdown menu. For example, you might have a 3 IOPS/GB general purpose profile you're increasing to a 5 IOPS/GB profile.

   * For a Custom IOPS, the current IOPS value is shown and volume size. Enter a new IOPS value in the range specified for that custom band.

6. Review the estimated monthly order summary for your geography and new pricing.

7. If you're satisfied, click **Save and continue**.

Your new IOPS allocation will be realized when you restart the instance.


## Adjust IOPS using the CLI
{: #adjust-vpc-iops-cli}
{: cli}

### Adjust IOPS for a Custom profile
{: #adjust-iops-cli}

From the CLI, use the `volume-update` command with the `--iops` parameter to indicate the new IOPS size for a custom profile. The IOPS you indicate must be within the range for the size of the volume (see [Custom IOPS profile](https://test.cloud.ibm.com/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#custom)).

```
ibmcloud is volume-update VOLUME_ID --iops IOPS
```
{: pre}

This example shows an increase of IOPS from 100 IOPS to 3,000 IOPS for a 100 GB volume based on a 100 - 499 custom profile. (The IOPS range for this custom band is 100 - 6,000 IOPS.)

```bash
$ ibmcloud is volume-update 933c8781-f7f5-4a8f-8a2d-3bfc711788ee --iops 3000
Updating volume 933c8781-f7f5-4a8f-8a2d-3bfc711788ee under account MyAccount 01 as user user1@mycompany.com...
ID                                      0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    demo-volume-update
Capacity                                100
IOPs                                    3000
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
{: screen}

### Adjust IOPS by specifying a higher or lower IOPS tier profile
{: #adjust-profile-cli}

From the CLI, use the `volume-update` command with the `--profile` parameter and indicate the name or href of the IOPS tier profile.

This example changes a 3 IOPS/GB profile to 5 IOPS/GB profile. In this case, the volume can't exceed 9,600 GB to move to the higher profile.

```
ibmcloud is volume-update {volume-id} --profile 5iops-tier
```
{: pre}


## Adjust IOPS with the API
{: #adjust-vpc-iops-api}
{: api}

You can adjust IOPS for existing data volumes by calling the Virtual Private Cloud (VPC) API.

### Adjust IOPS for a Custom profile
{: #adjust-iops-api}

Make a `PATCH /volumes` request and specify the `iops` parameter to adjust the IOPS within a custom IOPS profile band. 

You can't update the name of the volume and adjust IOPS in the same `PATCH /volumes` request. Make two `PATCH /volumes` requests.
{: note}

This example shows an increase of 100 IOPS to 3,000 IOPS for a 100 GB volume based on a 100 - 499 custom profile. The IOPS range for this custom band is 100 - 6,000 IOPS.

```
curl -X PATCH \
 "$vpc_api_endpoint/v1/volumes/$volume_id?version=2022-01-11&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "iops": 3000
    }'
```
{: codeblock}

The volume status shows `updating` while the IOPS are being adjusted. The current IOPS is shown until you restart the instance. In this example, IOPS are adjusted from 100 to 3000 for the 3 IOPS/GB profile.

```
{
	"iops": 100,
	"created_at": "2022-01-11T09:46:43.000Z",
	"crn": "crn:v1:staging:public:is:us-south-1:a/<Acc id>::volume:<Volume ID>",
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

When the IOPS expansion completes, restart the instance. The new value displays and the volume status is `available`.

```
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
			"href": "https://us-south.iaas.cloud.ibm.com/v1/instances/8f06378c-ed0e-481e-b98c-9a6dfbee1ed5,
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
{: codeblock}

### Adjust IOPS by specifying a higher or lower IOPS tier profile
{: #adjust-profile-api}

Make a `PATCH /volumes` request and specify the `profile` parameter and indicate the name or href of the IOPS tier profile.

This example changes a 3 IOPS/GB profile to 5 IOPS/GB profile. In this case, the volume can't exceed 9,600 GB to move to the higher profile.

```
curl -X PATCH \
 "$vpc_api_endpoint/v1/volumes/$volume_id?version=2022-01-11&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "profile": "5iops-tier"
    }'
```
{: codeblock}

## Next steps
{: #next-step-adjustable-iops}

Create more volumes or manage your existing block storage volumes:

* [Creating block storage volumes by using the UI](/docs/vpc?topic=vpc-creating-block-storage)
* [Managing block storage volumes by using the UI](/docs/vpc?topic=vpc-managing-block-storage)
