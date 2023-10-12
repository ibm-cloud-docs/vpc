---

copyright:
  years: 2019, 2022
lastupdated: "2022-06-24"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating virtual server instances with customer-managed encryption volumes
{: #creating-instances-byok}

You can create virtual servers for {{site.data.keyword.vpc_short}} that use your own encryption keys to protect data in the Block Storage volumes that are attached to your instance. When you provision an instance, you can specify customer-managed encryption for the boot and data volumes. Customer-managed encryption uses your customer root key, giving you complete control over your data. You can import your root key to a key management service (KMS) or create one in a KMS.  Customer-managed encryption protects your data while in transit and while at rest. For added security, enable the secure import of your root keys by using import tokens.
{: shortdesc}

Before you can create an instance, you need to [create an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{: important}

## Before you begin
{: #custom-managed-vol-prereqs}

To create virtual server instances and configure customer-managed encryption for boot and data volumes, you must first provision a key management service and create or import your customer root key (CRK). You must also authorize access between Cloud Block Storage and the key management service.

For information and prerequisite steps, see [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-encryption-prereqs).

## Provisioning virtual server instances with customer-managed encryption volumes in the UI
{: #provision-byok-ui}
{: ui}

When you provision a virtual server instance, you can specify customer-managed encryption for your boot volume and any data volumes that you want to add at that time. If you want, you can use a combination of provider-managed encryption and customer-managed encryption for the volumes that are associated with your instance.

Follow these steps to create an instance with a new Block Storage volume.

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc){: external}, navigate to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
1. Click **New instance** and complete the required fields. (For information about these required fields, see _Table 1 - Instance provisioning selections_ in [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers).)
1. In the **Boot volume** section, the default mode of encryption is _Provider managed_ encryption. To specify customer-managed encryption, click the pencil icon in the boot volume row. On the **Edit boot volume** page, update the fields in the **Encryption** section. See Table 1 for more information. When your changes are complete, click **Apply**.
1. In the **Attached Block Storage volume** section, you can click **New Block Storage volume** to add a data volume and specify customer-managed encryption. On the **New Block Storage volume** page, update the fields in the **Encryption** section. See Table 1 for more information. When your changes are complete, click **Attach**.

| Field | Value |
| ----- | ----- |
| Encryption | _Provider managed_ is the default encryption mode. To use customer-managed encryption, select a key management service from the drop-down list. The key management service instance must include the customer root key that you want to use for customer-managed encryption. |
| Encryption service instance | If you have multiple key management service instances that are provisioned in your account, select the one that includes the customer root key that you want to use for customer-managed encryption. |
| Key name | Select the root key within the key management service instance that you want to use for encrypting the volume. | 
| Key ID | Displays the key ID that is associated with the root key that you selected. | 
{: caption="Table 1. Values for specifying customer-managed encryption of volumes" caption-side="bottom"}

## Provisioning instances with customer-managed encrypted volumes from the CLI
{: #provision-byok-cli}
{: cli}

To provision instances with customer-managed encryption volumes from the CLI, you need to create JSON files to define your boot and data volumes, and then specify the `ibmcloud is instance-create` command to reference the JSON files.

### Step 1 - Obtain service instance and root key information
{: #byok-cli-setup-prereqs}

Obtain the ID of the key management service instance and the CRN of the root key in that instance. The following example is specific to {{site.data.keyword.keymanagementserviceshort}}, but HPCS is similar.

1. If you need to install the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in, run the following command:

   ```sh
   ibmcloud plugin install key-protect -r 'IBM Cloud'
   ```
   {: pre}

1. List the {{site.data.keyword.keymanagementserviceshort}} service instances for your account:

   ```sh
   ibmcloud resource service-instances
   ```
   {: pre}

   For example:

   ```text
   Retrieving all instances of all services in resource group Default and all locations
   under account MyCompany as myuserid@mycompany.com...
   OK
   Name             Location   State    Type
   Key Protect-17   us-south   active   service_instance
   Key Protect-60   us-south   active   service_instance
   ```
   {: screen}

3. Retrieve the instance ID for the {{site.data.keyword.keymanagementserviceshort}} service instance where your customer root keys are stored. `Key Protect-17` is the name of the {{site.data.keyword.keymanagementserviceshort}} service instance.

   ```sh
   ibmcloud resource service-instance "Key Protect-17" --id
   ```
   {: pre}

   For this example, you'll see the following output:

   ```text
   Retrieving service instance Key Protect-17 in resource group Default under account 
   MyCompany as myuserid@mycompany.com...
   crn:v1:bluemix:public:kms:us-south:a/abxx1c2xxxx34x567xxdex891xxx23fx:xxx4g5xx-6789-x1h2-
   ixxx-3jkl4xxxx567::7mnxxxo8-91xx-23px-q4rs-xxtuv5w6xxx7
   ```
   {: screen}

   The instance ID is the string that follows the final `::` in the CRN. In this example, it's `7mnxxxo8-91xx-23px-q4rs-xxtuv5w6xxx7`.

4. List the available keys and their associated CRNs for the {{site.data.keyword.keymanagementserviceshort}} service instance by specifying the instance ID:

   ```sh
   ibmcloud kp keys -c --instance-id 7mnxxxo8-91xx-23px-q4rs-xxtuv5w6xxx7
   ```
   {: pre}

   For this example, you'd see the following output:

   ```text
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
   {: screen}

### Step 2 - Specify customer-managed encryption for boot and data volumes in JSON format
{: #vsi-vol-attach-byok-json}

When you create a boot volume or data volume during instance provisioning using the CLI, you reference a JSON file that you create in advance. The JSON files (one for boot volumes and another for data volumes) define the customer-managed encryption volume parameters. In the file, the `encryption_key` parameter specifies a valid CRN for the root key in the key management service.

See the following JSON examples for boot or data volumes. Copy the example and replace the information with your volume information. The root key CRN is show as `crn:[...key:...]` in the examples, where you provide your root key CRN.

#### Example boot volume JSON file
{: #boot-vol-byok-json}

This example defines the data in a boot volume JSON file. The `encryption key` parameter defines the root key CRN for customer-managed encryption.

```json
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
{: codeblock}

#### Example secondary volume JSON file
{: #secondary-vol-byok-json}

This example defines two general-purpose secondary (data) volumes with customer-managed encryption.

```json
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
{: codeblock}

### Step 3 - Create a new instance with customer-managed encrypted volumes from the CLI
{: #procedure-byok-cli}
{: cli}

Use the [ibmcloud is instance-create](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#instance-create) command to create an instance with customer-managed encryption for your boot and data volumes. Specify the `--boot-volume` and `--volume-attach` parameters to reference JSON files that define volume encryption.

```sh
ibmcloud is instance-create INSTANCE_NAME VPC ZONE_NAME PROFILE_NAME SUBNET --image-id IMAGE_ID [--boot-volume @BOOT_VOLUME_JSON_FILE] [--volume-attach @VOLUME_ATTACH_JSON_FILE]...
```
{: pre}

## Provisioning instances with customer-managed encryption volumes with the API
{: #provision-byok-api}
{: api}

You can provision instances with customer-managed encryption volumes by calling the Virtual Private Cloud (VPC) API.

Make a `POST/instances` request to create an instance and new volume encrypted using your own encryption keys. The `encryption_key` parameter identifies the CRN of your customer root key, shown in the example as `crn:[...key:...]`.

The following example creates an instance with a boot volume with customer-managed encryption and two secondary volumes with customer-managed encryption.

```sh
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
{: codeblock}

A successful response looks like this. Note that the boot volume appears under both `boot_volume_attachment` and `volume_attachment`.

```text
{
    "id": "eb1b7391-2ca2-4ab5-84a8-b92157a633b0",
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/eb1b7391-2ca2-4ab5-84a8-
     b92157a633b0",
    "name": "my-test-vm2",
    "bandwidth": 4000,
    "resource_group": {
        "id": "08b7af6d-41d9-435a-8b22-fd8f640863a5",
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/
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
{: screen}

## Next steps
{: #next-steps-creating-byok-instances}

After the instance is created with the encrypted boot and data volumes, associate a floating IP address to the instance. Then, you can connect to your instance. For more information, see [Connecting to your Linux instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux) or [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows). If you have an existing instance with a floating IP address, then it's not necessary to assign a second floating IP to another instance. You can connect to the first with a floating IP, then SSH to the second instance using the private subnet IP address that's automatically assigned to it.

You can also independently create Block Storage volumes that use your encryption keys and later attach them to an instance. For information, see [Creating Block Storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).

Interested in setting up key rotation for your customer-managed root keys? For more information, see [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).
