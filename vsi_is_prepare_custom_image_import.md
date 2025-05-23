---

copyright:
  years: 2020, 2025
lastupdated: "2025-05-23"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Preparing a custom image for import
{: #prepare-custom-image-import}

Before you can upload your image to {{site.data.keyword.cos_full_notm}}, you must first create the image. After the image is created and uploaded to {{site.data.keyword.cos_full_notm}}, then you can [Import your custom image into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc).
{: shortdesc}

Use one of the following procedures to create a custom image.

* [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image)
* [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image)
* [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc)
* [Creating a z/OS Wazi aaS custom image](/docs/vpc?topic=vpc-create-zos-custom-image)

Did you know that you can use the {{site.data.keyword.cloud}} [Packer plug-in](https://github.com/IBM/packer-plugin-ibmcloud){: external} to create and manage custom images on {{site.data.keyword.cloud}}? For more information, see [A sample VPC custom image built with Packer](https://github.com/l2fprod/vpc-custom-image){: external}.
{: tip}
