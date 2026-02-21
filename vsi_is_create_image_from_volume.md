---

copyright:
  years: 2021, 2026
lastupdated: "2026-02-21"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an image from a volume
{: #create-ifv}

You can create an image from a volume that is attached to a stopped virtual server instance as a boot volume in the console, from the CLI, with the API, or Terraform.
{: shortdesc}

In the current release of second-generation block storage volumes, the creation of a custom image from an `sdp` boot volume with customer-managed encryption is not supported.
{: note}

## Scenarios for creating an image from a volume
{: #ifv-scenarios}

You can create an image from a volume in several ways.

* Select an instance and create an image from that instance's boot volume. The new image inherits the boot volume's encryption (customer-managed or IBM-managed).

* Select an instance, create an image from that instance's boot volume, and specify a different encryption. For example, if the boot volume was encrypted with IBM-managed encryption, you can select customer-managed encryption for the new image.

* Create an image from a boot volume from the list of Block Storage volumes. The volume must be a boot volume that is attached to a virtual server instance.

Depending on the size of the image that you're creating, the job might take from 5 minutes to 1.5 hours. You can cancel a job that's taking too long to queue. For more information, see [Performance considerations](/docs/vpc?topic=vpc-image-from-volume-vpc-manage#ifv-performance).
{: note}

When you select a name for the customer image, remember that the name can't be used by another image in the region and names that start with `ibm-` are reserved for system-provided images.

## Creating an image from a volume in the console
{: #create-image-from-volume-vpc-ui}
{: ui}

You can create an image from a volume that is attached to a virtual server instance as a boot volume in the console.

To create an image of the boot volume, the associated virtual server instance must be stopped.
{: requirement}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) > **Compute** > **Images**.

2. On the **Custom images** tab, click **Create**. The Custom image for VPC page is displayed.

3. Complete the required fields on the Import custom images page, and then continue with one of the **Source** options.

   | Field | Value |
   |-------|-------|
   | **Location** | Select the geographic area and location where you want your custom image to be available for provisioning. |
   | Geography | Select from the list.|
   | Region | Select from the list.|
   | **Details** |   |
   | Name  | A name is required for your custom image. |
   | Resource group | Select a resource group for the image. This selection can't be changed later. |
   | Tags |  You can assign a label to this resource so that you can easily filter resources in your resource list. |
   | Access management tags | As the name indicates, these tags can help organize access control without requiring updates to IAM policies. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial#tagging-create-access-group). |
   | Source | Select the source for the custom image by choosing either **Virtual server instance boot volume** (default) or **Block Storage boot volume**. |
   {: caption="Import custom image user interface fields" caption-side="bottom"}

4. Choose your image source:
   * Select the **Virtual server instance boot volume** as the source of your custom image to display a list of available instances.
       - Select the instance from the list.
       - If the instance is running and the volume selector is disabled, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Stop**.

   * Select a **Block Storage boot volume** as the source of your custom image to display a list of available boot volumes.
       - Select the volume from the list.
       - If the volume selector is disabled, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Stop attached instance**.

5. Select your encryption type, either IBM-managed encryption or customer-managed encryption.

   * The default selection is **Provider managed**. This encryption uses IBM-managed keys. You can't remove encryption later.
   * If you want to create an image that uses your own keys for encryption select the key management service where your customer root key (CRK) that protects your passphrase is stored: either **Key Protect** or **Hyper Protect Crypto Services**. Then, you can specify your key in two ways.
      * **Locate by instance**
        1. Select your key management service instance from the list.
        1. Select your key. If you don't have a key available, click **Create** to make one.
      * **Locate by CRN**
        1. Enter the CRN of your encryption key.

6. **Advanced options**: You can set allowed-use expressions and activate the Image lifecycle management.
   - Allowed-use expressions define the capabilities and restrictions of an image to help you find compatible image and profile combinations during server creation. You can enter [allowed-use expressions](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions) for virtual server instances and bare metal servers.
   - You can use the toggle to activate Image lifecycle Management. By selecting this option, you can manage when your image becomes `deprecated` and `obsolete`. You can schedule a single status change or schedule the complete lifecycle of the images. You can schedule status changes by using calendar date and time or number of days. The obsolescence date must always be after the deprecation date. For more information, see [Custom image lifecycle](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#custom-image-lifecycle).

6. On the right-side panel, click **Create custom image**.

## Viewing and using your custom image
{: #ifv-image-creation-completed}
{: ui}

When the image is created, it appears in the list of custom images.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure ![VPC icon](../../icons/vpc.svg) > Compute > Images**.
2. On the **Custom images** tab, click the image name to see the volume from which it was created.
3. Review the image details. Click the link in the **Source** section to go to the source volume.

To use this image when you create an instance, select it as the operating system type.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure![VPC icon](../../icons/vpc.svg) > Compute > Virtual server instances**.
2. Click **Create**. For more information about how to provision a new virtual server instance, see [Creating virtual server instances in the console](/docs/vpc?topic=vpc-creating-virtual-servers). For the **Operating system**, select the **Custom image** tile.
3. Click **Select custom image** and select the image from volume that you created.

## Create an image from a volume with the CLI
{: #image-from-volume-vpc-cli}
{: cli}

Use the command-line interface (CLI) to create an image from a volume that is attached to a virtual server instance as a boot volume.

### Before you begin
{: #before-creating-ifv-cli}

1. Make sure that you downloaded, installed, and initialized the following CLI plug-ins:
   * {{site.data.keyword.cloud_notm}} CLI
   * The infrastructure-service plug-in

   For more information, see the [CLI Reference](/docs/vpc?topic=vpc-vpc-reference&interface=cli).

1. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-getting-started).

### Create an image from a boot volume that is attached to an instance
{: #ifv-create-cli}

1. To locate the instance that you want to create an image from, [list the instances](/docs/vpc?topic=vpc-vpc-reference#instances-list) in the region. Take note of the instance ID in the command output.
   ```sh
   ibmcloud is instances
   ```
   {: pre}

1. [Stop the running instance](/docs/vpc?topic=vpc-vpc-reference#instance-stop) before you create the image from the volume.

   ```sh
   ibmcloud is instance-stop INSTANCE_ID
   ```
   {: pre}

1. Run the [`image-create` command](/docs/vpc?topic=vpc-vpc-reference#image-create) to create an image of a boot volume. Specify the ID of the source volume.

   ```sh
   ibmcloud is image-create IMAGE_NAME [--source-volume VOLUME_ID]
   ```
   {: pre}

   See the following example:

   ```sh
   $ ibmcloud is image-create test-ifv-vol1 --source-volume ecc68c2f-96a1-4862-bc86-14f47e5d9ed8

   Creating image test-ifv-vol1 in resource group  under account Test Account as user test.user@ibm.com...
   ID                 5b2fd4ee-c636-44c4-9673-453fca36832e
   Name               test-ifv-vol1
   CRN                crn:v1:bluemix:public:is:us-south:a/a123456::image:5b2fd4ee-c636-44c4-9673-453fca36832e
   Status             pending
   Status reason      Code                   Message                                                More info
                      image_request_queued   The image request is accepted and waiting for system   -
                                             resources to become available.
   Operating system   Name             Architecture   Vendor   Version                 Dedicated host only
                      centos-8-amd64   amd64          CentOS   8.x - Minimal Install   false
   Source volume      ID                                     Name
                      ecc68c2f-96a1-4862-bc86-14f47e5d9ed8   aa-1-bx-boot-1617035447000
   Created            2021-05-20T09:43:16+08:00
   Visibility         private
   File size(GB)      -
   Encryption         none
   Resource group     f22cf48f-8836-4527-9131-1d7c73ba85e9
   ```
   {: screen}

### Schedule custom image lifecycle status changes by using the CLI
{: #ifv-import-schedule-ilm-status-change-cli}



When you create a custom image, you can also schedule the lifecycle status changes of the {{site.data.keyword.vpc_short}} custom image at the same time by using the  `--deprecate-at` or `--obsolete-at` options of the `ibmcloud is image-create` command.

To schedule the `deprecate-at` or `obsolete-at` properties, specify a date in the ISO 8601 (`YYYY-MM-DDThh:mm:ss+hh:mm`) date and time format.

* `YYYY` is the four-digit year.
* `MM` is the two-digit month.
* `DD` is the two-digit day.
* `T` separates the date and time information.
* `hh` is the two-digit hours.
* `mm` is the two-digit minutes.
* `+hh:mm` or `-hh:mm` is the Coordinated Universal Time time zone.

For example, the date of 30 September 2025 at 8:00 PM in the North American Central Standard time zone (CST) is `2025-09-30T20:00:00-06:00`

When you schedule the date and time, you can't use your current date and time. The scheduled date and time must be in the future. If you define both the `deprecate-at` and `obsolete-at` dates and times, the `deprecate-at` date must be after the `obsolete-at` date and time.

```sh
ibmcloud is image-create IMAGE_NAME [--source-volume VOLUME_ID] [--deprecate-at YYYY-MM-DDThh:mm:ss+hh:mm] [--obsolete-at YYYY-MM-DDThh:mm:ss+hh:mm]
```
{: pre}

## Creating an image from a volume with the API
{: #image-from-volume-vpc-api}
{: api}

Create custom images from {{site.data.keyword.block_storage_is_short}} boot volumes programmatically by making calls to the [VPC REST APIs](/apidocs/vpc). You can list all instances and volumes, then use the details of the specific volume to create the image.

### Before you begin
{: #ifv-prereqs-api}

Before you begin, make sure that you [set up your API environment](/docs/vpc?topic=vpc-creating-block-storage#block-storage-api-prereqs).

### Creating an image from a boot of an instance with the API
{: #ifv-vpc-api-create-instance}

1. Locate the instance and boot volume that you want to create an image from. Get the list of the available instances with the [list instance](/apidocs/vpc/latest#list-instances) method.
      ```sh
      curl -X GET "$vpc_api_endpoint/v1/instances?version=2024-06-11&generation=2" -H "Authorization: Bearer $iam_token"
      ```
      {: pre}

1. Take a note of the instance ID, and use it to [retrieve the instance](/apidocs/vpc/latest#get-instance) details.
      ```sh
      curl -X GET "$vpc_api_endpoint/v1/instances/$instance_id?version=2024-06-11&generation=2" -H "Authorization: Bearer $iam_token"
      ```
      {: pre}

      The response contains the name, ID, and other details of the boot volume in the `volume_attachments` section.

      ```json
      "volume_attachments": [
       {
        "device": {
          "id": "a8a15363-a6f7-4f01-af60-715e85b28141"
          },
        "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-4aa2-a8d7-703aac843651/volume_attachments/7389-a8a15363-a6f7-4f01-af60-715e85b28141",
        "id": "a8a15363-a6f7-4f01-af60-715e85b28141",
        "name": "my-boot-volume-attachment",
        "volume": {
          "crn": "crn:[...]",
          "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/49c5d61b-41e7-4c01-9b7a-1a97366c6916",
          "id": "49c5d61b-41e7-4c01-9b7a-1a97366c6916",
          "name": "my-boot-volume"
          }
        }
      ]
      ```
      {: screen}

1. If the instance is running, stop it by specifying the `stop` action in a `POST /instances` call:
      ```sh
      curl -X POST \
      "$vpc_api_endpoint/v1/instances/{instance-id}/gen/actions?ersion=2024-06-11&generation=2"
      -H "Authorization: Bearer $iam_token"\
      -d '{"type":"stop"}'
      ```
      {: pre}

1. By using the boot volume ID, [create an image](/apidocs/vpc/latest#create-image). See the following example:
      ```sh
       curl -X POST "$vpc_api_endpoint/v1/images?version=2024-06-11&generation=2"
       -H "Authorization: Bearer $iam_token"
       -d '{
            "name": "my-image",
            "source_volume": {"id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7"}
            }`
      ```
      {: pre}

      In the example response, `source_volume` indicates the boot volume that is used to create the image. Also, notice that the image encryption appears as `none` because the source volume used the default IBM-managed encryption. If you [use you own root key](#ifv-use-crk), the response  shows `user-managed` instead.

      ```json
      {
       "created_at": "2021-05-20T00:05:13.873893Z",
       "crn": "crn:[...]",
       "encryption": "none",
       "file": {},
       "href": "https://us-south.iaas.cloud.ibm.com/v1/images/4fec84ef-baf9-405d-bf3b-9d5a60e068f7",
       "id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7",
       "name": "test-ifv-boot-volume",
       "operating_system": {
         "architecture": "amd64",
         "display_name": "CentOS 8.x - Minimal Install (amd64)",
         "family": "CentOS",
         "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/centos-8-amd64",
         "name": "centos-8-amd64",
         "vendor": "CentOS",
         "version": "8.x - Minimal Install"
         },
       "resource_group": {
         "href": "https://us-south.iaas.cloud.ibm.com/v1/resource_groups/3fad3f2204eb4998c3964d254ffcd771",
         "id": "3fad3f2204eb4998c3964d254ffcd771",
         "name": "Default"
         },
       "status": "pending",
       "status_reasons": [],
       "visibility": "private",
       "source_volume":  {
         "id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7",
         "crn": "crn:[...]",
         "href": "",
         "name": "my-boot-volume"
         }
       }
      ```
      {: screen}

### Creating an image from volume and specifying your own encryption key
{: #ifv-use-crk}

1. As described in the previous section, locate the instance, and boot volume information. Stop the instance.
1. Get your key management service instance ID and root key CRN.
   - [{{site.data.keyword.keymanagementserviceshort}} - Retrieving your instance ID and cloud resource name (CRN)](/docs/key-protect?topic=key-protect-retrieve-instance-ID&interface=api).
   - [{{site.data.keyword.hscrypto}} - Retrieving your instance ID](/docs/hs-crypto?topic=hs-crypto-retrieve-instance-ID&interface=api).

1. Make an API request to [create an image](/apidocs/vpc/latest#create-image). Specify the volume ID and the CRN of the root key as shown in the following example.
   ```sh
   curl -X POST "$vpc_api_endpoint/v1/images?version=2024-06-11&generation=2"
   -H "Authorization: Bearer $iam_token"
       -d '{
            "name": "my-encrypted-image",
            "source_volume": {"id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7"}
            "encryption_key":{"crn":"crn:[...key:...]"
           },
   ```
   {: pre}

   The response includes information about the root key.

   ```json
   {
    "created_at": "2021-05-20T00:05:13.873893Z",
    "crn": "crn:[...]",
    "encryption_key": {
      "crn": "crn:[...key:...]"
      },
    "encryption": "user_managed",
    "file": {},
    "href": "https://us-south.iaas.cloud.ibm.com/v1/images/4fec84ef-baf9-405d-bf3b-9d5a60e068f7",
    "id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7",
    "name": "test-ifv-boot-volume",
    "operating_system": {
      "architecture": "amd64",
      "display_name": "CentOS 8.x - Minimal Install (amd64)",
      "family": "CentOS",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/centos-8-amd64",
      "name": "centos-8-amd64",
      "vendor": "CentOS",
      "version": "8.x - Minimal Install"
      },
    "resource_group": {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/resource_groups/3fad3f2204eb4998c3964d254ffcd771",
      "id": "3fad3f2204eb4998c3964d254ffcd771",
      "name": "Default"
      },
    "status": "pending",
    "status_reasons": [],
    "visibility": "private",
    "source_volume":  {
      "id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7",
      "crn": "crn:[...]",
      "href": "",
      "name": "ifv-boot-vol"
      }
    }
    ```
    {: screen}

### Scheduling custom image lifecycle status changes with the API
{: #ifv-import-schedule-ilm-status-change-API}

When you create an image from a volume, you can schedule the lifecycle status changes of the custom image at the same time. 

To schedule the `deprecation_at` or `obsolescence_at` properties, specify a date in the ISO 8601 (`YYYY-MM-DDThh:mm:ss+hh:mm`) date and time format.

* `YYYY` is the four-digit year
* `MM` is the two-digit month
* `DD` is the two-digit day
* `T` separates the date and time information
* `hh` is the two-digit hours
* `mm` is the two-digit minutes
* `+hh:mm` or `-hh:mm` is the Coordinated Universal Time time zone

For example, the date of 30 September 2023 at 8:00 PM in the North American Central Standard time zone (CST) is `2023-09-30T20:00:00-06:00`

When you schedule the date and time, you can't use your current date and time. The scheduled date and time must be in the future. If you define both the `deprecation_at` and `obsolescence_at` dates and times, the `obsolescence_at` date must be after the `deprecation_at` date and time.

```sh
curl -X POST "$vpc_api_endpoint/v1/images?version=2023-02-21&generation=2"\
-H "Authorization: Bearer $iam_token"\
-d '{
      "name": "test-ifv-boot-volume",
      "source_volume": {
    	  "id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7"
      },
      "deprecation_at": "2025-11-28T06:11:28+05:30",
      "obsolescence_at": "2025-12-28T06:11:28+05:30"
    }'
```
{: pre}

## Creating an image from a volume with Terraform
{: #image-from-volume-vpc-terraform}
{: terraform}

Create custom images from {{site.data.keyword.block_storage_is_short}} boot volumes with Terraform by using [ibm_is_image](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_image){: external} resource. After specifying the name of the image, specify the source volume ID. 

Optionally, you can also specify the CRN of your customer root key to be used to encrypt the custom image, and set [allowed-use expressions](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=terraform#custom-image-allowed-use-expressions-terraform). See the following example:

```terraform
resource "ibm_is_image" "example" {
  name               = "example-image"
  source_volume      = "xxxx-xxxx-xxxxxxx"
  encrypted_data_key = "eJxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx0="
  encryption_key     = "crn:v1:bluemix:public:kms:us-south:a/6xxxxxxxxxxxxxxx:xxxxxxx-xxxx-xxxx-xxxxxxx:key:dxxxxxx-fxxx-4xxx-9xxx-7xxxxxxxx"
  allowed_use {
    api_version       = "2025-04-03"
    bare_metal_server = "enable_secure_boot == true"
    instance          = "enable_secure_boot == true"
  }  

  //increase timeouts as per volume size
  timeouts {
    create = "45m"
  }
}
```
{: codeblock}

You can also schedule the lifecycle status changes of an {{site.data.keyword.vpc_short}} custom image at the same time by using the `deprecation_at` or `obsolescence_at` attributes. Specify a date in the ISO 8601 (`YYYY-MM-DDThh:mm:ss+hh:mm`) date and time format.

* `YYYY` is the four-digit year
* `MM` is the two-digit month
* `DD` is the two-digit day
* `T` separates the date and time information
* `hh` is the two-digit hours
* `mm` is the two-digit minutes
* `+hh:mm` or `-hh:mm` is the Coordinated Universal Time time zone

For example, the date of 30 September 2025 at 8:00 PM in the North American Central Standard time zone (CST) is `2025-09-30T20:00:00-06:00`

When you schedule the date and time, you can't use your current date and time. The scheduled date and time must be in the future. If you define both the `deprecation_at` and `obsolescence_at` dates and times, the `obsolescence_at` date must be after the `deprecation_at` date and time.

* Schedule a status change to `deprecated`.

   ```terraform
   resource "ibm_is_image" "example" {
     name               = "example-image"
     source_volume      = "xxxx-xxxx-xxxxxxx"
     encrypted_data_key = "eJxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx0="
     encryption_key     = "crn:v1:bluemix:public:kms:us-south:a/6xxxxxxxxxxxxxxx:xxxxxxx-xxxx-xxxx-xxxxxxx:key:dxxxxxx-fxxx-4xxx-9xxx-7xxxxxxxx"
     deprecated_at      = "2025-11-28T15:10:00.000Z"
   }
   ```
   {: pre}

* Schedule a status change to `obsolete`.

   ```terraform
   resource "ibm_is_image" "example" {
     name               = "example-image"
     source_volume      = "xxxx-xxxx-xxxxxxx"
     encrypted_data_key = "eJxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx0="
     encryption_key     = "crn:v1:bluemix:public:kms:us-south:a/6xxxxxxxxxxxxxxx:xxxxxxx-xxxx-xxxx-xxxxxxx:key:dxxxxxx-fxxx-4xxx-9xxx-7xxxxxxxx"
     obsolescence_at    = "2025-12-28T15:10:00.000Z"
   }
   ```
   {: pre}

## Next steps
{: #ifv-next-steps-api}

- [Manage your image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc-manage).
- [Create a virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers) with your custom image.
