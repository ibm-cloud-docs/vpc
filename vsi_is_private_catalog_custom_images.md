---

copyright:
  years: 2022, 2026
lastupdated: "2026-01-02"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPC considerations when using custom images in a private catalog
{: #custom-image-cloud-private-catalog}

If you plan to share or publish a custom image to other accounts within your enterprise, you need to create a private catalog. A private catalog provides a way for you to manage access to products for multiple accounts while those accounts are within the same enterprise. You can share any existing x86 virtual server custom image with a private catalog, except for an encrypted image.
{: shortdesc}

On the console, you can find custom images in a private catalog by clicking the **Navigation Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images > Catalog images**.

For more information about private catalogs, see the tutorial [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).

Custom images can also be published to the {{site.data.keyword.cloud}} catalog and to other (nonenterprise) accounts. This process requires onboarding to the [{{site.data.keyword.cloud}} Partner Center](/partner-center/sell).

## Prerequisites and limitations
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

## Using cross-account image references in a private catalog in the console
{: #private-catalog-image-reference-vpc-ui}
{: ui}

Instances you provision belong to your account. Images shared to your account through a private catalog might belong to other accounts. When you provision an instance from an image that belongs to another account or from an instance template that specifies such an image, the image still belongs to the other account even though the instance belongs to your account. A reference to such an image might exist in details of the instance and related resources. For example, details about its boot volume and any snapshot that was created from that boot volume might reference that image. Regardless of your authorizations, you can't access the referenced image by its ID or CRN and attempts fail as if the image does not exist.

Additionally, it is possible that the referenced image might have the same name as a custom image in your account. But these image names are for two different images in two different accounts. The ID and CRN each uniquely identifies an image across {{site.data.keyword.cloud}}. To avoid possible retrieval or use of the wrong image, when you cut-and-paste an image’s identity outside of the UI, use its ID or CRN, rather than its name.

## Using cross-account image references in a private catalog in the CLI
{: #private-catalog-image-reference-vpc-cli}
{: cli}

Instances you provision belong to your account. Images shared to your account through a private catalog might belong to other accounts. When you provision an instance from an image that belongs to another account or from an instance template that specifies such an image, the image still belongs to the other account even though the instance belongs to your account. A reference to such an image might exist in details of the instance and related resources. For example, details about its boot volume and any snapshot that was created from that boot volume might reference that image. Regardless of your authorizations, you can't access this type of custom image by its ID and attempts fail as if the image does not exist.
{: cli}

Image references are used in a few different places in the CLI. For example, if you use the `ibmcloud is instance` command to retrieve instance data, you see the following within the output.

```text
Image                                 ID                                          Name
                                      r006-6cab2dbd-4e57-4dc5-810b-b7366cb78999   test-image
```
{: screen}

If you try to retrieve information about this image by using its ID,

```sh
ibmcloud is image r006-6cab2dbd-4e57-4dc5-810b-b7366cb78999
```
{: pre}

then this attempt fails with an error such as the following example:

```text
FAILED
Response HTTP Status Code: 404
Error code: not_found
Error message: The requested resource does not exist, or cannot be accessed.
Error target name: id, type: parameter
Trace ID: d8b4d382-2993-4a89-a371-1db991b510d8
```
{: screen}

This error means that the image doesn't exist in your account. If the instance was provisioned by using a shared catalog image in another account, this output is the output that you always see even if the image exists in the owning account.

Additionally, it is possible that the referenced image might have the same name as a custom image in your account. But these image names are for two different images in two different accounts. While the CLI uses both ID and Name to identify an image, only the ID uniquely identifies an image across {{site.data.keyword.cloud}}.

Verify that your CLI-based automation handles image reference lookup failures gracefully and do not assume that inaccessible images were deleted, even if it runs with full access to your images. To avoid possible retrieval or use of the wrong image by Name, specify the image ID instead.

## Using cross-account image references in a private catalog in the API
{: #private-catalog-image-reference-vpc-api}
{: api}

Instances that you provision belong to your account. Images that are shared to your account through a private catalog might belong to other accounts. When you provision an instance from an image that belongs to another account or from an instance template that specifies such an image, the image still belongs to the other account even though the instance belongs to your account. A reference to such an image might exist in details of the instance and related resources. For example, details about its boot volume and any snapshot that are created from that boot volume might reference that image. Regardless of your authorizations, you can't access such an image by its `id`, `crn`, or `href` and attempts fail as if the image does not exist.

In the API, an image reference appears in places such as the `image` properties of the `Instance` and `BareMetalServerInitialization` response schemas and in the `source_image` properties of the `Volume` and `Snapshot` response schemas. The following example uses retrieving instance data to illustrate how this error might occur.

```sh
curl -X GET "$vpc_api_endpoint/v1/instances/r006-44905aa4-119d-5622-00ce-face32b78999?version=2022-09-06&generation=2" -H "Authorization: Bearer $iam_token" | jq -r '(.image)'
```
{: pre}

The example uses `jq` as a parser, a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/). `jq` might not come preinstalled on all VPC images available when you create an instance. You might need to install `jq` before use or use another parser of your choice.
{: note}

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

If you try to retrieve information about this image by using its `id`,

```sh
curl -X GET "$vpc_api_endpoint/v1/images/r006-6cab2dbd-4e57-4dc5-810b-b7366cb78999?version=2022-09-06&generation=2" -H "Authorization: Bearer $iam_token"
```
{: pre}

then this attempt fails with HTTP status code 404 (not found). This status code means that the image doesn't exist in your account. If the instance was provisioned by using a shared catalog image in another account, this response is the response that you always get even if the image still exists in the owning account.

Additionally, it is possible that the referenced image might have the same name as a custom image in your account. But these image names are for two different images in two different accounts. The `id`, `crn`, and `href` each uniquely identifies an image across {{site.data.keyword.cloud}}.

Verify that your clients handle image reference lookup failures gracefully and do not assume that inaccessible images were deleted, even when it runs with full access to your images. To avoid possible retrieval or use of the wrong image by `name`, specify the image `id`, `crn`, or `href` instead.

## Using cross-account image references in a private catalog in Terraform
{: #private-catalog-image-reference-vpc-terraform}
{: terraform}

Instances that you provision belong to your account. Images that are shared to your account through a private catalog might belong to other accounts. When you provision an instance from an image that belongs to another account or from an instance template that specifies such an image, the image still belongs to the other account even though the instance belongs to your account. A reference to such an image might exist in details of the instance and related resources. For example, details about its boot volume and any snapshot that was created from that boot volume might reference that image. Regardless of your authorizations, you can't access this type of custom image by its ID and attempts fail as if the image does not exist.

Image references are used in a few different places in the terraform. For example, if you use the [instance data source](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_instance) to retrieve instance data, you see the following within the data source response.

```text
"image": "r006-e0d3b6fb-5421-4421-9061-0ca9f79a4990"
```
{: screen}

If you try to retrieve information about this image by using its [data source](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_image),

```text
data "ibm_is_image" "example" {
  identifier = "r006-e0d3b6fb-5421-4421-9061-0ca9f79a4990"
}
```
{: pre}

then this attempt fails with an error such as the following example:

```text
Error: [ERROR] No image found with id  r006-e0d3b6fb-5421-4421-9061-0ca9f79a4990
```
{: screen}

This error means that the image doesn't exist in your account. If the instance was provisioned by using a shared catalog image in another account, this output is the output that you always see even if the image still exists in the owning account.

Additionally, it is possible that the referenced image might have the same name as a custom image in your account. But these image names are for two different images in two different accounts. While the terraform uses both ID or Name to identify an image, only the ID uniquely identifies an image across {{site.data.keyword.cloud}}. To avoid possible retrieval or use of the wrong image by name, specify the image ID instead. For more information, see [image data source](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_image).

## Deleting a custom image in a private catalog
{: #deleting-private-catalog-custom-image-vpc}

A custom image can't be deleted, reused for a different version, or reused for a different product offering while that custom image is managed from a private catalog.

Deleting the private catalog that contains product offerings directly does not remove the offerings immediately. The custom images that are managed by the private catalog continue to be managed by the catalog for 7 days beyond the date that you deleted the private catalog.

If you want to immediately reuse or delete the custom image, make sure that you first remove that custom image from the associated version in the private catalog product offering or delete the product offering.

To delete a published or shared custom image from the private catalog, see the tutorial [Deprecating a private product](/docs/account?topic=account-deprecate-product&interface=ui). To delete the entire private catalog, see the tutorial [Deleting a private catalog by using the console](/docs/account?topic=account-restrict-by-user&interface=ui#delete-private-catalog-ui).

## Instance groups and private catalog
{: #private-catalog-instance-groups}

You can use a custom image in a private catalog to create an instance group. However, you must first create a service-to-service policy to `globalcatalog-collection.instance.retrieve` before you can create the instance group. For more information, see [Using a custom image in a private catalog with an instance group](/docs/vpc?topic=vpc-private-catalog-image-instance-group&interface=ui).

The information about which custom image that is in a private catalog that was used for provisioning an instance doesn't persist across all {{site.data.keyword.vpc_short}} resources. Information about the custom image in a private catalog isn't available on Snapshot for VPC or Image from Volume. The operating system information, which is required when you provision a virtual server, is available.

## Importing your custom image into a private catalog
{: #custom-image-using-private-catalog}

After your custom image is imported into {{site.data.keyword.vpc_short}}, you can import that custom image into a private catalog.

* [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui)
