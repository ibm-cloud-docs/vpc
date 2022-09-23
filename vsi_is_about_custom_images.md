---

copyright:
  years: 2022
lastupdated: "2022-09-23"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with custom images
{: #planning-custom-images}

Custom images are used to create new virtual servers with your own settings and configurations. You can create a Linux&reg; custom image, a Windows&reg; custom image, or a VMware custom image. You can import your custom image directly into {{site.data.keyword.vpc_full}} from {{site.data.keyword.cos_full}} or create one from an existing virtual server boot volume. You can also create an image template to migrate a virtual server from the Classic infrastructure. After a custom image is created and imported into {{site.data.keyword.vpc_short}}, you can import it into a private catalog, with some limitations. For more information about these limitations, see [Custom images in a private catalog](/docs/vpc?topic=vpc-planning-custom-images#custom-image-cloud-private-catalog).
{: shortdesc}

## Prerequisites and limitations
{: #custom-image-prerequisites}

Before you create a custom image, you must verify that your custom image meets the custom image requirements and that the operating system is supported.

### Custom image considerations
{: #custom-image-considerations}

{{site.data.content.custom-image-requirements-list}}

### Operating system considerations
{: #custom-image-operating-systems}

* Make sure that your image is supported as an [x86 stock image](/docs/vpc?topic=vpc-about-images) or an [s390x stock image](/docs/vpc?topic=vpc-vsabout-images) on {{site.data.keyword.vpc_full}}.
* If you choose to create a custom image with your own license, select the [BYOL](/docs/vpc?topic=vpc-byol-vpc-about) option.
* ISO images of licensed operating systems, such as Windows&reg; and Linux&reg;, and open source operating systems, such as CentOS and Ubuntu, aren't provided by {{site.data.keyword.cloud}}. If you need these ISO images, you can download them from the respective vendor website.

### {{site.data.keyword.cos_full_notm}} considerations
{: #custom-image-cloud-object-storage}

If you plan to import an image from a file, you must provision an instance of {{site.data.keyword.cos_full_notm}} if you don't already have one. You can then upload the file to a bucket there. You must also create an IAM authorization between the Image Service for VPC and {{site.data.keyword.cos_full_notm}}. For more information, see [Granting access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq&interface=cli).

## z/OS Wazi aaS custom images
{: #custom-image-zos}

You can use IBM Wazi Image Builder to create your own custom z/OS-based {{site.data.keyword.waziaas_full_notm}} (Wazi aaS) image and import the custom image into {{site.data.keyword.vpc_full}}.

IBM Wazi Image Builder is a separately orderable product from [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/){: external}. There are additional requirements to order and use Wazi Image Builder. The image cost is the premium applied to cover the cost of technologies that allows for z/OS dev and test images to run on IBM Z hardware in IBM’s cloud infrastructure as a service layer. 

The z/OS Wazi aaS custom image must meet the following requirements: 
* Is in qcow2 format
* The operating system is z/OS 2.4 or z/OS 2.5
* The zHYPaaS host control program creates a virtual machine (VM) runtime environment for each guest operating system (OS) and can host many guests at the same time. For required program temporary fixes (PTFs) on z/OS and other IBM software products, see [Hardware and software requirements](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=builder-hardware-software-requirements){: external}. 

For more information, see [Bringing your own image with Wazi Image Builder](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=bringing-your-own-image-wazi-image-builder){: external}.

## Custom images in a private catalog
{: #custom-image-cloud-private-catalog}

If you plan to share or publish a custom image to other accounts within your enterprise, you need to create a private catalog. A private catalog provides a way for you to manage access to products for multiple accounts as long as those accounts are within the same enterprise. You can share any existing x86 virtual server custom image with a private catalog, except for an encrypted image.

For more information about private catalogs, see the tutorial [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).

Custom images can also be published to the IBM Cloud catalog and to other (non-enterprise) accounts. This process requires onboarding to the [IBM Cloud Partner Center](https://cloud.ibm.com/partner-center/sell).

### Prerequisites and limitations
{: #private-catalog-custom-image-prerequisites}

Before you can import a custom image into a private catalog, the following items must be completed.

* Create a private catalog
* Create and import the custom image into {{site.data.keyword.vpc_short}}

Custom images in a private catalog have the following restrictions:

* Must be an x86 image
* Can't be encrypted
* Can't be used with a bare metal profile
* Can exist in only one version of one catalog product offering in one private catalog at a time
* Must be in `available` status

### Using cross-account image references in a private catalog in the UI
{: #private-catalog-image-reference-vpc-ui}
{: ui}

Instances you provision belong to your account. Images shared to your account through a private catalog might belong to other accounts. When you provision an instance from an image that belongs to another account or from an instance template specifying such an image, the image still belongs to the other account even though the instance belongs to your account. A reference to such an image might exist in details of the instance and related resources. For example, details about its boot volume and any snapshot created from that boot volume might reference that image. Regardless of your authorizations, you will be unable to access the referenced image by its ID or CRN and attempts will fail as if the image does not exist.

Additionally, it is possible that the referenced image might have the same name as a custom image in your account. But these image names are for two different images in two different accounts. The ID and CRN each uniquely identifies an image across IBM Cloud. To avoid possible retrieval or use of the wrong image, when using cut-and-paste to copy an image’s identity outside of the UI, use its ID or CRN, rather than its name.

### Using cross-account image references in a private catalog in the CLI
{: #private-catalog-image-reference-vpc-cli}
{: cli}

Instances you provision belong to your account. Images shared to your account through a private catalog might belong to other accounts. When you provision an instance from an image that belongs to another account or from an instance template specifying such an image, the image still belongs to the other account even though the instance belongs to your account. A reference to such an image might exist in details of the instance and related resources. For example, details about its boot volume and any snapshot created from that boot volume might reference that image. Regardless of your authorizations, you can't access this type of custom image by its ID and attempts fail as if the image does not exist.
{: cli}

Image references are used in a few different places in the CLI. For example, if you use the `ibmcloud is instance` command to retrieve instance data, you see the following within the output.

```text
Image                                 ID                                          Name
                                      r006-6cab2dbd-4e57-4dc5-810b-b7366cb78999   test-image
```
{: screen}

If you try to retrieve information about this image using its ID,

```sh
ibmcloud is image r006-6cab2dbd-4e57-4dc5-810b-b7366cb78999
```
{: pre}

then this attempt will fail with an error such as:

```text
FAILED
Response HTTP Status Code: 404
Error code: not_found
Error message: The requested resource does not exist, or cannot be accessed.
Error target name: id, type: parameter
Trace ID: d8b4d382-2993-4a89-a371-1db991b510d8
```
{: screen}

This error means that the image doesn't exist in your account. If the instance was provisioned by using a shared catalog image in another account, this output is the output that you will always see even if the image still exists in the owning account.

Additionally, it is possible that the referenced image might have the same name as a custom image in your account. But these image names are for two different images in two different accounts. While the CLI uses both ID and Name to identify an image, only the ID uniquely identifies an image across IBM Cloud.

Verify that your CLI-based automation handles image reference lookup failures gracefully and do not assume that inaccessible images were deleted, even when running with full access to your images. To avoid possible retrieval or use of the wrong image by Name, specify the image ID instead.

### Using cross-account image references in a private catalog in the API
{: #private-catalog-image-reference-vpc-api}
{: api}

Instances you provision belong to your account. Images shared to your account through a private catalog might belong to other accounts. When you provision an instance from an image that belongs to another account or from an instance template specifying such an image, the image still belongs to the other account even though the instance belongs to your account. A reference to such an image might exist in details of the instance and related resources. For example, details about its boot volume and any snapshot created from that boot volume might reference that image. Regardless of your authorizations, you can't access such an image by its `id`, `crn`, or `href` and attempts fail as if the image does not exist.

In the API, an image reference appears in places such as the `image` properties of the `Instance` and `BareMetalServerInitialization` response schemas and in the `source_image` properties of the `Volume` and `Snapshot` response schemas. The following example uses retrieving instance data to illustrate how this error can occur.

```sh
curl -X GET "$vpc_api_endpoint/v1/instances/r006-44905aa4-119d-5622-00ce-face32b78999?version=2022-09-06&generation=2" -H "Authorization: Bearer $iam_token" | jq -r '(.image)'
```
{: pre}

When you run this command, you see the following output.

```text
{
  "crn": "crn:v1:bluemix:public:is:us-south:a/123456::image:r006-6cab2dbd-4e57-4dc5-810b-b7366cb78999",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/images/r006-6cab2dbd-4e57-4dc5-810b-b7366cb78999",
  "id": "r006-6cab2dbd-4e57-4dc5-810b-b7366cb78999",
  "name": "test-image"
}
```
{: screen}

If you try to retrieve information about this image using its `id`

```sh
curl -X GET "$vpc_api_endpoint/v1/images/r006-6cab2dbd-4e57-4dc5-810b-b7366cb78999?version=2022-09-06&generation=2" -H "Authorization: Bearer $iam_token"
```
{: pre}

then this attempt will fail with HTTP status code 404 (not found). This status code means that the image doesn't exist in your account. If the instance was provisioned by using a shared catalog image in another account, this response is the response that you will always get even if the image still exists in the owning account.

Additionally, it is possible that the referenced image might have the same name as a custom image in your account. But these image names are for two different images in two different accounts. The `id`, `crn`, and `href` each uniquely identifies an image across IBM Cloud. 

Verify that your clients handle image reference lookup failures gracefully and do not assume that inaccessible images were deleted, even when running with full access to your images. To avoid possible retrieval or use of the wrong image by `name`, specify the image `id`, `crn`, or `href` instead.

### Deleting a custom image in a private catalog
{: #deleting-private-catalog-custom-image-vpc}

A custom image can't be deleted, reused for a different version, or reused for a different product offering while that custom image is managed from a private catalog.

Deleting the private catalog that contains product offerings directly does not remove the offerings immediately. The custom images that are managed by the private catalog continue to be managed by the catalog for 7 days beyond the date you deleted the private catalog.

If you want to immediately reuse or delete the custom image, ensure that you first remove that custom image from the associated version in the private catalog product offering or delete the product offering completely.

To delete a published or shared custom image from the private catalog, see the tutorial [Deprecating a private product](/docs/account?topic=account-deprecate-product&interface=ui). To delete the entire private catalog, see the tutorial [Deleting a private catalog by using the console](/docs/account?topic=account-restrict-by-user&interface=ui#delete-private-catalog-ui).

### Instance groups and private catalog
{: #private-catalog-instance-groups}

You can use a custom image in a private catalog to create an instance group. However, you must first create a service-to-service policy to `globalcatalog-collection.instance.retrieve` before you can create the instance group. See [Using a custom image in a private catalog with an instance group](/docs/vpc?topic=vpc-private-catalog-image-instance-group&interface=ui) for more information.

The information about which custom image that is in a private catalog that was used for provisioning an instance doesn't persist across all {{site.data.keyword.vpc_short}} resources. Information about the custom image in a private catalog isn't available on Snapshot for VPC or Image from Volume. The operating system information, which is required when you provision a virtual server, is available.


## Create a custom image
{: #custom-image-creation}

Use one of the following procedures to create the custom image.

* [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image)
* [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image)
* [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc)
* [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv&interface=ui)
* [Creating a z/OS Wazi aaS custom image](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=builder-creating-zos-images){: external}

Did you know that you can use the IBM Cloud [Packer Plug-in](https://github.com/IBM/packer-plugin-ibmcloud){: external} to create and manage custom images on IBM Cloud? For more information, see this [blog post](https://www.ibm.com/cloud/blog/build-hardened-and-pre-configured-vpc-custom-images-with-packer){: external} for an example of how to use the IBM Cloud plugin for Packer.
{: tip}

## Import your custom image into {{site.data.keyword.vpc_short}}
{: #custom-image-using-COS}

After your custom image is created and stored on {{site.data.keyword.cos_full_notm}}, you must import that custom image directly to {{site.data.keyword.vpc_short}}. This requirement includes any custom images that you plan to import into a private catalog. The custom images must be imported into {{site.data.keyword.vpc_short}} first and then imported into the private catalog.

* Upload your custom image to {{site.data.keyword.cos_full_notm}}.
* On the Objects page of your {{site.data.keyword.cos_full_notm}} bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images that are larger than 200 MB.
* Import your custom image to {{site.data.keyword.vpc_short}}. When a supported custom image is available in {{site.data.keyword.cos_full_notm}}, the image is ready to import. For more information, see [Importing and validating custom images into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc).

### Import your custom image into a private catalog
{: #custom-image-using-private-catalog}

After your custom image is imported into {{site.data.keyword.vpc_short}}, you can then import that custom image into a private catalog.

* [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui)

## Using a custom image to create a virtual server instance
{: #custom-image-create-vsi}

When you [create a virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers) using the UI, you select the custom image in the `Image` tab of the `Operating system` section of the Create virtual server for VPC page. To select from the list of {{site.data.keyword.vpc_short}} custom images, select `Custom image`. To select from the list of private catalog images, select `Catalog image` instead.

To use a private catalog custom image to create an instance using the API or CLI, see [Creating VPC resources with CLI and API](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=api) for examples.


## Additional information about custom images
{: #custom-image-additional-information}

* [Using granular RBAC permissions for custom images](/docs/vpc?topic=vpc-using-granular-RBAC-permissions-for-custom-images)
* [Configuration requirements for custom Linux kernel](/docs/vpc?topic=vpc-configuration-requirements-for-custom-linux-kernels)
* [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc&interface=ui)
* [Importing and validating custom images into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc).
* [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=ui)
* [Managing image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc-manage&interface=ui)
