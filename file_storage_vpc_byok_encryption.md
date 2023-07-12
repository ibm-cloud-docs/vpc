---

copyright:
  years: 2022, 2023
lastupdated: "2023-06-20"

keywords: file share, customer-managed encryption, encryption, byok, KMS, Key Protect, Hyper Protect Crypto Services,

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating file shares with customer-managed encryption at rest
{: #file-storage-vpc-encryption}

By default, {{site.data.keyword.filestorage_vpc_short}} shares are encrypted with IBM-managed encryption. However, you can also create encrypted file shares by using a supported key management service to create or import your own root keys. After you specify the encryption type for a file share, you can't change it.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Frankfurt, London, Madrid, Dallas, Toronto, Washington, Sao Paulo, Sydney, Osaka, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Before you begin
{: #custom-managed-vol-prereqs-file}

To create file shares with customer-managed encryption, you must first provision a key management service, and create or import your customer root key (CRK). You must also [authorize access](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-volumes-prereqs) between {{site.data.keyword.filestorage_vpc_short}} and the key management service. When you complete these prerequisites, you can start creating file shares that use customer-managed encryption.

For more information, see [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-encryption-prereqs).

## Create file shares with customer-managed encryption in the UI
{: #fs-byok-encryption-ui}
{: ui}

Follow this procedure to specify customer-managed encryption when you create a file share.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File Shares**.

2. Click **Create**.

3. Enter the information that is described in the Table 1. While you create the share, specify the information for the mount target as well.

   | Field | Value |
   |-------|-------|
   | Name  | Choose a meaningful name for your file share. The share name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want. |
   | Resource Group | Specify a [resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups). Resource groups help organize your account resources for access control and billing purposes. |
   | Location | Choose the zone where you want to create the file share. The zones inherited from the VPC, for example, _US South 3_. |
   | Mount targets (Optional) | Click **Create** to create a [mount target](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets) for the file share. You can create one mount target per VPC per file share. Provide a name for the mount target and select a VPC in that zone. You can add as many mount targets as you have VPCs. If you don't have one, first [create a VPC](/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc). For more information about creating mount targets as a separate operation, see [Create a mount target](#fs-create-mount-target-ui). |
   | Profile | Select an IOPS tier, custom, or dp2 profile. The profile that you select determines the input/output performance of a file share. For more information, see [file storage profiles](/docs/vpc?topic=vpc-file-storage-profiles). |
   | Size | Specify the size for the file share. |
   | Encryption | Choose **Customer Managed** and use your own encryption key.
   {: caption="Table 1. Values for creating a file share and mount target." caption-side="bottom"}

4. Update the fields in the **Encryption** section.

   | Field | Value |
   | ----- | ----- |
   | Encryption | To use customer-managed encryption, select a [key management service](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok) ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}). The key management service (KMS) instance includes the root key that is imported to or created in that KMS instance. |
   | Encryption service instance | If you provisioned multiple KMS instances in your account, select the one that includes the root key that you want to use for customer-managed encryption. |
   | Key name | Select the root key within the KMS instance that you want to use for encrypting the share. |
   | Key ID | Shows the key ID that is associated with the data encryption key that you selected. |
   {: caption="Table 2. Values for customer-managed encryption for file shares" caption-side="bottom"}

5. When all the required information is provided, click **Create file share**. You're returned to the File Shares for VPC page, where a message indicates that the file share is provisioning. When completed, the status changes to **Active**.

If you created your {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} instance by using a private endpoint, root keys that were created by using that instance are not shown in the UI. You must use the CLI or API to access and use those root keys.
{: important}

## Create customer-managed encryption file shares from the CLI
{: #fs-byok-cli}
{: cli}

To create a file share with customer-managed encryption from the CLI, use the `ibmcloud is share-create` command with the `--encryption-key` option. The `encryption_key` parameter specifies a valid CRN for the root key in the key management service.

Before you begin, verify that you completed the [prerequisites](/docs/vpc?topic=vpc-file-storage-create#before-creating-file-storage-cli), then follow these steps:

1. Use the procedure in [Step 1 - Obtain service instance and root key information](/docs/vpc?topic=vpc-creating-instances-byok#byok-cli-setup-prereqs) to obtain the ID of your key management service and the CRN of the root key in that service.

2. Specify the `ibmcloud is share-create` command with the `--encryption-key` option to a volume with customer-managed encryption. The `encryption_key` parameter specifies a valid CRN for the root key in the key management service.

As of 30 May 2023, you can use `--mount-targets` instead of `-targets` option. To see and use the updated option, set the feature environment variable `IBMCLOUD_IS_FEATURE_FILESHARE_CHANGE_TO_MOUNT_TARGETS` to true with the `export` command.
{: beta}

```bash
ibmcloud is share-create
   --zone ZONE_NAME
   --profile PROFILE
   --size SIZE [--name NAME]
   [--mount-targets TARGETS_JSON | @TARGETS_JSON_FILE]
   [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
   [--encryption-key ENCRYPTION_KEY]
   [--output JSON]
```
{: pre}

For example,

```bash
ibmcloud is share-create --zone us-south-2 --vpc {vpc_id} --name my-encrypted-share --profile tier-5iops --size 100 --encryption key {crn}
```
{: pre}

The following example shows a new file share that is created with customer-managed encryption.

```bash
$ ibmcloud is share-create --zone us-south-2 --vpc {vpc_id} --name my-encrypted-share --profile tier-5iops --encryption key abccorp-kp-vpc-2 fd57250e-908c-4785-a8a5-1f53176bcd2f
Creating volume demovolume in resource group Default under account VPC 01 as user rtuser1@mycompany.com...
ID                                      339c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    my-encrypted-share
Size                                    100
IOPS                                    1000
Profile                                 tier-5iops
Encryption Key                          crn:v1:bluemix:public:kms:us-south:a/8d65fb1cf5e99e86dd7229ddef9e5b7b:b1abf7c5-381d-4f34-836e-5db7193250bc:key:fd57250e-908c-4785-a8a5-1f53176bcd2f
Encryption                              customer_managed
Status                                  pending
Resource Group                          Default(dbb12715c2a22f2bb60df4ffd4a543f2)
Created                                 2023-02-07 10:09:28
Zone                                    us-south-2
Mount targets                           none
```
{: screen}

## Create customer-managed encryption file shares with the API
{: #fs-byok-api}
{: api}

You can create file shares with customer-managed encryption by calling the [Virtual Private Cloud (VPC) API](/apidocs/vpc).

Make a `POST /shares` request and specify the `encryption_key` parameter to identify your customer root key (CRK). It is shown in the example as `crn:[...key:...]`.

You can also specify the CRN of a root key from a different account in the `POST /shares` call. For more information, see [About cross account key access and use](/docs/vpc?topic=vpc-vpc-byok-cross-acct-key&interface=ui#byok-cross-acct-about).
{: note}

As described in the [Beta VPC API](/apidocs/vpc-beta) reference [versioning](/apidocs/vpc-beta#api-versioning-beta) policy, support for older versions of the beta API is limited to 45 days. Therefore, beta API requests must specify a `version` query parameter date value within the last 45 days. You must also provide `generation` parameter and specify `generation=2`. For more information, see **Generation** in the [Virtual Private Cloud API reference](/apidocs/vpc#api-generation-parameter).
{: requirement}

The following example creates a file share with a mount target, and specifies the CRN of the root key for customer-managed encryption.

   ```sh
   curl -X POST \
   "$vpc_api_endpoint/v1/shares?version=2023-07-11&generation=2&maturity=beta" \
   -H "Authorization: $iam_token" \
   -d '{
       "encryption_key": {
          "crn":"crn:[...key:...]"
        },
        "iops": 1000,
        "name": "my-encrypted-share",
        "profile": {
          "name": "tier-5iops"
        },
        "resource_group": {
           "id": "678523bcbe2b4eada913d32640909956"
         },
        "size": 100,
        "mount_targets": [
          {
            "name": "my-share-target",
            "subnet": {
              "id": "7ec86020-1c6e-4889-b3f0-a15f2e50f87e"
            }
          }
        ],
        "zone": {
          "name": "us-south-2"
        }
      }'
   ```
   {: codeblock}

A successful response looks like the following example.

   ```json
   {
     "created_at": "2023-07-11T22:58:49.000Z",
     "crn": "crn:[...]",
     "encryption": "customer_managed",
     "encryption_key": {
         "crn": "crn:[...key:...]"
     },
     "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/a0c07083-f411-446c-9316-7b08d6448c86",
     "id": "a0c07083-f411-446c-9316-7b08d6448c86",
     "iops": 1000,
     "lifecycle_state": "pending",
     "name": "my-encrypted-share",
     "profile": {
       "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/tier-5iops",
       "name": "tier-5iops",
       "resource_type": "share_profile"
     },
     "resource_group": {
       "crn": "crn:[...]",
       "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
       "id": "678523bcbe2b4eada913d32640909956",
       "name": "Default"
     },
     "resource_type": "share",
     "size": 100,
     "mount_targets": [
       {
         "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/a0c07083-f411-446c-9316-7b08d6448c86/mount_targets/   1b5571cb-536d-48d0-8452-81c05c6f7b80",
         "id": "1b5571cb-536d-48d0-8452-81c05c6f7b80",
         "name": "my-share-target",
         "resource_type": "share_target",
         "vpc": {
           "crn": "crn:[...]",
           "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/12bb28fc-856d-4902-813b-dc065d1ed084",
           "id": "12bb28fc-856d-4902-813b-dc065d1ed084",
           "name": "my-vpc",
           "resource_type": "vpc"
         }
       }
     ],
     "zone": {
       "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
       "name": "us-south-2"
     }
   }
   ```
   {: screen}

## Next steps
{: #next-step-fs-byok}

When you refresh the list of file shares in the UI, the new share appears at the beginning of the list with "customer managed" as the encryption type. When the share is created, it shows a status of `stable`.

Manage the root keys that are protecting your file share by [rotating](/docs/vpc?topic=vpc-vpc-key-rotation), [disabling](/docs/vpc?topic=vpc-vpc-encryption-managing&interface=ui#byok-disable-root-keys), or [deleting](/docs/vpc?topic=vpc-vpc-encryption-managing&interface=ui#byok-delete-root-keys) keys.
