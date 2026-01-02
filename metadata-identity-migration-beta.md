---

copyright:
  years: 2025, 2026
lastupdated: "2025-07-15"

keywords: Metadata, identity, api migration, versioned change

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to the `2025-07-15` version of the Beta VPC Identity API
{: #2025-07-15-migration-metadata-identity}

As described in the [Beta VPC Identity API](/apidocs/vpc-identity-beta/initial) reference [versioning](/apidocs/vpc-identity-beta/initial#versioning-identity-beta) policy, most changes to the VPC Identity API are fully compatible with earlier versions and are made available to all clients, regardless of the VPC Identity API version the client requests. However, the `2025-07-15` release of the VPC Identity API necessitated incompatible changes in support of identity methods.

When you adopt the release version `2025-07-15` or later, the VPC Identity API will operate this way:

- Methods with `/instance_identity` in the path cannot be used on bare metal servers and virtual server instances.
- Methods with `/identity` in the path can be used by both virtual server instances and bare metal servers. Methods with `/identity` in the path operate the same as `/instance_identity` methods when used in virtual server instances.

## Action needed
{: #action-needed-metadata-identity-beta}

Before you specify the `version` query parameter of `2025-07-15` or later, follow these actions to avoid regressions in client functionality.

If your clients continue to specify API version `2025-07-14` or earlier, no changes are required.

## Changed Identity API paths
{: #changed-paths-metadata-identity-beta}

The following table lists the methods and their changed paths for API requests that use a `version` query parameter of `2025-07-15` or later.

| Method   | Old path                            | New path                   |
|----------|-------------------------------------|----------------------------|
| `PUT`    | `/instance_identity/v1/token`       | `/identity/v1/token`       |
| `POST`   | `/instance_identity/v1/iam_token`   | `/identity/v1/iam_tokens`  |
| `POST`   | `/instance_identity/v1/certificates`| `/identity/v1/certificates`|
{: caption="Methods and their changed paths for API requests that use a version query parameter of 2025-07-15 or later." caption-side="bottom"}

## Examples
{: #examples-metadata-identity-beta}

These examples compare differences between before and after the `2025-07-15` versioned change.

### Creating an identity token
{: #migration-2025-07-15-examples}

The following example uses API version `2025-07-14` or earlier to create an identity token.
```sh
curl -X PUT "http://api.metadata.cloud.ibm.com/instance_identity/v1/token?version=2025-07-14&generation=2&maturity=beta"
  -H "Metadata-Flavor: ibm"
  -d '{"expires_in": 3600}'
```
{: codeblock}

The response includes the identity token:

```json
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0aGVfYmVzdCI6IkVyaWNhIn0.c4C_BKtyZ4g78TB6wjdsX_MNx4KPoYj8YiikB1jO4o8","created_at":"2025-07-15T15:09:45Z","expires_at": "2025-07-16T15:09:45Z","expires_in":3600}
```
{: codeblock}

The following example uses API version `2025-07-15` or later to create an identity token.

```sh
curl -X PUT "http://api.metadata.cloud.ibm.com/identity/v1/token?version=2025-07-15&generation=2&maturity=beta"
  -H "Metadata-Flavor: ibm"
  -d '{"expires_in": 3600}'
```
{: codeblock}

The response includes the identity token:

```json
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0aGVfYmVzdCI6IkVyaWNhIn0.c4C_BKtyZ4g78TB6wjdsX_MNx4KPoYj8YiikB1jO4o8","created_at":"2025-07-15T15:09:45Z","expires_at": "2025-07-16T15:09:45Z","expires_in":3600}
```
{: codeblock}
