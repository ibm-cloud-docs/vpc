---

copyright:
  years: 2025
lastupdated: "2025-06-12"

keywords: Block Storage for VPC, boot volume, data volume, volume, data storage, virtual server instance, instance, adjustable volume, throughput, bandwidth

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Adjusting throughput limit of a {{site.data.keyword.block_storage_is_short}} volume
{: #adjusting-volume-throughput}

Customers with special approval to preview the defined performance profile can use the `sdp` profile in the Dallas (`us-south`), Frankfurt (`eu-de`), London (`eu-gb`), Madrid (`eu-es`), Osaka (`jp-osa`), Sydney (`au-syd`), Tokyo (`jp-tok`), Toronto (`ca-tor`), and Washington (`us-east`) regions to specify custom capacity, custom throughput limit, and custom IOPS for their volumes.
{: preview}

For {{site.data.keyword.block_storage_is_full}} volumes that are provisioned with the `sdp` profile, you can increase or decrease the throughput limit to meet your performance needs. The maximum throughput for any volume with the `sdp` profile is 1024 MBps (8192 Mbps). The minimum throughput value is 125 MBps (1000 Mbps). The adjustment causes no outage or lack of access to the storage.
{: shortdesc}

With this feature, you can increase or decrease your volume's throughput limit in the console, from the CLI, with the API, or Terraform. To change this attribute, the volume must be in an _available_ state. It can be attached to a running instance, but it's not a requirement. Your user authorization is verified before the bandwidth limit is adjusted.

You can check the [Activity Tracking events](/docs/vpc?topic=vpc-at_events) to verify that the throughput limit was adjusted.

Billing for an updated volume is automatically updated. The prorated difference of the new price is added to the current billing cycle. The new full amount is then billed in the next billing cycle.

## Before you begin
{: #adjust-vpc-throughput-block-prereq}

Review the following table to make sure that the throughput limit you want is available for your volume. To set the maximum bandwidth value on the volume, it must have at least 151 GB capacity. Increasing your capacity and adjusting throughput are two separate operations.

| Capacity range (GB) | Min Throughput (mbps) | Max Throughput (mbps)  |
|---------------------|-----------------------|------------------------|
| 1 - 20              | 1000                  |          1000          |
| 21 - 50             | 1000                  |          4096          |
| 51 - 80             | 1000                  |          6144          |
| 81 - 100            | 1000                  |          8192          |
| 101 - 130           | 1000                  |          8192          |
| 131 - 150           | 1000                  |          8192          |
| 151 - 32000         | 1000                  |          8192          |
{: caption="Available capacity and performance ranges for the SSD defined performance profile." caption-side="bottom"}

## Adjusting throughput in the console
{: #adjust-vpc-throughput-ui-block}
{: ui}

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**. By default, {{site.data.keyword.block_storage_is_short}} volumes display for all resource groups in your region.
1. In the list of all **{{site.data.keyword.block_storage_is_short}} volumes**, click the name of the volume to see the volume details.

   Alternatively, go to the virtual server instance's detailss page and locate the volume that you want to update from the list of attached volumes.
   {: tip}

1. On the volume details page, locate **Profile** and click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") or use the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Edit throughput**.
1. In the side panel, you can specify a volume bandwidth limit in the range of 125-1024 MBps (1000-8192 Mbps).
1. Review the estimated monthly order summary for your geography and new pricing.
1. If you're satisfied, click **Save and continue**.

Your new bandwidth allocation is realized when you restart the instance.

## Adjusting throughput from the CLI
{: #adjust-vpc-throughput-cli-block}
{: cli}

From the CLI, use the `ibmcloud is volume-update` command with the `--bandwidth` option to indicate the new throughput value for a custom profile. The new value must be in the range of 1000-8192 Mbps. For more information, see [SSD defined performance profile](/docs/vpc?topic=vpc-block-storage-profiles&interface=cli#defined-performance-profile).

```sh
ibmcloud is volume-update VOLUME_ID --bandwidth BANDWIDTH
```
{: pre}

This example shows an increase of the throughput limit from 1000 Mbps to 3,000 Mbps.

```sh
$ ibmcloud is volume-update 933c8781-f7f5-4a8f-8a2d-3bfc711788ee --throughput 3000
Updating volume 933c8781-f7f5-4a8f-8a2d-3bfc711788ee under account Test Account as user test.user@ibm.com...
ID                                      0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    demo-volume-update
Capacity                                100
IOPs                                    3000
Bandwidth                               3000
Profile                                 sdp
Encryption Key                          -
Encryption                              provider managed
Status                                  available
Created                                 2024-10-23 10:09:28
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Zone                                    us-east-2
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Volume Attachment Instance Reference    Vdisk Name    Vdisk ID                                    Vdisk Type   Auto Delete
                                        Vdisk-data1   0738-fd146b1f-e1bb-4eab-ba78-3109e6bc3a2d   data         true
    									Instance Name   Instance ID
       									vsi-test1       0738-8b56da93-7990-4ccf-9dc5-5aee6a5f08f9
```
{: codeblock}

For more information about available command options, see [`ibmcloud is volume-update`](/docs/cli?topic=cli-vpc-reference#volume-update).

## Adjusting throughput with the API
{: #adjust-vpc-throughput-api-block}
{: api}

You can adjust throughput for existing data volumes by calling the Virtual Private Cloud (VPC) API. Make a `PATCH /volumes` request and specify the `bandwidth` parameter to adjust the throughput limit.

You can't update the name of the volume, increase the size, adjust IOPS, and adjust throughput limit in the same `PATCH /volumes` request. Make separate `PATCH /volumes` requests.
{: important}

This example shows an increase of 1000 Mbps to 3,000 Mbps. The throughput range for the `sdp` profile is 1000-8192 Mbps.

```sh
curl -X PATCH \
 "$vpc_api_endpoint/v1/volumes/$volume_id?version=2025-03-25&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "bandwidth": 3000
    }'
```
{: codeblock}

The volume status shows `updating` while the throughput is being adjusted. The current bandwidth is shown until you restart the instance.
```json
{
	"bandwidth": 100,
	"created_at": "2024-10-23T09:46:43.000Z",
	"crn": "crn:v1:bluemix:public:is:us-east-1:a/<Acc id>::volume:<Volume ID>",
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

When the operation completes, restart the instance. The new bandwidth value is displayed, and the volume status is `available`.

## Adjusting throughput limit with Terraform
{: #adjust-vpc-throughput-terraform-block}
{: terraform}

To modify the IOPS of a volume, use the `ibm_is_volume` resource. When applied, the following example updates the volume throughput limit to 4,000 Mbps.

```terraform
resource "ibm_is_volume" "storage" {
  name         = "demo-volume-update"
  size         = 8000
  iops         = 24000
  bandwidth    = 4000
  zone         = "us-east-2"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume){: external}.

## Next steps
{: #next-step-adjustable-throughput-block}

Monitor your {{site.data.keyword.block_storage_is_short}} volumes:
- [Monitoring Block Storage for VPC health states, volume status, and metrics](/docs/vpc?topic=vpc-block-storage-vpc-monitoring)
