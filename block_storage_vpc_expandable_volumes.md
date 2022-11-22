---

copyright:
  years: 2020, 2022
lastupdated: "2022-11-22"

keywords: block storage, boot volume, data volume, volume, data storage, virtual server instance, instance, expandable volume

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Increasing block storage volume capacity
{: #expanding-block-storage-volumes}

For {{site.data.keyword.block_storage_is_short}} data volumes, when you create and attach a volume to a virtual server instance, you can later increase the size of the volume in the UI, from the CLI, or with the API.
{: shortdesc}

## Expand block storage volumes in the UI
{: #expand-vpc-volumes-ui}
{: ui}

Follow these steps to expand volume capacity:

1. Navigate to the list of block storage volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**. By default, block storage volumes display for all resource groups in your region.

2. In the list of all **Block storage for VPC volumes**, click the name of the volume you want to expand to see the volume details.

   The volume that you select must be attached to a virtual server instance. In the list of volumes, its attachment type is _data_.
   {: note}

3. Alternatively, go to a virtual server instance with an attached volume that you want to expand and select it from the list of attached volumes.

4. On the volume details page, locate **Size**. The current IOPS profile and volume size are displayed.

5. Click the pencil icon. Alternatively, click the **Actions** menu and select **Expand block storage volume**.

6. In the panel, increase the volume size in GB up to 16,000 GB.

   The maximum size that you can expand to is based on the selected IOPS profile or custom volume settings. The UI indicates the maximum capacity for the selected profile. For a custom profile, you can expand the volume based on [sizing limits](#expandable-volume-limitations). When you increase the size of the volume, max IOPS and throughput are calculated for the expanded volume.

7. Review the estimated monthly order summary for your geography and new pricing.

8. If you're satisfied, click **Save and continue**.

Your new block storage allocation is available in a few minutes.

You can't change the volume to a smaller size after you expand its capacity. If your requirements change, you can expand the same volume again.
{: note}

## Expand block storage volumes from the CLI
{: #expand-vpc-volumes-cli}
{: cli}

From the CLI, use the `volume-update` command with the `--capacity` parameter to indicate the new size of the volume in GBs.

```zsh
ibmcloud is volume-update VOLUME_ID --capacity CAPACITY_GB
```
{: pre}

This example expands the capacity of a volume that is created from a 5 IOPS/GB tier profile to the maximum size for that profile.

```bash
$ ibmcloud is volume-update 933c8781-f7f5-4a8f-8a2d-3bfc711788ee --capacity 9600
Updating volume 933c8781-f7f5-4a8f-8a2d-3bfc711788ee under account MyAccount 01 as user user1@mycompany.com...
ID                                      0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    demo-volume-update
Capacity                                9600
IOPS                                    1000
Profile                                 5iops-tier
Encryption Key                          -
Encryption                              provider managed
Status                                  available
Created                                 2022-02-25 10:09:28
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Zone                                    us-south-2
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Volume Attachment Instance Reference    Vdisk Name    	Vdisk ID
					Vdisk Type   	Auto Delete
                                        Vdisk-data1  	0738-fd146b1f-e1bb-4eab-ba78-3109e6bc3a2d
					data         	true
    					Instance Name   Instance ID
       					vsi-test1       0738-8b56da93-7990-4ccf-9dc5-5aee6a5f08f9
```
{: screen}

## Expand block storage volumes with the API
{: #expand-vpc-volumes-api}
{: api}

You can expand existing data volumes by calling the Virtual Private Cloud (VPC) API.

Make a `PATCH /volumes` request to increase the capacity of a volume that is attached to an instance.

You can't update the name of the volume and expand capacity in the same `PATCH /volumes` request. Make two separate `PATCH/volumes` requests.
{: note}

This example call expands a volume with a capacity of 50 GB to 250 GB.

```curl
curl -X PATCH \
 "$vpc_api_endpoint/v1/volumes/$volume_id?version=2022-02-25&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "capacity": 250
    }'
```
{: codeblock}

The volume status shows `updating` while the volume is being expanded. The current capacity is shown.

```json
{
	"capacity": 50,
	"created_at": "2022-02-25T09:46:43.000Z",
	"crn": "crn:v1:staging:public:is:us-south-1:a/<Acc id>::volume:<Volume ID>",
    .
    .
    .
	"status": "updating",
    .
    .
    .
}
```
{: codeblock}

When the volume expansion completes, the new value displays, and the volume status is `available`.

```json
{
	"capacity": 250,
	"created_at": "2022-02-25T09:46:43.000Z",
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
{: codeblock}

## Next steps
{: #next-step-expandable-volumes}

Create more volumes or manage your existing block storage volumes.

* [Creating block storage volumes](/docs/vpc?topic=vpc-creating-block-storage).
* [Managing block storage volumes](/docs/vpc?topic=vpc-managing-block-storage).

Optionally, [increase the capacity of boot volumes](/docs/vpc?topic=vpc-resize-boot-volumes).
