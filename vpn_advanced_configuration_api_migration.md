---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-21"

keywords: VPN migration, api migration, versioned change

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to the `2024-04-30` version (VPN gateway connection)
{: #2024-04-30-migration-vpn-advanced-configuration}

As described in the [VPC API](/apidocs/vpc/latest) reference [versioning](/apidocs/vpc#api-versioning) policy, most changes to the VPC APIs are fully backward compatible and are made available to all clients, regardless of the API version the client requests. However, the `2024-04-30` release of the VPC API necessitated incompatible changes in support of VPN gateway connections.

Before you adopt the release version `2024-04-30` or later, review the changes described in this migration guidance that might require you to update your client.

## Changed VPN gateway connection properties
{: #changed-vpn-gateway-properties-vpn-advanced-configuration}

The following VPN gateway connection properties have changed for API requests that use a `version` query parameter of `2024-04-30` or later.

When [creating a connection for a VPN gateway](/apidocs/vpc/latest#create-vpn-gateway-connection) (`POST /vpn_gateways/{vpn_gateway_id}/connections`):

| Old property    | New property   |
|-----------------|----------------|
| `peer_address`  | `peer.address` |
| `peer_cidrs`    | `peer.cidrs`   |
| `local_cidrs`   | `local.cidrs`  |
{: caption="Old and new properties when creating a VPN connection." caption-side="bottom"}

When [updating a VPN gateway connection](/apidocs/vpc/latest#update-vpn-gateway-connection) (`PATCH /vpn_gateways/{vpn_gateway_id}/connections/{id}`):

| Old property    | New property   |
|-----------------|----------------|
| `peer_address`  | `peer.address` |
{: caption="Old and new property when updating a VPN connection." caption-side="bottom"}

When making the following requests for policy-based VPN gateways, the properties have changed in the response:

- [Creating](/apidocs/vpc/latest#create-vpn-gateway-connection) (`POST /vpn_gateways/{vpn_gateway_id}/connections`)
- [Retrieving](/apidocs/vpc/latest#get-vpn-gateway-connection) (`GET /vpn_gateways/{vpn_gateway_id}/connections/{id}`)
- [Updating](/apidocs/vpc/latest#update-vpn-gateway-connection) a VPN gateway connection (`PATCH /vpn_gateways/{vpn_gateway_id}/connections/{id}`)
- [Listing](/apidocs/vpc/latest#list-vpn-gateway-connections) all connections of a VPN gateway (`GET /vpn_gateways/{vpn_gateway_id}/connections`)
- [Listing](/apidocs/vpc/latest#list-ike-policy-connections) all VPN gateway connections that use a specified IKE policy (`GET /ike_policies/{id}/connections`)
- [Listing](/apidocs/vpc/latest#list-ipsec-policy-connections) all VPN gateway connections that use a specified IPsec policy (`GET /ipsec_policies/{id}/connections`)

| Old property    | New property                   |
|-----------------|--------------------------------|
| `local_cidrs`   | `local.cidrs`                  |
| `peer_cidrs`    | `peer.cidrs`                   |
| `peer_address`  | `peer.address` or `peer.fqdn`  |
{: caption="Old and new properties for policy-based VPN gateways." caption-side="bottom"}

Property `peer_address` is replaced by `peer.address` or `peer.fqdn`, depending on whether an IPv4 address or an FQDN was used to specify the peer when the connection was created. Update your code to check `peer.type` in the response. If `peer.type` is `address`, then the response has `peer.address`. If `peer.type` is `fqdn`, then the response has `peer.fqdn`.

If `peer.fqdn` was specified when the connection was created, and a `version` query parameter of `2024-04-29` or earlier is specified, `peer_address` has the IP address associated with `peer.fqdn`.

Old VPN gateway connection properties and methods remain supported for API requests that use a `version` query parameter of `2024-04-29` or earlier.
{: note}

## Changed VPN gateway connection API paths
{: #changed-paths-vpn-advanced-configuration}

The following table lists the methods and their changed paths for API requests that use a `version` query parameter of `2024-04-30` or later.

| Method   | Old path                                                                                    | New path                                                            |
|----------|---------------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| `GET`    | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/local_cidrs`                               | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/local/cidrs`       |
| `GET`    | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/peer_cidrs`                                | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/peer/cidrs`        |
| `DELETE` | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/local_cidrs/{cidr_prefix}/{prefix_length}` | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/local/cidrs/{cidr}`|
| `GET`    | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/local_cidrs/{cidr_prefix}/{prefix_length}` | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/local/cidrs/{cidr}`|
| `PUT`    | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/local_cidrs/{cidr_prefix}/{prefix_length}` | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/local/cidrs/{cidr}`|
| `DELETE` | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/peer_cidrs/{cidr_prefix}/{prefix_length}`  | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/peer/cidrs/{cidr}` |
| `GET`    | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/peer_cidrs/{cidr_prefix}/{prefix_length}`  | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/peer/cidrs/{cidr}` |
| `PUT`    | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/peer_cidrs/{cidr_prefix}/{prefix_length}`  | `/vpn_gateways/{vpn_gateway_id}/connections/{id}/peer/cidrs/{cidr}` |
{: caption="Methods and their changed paths for API requests that use a version query parameter of 2024-04-30 or later." caption-side="bottom"}

## Changed HTTP response codes when setting VPN gateway connection CIDRs
{: #changed-http-response-vpn-advanced-configuration}

When [setting a local CIDR](/apidocs/vpc/latest#add-vpn-gateway-connections-local-cidr) or [setting a peer CIDR](/apidocs/vpc/latest#add-vpn-gateway-connections-peer-cidr) on a VPN gateway connection, the response now includes a `201` HTTP success code if the CIDR was successfully set or a `204` HTTP success code if the CIDR is already set.

## Action needed
{: #action-needed-vpn-advanced-configuration}

Before you specify the `version` query parameter of `2024-04-30` or later, follow these actions to avoid regressions in client functionality.

If your clients continue to specify version `2023-04-29` or earlier, no changes are required.
{: note}

### Client migration
{: #client-migration-vpn-advanced-configuration}

Before you migrate a client to API version `2024-04-30` or later, review your code that uses the following methods:

- `POST /vpn_gateways/{vpn_gateway_id}/connections`
- `PATCH /vpn_gateways/{vpn_gateway_id}/connections/{id}`
- `GET /vpn_gateways/{vpn_gateway_id}/connections`
- `GET /vpn_gateways/{vpn_gateway_id}/connections/{id}`
- `GET /vpn_gateways/{vpn_gateway_id}/connections/{id}/local_cidrs`
- `DELETE /vpn_gateways/{vpn_gateway_id}/connections/{id}/local_cidrs/{cidr_prefix}/{prefix_length}`
- `GET /vpn_gateways/{vpn_gateway_id}/connections/{id}/local_cidrs/{cidr_prefix}/{prefix_length}`
- `PUT /vpn_gateways/{vpn_gateway_id}/connections/{id}/local_cidrs/{cidr_prefix}/{prefix_length}`
- `GET /vpn_gateways/{vpn_gateway_id}/connections/{id}/peer_cidrs`
- `DELETE /vpn_gateways/{vpn_gateway_id}/connections/{id}/peer_cidrs/{cidr_prefix}/{prefix_length}`
- `GET /vpn_gateways/{vpn_gateway_id}/connections/{id}/peer_cidrs/{cidr_prefix}/{prefix_length}`
- `PUT /vpn_gateways/{vpn_gateway_id}/connections/{id}/peer_cidrs/{cidr_prefix}/{prefix_length}`
- `GET /ike_policies/{id}/connections`
- `GET /ipsec_policies/{id}/connections`

Review the changes that were announced in the [API change log](/docs/vpc?topic=vpc-api-change-log#version-2024-04-30), and verify that your code adopts these changes in a manner that is appropriate for your programming language.

## Examples
{: #examples-vpn-advanced-configuration}

These examples compare differences between before and after the `2024-04-30` versioned change.

### Creating a VPN connection
{: #migration-vpn-advanced-configuration-create-connection-example}

The following example uses API version `2024-04-29` or earlier to create a VPN connection. The `-d` option specifies the `peer_address`, `local_cidrs`, and `peer_cidrs` properties.

```sh
curl -X POST "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections?version=2024-04-29&generation=2"
-H "Authorization: $iam_token"
 -d '{
    "name": "my-vpn-connection-1",
    "psk": "lkj14b1oi0alcniejkso",
    "peer_address": "1.2.3.4",
    "local_cidrs": ["192.168.0.0/24"],
    "peer_cidrs": ["192.168.210.0/24"]
  }'
```
{: pre}

The following example uses API version `2024-04-30` or later to create a VPN connection. The `-d` option specifies the `peer.address`, `local.cidrs`, and `peer.cidrs` properties.

```sh
curl -X POST "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections?version=2024-04-30&generation=2"
-H "Authorization: $iam_token"
 -d '{
    "name": "my-vpn-connection-1",
    "psk": "lkj14b1oi0alcniejkso",
    "local": {
      "cidrs": ["192.168.0.0/24"]
    },
    "peer": {
      "cidrs": ["192.168.210.0/24"],
      "address": "1.2.3.4"
    }
  }'
```
{: pre}

### Updating a VPN connection
{: #migration-vpn-advanced-configuration-update-connection-example}

The following example uses API version `2024-04-29` or earlier to update a VPN connection. The `-d` option specifies the `peer_address` property.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id?version=2024-04-29&generation=2"
-H "Authorization: $iam_token"
 -d '{
    "peer_address": "1.2.3.4"
  }'
```
{: pre}

The following example uses API version `2024-04-30` or later to update a VPN connection. The `-d` option specifies the `peer.address` property.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id?version=2024-04-30&generation=2"
-H "Authorization: $iam_token"
 -d '{
    "peer": {
      "address": "1.2.3.4"
    }
  }'
```
{: pre}

### Listing a VPN connection
{: #migration-vpn-advanced-configuration-list-connections-example}

The following example uses API version `2024-04-29` or earlier to list a VPN connection.

```sh
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections?version=2024-04-29&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The response includes the `peer_address`, `local_cidrs`, and `peer_cidrs` properties:

```json
{
  "connections": [
    {
      "admin_state_up": true,
      "authentication_mode": "psk",
      "created_at": "2018-12-13T19:40:12.124082Z",
      "dead_peer_detection": {
        "action": "none",
        "interval": 15,
        "timeout": 30
      },
      "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/a7d258d5-be1e-491d-83db-526d8d9a2ce9/connections/52f69dc3-6a5c-4bcf-b264-e7fae279b15c",
      "id": "52f69dc3-6a5c-4bcf-b264-e7fae279b15c",
      "local_cidrs": [
        "192.0.2.50/24"
      ],
      "mode": "policy",
      "name": "my-vpn-connection-1",
      "peer_address": "192.0.2.5",
      "peer_cidrs": [
        "192.0.2.40/24"
      ],
      "psk": "lkj14b1oi0alcniejkso",
      "resource_type": "vpn_gateway_connection",
      "status": "down",
      "status_reasons": []
    },
    {
      "admin_state_up": true,
      "authentication_mode": "psk",
      "created_at": "2018-12-13T19:39:47.938464Z",
      "dead_peer_detection": {
        "action": "none",
        "interval": 15,
        "timeout": 30
      },
      "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/a7d258d5-be1e-491d-83db-526d8d9a2ce9/connections/b67efb2c-bd17-457d-be8e-7b46404062dc",
      "id": "b67efb2c-bd17-457d-be8e-7b46404062dc",
      "ike_policy": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/ike_policies/e98f46a3-1e4e-4195-b4e5-b8155192689d",
        "id": "e98f46a3-1e4e-4195-b4e5-b8155192689d",
        "name": "my-ike-policy",
        "resource_type": "ike_policy"
      },
      "ipsec_policy": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/ipsec_policies/43c2f663-3960-4289-9253-f6eab23a6cd7",
        "id": "43c2f663-3960-4289-9253-f6eab23a6cd7",
        "name": "my-ipsec-policy",
        "resource_type": "ipsec_policy"
      },
      "local_cidrs": [
        "192.0.2.50/24"
      ],
      "mode": "policy",
      "name": "my-vpn-connection-2",
      "peer_address": "192.0.2.5",
      "peer_cidrs": [
        "192.0.2.40/24"
      ],
      "psk": "lkj14b1oi0alcniejkso",
      "resource_type": "vpn_gateway_connection",
      "status": "down",
      "status_reasons": []
    }
  ]
}
```
{: pre}

The following example uses API version `2024-04-30` or later to list a VPN connection.

```sh
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections?version=2024-04-30&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The response includes the `peer.address`, `local.cidrs`, and `peer.cidrs` properties:

```json
{
  "connections": [
    {
      "admin_state_up": true,
      "authentication_mode": "psk",
      "created_at": "2018-12-13T19:40:12.124082Z",
      "dead_peer_detection": {
        "action": "none",
        "interval": 15,
        "timeout": 30
      },
      "establish_mode": "peer_only",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/a7d258d5-be1e-491d-83db-526d8d9a2ce9/connections/52f69dc3-6a5c-4bcf-b264-e7fae279b15c",
      "id": "52f69dc3-6a5c-4bcf-b264-e7fae279b15c",
      "local": {
        "cidrs": [
          "192.0.2.0/24"
        ],
        "ike_identities": [
          {
            "type": "ipv4_address",
            "value": "192.0.2.4"
          }
        ]
      },
      "mode": "policy",
      "name": "my-vpn-connection-1",
      "peer": {
        "address": "192.0.2.5",
        "cidrs": [
          "192.0.3.0/24"
        ],
        "ike_identity": {
          "type": "ipv4_address",
          "value": "192.0.2.5"
        },
        "type": "address"
      },
      "psk": "lkj14b1oi0alcniejkso",
      "resource_type": "vpn_gateway_connection",
      "status": "down",
      "status_reasons": []
    },
    {
      "admin_state_up": true,
      "authentication_mode": "psk",
      "created_at": "2018-12-13T19:39:47.938464Z",
      "dead_peer_detection": {
        "action": "none",
        "interval": 15,
        "timeout": 30
      },
      "establish_mode": "bidirectional",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/a7d258d5-be1e-491d-83db-526d8d9a2ce9/connections/b67efb2c-bd17-457d-be8e-7b46404062dc",
      "id": "b67efb2c-bd17-457d-be8e-7b46404062dc",
      "ike_policy": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/ike_policies/e98f46a3-1e4e-4195-b4e5-b8155192689d",
        "id": "e98f46a3-1e4e-4195-b4e5-b8155192689d",
        "name": "my-ike-policy",
        "resource_type": "ike_policy"
      },
      "ipsec_policy": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/ipsec_policies/43c2f663-3960-4289-9253-f6eab23a6cd7",
        "id": "43c2f663-3960-4289-9253-f6eab23a6cd7",
        "name": "my-ipsec-policy",
        "resource_type": "ipsec_policy"
      },
      "local": {
        "cidrs": [
          "192.0.2.0/24"
        ],
        "ike_identities": [
          {
            "type": "ipv4_address",
            "value": "192.0.2.4"
          }
        ]
      },
      "mode": "policy",
      "name": "my-vpn-connection-2",
      "peer": {
        "address": "192.0.2.5",
        "cidrs": [
          "192.0.3.0/24"
        ],
        "ike_identity": {
          "type": "ipv4_address",
          "value": "192.0.2.5"
        },
        "type": "address"
      },
      "psk": "lkj14b1oi0alcniejkso",
      "resource_type": "vpn_gateway_connection",
      "status": "down",
      "status_reasons": []
    }
  ]
}
```
{: pre}

### Getting a VPN connection
{: #migration-vpn-advanced-configuration-get-connection-example}

The following example uses API version `2024-04-29` or earlier to get a VPN connection.

```sh
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id?version=2024-04-29&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The response includes the `peer_address`, `local_cidrs`, and `peer_cidrs` properties:

```json
{
  "admin_state_up": true,
  "authentication_mode": "psk",
  "created_at": "2018-12-13T19:40:12.124082Z",
  "dead_peer_detection": {
    "action": "none",
    "interval": 15,
    "timeout": 30
  },
  "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/a7d258d5-be1e-491d-83db-526d8d9a2ce9/connections/52f69dc3-6a5c-4bcf-b264-e7fae279b15c",
  "id": "52f69dc3-6a5c-4bcf-b264-e7fae279b15c",
  "local_cidrs": [
    "192.0.2.50/24"
  ],
  "mode": "policy",
  "name": "my-vpn-connection-1",
  "peer_address": "192.0.2.5",
  "peer_cidrs": [
    "192.0.2.40/24"
  ],
  "psk": "lkj14b1oi0alcniejkso",
  "resource_type": "vpn_gateway_connection",
  "status": "down",
  "status_reasons": []
}
```
{: pre}

The following example uses API version `2024-04-30` or later to get a VPN connection.

```sh
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id?version=2024-04-30&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The response includes the `peer.address`, `local.cidrs`, and `peer.cidrs` properties:

```json
{
  "admin_state_up": true,
  "authentication_mode": "psk",
  "created_at": "2018-12-13T19:40:12.124082Z",
  "dead_peer_detection": {
    "action": "none",
    "interval": 15,
    "timeout": 30
  },
  "establish_mode": "bidirectional",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/a7d258d5-be1e-491d-83db-526d8d9a2ce9/connections/52f69dc3-6a5c-4bcf-b264-e7fae279b15c",
  "id": "52f69dc3-6a5c-4bcf-b264-e7fae279b15c",
  "local": {
    "cidrs": [
      "192.0.2.0/24"
    ],
    "ike_identities": [
      {
        "type": "ipv4_address",
        "value": "192.0.2.4"
      }
    ]
  },
  "mode": "policy",
  "name": "my-vpn-connection-1",
  "peer": {
    "address": "192.0.2.5",
    "cidrs": [
      "192.0.3.0/24"
    ],
    "ike_identity": {
      "type": "ipv4_address",
      "value": "192.0.2.5"
    },
    "type": "address"
  },
  "psk": "lkj14b1oi0alcniejkso",
  "resource_type": "vpn_gateway_connection",
  "status": "down",
  "status_reasons": []
}
```
{: pre}

### Listing local CIDRs
{: #migration-vpn-advanced-configuration-list-local-cidrs-example}

The following example uses API version `2024-04-29` or earlier to list local CIDRs.

```sh
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/local_cidrs?version=2024-04-29&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The response has the `local_cidrs` property:

```json
{
  "local_cidrs": [
    "192.168.19.0/24"
  ]
}
```
{: pre}

The following example uses API version `2024-04-30` or later to list local CIDRs.

```sh
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/local/cidrs?version=2024-04-30&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The response has the `cidrs` property:

```json
{
  "cidrs": [
    "192.168.19.0/24"
  ]
}
```
{: pre}

### Removing a local CIDR
{: #migration-vpn-advanced-configuration-remove-local-cidr-example}

The following example uses API version `2024-04-29` or earlier to remove a local CIDR.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/local_cidrs/$cidr_prefix/$prefix_length?version=2024-04-29&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The following example uses API version `2024-04-30` or later to remove a local CIDR.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/local/cidrs/$cidr?version=2024-04-30&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

### Getting a local CIDR
{: #migration-vpn-advanced-configuration-get-local-cidr-example}

The following example uses API version `2024-04-29` or earlier to get a local CIDR.

```sh
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/local_cidrs/$cidr_prefix/$prefix_length?version=2024-04-29&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The success response code is `204`.

The following example uses API version `2024-04-30` or later to get a local CIDR.

```sh
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/local/cidrs/$cidr?version=2024-04-30&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The success response code is `204`.

### Setting a local CIDR
{: #migration-vpn-advanced-configuration-set-local-cidr-example}

The following example uses API version `2024-04-29` or earlier to set a local CIDR.

```sh
curl -X PUT "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/local_cidrs/$cidr_prefix/$prefix_length?version=2024-04-29&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The success response code is `204`.

The following example uses API version `2024-04-30` or later to set a local CIDR.

```sh
curl -X PUT "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/local/cidrs/$cidr?version=2024-04-30&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The response code is `201` if set successfully or `204` if the CIDR exists already.

### Listing peer CIDRs
{: #migration-vpn-advanced-configuration-list-peer-cidrs-example}

The following example uses API version `2024-04-29` or earlier to list peer CIDRs.

```sh
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/peer_cidrs?version=2024-04-29&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The response has the `peer_cidrs` property:

```json
{
  "peer_cidrs": [
    "192.0.2.40/24"
  ]
}
```
{: pre}

The following example uses API version `2024-04-30` or later to list peer CIDRs.

```sh
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/peer/cidrs?version=2024-04-30&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The response has the `cidrs` property:

```json
{
  "cidrs": [
    "192.168.20.0/24"
  ]
}
```
{: pre}

### Removing a peer CIDR
{: #migration-vpn-advanced-configuration-remove-peer-cidr-example}

The following example uses API version `2024-04-29` or earlier to remove a peer CIDR.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/peer_cidrs/$cidr_prefix/$prefix_length?version=2024-04-29&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The following example uses API version `2024-04-30` or later to remove a peer CIDR.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/peer/cidrs/$cidr?version=2024-04-30&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

### Getting a peer CIDR
{: #migration-vpn-advanced-configuration-get-peer-cidr-example}

The following example uses API version `2024-04-29` or earlier to get a peer CIDR.

```sh
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/peer_cidrs/$cidr_prefix/$prefix_length?version=2024-04-29&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The success response code is `204`.

The following example uses API version `2024-04-30` or later to get a peer CIDR.

```sh
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/peer/cidrs/$cidr?version=2024-04-30&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The success response code is `204`.

### Setting a peer CIDR
{: #migration-vpn-advanced-configuration-set-peer-cidr-example}

The following example uses API version `2024-04-29` or earlier to set a peer CIDR.

```sh
curl -X PUT "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/peer_cidrs/$cidr_prefix/$prefix_length?version=2024-04-29&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The success response code is `204`.

The following example uses API version `2024-04-30` or later to set a peer CIDR.

```sh
curl -X PUT "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/connections/$id/peer/cidrs/$cidr?version=2024-04-30&generation=2"
-H "Authorization: $iam_token"
```
{: pre}

The response code is `201` if set successfully or `204` if the CIDR exists already.
