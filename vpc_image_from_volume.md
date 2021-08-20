---

Copyright:
  years: 2021
lastupdated: "2021-08-20"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:note: .note}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}


# About creating an image from a volume
{: #image-from-volume-vpc}

You can create a custom image from a boot volume on a virtual server instance. The image is a full copy of the source volume. It includes the operating system and any user data. You can create new virtual server instances from the image created from the volume.
{:shortdesc}

## Image from volume overview
{: #vpc-ifv-overview}

With this service, you can create a custom image in your region from an instance's boot volume. The OS information for the custom image comes from the boot volume you select to create the image.

From the UI and CLI, you select a virtual server instance and its boot volume or select an instance's boot volume from the list of block storage volumes. In the regional API, you create an image by making a `POST /images` call and passing the boot volume ID of the instance.

The custom image you create is regional, inherited from the boot volume. All custom images are private to the account in which they are created and labeled as _custom_. The image is a full copy of the volume. It has the same capacity and contains identical data. The image also inherits the [encryption type](#ifv-encryption). If the volume used provider-managed encryption, you can specify customer-managed and provide your own root keys. If the original volume already used customer-managed encryption, the image inherits the root key, which you can change and use another key.

Images from a volume have a lifecycle independent from the original volume. You can update the image and delete it as needed. Finally, you can use the custom image from a volume to create a new instance.

### Image from volume features
{: #vpc-ifv-features}

Image from volume features include:

* Create an image from volume from the UI, CLI, or API.
* Independently manage the custom image. The custom image from a volume has an independent lifecycle. You can delete the original volume and the custom image persists.
* Delete an image in the process of being created or one that's available.
* Optionally specify resource groups and tags for your image.
* Access images regionally. By default, the region for the custom image is inherited from the volume. All custom images are private to the account in which they're created and labeled as _custom_.
* Specify your own encryption for the image and use your own root keys.

The custom image created from a volume:

* Contains a full copy of the original volume and has the same capacity.
* Includes the operating system data and metadata that specifies the minimum volume size.
* Is integrated with IAM, BSS, Ghost, and resource groups.

### Requirements
{: #ifv-requirements}

To create an image from a volume, you must meet these requirements:

* Instance must be in an available state.
* Available, running instance must be stopped before creating an image from volume.
* Volume must be attached to an instance.
* Volume must be a primary boot volume.
* Volume must be in the same region. 

### Limitations
{: #ifv-limitations}

These limitations apply to the release:

* Creating from a running instance is not allowed. The instance must be stopped.
* Instances must be in an available state. 
* Unattached boot volumes are not supported.
* Images must be created from a primary boot volume. Data volumes are not supported.
* Boot volumes used as the source for new encrypted images must have 100 GB capacity. For more information, see [Troubleshooting custom images](/docs/vpc?topic=vpc-ifv-troubleshooting-custom-images).
* You can't create an image from an encrypted boot volume that's not 100 GB. The operation will be blocked.

## Options for creating an image from a volume
{: #vpc-ifv-procedures}

You can create an image from a volume in several ways:

* By using the [UI](/docs/vpc?topic=vpc-create-ifv#image-from-volume-vpc-api):
  - Select a virtual server instance. This action selects the instance's boot volume.
  - List all block storage volumes, select a boot volume attached to an instance, and then create an image from that boot volume.
  - Create an image from the boot volume details page.
  - Create an image from the virtual server instance details page.
* By using [API](/docs/vpc?topic=vpc-create-ifv#image-from-volume-vpc-api) or [CLI](/docs/vpc?topic=vpc-create-ifv#image-from-volume-vpc-cli):
  - Create an image from a volume when you create a new instance.
  - Create an image from a volume of an existing instance.

  ## Image from volume encryption
  {: #ifv-encryption}

  When you create an image from a volume, the encryption for that image depends on the way that you created the image:

  * If you're creating a new instance and boot volume with default IBM-managed encryption, then the image from that boot volume inherits the IBM-managed encryption. However,
  
  * You can specify your own root key when you create the image. This key is called a [customer root key](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption), or CRK. In the API, for example, you'd make a `POST/images` call and specify the boot volume ID and the CRN of your root key.
  
  * If you're creating an image from an encrypted boot volume on an existing instance, and that boot volume uses a CRK, then the image from that volume inherits that root key. 

  * If you're creating an image from a boot volume by choosing a [block storage boot volume as the source](/docs/vpc?topic=vpc-create-ifv#import-custom-image-vol), the new image inherits the encryption from the source volume.

## Next Steps
{: #ifv-next-steps}

* Preview the end-to-end process for creating an image from volume, then using the image when creating a new virtual server instance. See the [Image from volume tutorial](/docs/vpc?topic=vpc-creating-and-using-an-image-from-volume).

* [Create an image from volume](/docs/vpc?topic=vpc-create-ifv).
