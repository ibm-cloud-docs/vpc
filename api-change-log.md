---

copyright:
  years: 2019
lastupdated: "2019-10-15"

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

This page contains information about {{site.data.keyword.vpc_full}} (VPC) [API](https://{DomainName}/apidocs/vpc) improvements and fixes, as well as guidance on client code updates required to use a new date-based version. By design, new features with backward-incompatible changes apply only to version dates on and after the feature's release. Changes that apply to older versions of the API are designed to maintain compatibility with existing applications and code.

If backward-incompatible changes require non-trivial client code changes in order to use an API version, this page will link to detailed migration instructions and examples about how to migrate client code. 
{:note}

The following changes are considered backward compatible:

* New or changed resources
* New or changed fields

To minimize regressions from changes, IBM recommends the following best practices when you call the API:

* Catch and log any `4xx` or `5xx` HTTP status code, along with the included `trace` property
* Follow HTTP redirect rules for any `3xx` HTTP status code
* Consume only the resources and properties your application needs to function
* Avoid depending on behavior that is not explicitly documented

## YYYY-MM-DD
Generic plan (content that might appear under each API version) *This section will be remove before pushing to production*.

### New API endpoints
{: #new-api-endpoints}

xxx

### API endpoint changes
{: #api-endpoint-changes}

xxx

### New fields
{: #new-fields}

xxx

### Field changes
{: #field-changes}

xxx

### Deprecated features
{: #deprecation}

xxx

## 2019-10-08
{: #2019-10-08}

Initial release:
* List APIs don't support pagination.
* GET /instances doesn't support the query parameter `network_interfaces.subnet.id`
* Virtual server instances (VSIs) must be stopped before they can be deleted

For more information, see [API application migration considerations](/docs/vpc?topic=vpc-api-integration-migration).
