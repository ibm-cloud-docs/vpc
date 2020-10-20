---

copyright:
  years: 2019, 2020
lastupdated: "2020-10-20"

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


# API change log
{: #api-change-log}

Read about {{site.data.keyword.vpc_full}} (VPC) [API](https://{DomainName}/apidocs/vpc) new features, improvements, and fixes. Also, find guidance on client code updates that might be required to use a new date-based version of the API.

By design, new features with backward-incompatible changes apply only to version dates on and after the feature's release. Changes that apply to older versions of the API are designed to maintain compatibility with existing applications and code. If backward-incompatible changes require non-trivial client code changes to use an API version, the API change log might provide links to instructions, tips, or best practices for updating client code. See also [API application migration considerations](/docs/vpc?topic=vpc-api-integration-migration).
{:note}

The following changes are considered backward compatible:

* New or changed resources
* New or changed fields

To minimize bugs caused by changes, use the following best practices when you call the API:

* Catch and log any `4xx` or `5xx` HTTP status code, along with the included `trace` property
* Follow HTTP redirect rules for any `3xx` HTTP status code
* Consume only the resources and properties your application needs to function
* Avoid depending on behavior that is not explicitly documented

<!--
## 2020-10-xx
{: #2020-10-xx}
The [Node SDK](https://{DomainName}/apidocs/vpc?code=node) is now generally available.
-->

<!--
## 2020-10-30
{: #2020-10-30}
Dedicated hosts are now supported in the VPC API. Learn more about using [dedicated hosts](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances) and explore [the new API operations](/apidocs/vpc#<TBD>).
-->

<!--
## 2020-10-30
{: #2020-10-30}
Custom routing tables are now supported in the VPC API. This feature lets you control where network traffic is directed on a per-subnet basis. Explore new API operations for [routing tables](https://{DomainName}/apidocs/vpc#list-all-routing-tables-for-a-vpc) and [routes](https://{DomainName}/apidocs/vpc#create-a-vpc-route). This feature subsumes the [VPC routing API](https://{DomainName}/apidocs/vpc#list-all-routes-in-the-vpc-s-default-routing-table), which remains supported but is deprecated and might be removed in a future API release.
-->

<!--
## 2020-10-23
{: #2020-10-23}
The [Virtual private endpoint](https://{DomainName}/apidocs/vpc#list-endpoint-gateways) (VPE) API is now generally available. Use VPE to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choice, allocated from a subnet within your VPC. For details, see [About Virtual Private Endpoints](/docs/vpc?topic=vpc-about-vpe).
-->

<!--
## 2020-10-16
{: #2020-10-16}
For enhanced security, application load balancers will soon be associated with your security groups. You can specify one or more security groups when you create the application load balancer, and associate security groups with your existing application load balancers. If you omit security groups during load balancer creation, the default security group for your VPC is used.
To prepare for this transition, we recommend that you update your default security group rules to minimize disruption in load balancer traffic on newly created application load balancers. 
{: note}
-->

<!--
The following methods return the new “security_groups” property:
* `GET` /load_balancers
* `GET` /load_balancers/{lb_id}
* `POST` /load_balancers
The following APIs are new in this release:
* `GET` /security_groups/{security_group_id}/targets
* `DELETE` /security_groups/{security_group_id}/targets/{id}
* `GET` /security_groups/{security_group_id}/targets/{id}
* `PUT` /security_groups/{security_group_id}/targets/{id}
For more information, see {need link to core doc topic}
-->

## 2020-10-05
{: #2020-10-05}

Encrypted images are now supported in the VPC API. Ceate your own image, encrypt it with your own key, and import it, encrypted, into {{site.data.keyword.cloud_notm}}. After you've imported the image, use it like any other image. If you use the image to provision an instance, its boot volume is encrypted using the image's root encryption key, or another root encryption key of your choosing.

Dive into the APIs to [import an encrypted image](https://{DomainName}/apidocs/vpc#create-image) and [provision an instance](https://DomainName}/apidocs/vpc#create-instance) from that encrypted image. See also [Creating an encrypted custom image](/docs/vpc?topic=vpc-create-encrypted-custom-image).

## 2020-09-24
{: #2020-09-24}

The [Java SDK](https://{DomainName}/apidocs/vpc?code=java) is now generally available.

## 2020-08-31
{: #2020-08-31}

The Sydney region endpoint (`au-syd`) is now in service at `https://au-syd.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

You can now use the [Load balancers API](https://{DomainName}/apidocs/vpc#list-load-balancer-profiles) to distribute traffic among multiple server instances within the same region of your VPC. To learn how to create and manage a network load balancer see [About IBM Cloud Network Load Balancer for VPC](/docs/vpc?topic=vpc-network-load-balancers).

The network load balancers API is shared between {{site.data.keyword.cloud_notm}} application load balancers and network load balancers.
{:note}

## 2020-08-25
{: #2020-08-25}

This API release supports the following changes for all API versions:

* Create an instance group of a fixed size and use it as a stand-alone feature
* Use the instance group manager to manage the instance group, which can associate an auto scale policy with that group

For more information, see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

The following new operations are available for [instance groups](https://{DomainName}/apidocs/vpc#list-instance-groups):

* `GET`, `POST` /instance_groups
* `DELETE`, `GET`, `PATCH` /instance_groups/{id}
* `DELETE` /instance_groups/{instance_group_id}/load_balancer
* `GET`, `POST` /instance_groups/{instance_group_id}/managers
* `DELETE`, `GET`, `PATCH` /instance_groups/{instance_group_id}/managers/{id}
* `GET`, `POST` /instance_groups/{instance_group_id}/managers/{instance_group_manager_id}/policies
* `DELETE`, `GET`, `PATCH` /instance_groups/{instance_group_id}/managers/{instance_group_manager_id}/policies/{id}
* `GET` /instance_groups/{instance_group_id}/memberships
* `DELETE`, `GET`, `PATCH` instance_groups/{instance_group_id}/memberships/{id}

The new [instance template](https://{DomainName}/apidocs/vpc#list-instance-templates) feature can also be used independently of auto scale. For example, you can create a template, and then create instances from that template without creating an instance group.

The following new endpoints are now available under Instances:
* `GET`, `POST` /instance/templates
* `GET`, `PATCH`, `DELETE` /instance/templates/{id}

The ability of `POST` /instances is now extended with `InstanceBySourceTemplate`.

## 2020-07-30
{: #2020-07-30}

The [Python SDK](https://{DomainName}/apidocs/vpc?code=python) is now generally available.

## 2020-07-23
{: #2020-07-23}

The [Flow log collectors](https://{DomainName}/apidocs/vpc#list-all-flow-log-collectors) API is now generally available.

## 2020-07-22
{: #2020-07-22}

The following enhancements support customer-managed encryption for block storage boot and data volumes:

* `POST` /instances -- Create an instance and new volume, encrypted, using your customer root key (CRK). CRKs are imported to {{site.data.keyword.cloud_notm}} or created in a key management service. See [Create an instance](https://{DomainName}/apidocs/vpc#create-an-instance).
* `POST` /volumes -- Create an unattached data volume, encrypted, using your CRK. See [Create a volume](https://{DomainName}/apidocs/vpc#create-a-volume).

## 2020-07-16
{: #2020-07-16}

The Tokyo region endpoint (`jp-tok`) is now in service at `https://jp-tok.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

## 2020-06-19
{: #2020-06-29}

The [Go SDK](https://{DomainName}/apidocs/vpc?code=go) is now generally available.

## 2020-05-12
{: #2020-05-12}

Load balancer pool resources and their health monitors can now be configured to use the HTTPS protocol. This enhancement enables end-to-end SSL encryption with HTTPS listeners, along with HTTPS health checks for increased availability. See the [Load Balancers](https://{DomainName}/apidocs/vpc#list-all-load-balancers) API.

## 2020-05-01
{: #2020-05-01}

This API release supports the following changes for all API versions:

* [Flow log collectors](https://{DomainName}/apidocs/vpc#list-all-flow-log-collectors) API is available as beta
* `GET` /security_groups now supports `vpc.crn` and `vpc.name` filters

## 2020-04-21
{: #2020-04-21}

The Frankfurt region endpoint (eu-de) is now in service at `http://eu-de.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

## 2020-04-17
{: #2020-04-17}

{{site.data.keyword.cloud_notm}} interprets volume capacity units in gibibytes, but the API documentation used gigabytes. This issue has been resolved in the documentation. 

## 2020-04-10
{: #2020-04-10}

Usage recommendations are provided for the following load balancer properties:

* `protocol` property for load balancer pools
* `type` property for load balancer pool health monitors

The guidance notes that new values for these properties might be added in the future, and unexpected values are handled gracefully. See the [Load Balancers](https://{DomainName}/apidocs/vpc#list-all-load-balancers) API.

## 2020-02-28
{: #2020-03-02}

The London region endpoint (eu-gb) is now in service at `http://eu-gb.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

## 2020-02-06
{: #2020-02-06}

Support is temporarily suspended for creating instances from an existing boot volume. This feature was available through the API only, with no CLI or UI support. In the interim, you must specify the `image` property when you call `POST` /instances.

You can still create instances that reference existing data volumes.
{: note}

## 2020-01-10
{: #2020-01-10}

The Washington DC	region endpoint (us-east) is now in service at `http://us-east.iaas.cloud.ibm.com`. For more information, see [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

## 2019-12-03
{: #2019-12-03}

Device IDs are now shown when you retrieve an instance's volume attachments. This change is available for all API versions.

## 2019-11-26
{: #2019-11-26}

This API release supports the following changes for all API versions:

* [Network access control list (ACL)](https://{DomainName}/apidocs/vpc#list-all-network-acls) methods
* Instance filtering by VPC

## 2019-11-21
{: #2019-11-21}

A VPC’s Cloud Service Endpoint source IPs now appear in output. This change is available for all API versions.

## 2019-11-05
{: #2019-11-05}

This API release supports the following changes for all API versions:

* [Load balancers](https://{DomainName}/apidocs/vpc#list-all-load-balancers) API is available as beta
* [VPN gateways](https://{DomainName}/apidocs/vpc#list-all-vpn-gateways) are available as beta
* Pagination is now supported for [instances](https://{DomainName}/apidocs/vpc#list-all-instances)
* Classic access to VPCs (also known as classic peering) is supported
