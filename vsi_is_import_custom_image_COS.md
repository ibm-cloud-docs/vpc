---

copyright:
  years: 2022, 2023

lastupdated: "2023-05-10"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Importing your custom image into {{site.data.keyword.vpc_short}}
{: #custom-image-using-COS}

After your custom image is created and stored on {{site.data.keyword.cos_full_notm}}, you must import that custom image directly to {{site.data.keyword.vpc_short}}. This requirement includes any custom images that you plan to import into a private catalog. The custom images must be imported into {{site.data.keyword.vpc_short}} first and then imported into the private catalog.
{: shortdesc}

* Upload your custom image to {{site.data.keyword.cos_full_notm}}.
* On the Objects page of your {{site.data.keyword.cos_full_notm}} bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images that are larger than 200 MB.
* Import your custom image to {{site.data.keyword.vpc_short}}. When a supported custom image is available in {{site.data.keyword.cos_full_notm}}, the image is ready to import. For more information, see [Importing and validating custom images into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc).
