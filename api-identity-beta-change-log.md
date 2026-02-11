---

copyright:
  years: 2026
lastupdated: "2025-07-15"

keywords: api, change log, beta, identity

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Beta VPC Identity API change log
{: #identity-beta-api-change-log}

Read the API change log to learn about updates and improvements to the Beta {{site.data.keyword.vpc_full}} (VPC) [Identity API](/apidocs/vpc-identity-beta). The change log lists changes that are ordered by the date that they were released.
{: shortdesc}

Some beta features are for accounts that have special approval to preview a particular beta feature. Contact your IBM sales representative if you are interested in getting access.
{: beta}

Compatibility with earlier versions is not guaranteed as a feature progresses through its beta phase or from the final beta release to its initial GA release. Using non-GA-mature features might introduce the risk of corrupting resources in your account. IBM strongly recommends that you do not use non-GA-mature features on production accounts.
{: important}

## 15 July 2025
{: #15-july-2025-identity-beta}

### For all version dates
{: #15-july-2025-all-version-dates-identity-beta}

This feature is now generally available. See the [VPC Identity API change log](/docs/vpc?topic=vpc-identity-api-change-log#26-august-2025-identity) and [Updating to the 2025-08-26 version of the VPC Identity API](/docs/vpc?topic=vpc-2025-08-26-migration-metadata-identity).

**Bare metal server support.** Accounts that have special approval to preview this feature can now access the identity service from bare metal servers. You can use the new [create identity access token](/apidocs/vpc-identity-beta/initial#create-access-token) method to create an access token for your bare metal server, and pass that token to the new [create identity certificate](/apidocs/vpc-identity-beta/initial#create-identity-certificate) method. Alternatively, you can pass that token to the [create IAM access token](/apidocs/vpc-identity-beta/initial#create-iam-token) method, and then use that token to access IAM-enabled services. You can also use these new methods from virtual server instances. For more information, see the Beta VPC [Identity API](/apidocs/vpc-identity-beta).

### For version `2025-07-15` or later
{: #version-2025-07-15}

**Instance identity methods.** When a beta `version` query parameter of `2025-07-15` or later is used, the request fails with an HTTP response of `404` because the `/instance_identity` methods were removed. Clients need to migrate to the new `/identity` methods.

For more information, see [VPC Identity API known issues](/docs/vpc?topic=vpc-known-issues#identity-api-known-issues).
