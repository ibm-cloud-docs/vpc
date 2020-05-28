---

Copyright:
  years: 2019, 2020
lastupdated: "2020-05-27"

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
{:beta: .beta}
{:table: .aria-labeledby="caption"}

# Creating block storage volumes with customer-managed encryption (Beta)
{: #block-storage-vpc-encryption}

By default, {{site.data.keyword.block_storage_is_short}} boot and data volumes are encrypted with IBM-managed encryption. You can also create customer-managed encrypted volumes by using a supported key management service to create or import your customer root key. Your data is protected while in transit and while at rest. 
{:shortdesc}

Customer-managed encryption is an option for boot and data volumes that are [created during instance provisioning](/docs/vpc?topic=vpc-creating-instances-byok#provision-byok-ui). You can also specify customer-managed encryption when you [create a stand-alone data volume](#data-vol-encryption-ui). This feature is available in the Dallas, Washington DC, London, and Frankfort regions.

This is a beta feature that is available for evaluation and testing purposes. 
{:beta}

## Key management services for block storage volumes
{: #key-mgt-services-for-storage}

The supported key management services for encrypting your block storage volumes are {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} (available in certain [regions](/docs/hs-crypto?topic=hs-crypto-regions#regions)). {{site.data.keyword.cloud_notm}} data centers provide a dedicated hardware security module (HSM) to protect your keys.
For more information about these services, see [Supported key management services for customer-managed encryption](/docs/vpc?topic=vpc-creating-instances-byok#kms-for-byok).

## Before you begin
{: #custom-managed-vol-prereqs}

To create block storage volumes with customer-managed encryption, you must first provision a key management service and create or import your customer root key (CRK). You must also authorize access between Cloud Block Storage and the key management service. When you complete these prerequisites, you can start creating block storage volumes that use customer-managed encryption.

For information and prerequisite steps, see [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-creating-instances-byok#byok-vsi-prereqs).

## Creating customer-managed encrypted data volumes by using the UI
{: #data-vol-encryption-ui}

You can specify customer-managed encryption when you create a new block storage volume during instance provisioning or as a stand-alone volume.

**Note**: To create an encrypted volume when you create a virtual server instance, see [Creating virtual server instances with customer-managed encryption](/docs/vpc?topic=vpc-creating-instances-byok).

Figure 1 shows the end-to-end procedure for creating a standalone volume and attaching it to an instance.

![Figure showing the procedure for creating block storage volumes with customer-managed encryption.](/images/vpc_flowchart_volumes_color.png "Figure showing the procedure for creating block storage volumes with customer-managed encryption"){: caption="Figure 1. Creating a block storage volume with customer-managed encryption" caption-side="bottom"}

To specify customer-managed encryption when you create a stand-alone volume, follow these steps:

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**. A list of all block storage volumes displays.
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

## Creating customer-managed encrypted data volumes by using the CLI
{: #data-vol-encryption-cli}

Follow [CLI step 1](/docs/vpc?topic=vpc-creating-instances-byok#provision-byok-cli) for obtaining the CRN of the root key in your key management service.

To create a block storage volume with customer-managed encryption by using the CLI, specify the `ibmcloud is volume-create` command with the `--encryption-key` parameter.  The `encryption_key` parameter specifies a valid CRN for the root key in the key management service.

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
Created                                 2020-04-20 10:09:28
Zone                                    us-south-1
Volume Attachment Instance Reference    none
```
{:screen}

You can also create volumes with customer-managed encryption during instance provisioning. For information, see [Using the CLI to provision instances and volumes with customer-managed encryption](/docs/vpc?topic=vpc-creating-instances-byok#provision-byok-cli).

## Creating customer-managed encrypted data volumes by using the API
{: #data-vol-encryption-api}

Make a `POST/volumes` request to create a new volume encrypted using your own encryption keys. Use the `encryption_key` parameter to specify your customer root key (CRK), represented in the example as `crn:[...key:...]`.

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
    "created_at": "2020-04-20T16:03:22.000Z"
}
```
{:codeblock}

## Editing boot volumes to use customer-managed encryption by using the UI

When you create an instance from the UI, you can specify customer-managed encryption by editing the boot volume properties. For more information, see [Provisioning virtual server instances with volumes that use customer-managed encryption](/docs/vpc?topic=vpc-creating-instances-byok#provision-byok-ui).

## Next steps
{: #next-step-create-byok-volumes-vpc}

When you refresh the list of block storage volumes using the UI, the new volume appears at the beginning of the list of volumes with "customer managed" as the encryption type. When the volume is created, it shows a status of Available. For stand-alone volumes, the Attachment Type column is blank (-). The Action menu (...) at the end of a table row provides a link for [attaching a block storage volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage).
