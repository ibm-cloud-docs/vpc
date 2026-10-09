---

copyright:
  years: 2026
lastupdated: "2026-10-09"

keywords: VPN, VPN gateway, regional VPN, API migration, subnet, availability mode, members

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

[Regional VPN start]{: tag-purple}

# Migration considerations for updating to regional VPN
{: #vpn-gateway-regional-migration-update}

Regional VPN gateways introduce schema changes that can affect integrations that read subnet information from VPN gateway responses. If your scripts, automation, CLI or API clients, or SDK code use the `vpn_gateway.subnet` property, update them before creating or migrating to a regional VPN gateway.
{: shortdesc}

## Before you begin
{: #vpn-regional-api-migration-prereqs}

Review the following information before you create or migrate a VPN gateway to regional mode.

- Update to the latest supported API, CLI, SDK, and Terraform release before creating or migrating to a regional VPN gateway.
- If your environment has an integration that reads the `vpn_gateway.subnet` property from the API response, update the integration before creating a regional VPN gateway. Regional VPN gateways do not return a top-level `subnet` field. Instead, read subnet information from `members[].private_ip.subnet`.
- If you use the CLI to inspect VPN gateways, the `Subnet` field shows `-` for regional gateways in `ibmcloud is vpn-gateway` output. Subnet information for each member is shown in the `Members` table under the `Subnet` column.
- If you use the VPC SDK to read subnet information from VPN gateway responses, update your integration to check `availability_mode` and the member-level `private_ip.subnet` field before reading the top-level `subnet` field.
- Migration from zonal to regional mode is a one-way change and is supported only through the API. Changing a regional gateway back to zonal mode, and migrating with the CLI or Terraform, is not supported.

## API schema changes for regional VPN gateways
{: #vpn-regional-api-schema-changes}
{: api}

Regional VPN gateways introduce the following API changes:

- A new `availability_mode` property is returned in all VPN gateway API responses. The value is `zonal` for existing gateways and `regional` for regional gateways.

  ```json
  {
    "id": "0717-ddf51bec-...",
    "name": "my-vpn-gateway",
    "availability_mode": "zonal"
  }
  ```
  {: screen}

  ```json
  {
    "id": "0717-ddf51bec-...",
    "name": "my-vpn-gateway",
    "availability_mode": "regional"
  }
  ```
  {: screen}

- For zonal VPN gateways, subnet information is returned in the top-level `subnet` property. For regional VPN gateways, the top-level `subnet` property is not returned. Instead, subnet information is available in each member's `private_ip.subnet` property (`members[].private_ip.subnet`).

  **Zonal gateway response:**

  ```json
  {
    "id": "0717-ddf51bec-...",
    "name": "my-vpn-gateway",
    "availability_mode": "zonal",
    "subnet": {
      "id": "0717-7ec86020-...",
      "name": "my-subnet",
      "resource_type": "subnet"
    }
  }
  ```
  {: screen}

  **Regional gateway response:**

  ```json
  {
    "id": "0717-abc12345-...",
    "name": "my-regional-vpn-gateway",
    "availability_mode": "regional"
  }
  ```
  {: screen}

- Subnet information for regional gateways must be read from each gateway member by using the `members[].private_ip.subnet` property.

  ```json
  {
    "members": [
      {
        "id": "0717-9dce0ab3-...",
        "private_ip": {
          "address": "10.240.0.4",
          "subnet": {
            "id": "0717-7ec86020-...",
            "name": "my-subnet-zone-1",
            "resource_type": "subnet"
          }
        }
      },
      {
        "id": "0717-3baf12cd-...",
        "private_ip": {
          "address": "10.241.0.5",
          "subnet": {
            "id": "0717-c4e5d6f7-...",
            "name": "my-subnet-zone-2",
            "resource_type": "subnet"
          }
        }
      }
    ]
  }
  ```
  {: screen}

## Handling API responses for zonal and regional gateways
{: #vpn-regional-api-availability-mode}
{: api}

The `availability_mode` property distinguishes between zonal and regional VPN gateways in API responses. Review the following behavior before updating your API integrations.

Zonal gateways
:   When `availability_mode` is `zonal`, the response includes the top-level `subnet` property, and the `members` array is embedded inline in the gateway response. Existing automation or scripts that read `gateway.subnet` continue to work for zonal gateways without changes.

Regional gateways
:   When `availability_mode` is `regional`, the top-level `subnet` property is absent from the API response. Each member contains a `private_ip.subnet` object with the subnet details for that member. Subnet information can be retrieved either from the inline `members` array in `GET /vpn_gateways/{id}` or by calling `GET /vpn_gateways/{id}/members`.

  If your integration currently reads `gateway.subnet`, update it to read the subnet from the appropriate member when `availability_mode` is `regional`.

Regional VPN gateway member operations
:   The following API operations are available for regional VPN gateway members:

  | Method | Path | Operation ID |
  | --- | --- | --- |
  | `GET` | `/vpn_gateways/{vpn_gateway_id}/members` | `list_vpn_gateway_members` |
  | `GET` | `/vpn_gateways/{vpn_gateway_id}/members/{id}` | `get_vpn_gateway_member` |
  | `PUT` | `/vpn_gateways/{vpn_gateway_id}/members/{id}` | `replace_vpn_gateway_member` |
  {: caption="Regional VPN gateway member operations" caption-side="bottom"}

  The `replace_vpn_gateway_member` operation is not available for zonal gateways.
  {: note}

## API response examples
{: #vpn-regional-api-response-examples}
{: api}

The following examples show how API responses differ depending on the gateway's `availability_mode` and status.

### Case 1: Zonal gateway
{: #vpn-regional-api-example-zonal}
{: api}

For a zonal gateway, the `GET /vpn_gateways/{id}` response includes the top-level `subnet` field and the `members` array inline. Both the top-level `subnet` and `members[].private_ip.subnet` reference the same subnet because a zonal gateway has only one subnet.

- The following example shows the API request:

  ```sh
  GET /v1/vpn_gateways/{id}
  ```
  {: codeblock}

- The following example shows the response with the relevant fields:

  ```json
  {
    "id": "0717-ddf51bec-...",
    "name": "my-zonal-vpn",
    "availability_mode": "zonal",
    "subnet": {
      "id": "0717-7ec86020-...",
      "name": "my-subnet",
      "resource_type": "subnet"
    },
    "members": [
      {
        "role": "active",
        "private_ip": {
          "address": "10.240.0.4",
          "subnet": {
            "id": "0717-7ec86020-...",
            "name": "my-subnet",
            "resource_type": "subnet"
          }
        },
        "health_state": "ok",
        "lifecycle_state": "stable"
      }
    ]
  }
  ```
  {: screen}

### Case 2: Regional gateway
{: #vpn-regional-api-example-regional}
{: api}

For a regional gateway, the API response to `GET /vpn_gateways/{id}` does not include a top-level `subnet` field. Subnet information for each member is available either from the inline `members` array in the gateway response or by calling `GET /vpn_gateways/{id}/members`. Each member returns its own `private_ip.subnet` because members in a regional gateway can reside in different subnets across zones.

- The following example shows the API request for the VPN gateway:

  ```sh
  GET /v1/vpn_gateways/{id}
  ```
  {: codeblock}

- The following example shows the response. Notice that no top-level `subnet` field exists and subnet information is available inline from each member:

  ```json
  {
    "id": "0717-abc12345-...",
    "name": "my-regional-vpn",
    "availability_mode": "regional",
    "members": [
      {
        "id": "0717-9dce0ab3-...",
        "role": "active",
        "private_ip": {
          "address": "10.240.0.4",
          "subnet": {
            "id": "0717-7ec86020-...",
            "name": "my-subnet-zone-1",
            "resource_type": "subnet"
          }
        },
        "health_state": "ok",
        "lifecycle_state": "stable"
      },
      {
        "id": "0717-3baf12cd-...",
        "role": "standby",
        "private_ip": {
          "address": "10.241.0.5",
          "subnet": {
            "id": "0717-c4e5d6f7-...",
            "name": "my-subnet-zone-2",
            "resource_type": "subnet"
          }
        },
        "health_state": "ok",
        "lifecycle_state": "stable"
      }
    ]
  }
  ```
  {: screen}

- Alternatively, the following example shows the request to retrieve the same subnet information by calling the members endpoint directly:

  ```sh
  GET /v1/vpn_gateways/{id}/members
  ```
  {: codeblock}

- The following example shows the response with each member's subnet details:

  ```json
  {
    "members": [
      {
        "id": "0717-9dce0ab3-...",
        "role": "active",
        "private_ip": {
          "address": "10.240.0.4",
          "subnet": {
            "id": "0717-7ec86020-...",
            "name": "my-subnet-zone-1",
            "resource_type": "subnet"
          }
        },
        "health_state": "ok",
        "lifecycle_state": "stable"
      },
      {
        "id": "0717-3baf12cd-...",
        "role": "standby",
        "private_ip": {
          "address": "10.241.0.5",
          "subnet": {
            "id": "0717-c4e5d6f7-...",
            "name": "my-subnet-zone-2",
            "resource_type": "subnet"
          }
        },
        "health_state": "ok",
        "lifecycle_state": "stable"
      }
    ]
  }
  ```
  {: screen}

- The following example shows the request to retrieve subnet information for a specific member:

  ```sh
  GET /v1/vpn_gateways/{id}/members/{member_id}
  ```
  {: codeblock}

- The following example shows the response with the subnet details for that member:

  ```json
  {
    "id": "0717-9dce0ab3-...",
    "role": "active",
    "private_ip": {
      "address": "10.240.0.4",
      "subnet": {
        "id": "0717-7ec86020-...",
        "name": "my-subnet-zone-1",
        "resource_type": "subnet"
      }
    },
    "health_state": "ok",
    "lifecycle_state": "stable"
  }
  ```
  {: screen}

### Case 3: Regional gateway in pending status
{: #vpn-regional-api-example-pending}
{: api}

When a gateway is being provisioned, a private IP address is not assigned to its members. In this case, the IP address `0.0.0.0` is returned in the response. To get the actual value, read the `private_ip` after the gateway status becomes available.

- The following example shows the request to retrieve members while the gateway is being provisioned:

  ```sh
  GET /v1/vpn_gateways/{id}/members
  ```
  {: codeblock}

- The following example shows the response when the gateway is in pending status. Notice that the `private_ip` address is `0.0.0.0` until provisioning is complete.

  ```json
  {
    "members": [
      {
        "id": "0717-9dce0ab3-...",
        "role": "active",
        "health_state": "inapplicable",
        "health_reasons": [],
        "lifecycle_state": "pending",
        "private_ip": {
          "address": "0.0.0.0",
          "subnet": {
            "id": "0717-7ec86020-...",
            "name": ""
          }
        }
      }
    ]
  }
  ```
  {: screen}

## CLI changes for regional VPN gateways
{: #vpn-regional-cli-changes}
{: cli}

The `availability_mode` field is now shown in all `ibmcloud is vpn-gateway` and `ibmcloud is vpn-gateways` output and is the primary differentiator between zonal and regional VPN gateways. Review the following behavior before working with regional VPN gateways from the CLI.

Zonal gateways
:   When `Availability Mode` is `zonal`, the `Subnet` field in the CLI shows the associated subnet ID and name. Each member row in the `Members` table also shows the subnet under the `Subnet` column. Both reference the same subnet.

Regional gateways
:   When `Availability Mode` is `regional`, the `Subnet` field shows `-`. Subnet information is not available as a single top-level field. Each member row in the `Members` table shows its own `Subnet` value because members in a regional gateway can reside in different subnets across zones.

Regional VPN gateway member CLI commands
:   The following CLI commands are available for regional VPN gateway members:

  | Command | Description |
  | --- | --- |
  | `ibmcloud is vpn-gateway-members VPN_GATEWAY` | List all members of a VPN gateway. |
  | `ibmcloud is vpn-gateway-member VPN_GATEWAY --member MEMBER` | Get details for a specific member. |
  | `ibmcloud is vpn-gateway-member-replace VPN_GATEWAY --member MEMBER --private-ip-subnet SUBNET` | Move a member to a different subnet. |
  {: caption="CLI commands for regional VPN gateway members" caption-side="bottom"}

  The `ibmcloud is vpn-gateway-member-replace` command is not available for zonal gateways.
  {: note}

## CLI output examples
{: #vpn-regional-cli-output-examples}
{: cli}

The following examples show how CLI output differs depending on the gateway's `Availability Mode` and status.

### Case 1: Zonal gateway
{: #vpn-regional-cli-example-zonal}
{: cli}

For a zonal gateway, the `ibmcloud is vpn-gateway` output includes a `Subnet` field and each member row in the `Members` table shows the associated subnet. Both reference the same subnet because a zonal gateway has only one subnet.

- The following example shows the CLI command:

```sh
ibmcloud is vpn-gateway my-vpn-gw2
```
{: pre}

- The following example shows the output. Notice that the `Subnet` field is present and each member row includes a `Subnet` column:

```text
ID                  0717-ca20c7ca-5bc0-4d6b-92ce-368720685022
Name                my-vpn-gw2
Mode                route
Availability Mode   zonal
Subnet              ID                                          Name
                    0717-43c9a989-1705-4b4a-806f-edf4677dcbda   my-subnet

Members             ID       Public IP        Reserved IP Address   Subnet        Role
                    8393b79f-913a-4f19-a622-a23381db43f1   174.37.174.147   10.240.0.6 0717-43c9a989-1705-4b4a-806f-edf4677dcbda   active
                    8b07cb71-094f-4473-b8d4-67ea312dd1d6   52.116.143.104   10.240.0.7 0717-43c9a989-1705-4b4a-806f-edf4677dcbda   active

Lifecycle state     stable
Health state        ok
```
{: screen}

### Case 2: Regional gateway
{: #vpn-regional-cli-example-regional}
{: cli}

For a regional gateway, the `Subnet` field shows `-` because no single top-level subnet exists. Each member row in the `Members` table shows its own `Subnet` value.

- The following example shows the CLI command:

```sh
ibmcloud is vpn-gateway my-vpn-gw
```
{: pre}

- The following example shows the output. Notice that the top-level `Subnet` field shows `-`, and each member row shows its own `Subnet` column:

```text
ID                  r006-496ff88a-b113-40ee-8cd8-5e92625629d2
Name                my-vpn-gw
Mode                route
Availability Mode   regional
Subnet              -

Members             ID        Public IP         Reserved IP Address   Subnet     Role
                    cba439bf-4bc9-4237-ac85-420456fdd7de   150.240.164.11    10.240.0.4 0717-43c9a989-1705-4b4a-806f-edf4677dcbda   active
                    6b46f779-8f05-4fce-82bc-80d421b7fb6a   150.240.238.157   10.240.64.4 0727-2666c554-f209-48c4-b0fc-c4cf3f23a629   active

Lifecycle state     stable
Health state        ok
```
{: screen}

- To retrieve member details, use the `ibmcloud is vpn-gateway-members` command. The following example shows the CLI command to retrieve all gateway members:

```sh
ibmcloud is vpn-gateway-members my-vpn-gw --vpc my-vpc
```
{: pre}

- The following example shows the output with the member IDs:

```text
ID                                     Private IP    Public IP         Role     Health state   Lifecycle state
cba439bf-4bc9-4237-ac85-420456fdd7de   10.240.65.4   52.116.193.73     active   ok             stable
6b46f779-8f05-4fce-82bc-80d421b7fb6a   10.240.64.4   150.240.238.157   active   ok             stable
```
{: screen}

- To retrieve subnet details for a specific member, use the `ibmcloud is vpn-gateway-member` command. The following example shows the CLI command:

```sh
ibmcloud is vpn-gateway-member my-vpn-gw --member cba439bf-4bc9-4237-ac85-420456fdd7de --vpc my-vpc
```
{: pre}

The following example shows the output with the member's subnet details:

```text
ID                 cba439bf-4bc9-4237-ac85-420456fdd7de
Private IP         ID                                          Name                               Address       Subnet
                   0727-b1b02a1c-f958-4b68-bd1e-7a90097abc66   revenge-trio-numerator-cornflake   10.240.65.4   0727-b27f6d11-8a31-4eda-ab22-106f22c8ba42

Role               active
Health state       ok
Public IP          52.116.193.73
Lifecycle state    stable
```
{: screen}

### Case 3: Regional gateway member in pending status
{: #vpn-regional-cli-example-pending}
{: cli}

When a member's subnet is being updated, the `Private IP` address shows `0.0.0.0` and `Lifecycle state` shows `pending` until provisioning is complete. To get the actual value, read the `Private IP` after the member's `Lifecycle state` is `stable`.

- The following example shows the CLI command to replace a member's subnet:

```sh
ibmcloud is vpn-gateway-member-replace my-vpn-gw --member cba439bf-4bc9-4237-ac85-420456fdd7de --private-ip-subnet 0727-b27f6d11-8a31-4eda-ab22-106f22c8ba42 --vpc my-vpc
```
{: pre}

- The following example shows the output immediately after the `replace` operation, while the member is still being provisioned:

```text
ID                 cba439bf-4bc9-4237-ac85-420456fdd7de
Private IP         ID   Name   Address   Subnet
                   -    -      0.0.0.0   0727-b27f6d11-8a31-4eda-ab22-106f22c8ba42

Role               active
Health state       inapplicable
Public IP          0.0.0.0
Lifecycle state    pending
```
{: screen}

## Related links
{: #vpn-regional-api-migration-related}

- [Move a regional VPN gateway member to a different subnet](/docs/vpc?topic=vpc-vpn-update-regional-member-subnet) - Learn how to move a gateway member to a different subnet within the same zone or across zones.
- [Create a VPN gateway](/docs/vpc?topic=vpc-vpn-create-gateway) - Learn how to create a zonal or regional VPN gateway.
- [VPN gateway API reference](/docs/apis/vpc/latest#list-vpn-gateways) - View the full API reference for VPN gateway operations, including the new member endpoints.

[Regional VPN end]{: tag-purple}
