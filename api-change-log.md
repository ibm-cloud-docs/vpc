---

copyright:
  years: 2019, 2020
lastupdated: "2020-09-01"

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

This page describes {{site.data.keyword.vpc_full}} (VPC) [API](https://{DomainName}/apidocs/vpc) new features, improvements, and fixes. This page may also provide guidance on client code updates required to use a new date-based version of the API. 

By design, new features with backward-incompatible changes apply only to version dates on and after the feature's release. Changes that apply to older versions of the API are designed to maintain compatibility with existing applications and code. If backward-incompatible changes require non-trivial client code changes in order to use an API version, this page may provide links to instructions, tips, or best practices for updating client code. For more information, see [API application migration considerations](/docs/vpc?topic=vpc-api-integration-migration).
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
## 2020-08-19
{: #2020-08-19}
The [Virtual Private Endpoint](https://{DomainName}/apidocs/vpc#list-endpoint-gateways) (VPE) API is now generally available. Use VPE to connect to supported IBM Cloud services from your VPC virtual network by using the IP addresses of your choosing, allocated from a subnet within your VPC. For details, see [About Virtual Private Endpoints](/docs/vpc?topic=vpc-about-vpe).
-->

<!--
## 2020-07-30
{: #2020-07-30}
Custom routing tables are now supported in the VPC API, providing control over where network traffic is directed on a per-subnet basis. Dive into new API operations for [routing tables](https://{DomainName}/apidocs/vpc#list-all-routing-tables-for-a-vpc) and [routes](https://{DomainName}/apidocs/vpc#create-a-vpc-route). This feature subsumes the [VPC routing API](https://{DomainName}/apidocs/vpc#list-all-routes-in-the-vpc-s-default-routing-table), which remains supported but is deprecated and may be removed in a future API release.
-->

## 2020-08-31
{: #2020-08-31}

The Sydney region endpoint (`au-syd`) is now in service at https://au-syd.iaas.cloud.ibm.com. See [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API. See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region)

The [Load balancers for VPC API](https://{DomainName}/apidocs/vpc#list-load-balancer-profiles) was updated to support general availability of {{site.data.keyword.cloud_notm}} Network Load Balancers for VPC. This API is shared between {{site.data.keyword.cloud_notm}} application load balancers and network load balancers.

## 2020-08-25
{: #2020-08-25}

This API release supports the following changes for all API versions:

Create an instance group of a fixed size and use it as a standalone feature, or have the instance group managed by an instance group manager, which can then have an auto scale policy associated with it. See [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

The following new operations are now available for [instance groups](https://{DomainName}/apidocs/vpc#list-instance-groups):

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

The following new endpoints have been added under Instances:
* `GET`, `POST` /instance/templates
* `GET`, `PATCH`, `DELETE` /instance/templates/{id}

We have also extended the ability of `POST` /instances with `InstanceBySourceTemplate`.

## 2020-07-30
{: #2020-07-30}

The [Python SDK](https://{DomainName}/apidocs/vpc?code=python) is now generally available.

## 2020-07-23
{: #2020-07-23}

The [Flow log collectors](https://{DomainName}/apidocs/vpc#list-all-flow-log-collectors) API is now generally available.

## 2020-07-22
{: #2020-07-22}

The following enchancements support customer-managed encryption for block storage boot and data volumes: 

* `POST` /instances lets you create an instance and new volume encrypted using your customer root key (CRK). CRKs are imported to the cloud or created in a key management service. See [Create an instance](https://{DomainName}/apidocs/vpc#create-an-instance).
* `POST` /volumes lets you create an unattached data volume encrypted using your CRK. See [Create a volume](https://{DomainName}/apidocs/vpc#create-a-volume).

## 2020-07-16
{: #2020-07-16}

The Tokyo region endpoint (`jp-tok`) is now in service at https://jp-tok.iaas.cloud.ibm.com. See [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API.

See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

<!--INSERT LINK to methods (e.g., https://{DomainName}/apidocs/vpc#list-all-blah-blah-woof-woof) and any other how-to topics. Also adjust release date below.
## 2020-07-30
{: #2020-07-30}
* Virtual private endpoint API is generally available.
-->

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

The Frankfurt region endpoint (eu-de) is now in service at http://eu-de.iaas.cloud.ibm.com. See [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the Virtual Private Cloud API.

See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

## 2020-04-10
{: #2020-04-10}

Usage recommendations are provided for the following load balancer properties:

* `protocol` property for load balancer pools
* `type` property for load balancer pool health monitors

The guidance notes that new values for these properties may be added in the future, and unexpected values will be handled gracefully. See the [Load Balancers](https://{DomainName}/apidocs/vpc#list-all-load-balancers) API.

## 2020-02-28
{: #2020-03-02}

The London region endpoint (eu-gb) is now in service at http://eu-gb.iaas.cloud.ibm.com. See [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the Virtual Private Cloud API.

See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

## 2020-02-06
{: #2020-02-06}

We temporarily suspended support for creating instances from an existing boot volume. This feature had been available through the API only, with no CLI or UI support. In the interim, you must specify the `image` property when you call `POST` /instances. 

You can still create instances that reference existing data volumes.
{: note}

## 2020-01-10
{: #2020-01-10}

The Washington DC	region endpoint (us-east) is now in service at http://us-east.iaas.cloud.ibm.com. See [Endpoint URLs]((https://{DomainName}/apidocs/vpc#endpoint-url) in the Virtual Private Cloud API.

See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

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
---

copyright:
  years: 2019, 2020
lastupdated: "2020-08-25"

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

This page describes {{site.data.keyword.vpc_full}} (VPC) [API](https://{DomainName}/apidocs/vpc) new features, improvements, and fixes. This page may also provide guidance on client code updates required to use a new date-based version of the API. 

By design, new features with backward-incompatible changes apply only to version dates on and after the feature's release. Changes that apply to older versions of the API are designed to maintain compatibility with existing applications and code. If backward-incompatible changes require non-trivial client code changes in order to use an API version, this page may provide links to instructions, tips, or best practices for updating client code. For more information, see [API application migration considerations](/docs/vpc?topic=vpc-api-integration-migration).
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
## 2020-08-19
{: #2020-08-19}
The [Virtual Private Endpoint](https://{DomainName}/apidocs/vpc#list-endpoint-gateways) (VPE) API is now generally available. Use VPE to connect to supported IBM Cloud services from your VPC virtual network by using the IP addresses of your choosing, allocated from a subnet within your VPC. For details, see [About Virtual Private Endpoints](/docs/vpc?topic=vpc-about-vpe).
-->

<!--
## 2020-07-30
{: #2020-07-30}
Custom routing tables are now supported in the VPC API, providing control over where network traffic is directed on a per-subnet basis. Dive into new API operations for [routing tables](https://{DomainName}/apidocs/vpc#list-all-routing-tables-for-a-vpc) and [routes](https://{DomainName}/apidocs/vpc#create-a-vpc-route). This feature subsumes the [VPC routing API](https://{DomainName}/apidocs/vpc#list-all-routes-in-the-vpc-s-default-routing-table), which remains supported but is deprecated and may be removed in a future API release.
-->

## 2020-08-25
{: #2020-08-25}

This API release supports the following changes for all API versions:

Create an instance group of a fixed size and use it as a standalone feature, or have the instance group managed by an instance group manager, which can then have an auto scale policy associated with it. See [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

The following new operations are now available for [instance groups](https://{DomainName}/apidocs/vpc#list-instance-groups):

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

The following new endpoints have been added under Instances:
* `GET`, `POST` /instance/templates
* `GET`, `PATCH`, `DELETE` /instance/templates/{id}

We have also extended the ability of `POST` /instances with `InstanceBySourceTemplate`.

## 2020-07-30
{: #2020-07-30}

The [Python SDK](https://{DomainName}/apidocs/vpc?code=python) is now generally available.

## 2020-07-23
{: #2020-07-23}

The [Flow log collectors](https://{DomainName}/apidocs/vpc#list-all-flow-log-collectors) API is now generally available.

## 2020-07-22
{: #2020-07-22}

The following enchancements support customer-managed encryption for block storage boot and data volumes: 

* `POST` /instances lets you create an instance and new volume encrypted using your customer root key (CRK). CRKs are imported to the cloud or created in a key management service. See [Create an instance](https://{DomainName}/apidocs/vpc#create-an-instance).
* `POST` /volumes lets you create an unattached data volume encrypted using your CRK. See [Create a volume](https://{DomainName}/apidocs/vpc#create-a-volume).

## 2020-07-16
{: #2020-07-16}

The Tokyo region endpoint (`jp-tok`) is now in service at https://jp-tok.iaas.cloud.ibm.com. See [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the {{site.data.keyword.vpc_short}} API.

See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

<!--INSERT LINK to methods (e.g., https://{DomainName}/apidocs/vpc#list-all-blah-blah-woof-woof) and any other how-to topics. Also adjust release date below.
## 2020-07-30
{: #2020-07-30}
* Virtual private endpoint API is generally available.
-->

## 2020-06-30
{: #2020-06-30}

Load balancer API was updated to support a Beta release of {{site.data.keyword.cloud_notm}} Network Load Balancers for VPC. This API is shared between {{site.data.keyword.cloud_notm}} application load balancers and network load balancers.  

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

The Frankfurt region endpoint (eu-de) is now in service at http://eu-de.iaas.cloud.ibm.com. See [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the Virtual Private Cloud API.

See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

## 2020-04-10
{: #2020-04-10}

Usage recommendations are provided for the following load balancer properties:

* `protocol` property for load balancer pools
* `type` property for load balancer pool health monitors

The guidance notes that new values for these properties may be added in the future, and unexpected values will be handled gracefully. See the [Load Balancers](https://{DomainName}/apidocs/vpc#list-all-load-balancers) API.

## 2020-02-28
{: #2020-03-02}

The London region endpoint (eu-gb) is now in service at http://eu-gb.iaas.cloud.ibm.com. See [Endpoint URLs](https://{DomainName}/apidocs/vpc#endpoint-url) in the Virtual Private Cloud API.

See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

## 2020-02-06
{: #2020-02-06}

We temporarily suspended support for creating instances from an existing boot volume. This feature had been available through the API only, with no CLI or UI support. In the interim, you must specify the `image` property when you call `POST` /instances. 

You can still create instances that reference existing data volumes.
{: note}

## 2020-01-10
{: #2020-01-10}

The Washington DC	region endpoint (us-east) is now in service at http://us-east.iaas.cloud.ibm.com. See [Endpoint URLs]((https://{DomainName}/apidocs/vpc#endpoint-url) in the Virtual Private Cloud API.

See also [Creating a VPC in a different region](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

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
