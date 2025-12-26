---

copyright:
  years: 2025
lastupdated: "2025-08-26"

keywords: api, change log, identity

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPC Identity API change log
{: #identity-api-change-log}

Read the API change log to learn about updates and improvements to the {{site.data.keyword.vpc_full}} (VPC) [Identity API](/apidocs/vpc-identity). Change log announcements are ordered by the date that they were released. Changes to existing API versions are designed to be compatible with existing client applications.
{: shortdesc}

By design, new features with backward-incompatible changes apply only to version dates on and after the feature's release. Changes that apply to older versions of the API are designed to maintain compatibility with existing applications and code. If backward-incompatible changes require nontrivial client code changes to use an API version, the API change log might provide links to instructions, tips, or best practices for updating client code.
{: note}

Some changes, such as new response properties or new optional request parameters, are considered compatible with the earlier versions. Other changes, such as new required request parameters, are not considered backward compatible. To avoid disruption from changes to the API, use the following best practices when you call the API:

- Catch and log any `4xx` or `5xx` HTTP status code, along with the included `trace` property
- Follow HTTP redirect rules for any `3xx` HTTP status code
- Consume only the resources and properties that your application needs to function
- Avoid depending on behavior that is not explicitly documented

## 26 August 2025
{: #26-august-2025-identity}

### For all version dates
{: #26-august-2025-all-version-dates-identity}

**Bare metal server support.** You can now access the VPC Identity API from bare metal servers. Use the new [create identity token](/apidocs/vpc-identity/latest#create-identity-token) method to create an identity token for your bare metal server, and pass that token to the new [create identity certificate](/apidocs/vpc-identity/latest#create-identity-certificate) method. Alternatively, you can pass that token to the [create IAM access token](/apidocs/vpc-identity/latest#create-identity-iam-token) method, and then use that token to access IAM-enabled services. You can also use these new methods from virtual server instances. For more information, see the [VPC Identity API](/apidocs/vpc-identity).

### For version `2025-08-26` or later
{: #version-2025-08-26}

**Instance identity methods.** When a `version` query parameter of `2025-08-26` or later is used, the request fails with an HTTP response of `404` because the `/instance_identity` methods were removed. Clients need to migrate to the new `/identity` methods. For more information, see the [`2025-08-26` API version migration guide](/docs/vpc?topic=vpc-2025-08-26-migration-metadata-identity). The behavior remains unchanged when a version query parameter of `2025-08-25` or earlier is used from virtual server instances.

For more information, see [VPC Identity API known issues](/docs/vpc?topic=vpc-known-issues#identity-api-known-issues).
