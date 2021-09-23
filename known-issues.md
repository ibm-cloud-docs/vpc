---

copyright:
  years: 2018, 2021
lastupdated: "2021-09-23"

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

## Route mode network load balancers support specific port settings only
{: #vnf-nlb-ports}

- **Symptom**: Network load balancers with route mode enabled will fail with port settings outside the supported range. 
- **Fix**: Configure your listener with `port_min=1`, `port_max=65535`, and `port` omitted. Other `port_min` or `port_max` values are not supported.

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
