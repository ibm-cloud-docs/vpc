---

Copyright:
  years: 2019, 2021
lastupdated: "2021-05-14"

keywords: block storage, IBM Cloud, VPC, virtual private cloud, Key Protect, encryption, key management, Hyper Protect Crypto Services, HPCS, volume, data storage, virtual server instance, instance, customer-managed encryption

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:preview: .preview}
{:screen: .screen}
{:pre: .pre}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Creating block storage volumes with customer-managed encryption
{: #block-storage-vpc-encryption}

By default, {{site.data.keyword.block_storage_is_short}} boot and data volumes are encrypted with IBM-managed encryption. You can also create customer-managed encrypted volumes by using a supported key management service to create or import your customer root key. Your data is protected while in transit and while at rest. 
{:shortdesc}

## Before you begin
{: #custom-managed-vol-prereqs}

To create block storage volumes with customer-managed encryption, you must first provision a key management service and create or import your customer root key (CRK). You must also authorize access between Cloud Block Storage and the key management service. When you complete these prerequisites, you can start creating block storage volumes that use customer-managed encryption.

For information and prerequisite steps, see [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-encryption-prereqs).

## Creating customer-managed encryption data volumes in the UI
{: #data-vol-encryption-ui}
{: ui}

This procedure explains how to specify customer-managed encryption when you create a stand-alone block storage volume. You can also specify customer-managed encryption for volumes created during instance provisioning. For more information, see [Creating virtual server instances with customer-managed encryption](/docs/vpc?topic=vpc-creating-instances-byok).

Follow these steps to specify customer-managed encryption from the UI:

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes** to view a list of your block storage volumes.
1. Select **New volume**.
1. On the **New block storage volume** page, update the fields in the **Encryption** section (see Table 2). When your changes are complete, click **Create Volume**.
1. Optionlly, attach the volume to an instance. See [Next steps](#next-step-create-byok-volumes-vpc) for more information.

| Field | Value |
| ----- | ----- |
| Encryption | _Provider managed_ is the default encryption mode. To use customer-managed encryption, select a key management service ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}). The key management service instance includes the customer root key that you want to use for customer-managed encryption. |
| Encryption service instance | If you provision multiple key management service instances in your account, select the one that includes the customer root key that you want to use for customer-managed encryption. |
| Key name | Select the data encryption key within the {{site.data.keyword.keymanagementserviceshort}} instance that you want to use for encrypting the volume. |
| Key ID | Displays the key ID that is associated with the data encryption key that you selected. |
{: caption="Table 2. Values for customer-managed encryption" caption-side="top"}

## Editing boot volumes to use customer-managed encryption in the UI
{: #edit-boot-vol-byok-ui}
{: ui}

When you create an instance from the UI, you can specify customer-managed encryption by editing the boot volume properties. For more information, see [Provisioning virtual server instances with customer-managed encryption volumes in the UI](/docs/vpc?topic=vpc-creating-instances-byok#provision-byok-ui).

## Creating customer-managed encryption data volumes from the CLI
{: #data-vol-encryption-cli}
{: cli}

To create a block storage volume with customer-managed encryption by using the CLI, use the `ibmcloud is volume-create` command with the `--encryption-key` parameter. The `encryption_key` parameter specifies a valid CRN for the root key in the key management service.

Follow these steps:

1. Use the procedure in [Step 1 - Obtain service instance and root key information](/docs/vpc?topic=vpc-creating-instances-byok#byok-cli-setup-prereqs) to obtain the ID of your key management service and the CRN of the root key in that service.

2. Specify the `ibmcloud is volume-create` command with the `--encryption-key` parameter to a volume with customer-managed encryption. The `encryption_key` parameter specifies a valid CRN for the root key in the key management service.

```bash
ibmcloud is volume-create VOLUME_NAME PROFILE_NAME ZONE_NAME [--encryption-key ENCRYPTION_KEY] [--capacity CAPACITY] [--iops IOPS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]
```
{: pre}

The following example shows a new volume that is created with customer-managed encryption.

```bash
$ ibmcloud is volume-create demo_volume custom us-south-1 --iops 1000 --encryption-key abccorp-kp-vpc-2 5437644a-c4b1-447f-9646-b1a2a4df61382
Creating volume demovolume in resource group Default under account VPC 01 as user rtuser1@mycompany.com...
ID                                      933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    demo_volume
Capacity                                100
IOPS                                    1000
Profile                                 custom
Encryption Key                          crn:v1:bluemix:public:kms:us-south:a/8d65fb1cf5e99e86dd7229ddef9e5b7b:b1abf7c5-381d-4f34-836e-5db7193250bc:key:fd57250e-908c-4785-a8a5-1f53176bcd2f
Encryption                              customer_managed
Status                                  pending
Resource Group                          Default(dbb12715c2a22f2bb60df4ffd4a543f2)
Created                                 2020-07-20 10:09:28
Zone                                    us-south-1
Volume Attachment Instance Reference    none
```
{:screen}

You can also create volumes with customer-managed encryption during instance provisioning. For information, see [Provisioning instances with customer-managed encrypted volumes from the CLI](/docs/vpc?topic=vpc-creating-instances-byok#provision-byok-cli).

## Creating customer-managed encryption data volumes with the API
{: #data-vol-encryption-api}
{: api}

You can create data volumes with customer-managed encryption by calling the Virtual Private Cloud (VPC) API.

Make a `POST/volumes` request to create a new volume encrypted using your own encryption keys. Use the `encryption_key` parameter to specify your customer root key (CRK), shown in the example as `crn:[...key:...]`.

The following example creates a general-purpose data volume with customer-managed encryption.

```
curl -X POST \
"$vpc_api_endpoint/v1/volumes?version=2020-03-10&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "name": "my-volume-1",
      "iops": 100,
      "capacity": 20,
      "zone": {
        "name": "us-south-3"
      },
      "profile": {
        "name": "general-purpose"
      },
      "encryption_key":{  
        "crn":"crn:[...key:...]"
      },
      "resource_group": {
        "id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4"
      }
    }
```
{:codeblock}

A successful response will look like this:

```
{
    "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8",
    "crn": "crn:[...]",
    "name": "my-volume-1",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/8948ad59-bc0f-7510-812f-5dc64f59fab8",
    "capacity": 20,
    "iops": 100,
    "encryption_key": {
        "crn": "crn:[...key:...]"
    },
    "encryption": "user_managed",
    "status": "available",
    "zone": {
        "name": "us-south-3",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/
         us-south-3"
    },
    "profile": {
        "name": "general-purpose",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/
       general-purpose"
    },
    "resource_group": {
        "id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4",
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/
         a342dbfb-3ea7-48d1-96e8-2825ec5feab4",
        "name": "Default"
    },
    "volume_attachments": [],
    "created_at": "2020-07-20T16:03:22.000Z"
}
```
{:codeblock}

## Next steps
{: #next-step-create-byok-volumes-vpc}

When you refresh the list of block storage volumes using the UI, the new volume appears at the beginning of the list of volumes with "customer managed" as the encryption type. When the volume is created, it shows a status of Available. For stand-alone volumes, the Attachment Type column is blank (-). The Action menu (...) at the end of a table row provides a link for [attaching a block storage volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage).

Interested in setting up key rotation for your customer-managed root keys? For more information, see [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).
