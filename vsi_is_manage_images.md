---

copyright:
  years: 2019, 2022
lastupdated: "2022-08-01"

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
| Copy | Copy the UUID of the custom image. |
| View checksum | View the checksum for the custom image. |
| Delete | Delete the custom image. \n \n {{site.data.content.delete-custom-image-private-catalog}} |
{: caption="Table 1. Custom image actions" caption-side="bottom"}

## Viewing custom image details 
{: #viewing-custom-image-details}

You can view details about a custom image, such as the image name, assigned resource group, image ID, size of the image, location, date that the image was created in VPC, the operating system and version, and type of encryption used for the image. The name of the custom image is editable, and you can add tags for the image. You can also view and copy the unique Cloud Resource Name (CRN) for the image. Finally, you can access the SHA256 checksum value for the image. 

To view details for a custom image, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, 
go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
2. From the list of **Custom images for VPC**, click the name of a custom image to view details about that image. 
3. On the **Image details** page you can edit the name of the image, add tags, and copy the CRN for the image. 
4. From the **Actions** menu of the Image details page, you can create a virtual server instance from the custom image, or delete the image. 
