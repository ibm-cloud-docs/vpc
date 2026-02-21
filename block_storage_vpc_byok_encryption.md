---

copyright:
  years: 2019, 2026
lastupdated: "2026-02-21"

keywords: Block Storage, IBM Cloud, VPC, virtual private cloud, Key Protect, encryption, key management, Hyper Protect Crypto Services, HPCS, volume, data storage, virtual server instance, instance, customer-managed encryption, Block Storage for vpc, customer-managed encryption,

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating Block Storage volumes with customer-managed encryption
{: #block-storage-vpc-encryption}

By default, {{site.data.keyword.block_storage_is_short}} boot and data volumes are encrypted with IBM-managed encryption. You can also use a supported key management service to create or import your customer root key, and use that to create an envelop encryption.
{: shortdesc}

## Before you begin
{: #custom-managed-vol-prereqs-block}

To create Block Storage volumes with customer-managed encryption, you must have your own customer root key. You can provision a key management service (KMS), and create or import your customer root key (CRK). You can choose between [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-getting-started-tutorial) and [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started). Then, [create a service-to-service authorization](/docs/vpc?topic=vpc-block-s2s-auth) between {{site.data.keyword.block_storage_is_short}} and the KMS instance that you created.

It's also possible to use a customer root key from another account. In {{site.data.keyword.cloud_notm}}, the KMS can be either located in the same or in another account as the service that is using an encryption key. This deployment pattern allows enterprises to centrally manage encryption keys for all corporate accounts. For more information, see [Encryption key management](/docs/solution-tutorials?topic=solution-tutorials-resource-sharing#resource-sharing-security-kms).

Configure all required [service-to-service authorizations](/docs/vpc?topic=vpc-block-s2s-auth&interface=ui#block-s2s-auth-encryption-ui){: ui}[service-to-service authorizations](/docs/vpc?topic=vpc-block-s2s-auth&interface=cli#block-s2s-auth-encryption-cli){: cli}[service-to-service authorizations](/docs/vpc?topic=vpc-block-s2s-auth&interface=api#block-s2s-auth-encryption-api){: api}[service-to-service authorizations](/docs/vpc?topic=vpc-block-s2s-auth&interface=terraform#block-s2s-auth-encryption-terraform){: terraform} between Cloud Block Storage (source service) and the KMS instance (target service) that holds the customer root key. If you're provisioning volumes with a CRK of another account, contact that account's administrator to set up the authorization and for the CRN of the root key that is being shared. If you're provisioning an instance with a custom image, you also need to authorize between Image Service for VPC (source service) and {{site.data.keyword.cos_full}} (target service). 
{: requirement}

## Creating data volumes with customer-managed encryption in the console
{: #data-vol-encryption-ui}
{: ui}

This procedure explains how to specify customer-managed encryption when you create a stand-alone Block Storage volume.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes** to view a list of your Block Storage volumes.
1. Click **Create**.
1. Review the **Location** information. The geography, region, and zone are inherited from the VPC (for example, North America, Dallas, Dallas-1). You can select a different zone in your location from the menu by clicking the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit"). 
1. In the **Details**, you must specify the name of the volume and the resource group that the volume is to be added to. Optionally, you can add user and access management tags.
    1. Specify a meaningful name for your volume. For example, provide a name that describes your compute or workload function.

       The volume name must begin with a lowercase letter. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-). Volume names must be unique across the entire VPC infrastructure. You can edit the name later.
       {: important}

    1. Specify a [Resource group](/docs/vpc?topic=vpc-iam-getting-started&interface=ui#iam-resource-groups).
    1. Specify [user tags](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-user-tags) to organize your resources and for use by [backup policies](/docs/vpc?topic=vpc-backup-service-about).
    1. Specify [access management tags](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-mgt-tags) that were created in IAM to help you manage access to your volumes.
1. In the **Optional configurations** section, you can specify whether you want to create the volume with data from a snapshot. Also, you can choose to apply a backup policy.
    - Import from snapshot: select **Import existing snapshot** to see the list of available snapshots, or **Import snapshot by CRN** and provide the CRN of the snapshot that you want to use. You can create data volumes with Nonbootable snapshots, and boot volumes with Bootable snapshots. The new block storage volume inherits its storage generation value from the snapshot and only volume profiles of the same generation can be applied to it.
    - Apply backup policy: click **Apply** to see available policies and plans.
1. In the **Profile** section, you can specify the performance profile of your volume, its IOPS, and capacity.
    - You can select the [`sdp` profile](/docs/vpc?topic=vpc-block-storage-profiles#defined-performance-profile). Then, specify the capacity of your volume and the required IOPS. Volume size can range from 1 - 32,000 GB. You can specify IOPS in the range of 100 - 64,000. You can also specify a custom throughput limit.
    - You can select one of the [_tiered_ profiles](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#tiers). After you select _general-purpose_, _5iops-tier_, or _10iops-tier_, the next step is to specify the volume capacity. Volume sizes can be 10 - 16,000 GB. 
    - You can select the _custom_ profile if your application performance requirements don't fall within any of the IOPS tiers. Then, specify the size of your volume and the IOPS in the appropriate range for the volume capacity. Volume sizes can be 10 - 16,000 GB. As you type the IOPS value, the UI shows the acceptable range. You can also click the **storage size** link to see the size and IOPS ranges of the [custom volume profile](/docs/vpc?topic=vpc-block-storage-profiles#custom).
1. In the **Encryption at rest** section, you can choose to keep the encryption with IBM-managed keys that is enabled by default on all volumes. Or you can choose to use [your own encryption key](/docs/vpc?topic=vpc-block-storage-vpc-encryption) by selecting your key management service: {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}. To locate your encryption key, select one of the following options:
    - **Locate by Instance**:
       1. Select the data encryption instance from the list. If you don't have an instance yet, you can click the link to create one.
       1. Select the data encryption key that is stored within the KMS instance to use for encrypting the volume.
    - **Locate by CRN**: Enter the CRN of the customer root key to be used for encrypting the volume. Choose this option if you're using the CRK of another account.

1. When your changes are complete, click **Create block storage volume**.

When you refresh the list of Block Storage volumes in the console, the new volume appears at the beginning of the list of volumes with the _customer-managed_ encryption type. When the volume is created, it shows a status of _Available_. For stand-alone volumes, the Attachment Type column is blank (-). The **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions") at the end of a table row provides a link for [attaching a Block Storage volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage).

## Creating data volumes with customer-managed encryption from the CLI
{: #data-vol-encryption-cli}
{: cli}

### Prerequisites
{: #byok-cli-setup-prereqs}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

1. Gather required information, such as the CRN of the root key that you want to use to encrypt your block storage volume.  
   1. Use the `ibmcloud resource service-instances` command to locate your KMS instances.
      ```sh
      $ ibmcloud resource service-instances
      Retrieving all instances of all services in resource group Default and all locations under account Test Account as test.user@ibm.com...
      OK
      Name             Location   State    Type
      Key Protect-17   us-south   active   service_instance
      HS-Crypto-60     us-south   active   service_instance
      ```
      {: screen}

   1. Retrieve the instance ID for the KMS instance with the `ibmcloud resource service-instance` command.
      ```sh
      ibmcloud resource service-instance "Key Protect-17" --id
      Retrieving service instance Key Protect-17 in resource group Default under account Test Account as test.user@ibm.com...
      crn:v1:bluemix:public:kms:us-south:a/a1234567-3jkl4xxxx567::7mnxxxo8-91xx-23px-q4rs-xxtuv5w6xxx7
      ```
      {: screen}

      The instance ID is the string that follows the final `::` in the CRN. In this example, it's `7mnxxxo8-91xx-23px-q4rs-xxtuv5w6xxx7`.
   1.  List the available keys and their associated CRNs for the {{site.data.keyword.keymanagementserviceshort}} service instance by specifying the instance ID.    
       ```sh
       $ ibmcloud kp keys -c --instance-id 7mnxxxo8-91xx-23px-q4rs-xxtuv5w6xxx7
       Retrieving keys...
              
       SUCCESS
                   
       Key ID                                 Key Name               CRN
       ef1gxxxh-ijxx-234x-56k7-xxxxlmnoxxp8   test-key               crn:v1:bluemix:public:kms:us-south:a/a1234567:key:ef1gxxxh-ijxx-234x-56k7-xxxxlmnoxxp8
       cdex12ef-xxxg-3hxx-i456-7xxx8jk9xl12   vsi_encrypt_root_key   crn:v1:bluemix:public:kms:us-south:a/a1234567:key:cdex12ef-xxxg-3hxx-i456-7xxx8jk9xl12
       c12xxxx3-45d6-7efg-xxx8-9xxx12345x6h   vsi_encrypt_key        crn:v1:bluemix:public:kms:us-south:a/a1234567:key:c12xxxx3-45d6-7efg-xxx8-9xxx12345x6h
       ```
       {: screen}

    By following the previous steps, you can retrieve the CRN of the customer root key from your account. If the customer root key is owned by another account, ask their account administrator for the CRN.
    {: note}

    Valid volume names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure.
    {: important}

### Create data volumes with customer-managed encryption from the CLI
{: #encrypt-data-vol-cli}
{: cli}

To create a data volume with customer-managed encryption from the CLI, first gather the CRN of the customer root key, then use the `ibmcloud is volume-create` command with the `--encryption-key` option. The `encryption_key` option must specify a valid CRN for the root key in the key management service.

The following example shows a volume that is created with customer-managed encryption.

```sh
ibmcloud is volume-create my-customer-key-volume custom us-east-1 --capacity 300 --iops 1500 --encryption-key crn:v1:bluemix:public:kms:us-east:a/a1234567:3b05b403-8f51-4dac-9114-c777d0a760d4:key:7a8a2761-08e3-455f-a348-144ed604bba9
```
{: pre}

```sh
Creating volume my-customer-key-volume under account Test Account as user test.user@ibm.com...
                                          
ID                                     r014-3984600c-6f4d-4940-82de-519a867fa3c0   
Name                                   my-customer-key-volume   
CRN                                    crn:v1:bluemix:public:is:us-east-1:a/a1234567::volume:r014-3984600c-6f4d-4940-82de-519a867fa3c0   
Status                                 pending   
Attachment state                       unattached   
Capacity                               300   
IOPS                                   1500   
Bandwidth(Mbps)                        3145   
Profile                                custom   
Encryption key                         crn:v1:bluemix:public:kms:us-east:a/a1234567:3b05b403-8f51-4dac-9114-c777d0a760d4:key:7a8a2761-08e3-455f-a348-144ed604bba9   
Encryption                             user_managed   
Resource group                         defaults   
Created                                2024-09-24T20:10:52+00:00   
Zone                                   us-east-1   
Health State                           inapplicable   
Volume Attachment Instance Reference   -   
Active                                 false  
Adjustable Capacity States             attached
Adjustable IOPS State                  attached
Busy                                   false   
Tags                                   - 
Storage Generation                     1
```
{: screen}

You can also create volumes with customer-managed encryption during instance provisioning.

## Creating data volumes with customer-managed encryption with the API
{: #data-vol-encryption-api}
{: api}

You can create data volumes with customer-managed encryption programmatically by calling the `/volumes` method in the [VPC API](/apidocs/vpc/latest#create-volume){: external} as shown in the following sample request. Use the `encryption_key` property to specify your customer root key (CRK), shown in the example as `crn:[...key:...]`.

Valid volume names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure.
{: important}

The following example creates a general-purpose data volume with customer-managed encryption.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/volumes?version=2025-02-18&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "name": "my-volume-1",
      "iops": 100,
      "capacity": 20,
      "zone": {"name": "us-south-3"},
      "profile": {"name": "general-purpose"},
      "encryption_key":{"crn":"crn:[...key:...]"},
      "resource_group": {"id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4"}
    }
```
{: screen}

A successful response looks like the following example.

```json
{
    "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8",
    "crn": "crn:[...]",
    "name": "my-volume-1",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/8948ad59-bc0f-7510-812f-5dc64f59fab8",
    "capacity": 20,
    "iops": 100,
    "encryption_key": {"crn": "crn:[...key:...]"},
    "encryption": "user_managed",
    "status": "available",
    "zone": {
        "name": "us-south-3",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/
         us-south-3"
    },
    "profile": {
        "name": "general-purpose",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose"
    },
    "resource_group": {
        "id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4",
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/
         a342dbfb-3ea7-48d1-96e8-2825ec5feab4",
        "name": "Default"
    },
    "storage_generation": 1,
    "volume_attachments": [],
    "created_at": "2025-02-18T16:03:22.000Z"
}
```
{: screen}

## Provisioning an instance with a boot volume that is encrypted with a customer-managed key in the console
{: #provision-byok-ui}
{: ui}

When you provision a virtual server instance, you can specify customer-managed encryption for your boot volume and any data volumes that you want to add. If you want, you can use a combination of provider-managed encryption and customer-managed encryption for the volumes that are associated with your instance.

Follow these steps to create an instance with a new Block Storage volume.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Compute > Virtual server instances**.
1. Click **New instance** and complete the required fields. For more information about these required fields, see _Table 1 - Instance provisioning selections_ in [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers).
1. In the **Boot volume** section, the default mode of encryption is _Provider-managed_ encryption. To specify customer-managed encryption, click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") in the boot volume row. 
1. On the **Edit boot volume** page, update the fields in the **Encryption** section. Select your key management service: ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}). To locate your encryption key, select one of the following options:
    - **Locate by Instance**:
       1. Select the data encryption instance from the list. If you don't have an instance yet, you can click the link to create one.
       1. Select the data encryption key that is stored within the {{site.data.keyword.keymanagementserviceshort}} instance to use for encrypting the volume.
    - **Locate by CRN**: Enter the CRN of the customer root key to be used for encrypting the volume. Choose this option if you're using a CRK from another account.

    If you selected a second-generation snapshot to create the volume, your volume's encryption type must match the snapshot's encryption type. If the snapshot has provider-managed keys, do not specify a key from a key management service.
    {: note}

1. When your changes are complete, click **Apply**.
1. In the **Attached Block Storage volume** section, you can click **New Block Storage volume** to add a data volume and specify customer-managed encryption. On the **New Block Storage volume** page, update the fields in the **Encryption** section. See Table 1 for more information. When your changes are complete, click **Attach**.

## Provisioning an instance with a boot volume that is encrypted with a customer-managed key from the CLI
{: #provision-byok-cli}
{: cli}

Use the [ibmcloud is instance-create](/docs/vpc?topic=vpc-vpc-reference#instance-create) command to create an instance with customer-managed encryption for your boot and data volumes. The following syntax shows how you can specify the `--boot-volume` and `--volume-attach` properties to include JSON files that define your volumes.

```sh
ibmcloud is instance-create INSTANCE_NAME VPC ZONE_NAME PROFILE_NAME SUBNET --image-id IMAGE_ID [--boot-volume @BOOT_VOLUME_JSON_FILE] [--volume-attach @VOLUME_ATTACH_JSON_FILE]...
```
{: pre}

The following `BOOT_VOLUME_JSON_FILE` example defines the properties of the boot volume. The `encryption key` property contains the root key's CRN for customer-managed encryption.

```json
{  
   "name":"volume-attachment-1",
   "volume":{  
      "name":"boot-volume-1",
      "capacity":250,
      "profile":{"name":"general-purpose"},
      "encryption_key":{"crn":"crn:[...key:...]"}
   },
   "delete_volume_on_instance_delete":true
}
```
{: screen}

The following `VOLUME_ATTACH_JSON_FILE` example defines a data volume with the 10iops-tier profile and customer-managed encryption.

```json
   {  
      "name":"volume-attachment-1",
      "volume":{  
         "name":"data-volume-1",
         "capacity":2000,
         "profile":{"name":"10iops-tier"},
         "encryption_key":{"crn":"crn:[...key:...]"}
      },
      "delete_volume_on_instance_delete":true
   }
```
{: screen}

## Provisioning an instance with a boot volume that is encrypted with a customer-managed key with the API
{: #provision-byok-api}
{: api}

You can create virtual server instances with boot volumes that use customer-managed encryption programmatically by calling the `/instances` method in the [VPC API](/apidocs/vpc/latest#create-instance){: external} as shown in the following sample request. Use the `encryption_key` property to specify your customer root key (CRK), shown in the example as `crn:[...key:...]`.

The following example creates an instance with a boot volume with customer-managed encryption and two secondary volumes with customer-managed encryption.

```sh
curl -X POST \
 "$vpc_api_endpoint/v1/instances?version=version=2020-03-10&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
     "boot_volume_attachment":{
           "volume": {
              "name":"boot-volume-1",
              "profile": {"name": "general-purpose"},
              "encryption_key": {"crn": "crn:[...key:...]"}}},
     "volume_attachments": [
            {"volume": {
              "name": "my-volume-1",
              "capacity": 1500,
              "profile": {"name": "general-purpose"},
              "encryption_key": {"crn": "crn:[...key:...]"}}},
            {"volume": {
              "name": "my-volume-2",
              "capacity": 2000,
              "profile": {"name": "general-purpose"},
              "encryption_key": {"crn": "crn:[...key:...]"}}}],
     "image": {"id": "9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366"},
     "keys": [{"id": "cf7678a3-d4fa-458b-993d-015bd4aeac80"}],
     "name": "my-test-vm2",
     "virtual_network_interface": {"subnet": {"id": "bea6a632-5e13-42a4-b4b8-31dc877abfe4"}},
     "profile": {"name": "cx2-2x4"},
     "vpc": {"id": "f0aae929-7047-46d1-92e1-9102b07a7f6f"},
     "zone": {"name": "us-south-3"}
    }'
```
{: screen}

A successful response looks like the following example. The boot volume appears under both `boot_volume_attachment` and `volume_attachment`.

```json
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

## Creating data volumes with customer-managed encryption with Terraform
{: #data-vol-encryption-terraform}
{: terraform}

### Prerequisites
{: #byok-terraform-setup-prereqs}

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

VPC infrastructure services use a specific regional endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the appropriate region in the provider block in the `provider.tf` file. See the following example of targeting a region other than the default `us-south`.
   
```terraform
provider "ibm" {
  region = "eu-de"
}
```
{: screen}

Gather required information such as a unique volume name, select a [profile](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_volume_profile){: external}, decide on the capacity and IOPS requirements. 

Valid volume names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure.

Retrieve the CRN of the customer root key for encryption by referring to the ibm_kms_key data source. For more information, see the Terraform documentation on [ibm_kms_key](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/kms_key){: external}. If the KMS is in another region or the customer root key is owned by another account, the instance cannot be retrieved by Terraform and the Terraform action fails. Contact the other account's administrator for the CRN.

### Creating stand-alone {{site.data.keyword.block_storage_is_short}} volumes with Terraform
{: #encrypted-standalone-vol-terraform}

To create a {{site.data.keyword.block_storage_is_short}} volume, use the `ibm_is_volume` resource. The following example creates a volume with a `custom` profile. The volume that is created has 200 MB capacity and can perform 1000 IOPS.

```terraform
resource "ibm_is_volume" "example" {
  name           = "my-example-volume"
  profile        = "custom"
  zone           = "us-south-1"
  iops           = 1000
  capacity       = 200
  encryption_key = "crn:v1:bluemix:public:kms:us-south:a/a1234567:e4a29d1a-2ef0-42a6-8fd2-350deb1c647e:key:5437653b-c4b1-447f-9646-b2a2a4cd6179"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume){: external}.

## Next steps
{: #next-steps-creating-byok-instances}

- After the instance is created with the encrypted boot and data volumes, associate a floating IP address to the instance that you can use to connect to your instance. For more information, see [Connecting to your Linux instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux) or [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows).
- Prepare your data volumes for use by formatting and configuring them to meet your requirements.
   - [Setting up your Block Storage for VPC data volume for use (Linux)](/docs/vpc?topic=vpc-start-using-your-block-storage-data-volume-lin)
   - [Setting up your Block Storage for VPC data volume for use (Windows)](/docs/vpc?topic=vpc-start-using-your-block-storage-data-volume-win)
- [Attach your stand-alone volumes](/docs/vpc?topic=vpc-attaching-block-storage) to a virtual server instance.
- Configure [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).
