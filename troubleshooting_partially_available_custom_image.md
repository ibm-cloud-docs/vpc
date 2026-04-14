---

copyright:
  years: 2025, 2026
lastupdated: "2026-04-14"

keywords: vpc, troubleshoot

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Custom image is in `invalid state` when I provision an instance
{: #custom-image-invalid-state-provision-instance}
{: troubleshoot}
{: support}

Provisioning an instance with a custom image fails because the image is in an invalid state.
{: tsSymptoms}

Even though you can provision an instance with a custom image in `available`, `partially_available`, or `deprecated` status, the `partially_available` status means that the image was imported in at least one zone, but is not yet available in all zones. After the image is imported into all zones, the status changes automatically to `available`.
{: tsCauses}

You can provision an instance with a custom image in `available`, `partially_available`, or `deprecated` status. The `partially_available` status is a temporary status that indicates that an image was imported into at least one zone but is not yet available in all zones. These images eventually reach `available` status without any extra user action. You can use an image that is in `partially_available` status to create virtual server instances or bare metal servers in available zones that are listed in the image zones array. You can also export these images into {{site.data.keyword.cos_full_notm}}. If you can't provision an instance with your `partially_available` custom image, verify what zones that image is available in. Then, try to provision the instance in one of those zones. Alternately, wait for the image to reach `available` status, or choose a different image that is available in the wanted zone, and try to provision the instance again.
{: tsResolve}
