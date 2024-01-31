---

copyright:
  years: 2021, 2023
lastupdated: "2023-12-18"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an image from a volume
{: #create-ifv}

Use the UI, CLI, or API to create an image from a volume that is attached to an available virtual server instance as the primary boot volume or as a secondary boot volume.
{: shortdesc}

## Scenarios for creating an image from a volume
{: #ifv-scenarios}

You can create an image from a volume in several ways.

* Select an instance and create an image from that instance's boot volume. The new image inherits the boot volume encryption (customer-managed or IBM-managed).

* Select an instance, create an image from that instance's boot volume, and specify different encryption. For example, if the instance's boot volume was encrypted with IBM-managed encryption, you can select customer-managed encryption for the new image.

* Create an image from a boot volume in the list of Block Storage volumes. The volume must be a boot volume that is attached to a virtual server instance.

Depending on the size of the image that you're creating, the job might take from 5 minutes to 1.5 hours. You can cancel a job that's taking too long to queue. For more information, see [Performance considerations](/docs/vpc?topic=vpc-image-from-volume-vpc-manage#ifv-performance).
{: note}

## Creating an image from a volume in the UI
{: #create-image-from-volume-vpc-ui}
{: ui}

Use the UI to create an image from a volume that is attached to an available virtual server instance as the primary boot volume or as a secondary boot volume.

### Choosing the source for your custom image
{: #import-custom-image-src-UI}

Use the UI to import your custom image by choosing to create and import an image from a volume.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external},click the **Navigation Menu** ![menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) Compute > Images**.

2. On the **Custom images** tab, click **Create**. The Import custom image page is displayed.

3. Complete the required fields on the Import custom images page (see Table 1), and then continue with one of the **Source** options.

| Field | Value |
|-------|-------|
| Name  | A name is required for your custom image. |
| Resource group | Select a resource group for the instance. |
| Tags |  You can assign a label to this resource so that you can easily filter resources in your resource list. |
| Region | Select the location, or specific geographic area, where you want your custom image to be available for provisioning.|
| Source | Select the source for the custom image by choosing either [Virtual server instance boot volume](#import-custom-image-vsi) (default) or [Block Storage boot volume](#import-custom-image-vol). |
| Manage image lifecycle (optional) | Select to schedule status changes for the image. You can schedule a single status change or schedule the complete lifecycle of the images. The image statuses are:  \n  \n * `available`: The image can be used to create an instance.  \n  \n * `deprecated`: The image is still available to use to provision and instance. Using the `deprecated` status can discourage use of the image before the status changes to `obsolete`.  \n * `obsolete`: The image is not available to use to provision an instancce.  \n  \n * Schedule complete lifecycle: You can schedule both the `deprecated` and `obsolete` status changes at the same time.  \n  \n You can move back and forth between the three statuses. Only the statuses you can change to are displayed. You can schedule status changes by using calendar date and time or number of days. The obsolescence date must always be after the deprecation date. |
{: caption="Table 1. Import custom image user interface fields" caption-side="bottom"}

### Create an image from a virtual server instance boot volume
{: #import-custom-image-vsi}

When you select **Virtual server instance boot volume** as the source of your custom image, a list of instances displays. To create an image from the instance's boot volume:

1. On the **Import custom image** page, select **Virtual server instance boot volume** (default).

   To create an image of the boot volume associated with an instance, the virtual server instance must be stopped.
   {: note}

1. Optionally, stop a running instance by clicking the overflow menu (ellipsis) and click **Stop**.

1. Select an instance from the list. You're selecting the boot volume of the instance.

1. Select the encryption type, either customer-managed encryption or IBM-managed encryption (see [Table 2](#encrypt-custom-images)).

1. On the side panel, click **Create custom image**.

### Create an image from the list of boot volumes
{: #import-custom-image-vol}

When you select **Block Storage boot volume** as the source of your custom image, a list of Block Storage volumes displays.

To create an image from the volume:

1. On the **Import custom image** page, select **Block Storage boot volume**.

   A list of Block Storage volumes shows attached **Boot** and **Data** volumes. Unattached volumes appear with a dash (-). To create an image from a volume:

   * The volume must be a boot volume
   * The volume must have an `available` status
   * The volume must be attached to a virtual server instance
   * The instance must be stopped

1. Optionally, stop a running instance by clicking the overflow menu (ellipsis) and click **Stop attached instance**. Click refresh and select the volume.

1. Select the volume from the list.

1. Select the encryption type, either customer-managed encryption or IBM-managed encryption (see [Table 2](#encrypt-custom-images)).

1. On the side panel, click **Create custom image**.


### Select the encryption type
{: #encrypt-custom-images}

To complete your image from volume, select either IBM-managed encryption or customer-managed encryption. Table 2 describes these options as they appear in the UI.

| Field | Value |
|-------|-------|
| Encryption | The default selection is **No encryption**. If you have not encrypted your image by using QEMU, use the default value, No encryption. If you are importing an image that you encrypted by using QEMU and your own passphrase, select the key management service where your customer root key (CRK) that protects your passphrase is stored. Select either **Key Protect** or **Hyper Protect Crypto Services**. |
| Encryption service instance | For an encrypted image, select the specific instance of the key management service where your CRK that wraps your encryption passphrase is stored. |
| Key name | Select the customer root key (CRK) that you used to wrap your encryption passphrase. |
| Key ID | Shows the key ID that is associated with the data encryption key that you selected. |
{: caption="Table 2. Encryption types" caption-side="bottom"}

For more information, see [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs).

### View and use your image from volume
{: #ifv-image-creation-completed}

When the image from a volume is created, it appears in the list of custom images.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images**.

2. On the **Custom images** tab, click the image name to see the volume from which it was created. The image details panel links to the source volume.

To use this image when you create a new instance, select it as operating system type when you create an instanceL

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.

2. Click **Create**. For more information about how to provision a new virtual server instance, see [Creating virtual server instances in the UI](/docs/vpc?topic=vpc-creating-virtual-servers). For the **Operating system**, select the **Custom image** tile.

3. Click **Select custom image** and select the image from volume that you created.


## Create an image from a volume with the CLI
{: #image-from-volume-vpc-cli}
{: cli}

Use the CLI to create an image from a volume that is attached to an available virtual server instance as the primary boot volume or as a secondary boot volume.

### Before you begin
{: #before-creating-ifv-cli}

1. Make sure that you downloaded, installed, and initialized the following CLI plug-ins:
   * {{site.data.keyword.cloud_notm}} CLI
   * The infrastructure-service plug-in

   For more information, see the [CLI Reference](/docs/vpc?topic=vpc-vpc-reference&interface=cli).

   After you install the vpc-infrastructure plug-in, set the target to generation 2 by running the command `ibmcloud is target --gen 2`.
   {: important}


2. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-getting-started).

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

   Example:

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

When you import a custom image by using the command-line interface (CLI), you can also schedule the lifecycle status changes of the {{site.data.keyword.vpc_short}} custom image at the same time by using options of the **`ibmcloud is image-create`** command.

Specify the name of the custom image to be created by using the `IMAGE_NAME` variable and the source by using the `--source-volume` option to indicate that the source is an existing boot volume.

To schedule the `deprecate-at` or `obsolete-at` properties, specify a date in the ISO 8601 (`YYYY-MM-DDThh:mm:ss+hh:mm`) date and time format.

* `YYYY` is the four digit year
* `MM` is the two digit month
* `DD` is the two digit day
* `T` separates the date and time information
* `hh` is the two digit hours
* `mm` is the two digit minutes
* `+hh:mm` or `-hh:mm` is the UTC time zone

Thus, the date of 30 September 2023 at 8:00 p.m. in the North American Central Standard Time Zone (CST) would be `2023-09-30T20:00:00-06:00`

When scheduling the date and time, you can't use your current date and time. For example, if it is 8 a.m. on June 12, then the scheduled date and time must be after 8 a.m. on June 12. If you define both the `deprecate-at` and `obsolete-at` dates and times, the `deprecate-at` date must be after the `obsolete-at` date and time.

```sh
ibmcloud is image-create IMAGE_NAME [--source-volume VOLUME_ID] [--deprecate-at YYYY-MM-DDThh:mm:ss+hh:mm] [--obsolete-at YYYY-MM-DDThh:mm:ss+hh:mm]
```
{: pre}

## Creating an image from a volume by using the API
{: #image-from-volume-vpc-api}
{: api}

Use the regional API to create an image from volume when you create an instance or from an existing instance.

### Before you begin
{: #ifv-prereqs-api}

Before you can use the API, you must take some preliminary steps. For more information, see [API prerequisites](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).

### Creating an image from volume when you create an instance (API)
{: #ifv-vpc-api-create-instance}

Follow these steps to create an image from volume when you create a new instance. The new instance's boot volume is used for the image, which inherits the encryption from the boot volume. In this scenario, the boot volume uses the default IBM-managed encryption, which is inherited by the new image. You can also specify your own root encryption key in the `POST/images` call. For more information, see [Create an image from volume and use your root encryption key](#ifv-use-crk).

This procedure:

1. Creates an instance and uses its boot volume ID as the source volume.
2. Stops the running instance.
3. Creates an image with the ID of the boot volume. In this scenario, the boot volume is encrypted with the default IBM-managed encryption. If the boot volume was encrypted with a [customer root key](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption), that root key is used to encrypt the new image. The API response displays the [CRN of the root key](#ifv-use-crk).

#### Step 1 - Creating an instance
{: #ifv-create-instance}

Specify a `POST /instances` request to create an instance and pass the required parameters. See the following example.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/instances?version=2021-05-20"\
-H "Authorization: Bearer $iam_token"\
-d '{
   "name":"test-ifv-instance",
   "profile":{
      "name":"bx2-2x8"
   },
   "vpc":{
      "id":"6054c339-8a17-4c75-958c-4a23358a37ed"
   },
   "image":{
      "id":"524d52aa-b267-4801-a7d2-6d4fe75baf65"
   },
   "zone":{
      "href":"https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-3"
   },
   "primary_network_interface":{
      "name":"ifv-nic",
      "subnet":{
         "id":"7382-4b33d349-1752-4bf9-ab73-751ec065e193"
      }
   }
}'
```
{: codeblock}

#### Step 2 - Stopping the running instance
{: #ifv-stop-instance-api}

Stop the running instance by specifying the `stop` action in a `POST /instances` call:

```sh
curl -X POST \
"$vpc_api_endpoint/v1/instances/{instance-id}/gen/actions?version=2021-05-20"
-H "Authorization: Bearer $iam_token"\
-d '{
   "type":"stop"
}'
```
{: codeblock}

#### Step 3 - Creating an image and provide the ID of the boot volume image
{: #ifv-create-image}

The source boot volume originates from an image. This boot volume is used to populate the new image's operating system information.

Create an image with a `POST /images` request and pass the boot volume ID of the instance that you created in [Step 1](#ifv-create-instance) as source volume. See the following example:

```sh
curl -X POST \
"$vpc_api_endpoint/v1/images?version=2021-05-20"
-H "Authorization: Bearer $iam_token"\
-d '{
    "name": "test-ifv-boot-volume",
    "source_volume": {
    	"id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7"
    }
}``
```
{: codeblock}

In the example response, `source_volume` indicates the boot volume that is used to create the image. Also, notice that the image encryption appears as `none` because the source volume used the default IBM-managed encryption. If you [used you own root key](#ifv-use-crk), the response would show `user-managed` instead.

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
    "name": "ifv-boot-vol"
  }
}
```
{: codeblock}

### Creating an image from volume and specifying your own encryption key
{: #ifv-use-crk}

In this scenario, you're creating an image from a volume and specifying your root key in the `POST /images` call.

The example request specifies the CRN of the root key in the `encryption_key` parameter:

```sh
curl -X POST \
"$vpc_api_endpoint/v1/images?version=2021-05-20"
-H "Authorization: Bearer $iam_token"\
-d '{
    "name": "test-ifv-boot-volume",
    "source_volume": {
        "id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7"
    },
    "encryption_key":{
      "crn":"crn:[...key:...]"
    },
}`
```
{: codeblock}

The response includes information about the root key:

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
{: codeblock}

### Creating an image from a volume of an existing instance
{: #ifv-vpc-api-list-instance}

You can also create an image from a boot volume that is attached to an existing instance. This procedure:

1. Lists all instances and then, gets the ID of a boot volume that is attached to an available, running instance.
2. Stops the running instance.
3. Creates an image by using the ID of the boot volume image.

#### Step 1 - Locating the instance and the boot volume ID
{: #ifv-locate-instance}

Make a `GET /instances` call to list all instances and locate the available, running instance that you need. See the following example:

```sh
curl -X GET \
"$vpc_api_endpoint/v1/instances?version=2023-08-04&generation=2" \
-H "Authorization: Bearer $iam_token"
```
{: pre}

Take the instance ID from the API response and make a `GET /instances/{id}` request to view details of the instance and locate its boot volume ID.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/instances/$instance_id?version=2023-08-04&generation=2"\
-H "Authorization: Bearer $iam_token"
```
{: pre}

The response shows the ID of the boot volume under `volume_attachments`:

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
{: codeblock}

#### Step 2 - Stopping the running instance
{: #ifv-stop-instance}

Stop the running instance by specifying the `stop` action in a `POST /instances` call:

```sh
curl -X POST \
"$vpc_api_endpoint/v1/instances/$instance_id/actions?version=2023-08-04&generation=2"\ 
-H "Authorization: Bearer $iam_token"\
-d '{
  "type": "stop"
  }'
```
{: pre}

#### Step 3 - Creating an image and providing the ID of the boot volume image
{: #ifv-create-image-boot-id}

Create an image with a `POST /images` request and pass the boot volume ID of the instance.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/images?version=2021-05-20"
-H "Authorization: Bearer $iam_token"\
-d '{
    "name": "test-ifv-boot-volume",
    "source_volume": {
    	"id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7"
    }
}`
```
{: codeblock}

### Scheduling custom image lifecycle status changes by using the API
{: #ifv-import-schedule-ilm-status-change-API}

When you create an image from volume by using the application programming interface (API), you can schedule the lifecycle status changes of the {{site.data.keyword.vpc_short}} image from volume at the same time.

The `name` can't be used by another image in the region and names that start with `ibm-` are reserved for system-provided images. Specify the `source.volume` subproperty to indicate the source of the image from volume.

To schedule the `deprecation_at` or `obsolescence_at` properties, specify a date in the ISO 8601 (`YYYY-MM-DDThh:mm:ss+hh:mm`) date and time format.

* `YYYY` is the four digit year
* `MM` is the two digit month
* `DD` is the two digit day
* `T` separates the date and time information
* `hh` is the two digit hours
* `mm` is the two digit minutes
* `+hh:mm` or `-hh:mm` is the UTC time zone

Thus, the date of 30 September 2023 at 8:00 p.m. in the North American Central Standard Time Zone (CST) would be `2023-09-30T20:00:00-06:00`

When scheduling the date and time, you can't use your current date and time. For example, if it is 8 a.m. on June 12, then the scheduled date and time must be after 8 a.m. on June 12. If you define both the `deprecation_at` and `obsolescence_at` dates and times, the `obsolescence_at` date must be after the `deprecation_at` date and time.

```sh
curl -X POST "$vpc_api_endpoint/v1/images?version=2023-02-21&generation=2"\
-H "Authorization: Bearer $iam_token"\
-d '{
      "name": "test-ifv-boot-volume",
      "source_volume": {
    	  "id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7"
      },
      "deprecation_at": "2023-03-01T06:11:28+05:30",
      "obsolescence_at": "2023-12-31T06:11:28+05:30"
    }'
```
{: pre}

## Next steps
{: #ifv-next-steps-api}

[Use your image from a volume](#ifv-image-creation-completed) when you create an instance.
[Manage your image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc-manage).
