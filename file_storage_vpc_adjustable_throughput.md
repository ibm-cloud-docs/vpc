---

copyright:
  years: 2025, 2025
lastupdated: "2025-11-20"

keywords: file share, regional, file storage, bandwidth, bandwidth

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Adjusting bandwidth limit for a regional file share
{: #file-storage-adjusting-bandwidth}

[Select availability]{: tag-green} Customers with special access to preview the new regional file share offering can use the `rfs` profile to create file shares with regional data availability and adjustable bandwidth values.

For regional file shares that are provisioned with the `rfs` profile, you can increase or decrease the bandwidth limit to meet your performance needs. The maximum bandwidth for any file share with the `rfs` profile is 8192 Mbps (1024 MBps). This bandwidth value represents the maximum allowed combined throughput for read and write operations. The minimum bandwidth value of the share is 800 Mbps.
{: shortdesc}

With this feature, you can increase or decrease your share's bandwidth limit in the console, from the CLI, or with the API. To change this attribute, the share must be in an _available_ state. Your user authorization is verified before the bandwidth limit is adjusted.

Billing for an updated file share is automatically updated. The prorated difference of the new price is added to the current billing cycle. The new full amount is then billed in the next billing cycle.

## Adjusting bandwidth in the console
{: #adjust-fs-bandwidth-ui-block}
{: ui}

1. Go to the list of all file shares. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.
1. Click the name of a file share to see the details page.
1. Locate the **Profile** section, and click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit"), or use the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Edit bandwidth**.
1. In the side panel, you can specify a file share bandwidth limit between the minimum value of 800 and the maximum of 8192 Mbps.
1. Review the estimated monthly order summary.
1. If you're satisfied, click **Save and continue**.

## Adjusting bandwidth from the CLI
{: #adjust-fs-bandwidth-cli-block}
{: cli}

From the CLI, use the `ibmcloud is share-update` command with the `--bandwidth` option to indicate the new bandwidth value for a custom profile.

```sh
ibmcloud is share-update share_ID --bandwidth BANDWIDTH
```
{: pre}

This example shows an increase of the bandwidth limit to 3,000 Mbps.

```sh
ibmcloud is share-update my-file-share --bandwidth 2000
```
{: pre}

```sh
Updating file share my-file-share under account Test Account as user test.user@ibm.com...

   ID                                 r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6
   Name                               my-file-share
   CRN                                crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6
   Lifecycle state                    stable
   Access control mode                security_group
   Accessor binding role              none
   Allowed transit encryption modes   stunnel,none 
   Zone                               us-south-1
   Profile                            rfs
   Size(GB)                           1000
   IOPS                               35000
   Bandwidth                          3000
   User Tags                          docs:test
   Encryption                         provider_managed   
   Mount Targets                      ID                                          Name
                                      r006-dd497561-c7c9-4dfb-af0a-c84eeee78b61   my-cli-share-mount-target-1

   Resource group                     ID                                 Name
                                      db8e8d865a83e0aae03f25a492c5b39e   Default

   Created                            2025-09-23T22:15:15+00:00
   Replication role                   none
   Replication status                 none
   Replication status reasons         Status code   Status message
                                      -             -
   Snapshot count                     0
   Snapshot size                      0
   Source snapshot                    -  
   Allowed Access Protocols           nsf4   
   Availability Mode                  zonal   
   Storage Generation                 2    
```
{: screen}

File shares with regional availability serve data in every zone of the region. The command response shows the first zone of the region by default.

For more information about available command options, see [`ibmcloud is share-update`](/docs/cli?topic=cli-vpc-reference#share-update).

## Adjusting bandwidth with the API
{: #adjust-fs-bandwidth-api-block}
{: api}

You can adjust bandwidth for existing data shares by calling the VPC API. Make a `PATCH /shares` request and specify the `bandwidth` parameter to adjust the bandwidth limit.

You can't update the name of the share, increase the size, adjust IOPS, and adjust bandwidth limit in the same `PATCH /shares` request. Make separate `PATCH /shares` requests.
{: important}

This example shows the bandwidth value increased to 3,000 Mbps.

```sh
curl -X PATCH \
 "$vpc_api_endpoint/v1/shares/$share_id?version=2025-09-23&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "bandwidth": 3000
    }'
```
{: codeblock}

The file share status shows `updating` while the bandwidth is being adjusted. The preset bandwidth is shown until after the update is complete.

```json
{
	"bandwidth": 400,
	"created_at": "2025-09-23T09:46:43.000Z",
	"crn": "crn:v1:bluemix:public:is:us-east-1:a/<Acc id>::share:<file shareID>",
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

## Adjusting bandwidth limit with Terraform
{: #adjust-vpc-bandwidth-terraform-block}
{: terraform}

To modify the IOPS of a share, use the `ibm_is_share` resource. When applied, the following example updates the file share bandwidth limit to 4,000 Mbps.

```terraform
resource "ibm_is_share" "storage" {
   name         = "demo-share-update"
   size         = 8000
   bandwidth    = 4000
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.
