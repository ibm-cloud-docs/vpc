---

copyright:
  years: 2019, 2020
lastupdated: "2020-05-27"

keywords: create virtual server with encryption, data encryption key, dek, encrypted volume, virtual server instance, create virtual server, provision virtual server, virtual machine, instance, virtual server, deploy virtual server, block storage volume

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:important: .important}
{:preview: .preview}
{:beta: .beta}
{:external: target="_blank" .external}

# Creating virtual server instances with customer-managed encryption (Beta)
{: #creating-instances-byok}

You can create {{site.data.keyword.vsi_is_full}} that use your encryption keys to protect data in your block storage volumes attached to the instance. When you provision an instance, you can specify customer-managed encryption for the boot and data volumes. Customer-managed encryption uses your customer root key, giving you complete control over your data. You can import your root key to a key management service (KMS) or create one in a KMS.  Customer-managed encryption protects your data while in transit and while at rest. For added security, enable the secure import of your CRKs by using import tokens. 
{:shortdesc}

This feature is available in the Dallas, Washington DC, London, and Frankfort regions.

This is a beta feature that is available for evaluation and testing purposes. 
{:beta}

Before you can create an instance, you need to [create an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{:important}

## Supported key management services for customer-managed encryption
{: #kms-for-byok}

Choose the key management service that best fits your needs. {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} (available in certain [regions](/docs/hs-crypto?topic=hs-crypto-regions#regions)) use a common key provider API to provide a consistent approach for managing encryption keys.

Table 1 describes the key management services that support by customer-managed encryption for block storage volumes.

| Key Management Service | HSM Encryption Certification | Description |
| ----- | ----- |
| [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect/concepts?topic=key-protect-getting-started-tutorial) | FIPS 140-2 *Level 3* compliance | A multi-tenant KMS; lets you import or create your root keys and securely manage them.  |
| [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started) | FIPS 140-2 *Level 4* compliance | Highest level of security; a single-tenant KMS and hardware security module (HSM) that's controlled by you. Import or create your root keys and securely manage them. Only you have access to your keys and data. |
{: caption="Table 1. Available key management service options" caption-side="top"}

## Prerequisites for setting up customer-managed encryption
{: #byok-vsi-prereqs}

To create a virtual server instance that uses customer-managed encryption for the block storage volumes, you must first:

* Provision a key management service, {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}.
* [Import](/docs/key-protect?topic=key-protect-import-root-keys) a customer root key (CRK) to the key management service or [create](/docs/key-protect?topic=key-protect-create-root-keys) one in the key management service.
* Authorize access between Cloud Block Storage and the key management service.

The following example steps are specific to {{site.data.keyword.keymanagementserviceshort}}, but the general flow also applies to {{site.data.keyword.hscrypto}}. If you're using {{site.data.keyword.hscrypto}}, see the [{{site.data.keyword.hscrypto}} information](/docs/hs-crypto?topic=hs-crypto-get-started) for corresponding instructions.
{:note}

1. Provision the [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-provision) service.
   
   Provisioning a new {{site.data.keyword.keymanagementserviceshort}} service instance ensures that it includes the most recent updates that are required for customer managed encryption of block storage volumes.
   {: tip}
   
2. [Create](/docs/key-protect?topic=key-protect-create-root-keys) or 
[import](/docs/key-protect?topic=key-protect-import-root-keys) a customer root key in
{{site.data.keyword.keymanagementservicelong_notm}}. 

   Plan ahead for importing keys by [reviewing your options for creating and encrypting key material](/docs/key-protect?topic=key-protect-importing-keys#plan-ahead). For added security, you can enable the secure import of the key material by using an [import token](/docs/key-protect?topic=key-protect-importing-keys#using-import-tokens) to encrypt your key material before you bring it to the cloud.
   {: tip}

3. From IBM {{site.data.keyword.iamshort}} (IAM), [authorize access](/docs/iam?topic=iam-serviceauth#serviceauth) between **Cloud Block Storage** (source service) and **{{site.data.keyword.keymanagementserviceshort}}** (target service).

## Provisioning virtual server instances with volumes that use customer-managed encryption
{: #provision-byok-ui}

When you provision a virtual server instance, you can specify customer-managed encryption for your boot volume and any data volumes that you want to add at that time. If you want, you can use a combination of provider-managed encryption and customer-managed encryption for the volumes that are associated with your instance.

Figure 1 shows the end-to-end procedure for creating an instance with a customer-managed encrypted volume.

![Figure showing the procedure for creating an instance with a customer-managed encrypted volume.](/images/vpc_flowchart_instances_color.png "Figure showing the procedure for creating an instance with a customer-managed encrypted volume"){: caption="Figure 1. Creating an instance with a customer-managed encrypted volume" caption-side="bottom"}

Follow these steps to create an instance with a new block storage volume.

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc){: external}, navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

2. Click **New instance** and complete the required fields. (For information about these required fields, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers).)
3. In the **Boot volume** section, the default mode of encryption is _Provider managed_ encryption. To specify customer-managed encryption, click the pencil icon in the boot volume row. On the **Edit boot volume** page, update the fields in the **Encryption** section. See Table 2 for more information. When your changes are complete, click **Apply**.
4. In the **Attached block storage volume** section, you can click **New block storage volume** to add a data volume and specify customer-managed encryption. On the **New block storage volume** page, update the fields in the **Encryption** section. See Table 2 for more information. When your changes are complete, click **Attach**.

| Field | Value |
| ----- | ----- |
| Encryption | _Provider managed_ is the default encryption mode. To use customer-managed encryption, select a key management service from the drop-down list. The key management service instance must include the customer root key that you want to use for customer-managed encryption. |
| Encryption service instance | If you have multiple key management service instances that are provisioned in your account, select the one that includes the customer root key that you want to use for customer-managed encryption. |
| Key name | Select the data encryption key within the key management service instance that you want to use for encrypting the volume. | 
| Key ID | Displays the key ID that is associated with the data encryption key that you selected. | 
{: caption="Table 2. Values for specifying customer-managed encryption of volumes" caption-side="top"}

## Using the CLI to provision instances and volumes with customer-managed encryption
{: #provision-byok-cli}

To use the CLI to create a virtual server instance with volumes that use customer-managed encryption, you can use the `ibmcloud is instance-create` command and reference a JSON file to attach volumes that use customer-managed encryption. Follow these steps:

1. Obtain the CRN of the root key in your desired key management service instance. The following example is specific to {{site.data.keyword.keymanagementserviceshort}}.
    
    1. If you need to install the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in, run the following command: 
       ```
       ibmcloud plugin install key-protect -r 'IBM Cloud'
       ```
       {: pre}
    
    2. List the {{site.data.keyword.keymanagementserviceshort}} service instances for your account by running the following command:
       ```
       ibmcloud resource service-instances
       ```
       {: pre}
    
       For this example, you'd see a response similar to the following output:
       ```
       Retrieving all instances of all services in resource group Default and all locations
       under account MyCompany as myuserid@mycompany.com...
       OK
       Name             Location   State    Type
       Key Protect-17   us-south   active   service_instance
       Key Protect-60   us-south   active   service_instance
       ```
       {:screen}
         
    3. Retrieve the instance ID for the {{site.data.keyword.keymanagementserviceshort}} service instance where your customer root keys are stored. Run the following command:
       ```
       ibmcloud resource service-instance "Key Protect-17" --id
       ```
       {: pre}
    
       where `Key Protect-17` is your desired {{site.data.keyword.keymanagementserviceshort}} service instance.
    
       For this example, you'd see a response similar to the following output:
       ```
       Retrieving service instance Key Protect-17 in resource group Default under account 
       MyCompany as myuserid@mycompany.com...
       crn:v1:bluemix:public:kms:us-south:a/abxx1c2xxxx34x567xxdex891xxx23fx:xxx4g5xx-6789-x1h2-
       ixxx-3jkl4xxxx567::7mnxxxo8-91xx-23px-q4rs-xxtuv5w6xxx7
       ```
       {:screen}
       
       The instance ID is the string that follows the final `::` in the CRN. In this example, it's `7mnxxxo8-91xx-23px-q4rs-xxtuv5w6xxx7`.
    
    4. List the available keys and their associated CRNs within your desired {{site.data.keyword.keymanagementserviceshort}} service instance by running the following command:
       ```
       ibmcloud kp list -c --instance-id 7mnxxxo8-91xx-23px-q4rs-xxtuv5w6xxx7
       ```
       {: pre}
       
       where `7mnxxxo8-91xx-23px-q4rs-xxtuv5w6xxx7` is the instance ID of your desired {{site.data.keyword.keymanagementserviceshort}} service instance.
       
       For this example, you'd see a response similar to the following output:
       ```
       Retrieving keys...
              
       SUCCESS
               
       Key ID                                 Key Name               CRN
       ef1gxxxh-ijxx-234x-56k7-xxxxlmnoxxp8   test-key               crn:v1:bluemix:public:
       kms:us-south:a/abxx1c2xxxx34x567xxdex891xxx23fx:xxx4g5xx-6789-x1h2-ixxx-3jkl4xxxx567:
       key:ef1gxxxh-ijxx-234x-56k7-xxxxlmnoxxp8
       cdex12ef-xxxg-3hxx-i456-7xxx8jk9xl12   vsi_encrypt_root_key   crn:v1:bluemix:public:
       kms:us-south:a/abxx1c2xxxx34x567xxdex891xxx23fx:xxx4g5xx-6789-x1h2-ixxx-3jkl4xxxx567:
       key:cdex12ef-xxxg-3hxx-i456-7xxx8jk9xl12
       c12xxxx3-45d6-7efg-xxx8-9xxx12345x6h   vsi_encrypt_key        crn:v1:bluemix:public:
       kms:us-south:a/abxx1c2xxxx34x567xxdex891xxx23fx:xxx4g5xx-6789-x1h2-ixxx-3jkl4xxxx567:
       key:c12xxxx3-45d6-7efg-xxx8-9xxx12345x6h
       ```
       {:screen}
       
2. Use the [ibmcloud is instance-create](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#instance-create) command and attach the necessary JSON files that specify customer-managed encryption for the boot volume and any secondary data volumes. See the following [JSON file examples](#vsi-vol-attachment-json) of a boot volume attachment JSON and secondary volume attachment JSON. (For more information about creating instances, see [Creating virtual server instances (CLI)](/docs/vpc?topic=vpc-creating-virtual-servers-cli).)

```
ibmcloud is instance-create INSTANCE_NAME VPC ZONE_NAME PROFILE_NAME SUBNET --image-id IMAGE_ID [--boot-volume @BOOT_VOLUME_JSON_FILE] [--volume-attach @VOLUME_ATTACH_JSON_FILE]...
```
{:pre}

The `encryption_key` parameter in the JSON file must include a valid CRN for the root key in the Key Protect service. See the following [JSON file examples](#vsi-vol-attachment-json) of a boot volume attachment JSON and secondary volume attachment JSON. (For more information about creating instances, see [Creating virtual server instances (CLI)](/docs/vpc?topic=vpc-creating-virtual-servers-cli).)

## Creating a customer-managed encryption volume in JSON format
{: #vsi-vol-attachment-json}

When you create either a boot volume or data volume during VSI provisioning, you must specify a JSON file to define the customer-managed encryption volume parameters. The `encryption_key` parameter in the JSON file must include a valid CRN for the root key in the Key Protect service.

See the following examples for a boot or data volume attachment. You can copy the example replace the information with your volume information. Your root key CRN is show as `crn:[...key:...]` in the examples.

### Example boot volume JSON file
{: #boot-volume-byok-json}

This example defines the data in a boot volume JSON file. The `encryption key` parameter defines the root key CRN for customer-managed encryption.

```
{  
   "name":"volume-attachment-1",
   "volume":{  
      "name":"boot-volume-1",
      "capacity":100,
      "profile":{  
         "name":"general-purpose"
      },
      "encryption_key":{  
         "crn":"crn:[...key:...]"
      }
   },
   "delete_volume_on_instance_delete":true
}
```
{:codeblock}

### Example secondary volume JSON file
{: #secondary-volume-byok-json}

This example defines two general-purpose secondary (data) volumes with customer-managed encryption.

```
   {  
      "name":"volume-attachment-2",
      "volume":{  
         "name":"data-volume-2",
         "capacity":200,
         "profile":{  
            "name":"general-purpose"
         },
         "encryption_key":{  
            "crn":"crn:[...key:...]"
         }
      },
      "delete_volume_on_instance_delete":true
   }
```
{:codeblock}

## Using the API to provision virtual server instances with volumes that use customer-managed encryption
{: #provision-byok-api}

Make a `POST/instances` request to create an instance and new volume encrypted using your own encryption keys. The `encryption_key` parameter identifies the CRN of your customer root key (CRK), represented in the example as `crn:[...key:...]`.

The following example creates an instance with a BYOK-encrypted boot volume and two BYOK-encrypted secondary volumes.

```
curl -X POST \
 "$vpc_api_endpoint/v1/instances?version=version=2020-03-10&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
        "boot_volume_attachment": {
           "volume": {
                "name": "boot-volume-1"
                "profile": {
                    "name": "general-purpose"
                },
                "encryption_key": {
                    "crn": "crn:[...key:...]"
                }
            }
        },
        "volume_attachments": [
            {
                "volume": {
                    "name": "my-volume-1"
                    "capacity": 50,
                    "profile": {
                        "name": "general-purpose"
                    },
                    "encryption_key": {
                        "crn": "crn:[...key:...]"
                    }
                }
            },
            {
                "volume": {
                    "name": "my-volume-2"
                    "capacity": 20,
                    "profile": {
                        "name": "general-purpose"
                    },
                    "encryption_key": {
                        "crn": "crn:[...key:...]"
                    }
                }
            }
        ],
        "image": {
            "id": "9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366"
        },
        "keys": [
            {
                "id": "cf7678a3-d4fa-458b-993d-015bd4aeac80"
            }
        ],
        "name": "my-test-vm2",
        "primary_network_interface": {
            "subnet": {
                "id": "bea6a632-5e13-42a4-b4b8-31dc877abfe4"
            }
        },
        "profile": {
            "name": "cx2-2x4"
        },
        "vpc": {
            "id": "f0aae929-7047-46d1-92e1-9102b07a7f6f"
        },
        "zone": {
            "name": "us-south-3"
        }
    }'
```
{:codeblock}

A successful response will look like this. Note that the boot volume appears under both `boot_volume_attachment` and `volume_attachment`.

```
{
    "id": "eb1b7391-2ca2-4ab5-84a8-b92157a633b0",
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/eb1b7391-2ca2-4ab5-84a8-
     b92157a633b0",
    "name": "my-test-vm2",
    "bandwidth": 4000,
    "resource_group": {
        "id": "08b7af6d-41d9-435a-8b22-fd8f640863a5",
        "href": "https://resource-controller.test.cloud.ibm.com/v2/resource_groups/
         08b7af6d-41d9-435a-8b22-fd8f640863a5",
        "name": "Default"
    },
    "boot_volume_attachment": {
        "id": "a8a15363-a6f7-4f01-af60-715e85b28141",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/eb1b7391-2ca2-4ab5-
         84a8-b92157a633b0/volume_attachments/7389-a8a15363-a6f7-4f01-af60-
         715e85b28141",
        "name": "volume-attachment",
        "volume": {
            "id": "49c5d61b-41e7-4c01-9b7a-1a97366c6916",
            "crn": "crn:[...]",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/49c5d61b-41e7-
             4c01-9b7a-1a97366c6916",
            "name": "boot-volume-1"
        }
    },
    "created_at": "2020-04-20T16:11:57Z",
    "image": {
        "id": "9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366",
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/images/
         9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366",
        "name": "ubuntu-amd64-1"
    },
    "memory": 4,
    "network_interfaces": [
        {
            "id": "7ca88dfb-8962-469d-b1de-1dd56f4c3275",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-
             4aa2-a8d7-703aac843651/network_interfaces/7ca88dfb-8962-469d-b1de-
             1dd56f4c3275",
            "name": "helpless-profanity-unmixable-fool-hazard-staging",
            "primary_ipv4_address": "",
            "subnet": {
                "id": "bea6a632-5e13-42a4-b4b8-31dc877abfe4",
                "crn": "crn:[...]",
                "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/7389-bea6a632-
                 5e13-42a4-b4b8-31dc877abfe4",
                "name": "my-byok-vpc-subnet"
            }
        }
    ],
    "primary_network_interface": {
        "id": "7ca88dfb-8962-469d-b1de-1dd56f4c3275",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-4aa2-
         a8d7-703aac843651/network_interfaces/7ca88dfb-8962-469d-b1de-1dd56f4c3275",
        "name": "network-interface-1",
        "primary_ipv4_address": "10.0.0.32",
        "subnet": {
            "id": "bea6a632-5e13-42a4-b4b8-31dc877abfe4",
            "crn": "crn:[...]",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/bea6a632-5e13-
             42a4-b4b8-31dc877abfe4",
            "name": "my-byok-vpc-subnet"
        }
    },
    "profile": {
        "name": "cx2-2x4",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/instance/profiles/cx2-2x4"
    },
    "status": "running",
    "vcpu": {
        "architecture": "amd64",
        "count": 2
    },
    "volume_attachments": [
        {
            "id": "a8a15363-a6f7-4f01-af60-715e85b28141",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-
             4aa2-a8d7-703aac843651/volume_attachments/7389-a8a15363-a6f7-4f01-af60-
             715e85b28141",
            "name": "volume-attachment",
            "volume": {
                "id": "49c5d61b-41e7-4c01-9b7a-1a97366c6916",
                "crn": "crn:[...]",
                "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/49c5d61b-41e7-
                 4c01-9b7a-1a97366c6916",
                "name": "boot-volume-1"
            }
        },
        {
            "id": "e77125cb-4df0-4988-a878-531ae0ae0b70",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-
             4aa2-a8d7-703aac843651/volume_attachments/7389-e77125cb-4df0-4988-a878-
             531ae0ae0b70",
            "name": "volume-attachment",
            "volume": {
                "id": "2cc091f5-4d46-48f3-99b7-3527ae3f4392",
                "crn": "crn:[...]",
                "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/2cc091f5-4d46-
                 48f3-99b7-3527ae3f4392",
                "name": "my-volume-2"
            }
        },
        {
            "id": "a7641494-5724-46de-9c72-c6b16971ddf4",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-
             4aa2-a8d7-703aac843651/volume_attachments/a7641494-5724-46de-9c72-
             c6b16971ddf4",
            "name": "volume-attachment",
            "volume": {
                "id": "ec419496-d79e-4fca-bce7-b4be72e77654",
                "crn": "crn:[...]",
                "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/ec419496-d79e-
                 4fca-bce7-b4be72e77654",
                "name": "my-volume-2"
            }
        }
    ],
    "vpc": {
        "id": "f0aae929-7047-46d1-92e1-9102b07a7f6f",
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/f0aae929-7047-46d1-92e1-
         9102b07a7f6f",
        "name": "my-byok-vpc"
    },
    "zone": {
        "name": "us-south-3",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/
         us-south-3"
    }
}
```
{:codeblock}

## Making your data inaccessible after setting up customer-managed encryption
{: #instance-byok-inaccessible-data}

After you set up customer-managed encryption on a volume, you might decide to later [disable an encryption key](/docs/key-protect?topic=key-protect-disable-keys) and temporarily revoke access to the key's associated data on the cloud.

You can take further steps to make your data inaccessible, but retain it on the cloud.

When you [authorize use](/docs/iam?topic=iam-serviceauth#serviceauth) of your root key, you grant permission for IBM to use the key to encrypt your volume. Authorization is done at the key management service level through IAM, when you authorize service between Cloud Block Storage and the key management service you set up (for example, {{site.data.keyword.keymanagementserviceshort}}).

You can remove any authorization between services in your account when you have the Administrator role on the target service (in this case, the key management service). If you remove any access policies created by the source service for its dependent services, the source service is unable to complete the workflow or access the target service.

Because they keys are under your control, you don't have to contact IBM to remove authorization.

To make your data inaccessible, but retain it on the IBM Cloud:

1. [Remove IAM authorization](/docs/iam?topic=iam-serviceauth#remove-auth) from the source Cloud Block Storage service to your target key management service instance.
2. [Power off all virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances#stop-and-start) that have attached encrypted volumes secured by that root key.

## Next steps
{: #next-steps-creating-byok-instances}

After the instance is created with the encrypted boot and data volumes, associate a floating IP address to the instance. Then, you can connect to your instance. For more information, see [Connecting to your Linux instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux) or [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows). If you have an existing instance with a floating IP address, then it is not necessary to assign a second floating IP to another instance. You can connect to the first with a floating IP, then SSH to the second instance using the private subnet IP address that is automatically assigned to it.

You can also independently create block storage volumes that use your encryption keys and later attach them to an instance. For information, see [Creating block storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption#block-storage-vpc-encryption).
