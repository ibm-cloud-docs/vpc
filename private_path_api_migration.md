---

copyright:
  years: 2024
lastupdated: "2024-09-17"

keywords: private path service gateway, api migration, versioned change

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to the beta `2024-09-17` version (Private Path service gateway)
{: #2024-09-17-migration-private-path-service-gateway}

As described in the [VPC API](/apidocs/vpc/latest) reference [versioning](/apidocs/vpc#api-versioning) policy, most changes to the VPC APIs are fully backward compatible and are made available to all clients, regardless of the API version the client requests. However, the `2024-09-17` release of the VPC beta API necessitated incompatible changes in support of private path service gateways.

Before you adopt the beta release version `2024-09-17` or later, review the changes described in this migration guidance that might require you to update your client.

## Changed private path service gateway publish and unpublish methods
{: #changed-private-path-service-gateway-publish-and-unpublish-methods-beta}

In the [12 March 2024](/docs/vpc?topic=vpc-api-change-log-beta&interface=api#12-march-2024-beta) release, publishing or unpublishing a private path service gateway was accomplished by [updating](/apidocs/vpc-beta/latest#update-private-path-service-gateway) the `published` property to `true` (any account can request access to this private path service gateway) or `false` (access is restricted to the account that created this private path service gateway). In version `2024-09-17` or later, the `published` property is read-only, and new methods must be used to [publish](/apidocs/vpc-beta/latest#publish-private-path-service-gateway) or [unpublish](/apidocs/vpc-beta/latest#unpublish-private-path-service-gateway) a private path service gateway.

## Changed private path service gateway property
{: #changed-private-path-service-gateway-property-beta}

In the [12 March 2024](/docs/vpc?topic=vpc-api-change-log-beta&interface=api#12-march-2024-beta) release, a private path service gateway response contained the property `endpoint_gateways_count`. Now, when using a version query parameter of `2024-09-17` or later, the property is renamed to `endpoint_gateway_count`.

## Changed HTTP response code
{: #changed-http-response-code-beta}

When using a version query parameter of `2024-09-17` or later, the HTTP response code for the following methods has changed from `200` to `204`.

- [Deny an endpoint gateway binding for a private path service gateway](/apidocs/vpc-beta/latest#deny-private-path-service-gateway-endpoint-gateway)
- [Permit an endpoint gateway binding for a private path service gateway](/apidocs/vpc-beta/latest#permit-private-path-service-gateway-endpoint-gatew)
- [Revoke access to a private path service gateway for an account](/apidocs/vpc-beta/latest#revoke-account-for-private-path-service-gateway)
- [Delete a private path service gateway](/apidocs/vpc-beta/latest#delete-private-path-service-gateway)

## Action needed
{: #action-needed-private-path-beta}

### Migrating to new publish and unpublish method for private path service gateways
{: #publish-and-unpublish-client-migration-beta}

Before you migrate a client to API version `2024-09-17` or later, review your code that [updates a private path service gateway](/apidocs/vpc-beta/latest#update-private-path-service-gateway). Review the changes that were announced in the [API change log](/docs/vpc?topic=vpc-api-change-log-beta#version-2024-09-17-beta), and verify that your code adopts these changes in a manner that is appropriate for your programming language.

### Examples (publish and unpublish)
{: #examples-publish-and-unpublish-beta}

These examples compare differences between before and after the `2024-09-17` versioned change.

The following example uses API version `2024-09-16` or earlier to publish a private path service gateway. The `published` property in the request is specified as `true`.

```sh
curl -H "Authorization: $token" -X PATCH "$e/v1/private_path_service_gateways/$ppsId?version=2024-09-16&generation=2&maturity=beta" -d '{"published": true}' | jq '"published: \(.published)"'

"published: true"
```
{: pre}

The following example uses API version `2024-09-17` or later to publish a private path service gateway. The HTTP response code is `204`, and the response body is empty.

```sh
curl  -H "Authorization: $token" -X POST "$e/v1/private_path_service_gateways/$ppsId/publish?version=2024-09-17&generation=2&maturity=beta" -d "" | jq
```
{: pre}

[Retrieve the private path service gateway](/apidocs/vpc-beta/latest#get-private-path-service-gateways) to confirm that it has been published.

```sh
curl -H "Authorization: $token" -X GET "$e/v1/private_path_service_gateways/$ppsId?version=2024-09-17&generation=2&maturity=beta"  | jq '"published: \(.published)"'

"published: true"
```
{: pre}

The following example uses API version `2024-09-16` or earlier to unpublish a private path service gateway. The `published` property in the request is specified as `false`.

```sh
curl -H "Authorization: $token" -X PATCH "$e/v1/private_path_service_gateways/$ppsId?version=2024-09-16&generation=2&maturity=beta" -d '{"published": false}' | jq '"published: \(.published)"'

"published: false"
```
{: pre}

The following example uses API version `2024-09-17` or later to unpublish a private path service gateway. The HTTP status code response is `204` and the response body is empty.

```sh
curl  -H "Authorization: $token" -X POST "$e/v1/private_path_service_gateways/$ppsId/unpublish?version=2024-09-17&generation=2&maturity=beta" -d "" | jq '"published: \(.published)"'
```
{: pre}

[Retrieve the private path service gateway](/apidocs/vpc-beta/latest#get-private-path-service-gateway) to confirm that it has been unpublished.

```sh
curl -H "Authorization: $token" -X GET "$e/v1/private_path_service_gateways/$ppsId?version=2024-09-17&generation=2&maturity=beta" | jq '"published: \(.published)"'

"published: false"
```
{: pre}

### Migrating  HTTP response codes from `200` to `204` for private path service gateway methods
{: #response-code-migration-beta}

Review and update any logic in your clients that expects a `200` response code to now expect a `204` response code for [permitting](/apidocs/vpc-beta/latest#permit-private-path-service-gateway-endpoint-gatew) or [denying](/apidocs/vpc-beta/latest#deny-private-path-service-gateway-endpoint-gateway) an endpoint gateway binding for a private path service gateway, [revoking access to a private path service gateway for an account](/apidocs/vpc-beta/latest#revoke-account-for-private-path-service-gateway), or [deleting a private path service gateway](/apidocs/vpc-beta/latest#delete-private-path-service-gateway). Because the `204` response code does not include a response body, confirm that your clients are set to correctly handle a no-content response without attempting to process any response body.

### Migrating `endpoint_gateways_count` property to `endpoint_gateway_count`
{: #gateway-count-migration-beta}

Before you migrate a client to API version `2024-09-17` or later, review your code for the use of `endpoint_gateways_count` property, and replace it with the the renamed property `endpoint_gateway_count`.

### Examples (`endpoint_gateways_count` and `endpoint_gateway_count`)
{: #examples-endpoint-gateway-count-beta}

The following example uses API version `2024-09-16` or earlier to retrieve a private path service gateway. The `endpoint_gateways_count` property is provided in the response.

```sh
curl -H "Authorization: $token" -X GET "$e/v1/private_path_service_gateways/$ppsId?version=2024-09-16&generation=2&maturity=beta" | jq '"endpoint_gateways_count: \(.endpoint_gateways_count)"'

"endpoint_gateways_count: 1",
```
{: pre}

The following example uses API version `2024-09-17` or later to retrieve a private path service gateway. The `endpoint_gateway_count` property is provided in the response.

```sh
curl -H "Authorization: $token" -X GET "$e/v1/private_path_service_gateways/$ppsId?version=2024-09-17&generation=2&maturity=beta" | jq '"endpoint_gateway_count: \(.endpoint_gateway_count)"'

"endpoint_gateway_count: 1",
```
{: pre} 
