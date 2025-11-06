---

copyright:
  years: 2021, 2025
lastupdated: "2024-06-12"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About creating an image from a volume
{: #image-from-volume-vpc}

You can create a custom image from a boot volume of a virtual server instance in the console, from the CLI, or with the API. The image is a full copy of the source volume. The image includes the operating system and any user data. The image has the same capacity and contains identical data. The OS information for the custom image comes from the originating boot volume. Optionally, you can specify resource groups and tags for your image. You can use the custom image to create new virtual server instances.
{: shortdesc}

The custom image that you create is regional, based on the location of the original boot volume. All custom images are private to the account in which they are created and labeled as _custom_.

The image also inherits the [encryption type](#ifv-encryption). If the original volume used provider-managed encryption, you can keep that or you can specify customer-managed encryption and provide your own root keys. If the original volume used customer-managed encryption, the image inherits the root key. However, you can change that root key if you want to use another key. You can't switch from customer-managed encryption to provider-managed encryption.

Custom images that are created from a volume are independent from the original volume. You can update the image and delete it as needed. You can delete the original volume and the custom image persists.

## IAM permissions for creating an image from volume
{: #ifv-iam}

With {{site.data.keyword.iamshort}} (IAM), you can securely authenticate users for platform services and control access to resources consistently across {{site.data.keyword.cloud}}. The following table shows the roles that are required to create an image from a volume.

| Operation                          | Action                      |  IAM access roles     |   API method      |
|------------------------------------|-----------------------------|-----------------------|------------------|
| Create an instance                 | is.instance.instance.create | Administrator, Editor| `POST /instances` |
| List all instances                 | is.instance.instance.list   | Administrator, Editor, Operator, Viewer |`GET /instances`|
| Create an image from a boot volume | is.image.image.create       | Administrator, Editor| `POST /images` |
| List all images                    | is.image.image.list         | Administrator, Editor, Operator, Viewer | `GET /images` |
| Delete image                       | is.image.image.delete       | Administrator, Editor| `DELETE /images` |
| Schedule deprecation               | is.image.image.deprecate    | Administrator, Editor| `POST /images` with `deprecation_at` value specified|
| Schedule obsolescence              | is.image.image.obsolete     | Administrator, Editor | `POST /images` with `obsolescence_at` value specified|
{: caption="IAM user permissions" caption-side="bottom"}

## Volume requirements
{: #ifv-requirements}

To create an image from a volume, the volume must meet the following requirements:

* The volume must be in the region where you want to create the custom image.
* The volume must be a primary boot volume with 100 GB capacity. Data volumes are not supported.
* The volume must be attached to an instance. Unattached boot volumes are not supported.
* The instance must be in an available state.
* The available, the running instance must be stopped before you create the custom image. Creating an image from a running instance is not allowed.

## Options for creating an image from a volume
{: #vpc-ifv-procedures}

You can create a custom image from a boot volume in several ways.

* From the [console](/docs/vpc?topic=vpc-create-ifv#create-image-from-volume-vpc-ui), you can create a custom image from any of the following pages.
   - The Custom images for VPC page
   - The list of instances on the Virtual server instances for VPC page
   - The Instance details page
   - The list of volumes on the Block Storage volumes for VPC page
   - The Volume details page

* In the [API](/docs/vpc?topic=vpc-create-ifv#image-from-volume-vpc-api), you can create a custom image at the same time as you create an instance, or you can create an image from an existing instance. With the regional API, you create an image by making a `POST /images` request with the boot volume ID.
* From the [CLI](/docs/vpc?topic=vpc-create-ifv#image-from-volume-vpc-cli), you can create a custom image at the same time as you create an instance, or you can create an image from an existing instance. Issue the `ibmcloud is image-create` command and specify the boot volume's ID.

## Custom image encryption
{: #ifv-encryption}

When you create an image from a volume, you have the following encryption choices.

* If the volume uses the default IBM-managed encryption, then the image from that boot volume inherits the IBM-managed encryption. You can keep this encryption, or you can specify your own [root key](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption) when you create the image.

* If the volume uses a customer-managed encryption, then the image inherits the customer root key (CRK) from the volume. You can keep this encryption, or you can specify another CRK.

## Custom image lifecycle
{: #ifv-image-lifecycle}


{{_include-segments/image_lifecycle.md}}

## Next steps
{: #ifv-next-steps}

* Check out the [Creating a custom image from volume and provisioning an instance in the console](/docs/vpc?topic=vpc-creating-and-using-an-image-from-volume) tutorial.
* [Create an image from a volume](/docs/vpc?topic=vpc-create-ifv).
* [Troubleshooting custom images](/docs/vpc?topic=vpc-ifv-troubleshooting-custom-images).
