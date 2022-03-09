---

copyright:
  years: 2018, 2022
lastupdated: "2022-03-07"

keywords: vpc, known issues, bugs, defects

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues
{: #known-issues}

Known issues might change over time, so check back occasionally.
{: shortdesc}

## Instance metadata service known issues
{: #instance-metadata-known-issues}

The following issues apply to the VPC API or Instance Metadata API. These issues will be resolved in a future release.

- **Issue:** When using the [VPC API](/apidocs/vpc) to manage instances, if the metadata service is disabled at the time an instance is created, and is subsequently enabled while the instance is running, the metadata service will appear to be enabled but will not be fully functional for the running instance.

- **Workaround:** After enabling the metadata service for the first time, use the VPC API to stop the instance. After the instance is stopped, start the instance. This workaround is necessary once only. The metadata service will be enabled and disabled, as expected, after you've taken this action.

- **Issue:** When using instance templates in the VPC API, default trusted profile information is not included in response. As a result, default trusted profile information does not appear in instance template information as expected. This issue occurs when you use the following methods:

   - [Create an instance template](/apidocs/vpc#create-instance-template) and specify a default trusted profile
   - [Retrieve an instance template](/apidocs/vpc#get-instance-template) to view its contents
   - [Update an instance template](/apidocs/vpc#update-instance-template) to change the name of a template that has 'default_trusted_profile' information stored in its prototype.

- **Workaround:** Do not use instance templates to create instances with a default trusted profile.

- **Issue:** When using the Instance Metadata API to retrieve instance information for a calling instance that is not on a dedicated host, an invalid `dedicated_host` `{ }` value is returned.

- **Workaround:** Do not parse this response value when using automation.

- **Issue:** When using the Instance Metadata API to retrieve initialization information for the calling instance, the `resource_type` sub-property for the `default_trusted_profile` property is not returned.

## Instance metadata service Activity Tracker events issues
{: #instance-metadata-activity-tracker-event-known-issues}

**Issue:** Activity Tracker events for the metadata service are undergoing changes and might not match the [events as documented](https://test.cloud.ibm.com/docs/vpc?topic=vpc-at-events#events-metadata). The documented events are still useful for audit purposes but should not be used for automation.

## Network load balancers don't support selectable port ranges
{: #nlb-port-range}

- **Issue**:  The `port_min` and `port_max` properties are supported only when route mode is enabled, and only when the entire port range is specified (`port_min` of `1` and `port_max` of `65535`). 
- **Workaround**: Support is planned for a future release.  In the meantime, you can create separate single-port load balancer listeners for each port in the desired range.

## Virtual server instances must be stopped before they can be deleted
{: #API-1144}

- **Issue:** The virtual server instances cannot be deleted.
- **Workaround:** Stop the instance before you attempt to delete it.

## Checksum not available for some public images
{: #RIOS-1410}

- **Issue:** When you use the API or CLI to list images, some public stock images might not include a checksum. 
- **Fix:** The checksum is for informational purposes only for stock images. No fix is available at this time. 

## Boot volume has larger minimum provisioned size when creating a custom image using IFV 
{: #boot-volume-larger-minimum-provisioned-size}

- **Issue**: If your custom image is not encrypted and the image is under 100GB virtual disk size, deploying that image to an instance and creating a custom image from that instance's boot volume (Image from a volume feature) results in a `minimum_provisioned_size` of 100GB.
- **Fix**: No fix is available at this time.

## Bare metal servers limitations
{: #bare-metal-servers-limitations}

- **Issue:** Flow log collectors are not integrated with bare metal servers. As a result, if you create a flow log collector for a VPC, traffic flows to and from bare metal servers in that VPC will not be logged.
- **Issue:** Network load balancers are not integrated with bare metal servers. As a result, if you create a network load balancer, you won't be able to target a bare metal server as a load balancer pool member target.

Because all bare metal profiles are VMware&reg; certified, the `supported_image_flags` image property and `required_image_flags` profile property that expressed this ability during the beta period have been discontinued. These properties might still be visible to API and CLI consumers, but they aren't supported and must not be used. These properties will be removed entirely in a future release.
{: note}
