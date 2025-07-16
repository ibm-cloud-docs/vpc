---

copyright:
  years: 2025
lastupdated: "2025-07-15"

keywords: Metadata, identity, api migration, versioned change

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to the `2025-07-15` version (metadata identity)
{: #2025-07-15-migration-metadata-identity}

As described in the [Beta VPC Identity API](/apidocs/vpcidentity-beta) reference [versioning](/apidocs/vpc-identity-beta#versioning-identity-beta) policy, most changes to the VPC APIs are fully compatible with earlier versions and are made available to all clients, regardless of the API version the client requests. However, the `2025-07-15` release of the VPC API necessitated incompatible changes in support of metadata identity methods.

When you adopt the release version `2025-07-15` or later, the metadata identity APIs will operate this way:

- Methods with `/instance_identity` in the path cannot be used on bare metal servers and virtual server instances.
- Methods with `/identity` in the path can be used by both virtual server instances and bare metal servers. Methods with `/identity` in the path operate the same as `/instance_identity` methods when used in virtual server instances.

## Action needed
{: #action-needed-metadata-identity}

Before you specify the `version` query parameter of `2025-07-15` or later, follow these actions to avoid regressions in client functionality.

If your clients continue to specify API version `2025-07-14` or earlier, no changes are required.

## Changed Identity API paths
{: #changed-paths-metadata-identity}

The following table lists the methods and their changed paths for API requests that use a `version` query parameter of `2025-07-15` or later.

| Method   | Old path                            | New path                   |
|----------|-------------------------------------|----------------------------|
| `PUT`    | `/instance_identity/v1/token`       | `/identity/v1/token`       |
| `POST`   | `/instance_identity/v1/iam_token`   | `/identity/v1/iam_tokens`  |
| `POST`   | `/instance_identity/v1/certificates`| `/identity/v1/certificates`|

{: caption="Methods and their changed paths for API requests that use a version query parameter of 2025-07-15 or later." caption-side="bottom"}

## Examples
{: #examples-metadata-identity}

These examples compare differences between before and after the `2025-07-15` versioned change.

### Creating an identity access token
{: #migration-2025-07-15-examples}

The following example uses API version `2025-07-14` or earlier to create an identity access token.
```sh
curl -X PUT "http://api.metadata.cloud.ibm.com/instance_identity/v1/token?version=2025-07-14&generation=2&maturity=beta"
  -H "Metadata-Flavor: ibm"
  -d '{"expires_in": 3600}'
```
{: pre}

The response includes the access token:

```sh
{"access_token":"","expires_in":3600}
```
{: pre}

The following example uses API version `2025-07-15` or later to create an identity access token.  

```sh
curl -X PUT "http://api.metadata.cloud.ibm.com/identity/v1/token?version=2025-07-15&generation=2&maturity=beta"
  -H "Metadata-Flavor: ibm"
  -d '{"expires_in": 3600}'
```
{: pre}

The response includes the access token:

```sh
{"access_token":"","expires_in":3600}
```
{: pre}
