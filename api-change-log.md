---

copyright:
  years: 2019, 2020
lastupdated: "2020-07-16"

keywords: vpc, api, change log, new features, restrictions, migration, generation 2, gen2,

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

This page describes {{site.data.keyword.vpc_full}} (VPC) [API](https://{DomainName}/apidocs/vpc) improvements and fixes, as well as guidance on client code updates required to use a new date-based version. By design, new features with backward-incompatible changes apply only to version dates on and after the feature's release. Changes that apply to older versions of the API are designed to maintain compatibility with existing applications and code.

If backward-incompatible changes require non-trivial client code changes in order to use an API version, this page may provide links to instructions, tips, or best practices for migrating client code. See [API application migration considerations](/docs/vpc?topic=vpc-api-integration-migration).
{:note}

The following changes are considered backward compatible:

* New or changed resources
* New or changed fields

To minimize bugs caused by changes, use the following best practices when you call the API:

* Catch and log any `4xx` or `5xx` HTTP status code, along with the included `trace` property
* Follow HTTP redirect rules for any `3xx` HTTP status code
* Consume only the resources and properties your application needs to function
* Avoid depending on behavior that is not explicitly documented

## 2020-07-16
{: #2020-07-16}

The Tokyo region endpoint (jp-tok) is now in service at https://jp-tok.iaas.cloud.ibm.com.

## 2020-05-12
{: #2020-05-12}
Load balancer pool resources and their health monitors can now be configured to use the https protocol. This enables end-to-end SSL encryption with HTTPS listeners, along with HTTPS health checks for increased availability.

## 2020-05-01
{: #2020-05-01}
Flow logs are available as beta in this release.

## 2020-04-21
{: #2020-04-21}
The Frankfurt region endpoint (eu-de) is now in service at http://eu-de.iaas.cloud.ibm.com.

## 2020-04-10
{: #2020-04-10}
Added usage recommendations for the protocol property for load balancer pools and the type property for load balancer pool health monitors. The guidance notes that new values for these properties may be added in the future, and unexpected values will be handled gracefully.

## 2020-03-02
{: #2020-03-02}
The London region endpoint (eu-gb) is now in service at http://eu-gb.iaas.cloud.ibm.com.

## 2020-02-06
{: #2020-02-06}
We have temporarily suspended support for creating instances from an existing boot volume. This feature had been available only through the API, with no CLI or UI support. In the interim you must specify the image property when you call POST /instances. You can still create instances that reference existing data volumes.

## 2020-01-10
{: #2020-01-10}
The Washington DC	region endpoint (us-east) is now in service at http://us-east.iaas.cloud.ibm.com.

## 2019-11-26
{: #2019-11-26}
This API release supports the following changes for all API versions:
* Network access control list (ACL) methods 
* Instance filtering by VPC


## 2019-11-21
{: #2019-11-21}

A VPC’s Cloud Service Endpoint source IPs now appear in output. This change is available for all API versions.

## 2019-11-05
{: #2019-11-05}

* Load balancers are available as beta in this release
* VPN gateways are available as beta in this release
* Pagination is now supported for instances
* Classic access to VPCs (also known as classic peering) is supported.
