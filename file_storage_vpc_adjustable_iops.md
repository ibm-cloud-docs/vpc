---

copyright:
  years: 2022, 2025
lastupdated: "2025-11-18"

keywords: file share, file storage, IOPS, performance needs, adjust IOPS

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Adjusting IOPS for a zonal file share
{: #file-storage-adjusting-iops}

For zonal file shares, you can increase or decrease IOPS to meet your performance needs. Adjust IOPS by specifying a different value when you use the `dp2` or `custom` profiles, or by switching to another `tiered` share profile (IOPS are adjusted within the selected profile). The process of adjusting the IOPS causes no outage or lack of access to the storage. You can use the UI, the CLI, the API, or Terraform to adjust IOPS. You can adjust the file share's IOPS multiple times up to its maximum limit or reduce IOPS to its minimum limit.
{: shortdesc}

Billing for an updated share is automatically updated. The prorated difference of the new price is added to the current billing cycle. The new full amount is then billed in the next billing cycle.

[Select availability]{: tag-green} You can't modify the IOPS value of regional file shares that are created with the `rfs` profile. However, you can adjust the bandwidth value to fine-tune the share's performance.

## Adjustable IOPS concepts
{: #fs-adj-iops-concepts}

You can adjust IOPS for your zonal file shares to tailor performance to meet your requirements.

- For example, you might find that an application scaled such that the current IOPS limit is now a performance bottleneck. You can change the performance characteristics of the existing file share by increasing IOPS.

- In another scenario, you might want to initially set your file storage at a higher IOPS level to expedite a data upload. After the upload completes, you can reset storage to 5 IOPS/GB for normal operations. Also, you might want to increase your file share's IOPS during peak times for your application and decrease IOPS during off-peak hours.

When your share uses the [dp2](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile) profile, share IOPS can range from 100 to 96,000. You can specify your IOPS value with minimal restrictions. IOPS can be adjusted multiple times until the maximum or minimum limit is reached.

   For the 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share that is accessed by one client is limited to 48,000 IOPS.
   {: note}

When your file share uses the [Deprecated]{: tag-red} [custom](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#fs-custom) share profile, the IOPS range is based on the file share size. 
* For example, for a file share that's 25 GB, you can increase IOPS anywhere in the range 100-1,000 IOPS with a custom profile. If you later [increase the size of a file share](/docs/vpc?topic=vpc-file-storage-expand-capacity) to the next highest band, you can increase the IOPS again. IOPS can be adjusted multiple times until the maximum or minimum limit is reached.
* Or you can switch from a custom profile to an IOPS tier profile. The IOPS tier selection is restricted by the file share size. When you move from a custom profile to any tiered profile, billing is updated, and the IOPS is adjusted according to the profile.

When your file share uses one of the [Deprecated]{: tag-red} [tiered share profiles](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#fs-v2-profiles), you can adjust your IOPS limit by:

* Switching from an IOPS tier profile to the `dp2` or the `custom` profile. In this case, the file share size restricts the IOPS range that you select. 
   - For example, if you're adjusting IOPS for file share with a 3 IOPS/GB profile and it's 12,000 GB, you can adjust the IOPS by using a custom or dp2 profile. It's possible because both support a share size of 12,000 GB. 
   - If the file share size was greater than 16,000 GB, the custom profile would not support adjusting IOPS, but the dp2 profile would because it supports share sizes to 32,000 GB. The IOPS tier maximum sizes are 32,000 GB for 3 IOPS/GB, 9,600 GB for 5 IOPS/GB, and 4,800 GB for 10 IOPS/GB. 
   
   When you move from a tiered profile to a custom or dp2 profile, billing is update based on the specified IOPS.
   {: important}

* Selecting a different [IOPS tier](/docs/vpc?topic=vpc-file-storage-profiles#fs-tiers). IOPS is adjusted within the tier. The degree to which IOPS can be increased is determined by the maximum that is allowed by the file share's [IOPS tier profile](/docs/vpc?topic=vpc-file-storage-profiles). You can adjust IOPS for an IOPS tier based on share size or select the next profile that allows for increased performance.

   The maximum IOPS for a file share for a 3 IOPS/GB tier profile is 96,000 IOPS. All other profiles are capped at 48,000 IOPS. For 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share that is accessed by one instance is limited to 48,000 IOPS.
   {: note}   

To adjust a file share's IOPS, the file share must be in a _stable_ state. Your user authorization is verified before IOPS is adjusted.

You can monitor the progress of your file share's IOPS change from the UI or CLI. You can also check the [Activity tracking events](/docs/vpc?topic=vpc-at_events) to verify that the IOPS were adjusted.

## Limitations
{: #adjustable-iops-limitations-file}

The following limitations apply.

* To adjust IOPS, the file share must be in a _stable_ state.
* A [dp2 profile](/docs/vpc?topic=vpc-file-storage-profiles&nterface=ui#dp2-profile) can expand to the maximum allowed by the dp2 band. You can't switch dp2 IOPS bands unless you increase the file share size and then move to a higher band.
* For a file share created with an [IOPS tier profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#fs-tiers) to increase or decrease IOPS, select a different profile for the file share size. If the file share size exceeds that of the new IOPS tier profile, you can't change the profile.
* A [custom IOPS profile](/docs/vpc?topic=vpc-file-storage-profiles#fs-custom) can expand to the maximum allowed by the custom band. You can't switch custom bands unless you increase the file share size and then move to a higher band.
* Adjusting IOPS by moving between profiles is restricted by the file share size.
* The maximum IOPS for a file share for all profiles is [96,000 IOPS](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#fs-tiers). For 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share in an instance is limited to 48,000 IOPS.

## Adjusting IOPS in the console
{: #adjust-vpc-iops-ui-file}
{: ui}

Follow these steps to adjust IOPS by selecting a new IOPS tier or custom IOPS band:

1. Go to the list of file shares. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**. By default, file shares display for all resource groups in your region.

2. In the **File shares for VPC** list page, click the name of a file share to see its details.

3. On the file share details page, locate **Profile** and click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") or use the **Actions** menu and select **Edit IOPS profile**. Volumes must be attached to a virtual server instance for these actions.

4. In the side panel, adjust IOPS as follows:

   * For the dp2 or custom profile, the current IOPS value is shown and file share size. Enter a new IOPS value within the allowable range for that size.
   
   * For an IOPS tier, select a different tier from the menu. For example, you might have a 3 IOPS/GB general-purpose profile that you want to increase to a 5 IOPS/GB profile.


5. Review the estimated monthly order summary for your geography and new pricing.

6. If you're satisfied, click **Save and continue**. Your new IOPS allocation is realized when you restart the instance.

## Adjusting IOPS from the CLI
{: #adjust-vpc-iops-cli-file}
{: cli}

### Adjusting IOPS for a Custom or dp2 profile
{: #adjust-iops-cli-file}

From the CLI, use the `share-update` command with the `--iops` property to indicate the new IOPS size for a custom or dp2 profile. The IOPS that you indicate must be within the range for the size of the file share. For more information, see the [dp2](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile) or the [Custom IOPS profile](/docs/vpc?topic=vpc-file-storage-profiles#fs-custom).

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Locate your share from the CLI by listing your file shares in the region with the `ibmcloud is shares` command.

   ```sh
   ibmcloud is shares
   ```
   {: pre}

   ```sh
   Listing shares in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
   
   ID                                          Name                    Lifecycle state   Zone         Profile   Size(GB)   Resource group   Replication role   
   r006-dc6a644d-c7da-4c91-acf0-d66b47fc8516   my-replica-file-share   stable            us-south-1   dp2       1500       Default          replica   
   r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a   my-source-file-share    stable            us-south-2   dp2       1500       Default          source   
   r006-6d1719da-f790-45cc-9f68-896fd5673a1a   my-replica-share        stable            us-south-3   dp2       1500       Default          replica   
   r006-925214bc-ded5-4626-9d8e-bc4e2e579232   my-new-file-share       stable            us-south-2   dp2       500        Default          none   
   r006-b1707390-3825-41eb-a5bb-1161f77f8a58   my-vpc-file-share       stable            us-south-2   dp2       1000       Default          none   
   r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   my-file-share           stable            us-south-2   dp2       1500       Default          source 
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

1. Use the `ibmcloud is share-update` command to increase or decrease the IOPS of your file share.

   ```sh
   ibmcloud is share-update my-file-share --iops 2000
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
   Size(GB)                     1000   
   IOPS                         2000   
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
   Allowed Access Protocols     nfs4    
   Availability Mode            zonal   
   Bandwidth(Mbps)              1    
   Storage Generation           1  
   ```
   {: screen}

For more information about the command options, see [`ibmcloud is share-update my-file-share`](/docs/vpc?topic=vpc-vpc-reference#share-update).

### Adjusting IOPS by specifying a different IOPS tier profile
{: #adjust-profile-cli-file}

These instructions are for the previous generation of file share profiles (general purpose, 5-iops, 10-iops, or custom). New file shares can be provisioned with only the dp2 profile. To access the latest features, you must change the IOPS profile of your share to dp2.
{: deprecated}

From the CLI, use the `ibmcloud is share-update` command with the `--profile` property and indicate the name or href of the IOPS tier profile.

The following example changes a 3 IOPS/GB profile to a 5 IOPS/GB profile. In this case, the file share can't exceed 9,600 GB to move to the higher profile.

```sh
ibmcloud is share-update my-file-share --profile tier-5iops
```
{: pre}

```sh
Updating file share my-file-share under account VPC1 as user user@mycompany.com...

ID                           ba7c7c8a-c111-4f54-a7fe-bb6d3d66eb2a
Name                         my-file-share
CRN                          crn:v1:bluemix:public:is:us-south-1:a/a1234567::share:ba7c7c8a-c111-4f54-a7fe-bb6d3d66eb2a
Lifecycle state              updating
Access control mode          security_group   
Accessor binding role        none 
Zone                         us-south-1
Profile                      tier-5iops
Size(GB)                     100
IOPS                         3000
User Tags                    docs:test
Encryption                   provider_managed
Mount targets                ID                          Name   VPC ID   VPC Name
                             No mount targets found.

Resource group               ID                                     Name
                             7f1645c5-8afa-4a7e-860d-3df563e0aa8d   Default

Created                      2023-02-26T20:01:18+05:30

Snapshot count               0
Snapshot size                0        
Source snapshot              -
```
{: screen}                    

For more information about the command options, see [`ibmcloud is share-update my-file-share`](/docs/vpc?topic=vpc-vpc-reference#share-update).

## Adjusting IOPS with the API
{: #adjust-vpc-iops-api-file}
{: api}

You can adjust IOPS for existing data file shares by calling the Virtual Private Cloud (VPC) API.

### Adjusting IOPS for the dp2 or the custom profile
{: #adjust-iops-api-file}

Make a `PATCH /shares` request and specify the `iops` property to adjust the IOPS within the allowable range for a custom pr dp2 profile.

You can't update the name of the file share and adjust IOPS in the same `PATCH /shares` request. Make two `PATCH /shares` requests.
{: note}

The following example shows an increase of 100 IOPS to 3,000 IOPS for a 100 GB file share based on a 100 - 499 custom profile. The IOPS range for this custom band is 100 - 6,000 IOPS. For more information about available IOPS ranges, see [File storage profile overview](/docs/vpc?topic=vpc-file-storage-profiles&interface=api).

```sh
curl -X PATCH \
 "$vpc_api_endpoint/v1/shares/$share_id?version=2023-08-086&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "iops": 3000
    }'
```
{: codeblock}

The file share status shows `updating` while the IOPS is being adjusted. The current IOPS is shown until you restart the instance. In the following example, the IOPS value is adjusted from 100.

```json
{
   "created_at": "2023-08-08T22:58:49.000Z",
   "crn": "crn:[...]",
   "encryption": "provider_managed",
   "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-a0c07083-f411-446c-9316-7b08d6448c86",
   "id": "r006-a0c07083-f411-446c-9316-7b08d6448c86",
   "iops": 100,
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

When the IOPS expansion completes, restart the instance. The new value is displayed, and the file share status is `stable`.

```json
{
  "created_at": "2023-08-08T22:58:49.000Z",
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
  "access_control_mode": "security-group",
  "allowed_transit_encryption_modes": ["none", "user-managed"],
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
      "id": "r006-1b5571cb-536d-48d0-8452-81c05c6f7b80",
      "name": "my-mount-target",
      "resource_type": "share_target",
      "vpc": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/r006-12bb28fc-856d-4902-813b-dc065d1ed084",
        "id": "12bb28fc-856d-4902-813b-dc065d1ed084",
        "name": "my-vpc",
        "resource_type": "vpc"
      }
    }
  ],
  "snapshot_count": 10, 
  "snapshot_size": 10,
  "user_tags": [],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1",
    "resource_type": "zone"
  }
}
```
{: codeblock}

### Adjusting IOPS by specifying a different IOPS tier profile
{: #adjust-profile-api-file}

These instructions are for the previous generation of file share profiles (general purpose, 5-iops, 10-iops, or custom). New file shares can be provisioned with only the dp2 profile. To access the latest features, you must change the IOPS profile of your share to dp2.
{: deprecated}

Make a `PATCH /shares` request and specify the `profile` property and indicate the name or href of the IOPS tier profile.

The following example changes a 3 IOPS/GB profile to a 5 IOPS/GB profile. In this case, the file share can't exceed 9,600 GB to move to the higher profile.

```sh
curl -X PATCH \
 "$vpc_api_endpoint/v1/shares/$share_id?version=2023-07-18&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "profile": "5iops-tier"
    }'
```
{: pre}

## Adjusting IOPS with Terraform
{: #adjust-vpc-iops-terrafom-file}
{: terraform}

You can adjust IOPS for existing data file shares in Terraform.

### Adjusting IOPS for a Custom or dp2 profile
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

### Adjusting IOPS by specifying a different IOPS tier profile
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
