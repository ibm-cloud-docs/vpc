---

copyright:
  years: 2022, 2025
lastupdated: "2025-09-05"

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

Capacity can be increased for file shares that are in a `stable` state. Your user authorization is verified before the file share is expanded. You can use the UI, the CLI, the API, or Terraform to increase file share capacity. You can expand a file share multiple times up to its maximum capacity limit. After you expanded the file share, you can't reduce the capacity.

Expanded capacity is determined by the maximum that is allowed by the file share profile. File shares that are created from a [Custom](/docs/vpc?topic=vpc-file-storage-profiles#fs-custom) profile or a [dp2](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile) profile can be expanded within the allowable IOPS range for that file share size.

File shares that are created from an [IOPS tier profile](/docs/vpc?topic=vpc-file-storage-profiles#fs-tiers) can be expanded to the maximum size for its IOPS tier:

* A general-purpose, 3 IOPS/GB profile can be expanded up to 32,000 GB.
* A 5 IOPS/GB profile can be expanded up to 9,600 GB.
* A 10 IOPS/GB profile can be expanded up to 4,800 GB.

IOPS is automatically adjusted for tiered file share profiles, based on the size of the file share. For example, if you expand a share with a 5 IOPS/GB profile from 250 GB to 1,000 GB, it has a maximum IOPS of 5,000 IOPS (1,000 GB capacity _x_ 5 IOPS). Because a 5 IOPS/GB file share can potentially expand to 9,600 GB, the max IOPS is adjusted to 48,000 IOPS. The capacity and the IOPS are immediately changed and you don't need to restart the instance.

You can monitor the progress of your file share expansion from the UI or CLI. You can also check the [Activity tracking events](/docs/vpc?topic=vpc-at_events) to verify that the file share was expanded. After a file share is expanded, you can't reduce capacity.

## Requirements
{: #expandable-share-prereqs}

The file share must be in a `stable` state before you can request the capacity to be increased.

## Limitations
{: #expandable-share-limitations}

The following limitations apply to this release.

* File shares can expand, with the following restrictions:
    * If the file share was created with a [Tiered profile](/docs/vpc?topic=vpc-file-storage-profiles#fs-tiers) that limits capacity to less than 32,000 GB, it can expand only to the allowed capacity for that tier.
    * If the file share was created with the [Custom profile](/docs/vpc?topic=vpc-file-storage-profiles#fs-custom) in an IOPS range that doesn't allow expanding to 16,000 GB, the share can expand only to the maximum capacity of the specified range.
    * If the file share was created with the [dp2 profile](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile) in an IOPS range that doesn't allow expanding to 32,000 GB, the share can expand only to the maximum capacity of the specified range.
    * File shares can expand multiple times until maximum capacity is reached.
* IOPS increase to the maximum allowed by the profile.
* You can't independently modify IOPS for a file share that was created from an IOPS tier profile. IOPS is adjusted when you expand capacity.
* When you expand a file share that was created from a custom or dp2 profile, the capacity is increased, but the IOPS remains the same unless you choose to [adjust the IOPS](/docs/vpc?topic=vpc-file-storage-adjusting-iops).
* [Beta]{: tag-cyan} When you expand a file share that was created with the `rfs` profile, the capacity and minimum bandwidth are increased.
* The maximum IOPS for a zonal file share is capped at 48,000 IOPS if it is accessed by a single host. For a zonal file share that is accessed by multiple hosts, IOPS can reach up to 96,000 IOPS.
* After a file share is expanded, you can't reduce its size.

## Expanding file share capacity in the console
{: #expand-vpc-shares-ui}
{: ui}

Follow these steps for expanding file share capacity in the console:

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.

2. The File Shares for VPC list page shows all file shares that were created in that zone. Click the name of the file share to get to the details page.

3. On the file shares details page, expand the **Actions** menu.

4. Select **Expand file share**. The Expand file share size panel shows the current size of the file share (in GBs), and the profile to which it is associated. Alternatively, you can also click the edit icon next to the file share size information.

5. Enter a larger file share size, based on the maximum allowed for the [file share profile](/docs/vpc?topic=vpc-file-storage-profiles). The default is the current size + 1 GB.

6. Review the estimated monthly order summary for your geography and new pricing.

7. If you're satisfied, click **Save and continue**.

Your new file storage allocation is available in a few minutes. If your requirements change, you can increase capacity again after the file share size is increased and when it's in `stable` state.

You can't change the file share to a smaller size after you expand its capacity.
{: important}

## Expanding file shares from the CLI
{: #expand-vpc-shares-cli}
{: cli}

To increase the capacity of a file share from the CLI, use the `share-update` command with the `--size` option and indicate the new size of the file share in GBs.

1. Locate your share from the CLI by listing your file shares in the region with the `ibmcloud is shares` command.

   ```sh
   ibmcloud is shares
   ```
   {: pre}

   ```sh
   Listing shares in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
   ID                                          Name                    Lifecycle state   Zone         Profile   Size(GB)   Resource group   Replication role   Accessor binding role   Snapshot count   Snapshot size   
   r006-a8d6af48-0c97-4c6b-bab1-fbefdc1e1e03   my-file-share           stable            us-south-2   dp2       10         defaults         none               none                    0                0   
   r006-aaf4bfe9-358c-4faa-a4ec-0b955090b940   my-file-share-2         stable            us-south-2   dp2       10         defaults         none               none                    0                0   
   r006-a60bfa90-a893-40ad-be34-28ab51a963f9   replica-dal-2           stable            us-south-2   dp2       10         defaults         replica            none                    0                0   
   r006-3f21e3c3-e12d-425f-ab77-810cabfde8df   source-dal-1            stable            us-south-1   dp2       10         defaults         source             none                    0                0   
   r006-455b601c-8fc1-4476-8771-4708c49c8ef7   my-replica-share-dal-1  stable            us-south-1   dp2       10         defaults         replica            none                    0                0   
   r006-4dadac27-cd17-42df-a5fe-1388705d33e0   my-source-share-dal-2   stable            us-south-2   dp2       10         defaults         source             none                    0                0   
   ```
   {: screen}

1. View the details of the file share that you want to modify with the `ibmcloud is share` command.

  ```sh
   ibmcloud is share my-file-share
   ```
   {: pre}

   ```sh
   Getting file share my-file-share under account Test Account as user test.user@ibm.com...
                                
   ID                           r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   
   Name                         my-file-share   
   CRN                          crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   
   Lifecycle state              stable   
   Access control mode          security_group   
   Accessor binding role        none   
   Zone                         us-south-2   
   Profile                      dp2   
   Size(GB)                     1000   
   IOPS                         1000
   User Tags                    docs:test
   Encryption                   provider_managed   
   Mount Targets                ID                                          Name
                                r006-dd497561-c7c9-4dfb-af0a-c84eeee78b61   my-cli-share-mount-target-1      
                                
   Resource group               ID                                 Name      
                                db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
   Created                      2023-10-18T22:15:15+00:00   
   Replication role             none   
   Replication status           none   
   Replication status reasons   Status code   Status message      
                                -             -      
   
   Snapshot count               0
   Snapshot size                0  
   Source snapshot              -               
   ```
   {: screen}

1. Run the `ibmcloud is share-update` command to increase the capacity of your file share.
   
   ```sh
   ibmcloud is share-update my-file-share --size 1500
   ```
   {: pre}

   ```sh
   Updating file share my-file-share under account Test Account as user test.user@ibm.com...
                                
   ID                           r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   
   Name                         my-file-share   
   CRN                          crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   
   Lifecycle state              updating   
   Access control mode          security_group  
   Accessor binding role        none  
   Zone                         us-south-2   
   Profile                      dp2   
   Size(GB)                     1500   
   IOPS                         1000   
   Encryption                   provider_managed   
   Mount Targets                ID                                          Name      
                                r006-dd497561-c7c9-4dfb-af0a-c84eeee78b61   my-cli-share-mount-target-1      
                                   
   Resource group               ID                                 Name      
                                db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
   Created                      2023-10-18T22:15:15+00:00   
   Latest job                   Job status   Job status reasons      
                                succeeded    -      
                                
   Replication share            ID                                          Name               Resource type      
                                r006-6d1719da-f790-45cc-9f68-896fd5673a1a   my-replica-share   share      
                                
   Replication role             source   
   Replication status           active   
   Replication status reasons   Status code   Status message      
                                -             -      

   Snapshot count               0
   Snapshot size                0 
   Source snapshot              -
   ```
   {: screen}

For more information about the command options, see [`ibmcloud is share-update my-file-share`](/docs/vpc?topic=vpc-vpc-reference#share-update).

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
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/199d78ec-b971-4a5c-a904-8f37ae710c63",
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
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/199d78ec-b971-4a5c-a904-8f37ae710c63",
  "id": "199d78ec-b971-4a5c-a904-8f37ae710c63",
  "iops": 12500,
  "lifecycle_state": "stable",
  "name": "share-name1",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/dp2",
    "name": "dp2",
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

Customers with special access to review the regional file share offering can use the Beta VPC API to expand their regional file shares. See the following example.

```sh
curl -X PATCH \
 "$vpc_api_endpoint/v1/shares/$share_id?version=2025-07-22&generation=2&maturity=beta" \
 -H "Authorization: $iam_token" \
 -d '{
      "size": 2500
    }'
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

* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-mount-RHEL)
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos)
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-mount-ubuntu)
