---

copyright:
  years: 2019
lastupdated: "2019-11-06"

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


## 2019-11-05
{: #2019-11-05}

* Load balancers are available as beta in this release
* VPN gateways are available as beta in this release
* Pagination is now supported for instances
* Classic access to VPCs (also known as classic peering) is supported.
