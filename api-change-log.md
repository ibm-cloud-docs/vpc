---

copyright:
  years: 2019, 2021
lastupdated: "2021-02-24"

keywords: api, change log, new features, restrictions, migration, generation 2, gen2,

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}


# Change log
{: #api-change-log}

Read this change log to learn about updates and improvements to the {{site.data.keyword.vpc_full}} (VPC) [API](/apidocs/vpc). The change log lists changes that are ordered by the date they were released. Changes to existing API versions are designed to be compatible with existing client applications.
{:shortdesc}

By design, new features with backward-incompatible changes apply only to version dates on and after the feature's release. Changes that apply to older versions of the API are designed to maintain compatibility with existing applications and code. If backward-incompatible changes require non-trivial client code changes to use an API version, the API change log might provide links to instructions, tips, or best practices for updating client code. See also [API application migration considerations](/docs/vpc?topic=vpc-api-integration-migration).
{:note}

Some changes, such as new response properties or new optional request parameters, are considered backward compatible. Other changes, such as new required request parameters, are not considered backward compatible. To avoid disruption from changes to the API, use the following best practices when you call the API:

* Catch and log any `4xx` or `5xx` HTTP status code, along with the included `trace` property
* Follow HTTP redirect rules for any `3xx` HTTP status code
* Consume only the resources and properties your application needs to function
* Avoid depending on behavior that is not explicitly documented

## Upcoming changes
{: #upcoming-changes}

### For all API version dates

**Block storage volumes**  In an upcoming release, a new value in the `status` enumeration will be added to the [volume](/apidocs/vpc#list-volume-profiles) APIs. When you expand an existing data volume, the volume goes into an updating state and shows a new API status `updating`. You can still access the data while the volume is being resized. For more information, see [Expanding block storage volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

## 23 February 2021
{: #23-february-2021}

### For all API version dates
{: 23-february-2021-all-version-dates}

**Application load balancer security group integration.** For enhanced security, application load balancers can now be associated with security groups. You can specify one or more security groups when you create the application load balancer, and associate security groups with your existing application load balancers. If you omit security groups during load balancer creation, the default security group for the VPC is used.

If you plan to use default security groups for new application load balancers, review your default security group rules. If necessary, edit the rules to accommodate your expected application load balancer traffic.
{: tip}   
   
Existing load balancer methods were updated as follows:

   * [Create a load balancer](/apidocs/vpc#create-load-balancer) (`POST /load_balancers`) can now accept a list of security groups
   * [Get load balancer details](/apidocs/vpc#get-load-balancer) (`GET /load_balancers/{id}`) now returns references to the security groups to which a load balancer is attached
    
New security group methods were added for managing security group targets:

   * [Attach a security group to a target network interface or load balancer](/apidocs/vpc#create-security-group-target-binding) (`PUT /security_groups/{security_group_id}/targets/{id}`)
   * [List targets attached to a security group](/apidocs/vpc#list-security-group-targets) (`GET /security_groups/{security_group_id}/targets`)
   * [Retrieve a target in a security group](/apidocs/vpc#get-security-group-target) (`GET /security_groups/{security_group_id}/targets/{id}`)
   * [Delete targets from a security group](/apidocs/vpc#delete-security-group-target-binding) (`DELETE /security_groups/{security_group_id}/targets/{id}`)
  
Use the security group target methods to manage security group attachments to both load balancers and network interfaces. The original methods specific to network interfaces are now deprecated:

   * `GET /security_groups/{security_group_id}/network_interfaces`
   * `DELETE /security_groups/{security_group_id}/network_interfaces/{id}`
   * `GET /security_groups/{security_group_id}/network_interfaces/{id}`
   * `PUT /security_groups/{security_group_id}/network_interfaces/{id}`

For more information, see [Integrating an IBM Cloud Application Load Balancer for VPC with security groups](/docs/vpc?topic=vpc-alb-integration-with-security-groups).

**Bring Your Own IP (BYOIP) support for VPC.** VPC address prefixes are no longer restricted to [RFC-1918](https://tools.ietf.org/html/rfc1918) addresses. You must now configure VPCs that use both non-RFC-1918 addresses and have public connectivity (floating IPs or public gateways) using a custom route that contains the new `delegate_vpc` property. You must specify this property for destination CIDRs that are non-RFC-1918 compliant and outside of the VPC, such as for destinations that are reachable through {{site.data.keyword.dl_full}}, {{site.data.keyword.cloud_notm}} Transit Gateway, or VPC classic access.

The `delegate_vpc` property is not required if a VPC uses only RFC-1918 addresses or has no public connectivity.
{: note}

The following API methods have been updated:

* View the `delegate_vpc` property in the requests and responses for `/vpcs/{vpc_id}/routing_tables/{routing_table_id}/routes`.
* View reserved IP ranges in `POST /vpcs/{vpc_id}/address_prefixes`, which creates an address pool prefix. 

## 27 January 2021
{: #27-january-2021}

### For all API version dates
{: #27-january-2021-all-version-dates}

**Checksum (SHA256) for imported images.** When you import a custom image to {{site.data.keyword.vpc_short}}, you can now view the checksum that was generated for the image during the import operation. By generating a checksum for the image locally, and checking that the checksums match, you can verify the integrity of the imported image. For more information, see [Validating a custom image after importing](/docs/vpc?topic=vpc-managing-images#validate-custom). 

The `sha256` checksum is available in the `file` details in API method `GET /images/{id}`. See [Retrieve an image](/apidocs/vpc#get-image).

## 19 January 2021
{: #19-january-2021}

### For all API version dates
{: #19-january-2021-all-version-dates}

The quantity of memory for virtual server instance profiles is now provisioned in gibibytes (GiB), instead of gigabytes (GB). For example, creating a new `bx2-4x16` virtual server instance provisions the instance with 16 GiB (17,179,869,184 bytes), instead of 16 GB (16,000,000,000 bytes). Virtual server instances that have already been provisioned are not affected.

### For version `2021-01-19` or later
{: #version-2021-01-19}

Memory for virtual server instances is now expressed in gibibytes (GiB), instead of gigabytes (GB). For example, the `memory` property returned from `GET /instances/{id}` now reports in GiB (truncated to a whole number).


## 16 December 2020
{: 16-december-2020}

### For all API version dates
{: 16-december-2020-all-version-dates}

**Customer-managed encryption for block storage volumes and encrypted custom images.** When you disable or delete a customer root key (CRK) that is encrypting your block storage or custom image resources, the API displays a status of `unusable` for these resources, along with the reason codes `encryption_key_deleted` or `encryption_key_disabled`. 

The `unusable` status appears in the following API methods:

* [List all volumes](/apidocs/vpc#list-volumes) (`GET /volumes`)
* [Retrieve a specific volume](/apidocs/vpc#get-volume) (`GET /volume{id}`)
* [List all images](/apidocs/vpc#list-volumes) (`GET /images`)
* [Retrieve the specified image](/apidocs/vpc#get-image) (`GET /images{id}`)

For more information on key states and resource statuses, see [User actions that impact root key states and resource status](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-root-key-states).

**Dedicated hosts** are now supported in the VPC API. Learn more about using [dedicated hosts](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances) and explore the new [API operations](/apidocs/vpc#list-dedicated-host-groups).

## 20 November 2020
{: 20-november-2020}

### For all API version dates
{: 20-november-2020-all-version-dates}

**Datapath log forwarding with LogDNA** is now available for [IBM Cloud Application Load Balancer for VPC](/docs/vpc?topic=vpc-load-balancers#load-balancers). Data and health check logs are valuable for debugging, analysis, and maintenance purposes. With the datapath logging feature enabled, your load balancer forwards these logs to your account's [IBM Log Analysis with LogDNA](https://cloud.ibm.com/observe/logging){:external} dashboard. 

View the `logging` property in the following API methods:

* [List all load balancers](/apidocs/vpc#list-load-balancers) (`GET /load_balancers`)
* [Retrieve a load balancer](/apidocs/vpc#get-load-balancer) (`GET /load_balancers/{id}`)

For more information, see [Datapath log forwarding with LogDNA](/docs/vpc?topic=vpc-datapath-logging#datapath-logging).

## 19 November 2020
{: 19-november-2020}

### For all API version dates
{: 19-november-2020-all-version-dates}

**Support for ingress routing** is included as part of [routing tables](/apidocs/vpc#list-vpc-routing-tables), which were released on 30 October 2020. Use [ingress routing](/apidocs/vpc#create-vpc-routing-table) to control the policy for packets that are coming in to your VPC or one of its zones. The policy can vary, depending on the type of source and the destination IP address range.

Routing tables for the VPC API are the same for both egress and ingress routing, with the following additional properties that you can specify for ingress routing:

* `route_direct_link_ingress`
* `route_transit_gateway_ingress`
* `route_transit_gateway_ingress`

For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

## 13 November 2020
{: #13-november-2020}

### For version `2020-11-13` or later
{: #version-2020-11-13}

**Static-route-based VPN gateways** are now available. For a [static-route-based VPN gateway](/apidocs/vpc#create-vpn-gateways), virtual tunnel interfaces are created. Any traffic that is routed to these interfaces with [user-defined routes](/docs/vpc?topic=vpc-create-vpc-route) is encrypted. For more information, see [About VPN gateways](/docs/vpc?topic=vpc-using-vpn).

## 30 October 2020
{: #30-october-2020}

### For all API version dates
{: #30-october-2020-all-version-dates}

* **Custom routing tables** are now supported in the VPC API. This feature controls where network traffic is directed on a per-subnet basis. Explore new API operations for [routing tables](/apidocs/vpc#list-all-routing-tables-for-a-vpc) and [routes](/apidocs/vpc#create-a-vpc-route). This feature subsumes the [VPC routing API](/apidocs/vpc#list-all-routes-in-the-vpc-s-default-routing-table), which remains supported but is deprecated and might be removed in a future API release.

* **Virtual private endpoint gateways.** Use [virtual private endpoint gateways](/apidocs/vpc#list-endpoint-gateways) to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choice, which is allocated from a subnet within your VPC. For more information, see [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).

* **VPC network interfaces.** IP anti-spoofing checks had already been provided for enhanced security. However, certain use cases, such as having a virtual server instance act as a network gateway, require selective disabling of these checks. To accommodate these use cases, if you have the `is.instance.instance.ip-spoofing` IAM action, you can now enable the `allow_ip_spoofing` property when you [create a network interface](/apidocs/vpc#create-instance-network-interface). Alternatively, toggle the property when you [update an existing network interface](/apidocs/vpc#update-instance-network-interface). See also [About IP spoofing checks](/docs/vpc?topic=vpc-ip-spoofing-about).

* **Proxy protocol for application load balancers for VPC.** When you configure a load balancer [pool](/apidocs/vpc#create-load-balancer-pool) to use proxy protocol, the pool will pass information about the client to a back-end pool member when a connection is opened.

    You can also configure a load balancer [listener](/apidocs/vpc#create-load-balancer-listener) to accept proxy protocol information. This feature is useful when the client is, itself, a proxy (which, in turn, was connected to by the actual client) that supports the proxy protocol. This allows client information to be obtained and passed on to any pools that, themselves, have the proxy protocol enabled.

    For more information, see [Enabling proxy protocol](/docs/vpc?topic=vpc-advanced-traffic-management#proxy-protocol-enablement).


## 5 October 2020
{: #2020-10-05}

### For all API version dates
{: #2020-10-05-all-version-dates}

**Encrypted images.** Use the VPC API to create your own image, encrypt it with your own key, and import it, encrypted, into {{site.data.keyword.cloud_notm}}. After you import the image, use it like any other image. If you use the image to provision an instance, its boot volume is encrypted using the image's root encryption key or another root encryption key of your choosing.

Dive into the APIs to [import an encrypted image](/apidocs/vpc#create-image) and [provision an instance](/apidocs/vpc#create-instance) from that encrypted image. See also [Creating an encrypted custom image](/docs/vpc?topic=vpc-create-encrypted-custom-image).

## 31 August 2020
{: #2020-08-31}

### For all API version dates
{: #2020-08-31-all-version-dates}

**Network load balancers.** You can now use the [load balancers API](/apidocs/vpc#list-load-balancer-profiles) to distribute traffic among multiple server instances within the same region of your VPC. To learn how to create and manage a network load balancer, see [About IBM Cloud Network Load Balancer for VPC](/docs/vpc?topic=vpc-network-load-balancers).

The network load balancers API is shared between {{site.data.keyword.cloud_notm}} application load balancers and network load balancers.
{:note}

## 25 August 2020
{: #2020-08-25}

### For all API version dates
{: #2020-08-25-all-version-dates}

This API release supports the following changes:

* Create an instance group of a fixed size and use it as a stand-alone feature
* Use the instance group manager to manage the instance group, which can associate an auto scale policy with that group

For more information, see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

The following new operations are available for [instance groups](/apidocs/vpc#list-instance-groups):

* `GET` and `POST` for `/instance_groups`
* `DELETE`, `GET`, and `PATCH` for `/instance_groups/{id}`
* `DELETE` for `/instance_groups/{instance_group_id}/load_balancer`
* `GET` and `POST` for `/instance_groups/{instance_group_id}/managers`
* `DELETE`, `GET`, and `PATCH` for `/instance_groups/{instance_group_id}/managers/{id}`
* `GET` and `POST` for `/instance_groups/{instance_group_id}/managers/{instance_group_manager_id}/policies`
* `DELETE`, `GET`, and `PATCH` for `/instance_groups/{instance_group_id}/managers/{instance_group_manager_id}/policies/{id}`
* `GET` for `/instance_groups/{instance_group_id}/memberships`
* `DELETE`, `GET`, and `PATCH` for `instance_groups/{instance_group_id}/memberships/{id}`

You can also use the new [instance template](/apidocs/vpc#list-instance-templates) feature independently of auto scale. For example, create a template, and then create instances from that template, without creating an instance group.

The following new endpoints are now available for instances:
* `GET` and `POST` for `/instance/templates`
* `GET`, `PATCH`, and `DELETE` for `/instance/templates/{id}`

`POST /instances` now supports a `source_template`.

## 23 July 2020
{: #2020-07-23}

### For all API version dates
{: #2020-07-23-all-version-dates}

The [flow log collectors API](/apidocs/vpc#list-all-flow-log-collectors) is now generally available.

## 22 July 2020
{: #2020-07-22}

### For all API version dates
{: #2020-07-22-all-version-dates}

This API release supports the following enhancements for customer-managed encryption for block storage boot and data volumes:

* `POST /instances` -- Create an instance and new volume, encrypted, using your customer root key (CRK). CRKs are imported to {{site.data.keyword.cloud_notm}} or created in a key management service. See [Create an instance](/apidocs/vpc#create-an-instance).
* `POST /volumes` -- Create an unattached data volume, encrypted, using your CRK. See [Create a volume](/apidocs/vpc#create-a-volume).

## 12 May 2020
{: #2020-05-12}

### For all API version dates
{: #2020-05-12-all-version-dates}

Configure load balancer pool resources and their health monitors to use the HTTPS protocol. This enhancement enables end-to-end SSL encryption with HTTPS listeners, along with HTTPS health checks for increased availability. See the [load balancers API](/apidocs/vpc#list-all-load-balancers).

## 1 May 2020
{: #2020-05-02}

### For all API version dates
{: #2020-05-02-all-version-dates}

This API release supports the following changes:

* [Flow log collectors](/apidocs/vpc#list-all-flow-log-collectors) API is available as beta
* `GET` /security_groups now supports `vpc.crn` and `vpc.name` filters

## 17 April 2020
{: #2020-04-17}

### For all API version dates
{: #2020-04-17-all-version-dates}

{{site.data.keyword.cloud_notm}} interprets volume capacity units in gibibytes, but the API documentation used gigabytes. This issue is now resolved in the documentation.

## 10 April 2020
{: #2020-04-10}

### For all API version dates
{: #2020-04-10-all-version-dates}

Usage recommendations are provided for the following load balancer properties:

* `protocol` property for load balancer pools
* `type` property for load balancer pool health monitors

The guidance notes that new values for these properties might be added in the future, and unexpected values are handled gracefully. See the [load balancers API](/apidocs/vpc#list-all-load-balancers).

## 6 February 2020
{: #2020-02-06}

### For all API version dates
{: #2020-02-06-all-version-dates}

Support is temporarily suspended for creating instances from an existing boot volume. This feature was available through the API only, with no CLI or UI support. In the interim, you must specify the `image` property when you call `POST /instances`.

You can still create instances that reference existing data volumes.
{: note}

## 3 December 2019
{: #2019-12-03}

### For all API version dates
{: #2019-12-03-all-version-dates}

Device IDs are now shown when you retrieve an instance's volume attachments.

## 26 November 2019
{: #2019-11-26}

### For all API version dates
{: #2019-11-26-all-version-dates}

This API release supports the following changes:

* [Network access control list (ACL)](/apidocs/vpc#list-all-network-acls) methods
* Instance filtering by VPC

## 21 November 2019
{: #2019-11-21}

### For all API version dates
{: #2019-11-21-all-version-dates}

A VPC’s cloud service endpoint source IPs now appear in output. Learn about [cloud service endpoint source addresses](/docs/vpc?topic=vpc-vpc-behind-the-curtain#cse-source-addresses) and how [DNS resolves shared cloud service endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc#dns-domain-name-system-resolver-endpoints).

## 5 November 2019
{: #2019-11-05}

### For all API version dates
{: #2019-11-05-all-version-dates}

This API release supports the following changes:

* [Load balancers](/apidocs/vpc#list-all-load-balancers) API is available as beta
* [VPN gateways](/apidocs/vpc#list-all-vpn-gateways) are available as beta
* Pagination is now supported for [instances](/apidocs/vpc#list-all-instances)
* Classic access to VPCs (also known as classic peering) is supported
