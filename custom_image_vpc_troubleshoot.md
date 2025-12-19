---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-19"

keywords: custom image, image from volume, troubleshooting, troubleshoot

subcollection: vpc

content-type: troubleshoot

---

{site.data.keyword.attribute-definition-list}}

# Troubleshooting custom images
{: #ifv-troubleshooting-custom-images}

When you create or manage custom images, you might encounter issues. Issues, symptoms, likely causes, and resolutions are described in the following sections.
{: shortdesc}

## Cannot retrieve a volume in a specified region
{: #troubleshoot-ifv1}
{: troubleshoot}
{: support}

You are trying to create a custom image from a volume that is not 100 GB. When you try to select a volume that is not the right capacity to create your image, the system cannot retrieve it.
{: tsSymptoms}

When you create a customer image from an existing boot volume, the image inherits the encryption from the originating volume. If the boot volume was encrypted by a customer-managed root key, the new customer image is also encrypted with the same key. If the volume had the default, IBM-managed encryption, and you specify a new customer key, the new custom is encrypted by the customer-managed key. The volumes that are to be used as the source for new encrypted images with customer keys must have 100 GB capacity. Custom images can be between 10 - 250 GB but volumes that are intended to be the source for new encrypted images must be 100 GB capacity.
{: tsCauses}

Specify a source volume with 100 GB capacity. Optionally, resize the source volume and try the request again.
{: tsResolve}
