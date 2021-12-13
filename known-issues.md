---

copyright:
  years: 2018, 2021
lastupdated: "2021-09-28"

keywords: vpc, known issues, bugs, defects

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Known issues
{: #known-issues}

Known issues might change over time, so check back occasionally.
{: shortdesc}

## Network load balancers don't support selectable port ranges
{: #nlb-port-range}

**Symptom**:  The `port_min` and `port_max` properties are supported only when route mode is enabled, and only when the entire port range is specified (`port_min` of `1` and `port_max` of `65535`).

**Workaround**: Support is planned for a future release.  In the meantime, you can create separate single-port load balancer listeners for each port in the desired range.

## Virtual server instances must be stopped before they can be deleted
{: #API-1144}

- **Symptom:** The virtual server instances cannot be deleted.
- **Workaround:** Stop the instance before you attempt to delete it.

## Checksum not available for some public images
{: #RIOS-1410}

- **Symptom:** When you use the API or CLI to list images, some public stock images might not include a checksum.
- **Fix:** The checksum is for informational purposes only for stock images. No fix is available at this time.

## Boot volume has larger minimum provisioned size when creating a custom image using IFV
{: #boot-volume-larger-minimum-provisioned-size}

**Symptom**: If your custom image is not encrypted and the image is under 100GB virtual disk size, deploying that image to an instance and creating a custom image from that instance's boot volume (Image from a volume feature) results in a `minimum_provisioned_size` of 100GB.

**Fix**: No fix is available at this time.

## No IAM access tag support for instance template and dedicated host group resources
{: #no-iam-access-tag-support-for-instance-template-dedicated-host-group}

**Symptom**: It is not possible to attach Access tags to these resources as
they are currently not registered with GhoST

**Fix**: GhoST resource registration is currently in progress for these
resources to provide customers access tag support to all VPC resources
