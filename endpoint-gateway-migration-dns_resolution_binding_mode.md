---

copyright:
  years: 2025, 2025
lastupdated: "2025-12-16"

keywords: endpoint gateway migration

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# `2025-11-18` API migration (endpoint gateways)
{: #2025-11-18-migration-endpoint-gateways}

As described in the VPC API reference [versioning](/apidocs/vpc/latest#api-versioning) policy, most changes to the VPC APIs are fully backward compatible and are made available to all clients, regardless of the API version the client requests. However, the `2025-11-18` release of the VPC API necessitated incompatible changes in support of local-access endpoint gateways with DNS sharing for VPE gateways.

Before you adopt the release version `2025-11-18` or later, review the changes described in this migration guidance that might require you to update your client.

## Changed endpoint gateway properties
{: changed-properties-endpoint-gateway-api-version-2025-11-18}

The following properties have changed for API requests that use a version query parameter of `2025-11-18` or later.

When [creating](/apidocs/vpc/2025-11-18#create-endpoint-gateway), [updating](/apidocs/vpc/2025-11-18#update-endpoint-gateway), [retrieving](/apidocs/vpc/2025-11-18#get-endpoint-gateway) or [listing](/apidocs/vpc/2025-11-18#list-endpoint-gateways), the following properties have changed for API requests that use a version query parameter of `2025-11-18` or later.

| Old properties                                           | New property                  |
|----------------------------------------------------------|-------------------------------|
| `allow_dns_resolution_binding`, `allow_resource_binding` | `dns_resolution_binding_mode` |
{: caption="Old and new properties when creating an endpoint gateway." caption-side="bottom"}

## Action needed
{: #action-needed-endpoint-gateway-api-version-2025-11-18}

Before you specify the `version` query parameter of `2025-11-18` or later, follow these actions to avoid regressions in client functionality.

Map the `allow_dns_resolution_binding` and `allow_resource_binding` property values that were used for API versions before `2025-11-18` to the `dns_resolution_binding_mode` property value as follows:

| `allow_dns_resolution_binding` | `allow_resource_binding` | `dns_resolution_binding_mode` |
|--------------------------------|--------------------------|-------------------------------|
| `false`                        | `false`                  | `disabled`                    |
| `true`                         | `false`                  | `primary`                     |
| `true`                         | `true`                   | `per_resource_binding`        |
{: caption="Mapping between old and new properties for endpoint gateways." caption-side="bottom"}

In API versions before `2025-11-18`, when `allow_dns_resolution_binding` is `false` then `allow_resource_binding` cannot be `true`.

If your clients continue to specify version `2025-11-18` or earlier, no changes are required.
{: note}

### Client migration
{: #client-migration-endpoint-gateway-api-version-2025-11-18}

Before you migrate a client to API version `2025-11-18` or later, review your code that uses the following methods:

- `POST /endpoint_gateways`
- `GET /endpoint_gateways`
- `PATCH /endpoint_gateways`

Review the changes that were announced in the [API change log](/docs/vpc?topic=vpc-api-change-log#18-november-2025), and verify that your code adopts these changes in a manner that is appropriate for your programming language.

## Examples
{: #example-endpoint-gateway-api-version-2025-11-18}

These examples compare differences between before and after the `2025-11-18` versioned change.

### Creating an endpoint gateway
{: #create-endpoint-gateway-api-version-2025-11-18}

This request creates an endpoint gateway, specifying API version `2025-11-17` or earlier. This request uses the default value of `false` for `allow_resource_binding` by not specifying it in the request body.

```sh
curl -X POST \
  --url "$vpc_api_endpoint/v1/endpoint_gateways?generation=2&version=2025-11-17" \
  --header "Authorization: Bearer $iam_token" \
  --header 'Content-Type: application/json' \
  --data '{
      "name": "my-endpoint-gateway-1",
      "allow_dns_resolution_binding": false,
      "target": {
        "name": "ibm-ntp-server",
        "resource_type":"provider_infrastructure_service"
      },
      "vpc": {
          "id": "r006-4727d842-f94f-4a2d-824a-9bc9b02c523b"
      },
      "ips": [
          { "id": "0717-6d353a0f-aeb1-4ae1-832e-1110d10981bb" }
      ]
    }'
```
{: pre}

This request creates an endpoint gateway, specifying API version `2025-11-18` or later.

```sh
curl -X POST \
  --url "$vpc_api_endpoint/v1/endpoint_gateways?generation=2&version=2025-11-18" \
  --header "Authorization: Bearer $iam_token" \
  --header 'Content-Type: application/json' \
  --data '{
      "name": "my-endpoint-gateway-1",
      "dns_resolution_binding_mode": "disabled",
      "target": {
        "name": "ibm-ntp-server",
        "resource_type":"provider_infrastructure_service"
      },
      "vpc": {
          "id": "r006-4727d842-f94f-4a2d-824a-9bc9b02c523b"
      },
      "ips": [
          { "id": "0717-6d353a0f-aeb1-4ae1-832e-1110d10981bb" }
      ]
    }'
```
{: pre}

### Updating an endpoint gateway
{: #update-endpoint-gateway-api-version-2025-11-18}

This request updates an endpoint gateway, specifying API version `2025-11-17` or earlier.

```sh
curl -X PATCH \
  --url "$vpc_api_endpoint/v1/endpoint_gateways?generation=2&version=2025-11-17" \
  --header "Authorization: Bearer $iam_token" \
  --header 'Content-Type: application/json' \
  --data '{
      "allow_dns_resolution_binding": true
    }'
```
{: pre}

This request updates an endpoint gateway, specifying API version `2025-11-18` or later.

```sh
curl -X PATCH \
  --url "$vpc_api_endpoint/v1/endpoint_gateways?generation=2&version=2025-11-18" \
  --header 'Authorization: Bearer $iam_token' \
  --header 'Content-Type: application/json' \
  --data '{
      "dns_resolution_binding_mode": "primary"
    }'
```
{: pre}
