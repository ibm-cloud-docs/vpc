---

copyright:
  years: 2019, 2022
lastupdated: "2022-09-22"

keywords: 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing custom images 
{: #managing-custom-images}

After you import custom images, you can deploy and manage them from the **Custom images** page. You can also access details for a custom image.
{: shortdesc}

To manage an image from volume, see [Managing image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc-manage&interface=ui). To use a custom image in a private catalog, see [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).
{: note}

You can manage an image by using the {{site.data.keyword.cloud_notm}} console.

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, 
go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
2. From your list of custom images, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for a specific image and select from the available options. Encrypted custom images are identified by a lock icon after the image name. You can select from the following actions:

| Action | Description |
|--------|-------------|
| Rename | Rename the custom image. |
| Create virtual server instance | Create a new virtual server from the custom image. |
| Share to catalog | Share the {{site.data.keyword.vpc_short}} custom image to an existing private catalog. |
| Copy | Copy the UUID of the custom image. |
| View checksum | View the checksum for the custom image. |
| Delete | Delete the custom image.  \n  \n  If you want to delete a custom image in a private catalog, see [Deleting a custom image in a private catalog](/docs/vpc?topic=vpc-planning-custom-images#deleting-private-catalog-custom-image-vpc). |
{: caption="Table 1. Custom image actions" caption-side="bottom"}

## Viewing custom image details 
{: #viewing-custom-image-details}

You can view the following details of a custom image:

* Name and ID
* Assigned resource group 
* Size 
* Location 
* Creation date 
* Operating system and version 
* Encryption type

You can edit the name of the custom image, and adds tags for searchability, and view and copy the unique Cloud Resource Name (CRN). The SHA256 checksum value for the image is accessible.
{: tip} 

To view details for a custom image, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, 
go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
2. From the list of **Custom images for VPC**, click the name of a custom image to view details about that image. 
3. On the **Image details** page you can edit the name of the image, add tags, and copy the CRN for the image. 
4. From the **Actions** menu of the Image details page, you can create a virtual server instance from the custom image, or delete the image. 

## Sharing a custom image to a private catalog
{: #sharing-custom-image-private-catalog-ui}

You can share an existing {{site.data.keyword.vpc_short}} custom image to an existing private catalog. If you don't already have a private catalog, see [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).

The **Share image to private catalog** action is not available for the following images:
* The image is already shared with a private catalog. An image in a private catalog can exist in only one version within one product offering in one private catalog. You must remove the image from the version it is in first before that image can be shared in a different version.
* The image is encrypted.
* The image is not an x86 image.

To share a custom image to a private catalog, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, 
go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
1. From your list of custom images, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) and select **Share to catalog**.
1. On the **Share image to private catalog** page, select a **Private catalog** from the list of available private catalogs.
1. Enter a version number in **Software version**.
1. Click **Create catalog offering** to share the image to the private catalog.
1. After the image is created, click **Validate image**.

This process shares only the custom image to the private catalog. The validation process occurs within the private catalog. Clicking **Validate image** takes you to a new page and from this screen, you can start with [Step 3: Validate the virtual server image in Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui&locale=en#catalog-vsivpc-review-images) to complete the validation process.
{: note}
