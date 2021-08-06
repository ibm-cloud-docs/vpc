---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-17"

keywords: block storage, VPC, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance, expandable volume

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:beta: .beta}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Expanding block storage volume capacity (beta)
{: #expanding-block-storage-volumes}

For {{site.data.keyword.block_storage_is_short}} data volumes, when you create and attach a volume to a virtual server instance, you can later increase the size of the volume. You indicate capacity in GB increments up to 16,000 GB capacity, depending on your volume profile. This process doesn't require you to perform manual steps. For example, you don't need to migrate your data to a larger volume. There's no outage or lack of access to the storage while the volume is being resized.
{:shortdesc}

Billing for the volume is automatically updated to add the pro-rated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

Expandable volumes is a beta feature that is available for evaluation and testing purposes. This feature is available in the US South, US East, London, and France regions. Contact your IBM Sales representative if you are interested in getting access.
{:beta}

## Expandable volume concepts
{: #expandable-volume-concepts}

Volume capacity can be expanded for data volumes that are attached to a virtual server instance. The volume must be in an _available_ state and the instance must be running. Your user authorization is verified before expanding the volume. You can use the [UI](#expand-vpc-volumes-ui), [CLI](#expand-vpc-volumes-cli), or [API](#expand-vpc-volumes-api) to expand volume capacity. You can expand the volume multiple times up to its [maximum capacity limit](#exp-vols-capacity-IOPS-limitations). After the expanding volume, you can't reduce the volume capacity.

Volumes created using an [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers-beta) can be expanded to the maximum size for its IOPS tier:

* A general-purpose, 3 IOPS/GB profile can be expanded up to 16,000 GB
* A 5 IOPS/GB profile can be expanded up to 9,600 GB
* A 10 IOPS/GB profile can be expanded up to 4,800 GB

For the Beta release, volumes with a capacity of 10 GB to 249 GB can be expanded up to 250 GB. Volumes 251 GB and over can potentially expand up to 16,000 GB, limited by the capacity range for an IOPS profile or custom range. For more information, see [Volume capacity and IOPS limitations](#exp-vols-capacity-IOPS-limitations).
{:note}

IOPS are automatically adjusted for tiered profiles, based on the size of the volume. For example, if you expand a volume that was created by using a 5 IOPS/GB profile from the original size of 251 GB to the expanded size of 1,000 GB, it has a max IOPS of 5,000 IOPS (1,000 GB capacity _x_ 5 IOPS). Because a 5 IOPS/GB volume can potentially expand to 9,600 GB, the max IOPS would adjust to 48,000 IOPS. While the volume capacity is immediately changed, to realize increased IOPS, you must restart the instance.

Volumes that are created from a [Custom profile](/docs/vpc?topic=vpc-block-storage-profiles#custom) can be expanded within their custom IOPS range. Depending on the range you originally set, this can be up to 16,000 GB. IOPS remain constant at the level you set when you created the custom volume and can't be increased.

You can monitor the progress of your volume expansion from the UI or CLI. You can also use the [Activity Tracker](/docs/vpc?topic=vpc-at-events) to verify that the volume was expanded. After a volume is expanded, you can't reduce capacity. 

## Expandable volume requirements
{: #exp-vol-requirements}

You must meet these requirements to increase a volume's capacity.

* The volume must be a data volume, which are expanded based on their predefined IOPS profile or custom IOPS range. 
* The volume must be attached to a virtual server instance.
* The volume must be in an _available_ state.
* The instance must be powered on and in a _running_ state.

## Limitations
{: #expandable-volume-limitations}

The following limitations apply in this Beta release.

### General limitations
{: #exp-vols-general-limitations}

To increase volume capacity, these limitations apply:

* You can expand data volumes only; boot volumes can't be expanded.
* To expand volume capacity, the volume must be attached to a virtual server instance.
* The volume must be in an _available_ state with the instance powered on and in a _running_ state.
* You can't expand a volume that is at its maximum capacity for its [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers-beta) or [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) volume range.
* Your account must allow creating volumes with capacities greater than 2,000 GB. To create greater capacity volumes, you must request Beta access.
* Your account must be in a region that supports this feature. For this Beta release, they are US South, US East, London, and France.

### Volume capacity and IOPS limitations
{: #exp-vols-capacity-IOPs-limitations}

* Volumes with a capacity of 249 GB and less can be expanded up to 250 GB only.
* Volumes with a capacity of 251 GB and more can expand to 16,000 GB, with the following restrictions: 
  * If the volume was created by using an [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers-beta) that limits capacity to less than 16,000 GB, it can expand only to the allowed capacity for that tier.
  * If the volume is a [custom volume](/docs/vpc?topic=vpc-block-storage-profiles#custom) created in a lower range that doesn't allow expanding to 16,000 GB, it can expand only to its maximum capacity for that custom range.
  * Volumes can expand multiple times until maximum capacity is reached.
* IOPS increase to the maximum allowed by the IOPS tier profile. While volume resizing does not incur downtime (that is, you don't need to restart the instance and reattach the volume), for the increased IOPS to take effect, you must restart the virtual server instance.
* After you create a volume, can't change a volume's IOPS tier profile.
* You can't independently modify IOPS for a volume that is created from an IOPS tier profile. IOPS are adjusted when you expand a volume's capacity and then restart the instance.
* When you expand a volume created from a custom profile, the capacity is increased but the IOPS remain the same. You can't independently increase the IOPS.
* Maximum IOPS for a volume is capped at [48,000 IOPS](/docs/vpc?topic=vpc-block-storage-profiles#tiers-beta).
* After a volume is expanded, you can't reduce the size of the volume.

### Additional limitations
{: #exp-vols-additional-limitations}

* When a volume is in transition, for example, the volume is detached while the volume expansion is in progress, the volume remains in an _updating_ state until you reattach it to an instance. The volume expansion resumes and completes.
* When you delete an instance, volumes that are marked for auto-deletion are not deleted when volume expansion is underway. For more information, see [troubleshooting](/docs/vpc?topic=vpc-troubleshooting-block-storage#troubleshoot-topic-4).

## Expand block storage volumes in the UI
{: #expand-vpc-volumes-ui}
{:ui}

Follow these steps for expanding volume capacity in the UI:

Follow these steps for expanding volume capacity in the UI:

1. Navigate to the list of block storage volumes. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**. By default, block storage volumes display for all resource groups in your region.

2. In the list of all **Block storage for VPC volumes**, click the name of the volume you want to expand to see the volume details. 

   The volume that you select must be attached to a virtual server instance. In the list of volumes, its attachment type is _data_.
   {:note}

3. Alternatively, go to to a virtual server instance with an attached volume that you want to expand and select it from the list of attached volumes.

4. On the volume details page, locate **Size**. The current IOPS profile and volume size are displayed.

5. Click the pencil icon. Alternatively, click the **Actions** menu and select **Expand block storage volume**.

6. In the panel, increase the volume size in GB up to 16,000 GB.

   The maximum expansion size is based on the selected IOPS profile or custom volume settings. The UI indicates the maximum capacity for the selected profile. For a custom profile, you can expand the volume based on [sizing limits](#expandable-volume-limitations). When you increase the size of the volume, max IOPS and throughput are calculated for the expanded volume.

7. Review the estimated monthly order summary for your geography and new pricing.

8. If you're satisfied, click **Save and continue**.

Your new block storage allocation is available in a few minutes. 

**Note**: You can't change the volume to a smaller size after you expand its capacity. If your requirements change, you can expand the same volume again.

## Expand block storage volumes by using the CLI
{: #expand-vpc-volumes-cli}
{:cli}

From the CLI, use the `volume-update` command with the `--capacity` parameter to indicate the new size of the volume in GBs.

```
ibmcloud is volume-update VOLUME_ID --capacity CAPACITY_GB
```
{:pre}

This example expands the capacity of a volume that is created from a 5 IOPS/GB tier profile to the maximum size for that profile.

```bash
$ ibmcloud is volume-update 933c8781-f7f5-4a8f-8a2d-3bfc711788ee --capacity 9600
Updating volume 933c8781-f7f5-4a8f-8a2d-3bfc711788ee under account MyAccount 01 as user user1@mycompany.com...
ID                                      0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    demo-volume-update
Capacity                                9600
IOPS                                    1000
Profile                                 5IOPS-tier
Encryption Key                          -
Encryption                              provider managed
Status                                  available
Created                                 2021-06-14 10:09:28
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Zone                                    us-south-2
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Volume Attachment Instance Reference    Vdisk Name    Vdisk ID                                    Vdisk Type   Auto Delete
                                        Vdisk-data1   0738-fd146b1f-e1bb-4eab-ba78-3109e6bc3a2d   data         true   
    									Instance Name   Instance ID 
       									vsi-test1       0738-8b56da93-7990-4ccf-9dc5-5aee6a5f08f9
```
{:screen}

## Expand block storage volumes with the API
{: #expand-vpc-volumes-api}
{:api}

You can expand existing data volumes by calling the Virtual Private Cloud (VPC) API.

Make a `PATCH /volumes` request to increase the capacity of a volume that is attached to an instance. 

You can't update the name of the volume and expand capacity in the same `PATCH /volumes` request. Make two separate `PATCH /volumes` requests.
{:note}

This example call expands a volume with a capacity of 50 GB to 250 GB.

```
curl -X PATCH \
 "$vpc_api_endpoint/v1/volumes/$volume_id?version=2021-06-17&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "capacity": 250
    }'
```
{: codeblock}

The volume status shows `updating` while the volume is being expanded. The current capacity is shown.

```
{
	"capacity": 50,
	"created_at": "2021-06-17T09:46:43.000Z",
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
{:codeblock}

When the volume expansion completes, the new value displays and the volume status is `available`.

```
{
	"capacity": 250,
	"created_at": "2021-06-17T09:46:43.000Z",
	"crn": "crn:[...]",
	"encryption": "provider_managed",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/2d1bb5a8-40a8-447a-acf7-0eadc8aeb054",
    "id": "2d1bb5a8-40a8-447a-acf7-0eadc8aeb054",
	"IOPS": 100,
	"name": "my-volume-1",
    "profile": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
		"name": "general-purpose"
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
{:codeblock}

## Next steps
{: #next-step-expandable-volumes}

Create more volumes or manage your existing block storage volumes.

* [Creating block storage volumes by using the UI](/docs/vpc?topic=vpc-creating-block-storage)
* [Managing block storage volumes by using the UI](/docs/vpc?topic=vpc-managing-block-storage)
