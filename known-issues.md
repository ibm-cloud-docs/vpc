---

copyright:
  years: 2018, 2022
lastupdated: "2022-02-01"

keywords: vpc, known issues, bugs, defects

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

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

## Bare metal servers limitations
{: #bare-metal-servers-limitations}

- **Issue:** Flow log collectors are not integrated with bare metal servers. As a result, if you create a flow log collector for a VPC, traffic flows to and from bare metal servers in that VPC will not be logged.

- **Issue:** Network load balancers are not integrated with bare metal servers. As a result, if you create a network load balancer, you won't be able to target a bare metal server as a load balancer pool member target.

Because all bare metal profiles are VMware&reg; certified, the `supported_image_flags` image property and `required_image_flags` profile property that expressed this ability during the beta period have been discontinued. These properties might still be visible to API and CLI consumers, but they aren't supported and must not be used. These properties will be removed entirely in a future release.
{: note}
