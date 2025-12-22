---

copyright:
  years: 2025
lastupdated: "2025-12-09"

keywords: security group rules, network acl rules, protocol, api migration, versioned change

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to the `2025-12-09` version (security group and network ACL rules)
{: #2025-12-09-migration-security-group-network-acl-rules}

**Warning**: See [Networking known issues](/docs/vpc?topic=vpc-known-issues#networking-vpc-known-issues) before creating a rule with a previously unsupported `protocol` value. Do NOT use these new `protocol` values in your production environments until further notice. Creating a rule with these new protocol values anywhere in your VPC can break your existing Kubernetes and OpenShift clusters. They can also break any other components that make security group or network ACL API requests via an SDK, or via a custom API client that does not handle these new protocol values properly. Work is ongoing to resolve these issues. Until the known issues have been resolved, the UI and the current latest version of CLI will not allow rules with previously unsupported `protocol` values to be created.
{: important}

As described in the VPC API reference [versioning](/apidocs/vpc/latest#api-versioning) policy, most changes to the VPC APIs are fully backward compatible and are made available to all clients, regardless of the API version the client requests. However, the `2025-12-09` release of the VPC API necessitated incompatible changes in support of any protocol in security group and network ACL rules.

Before you adopt the release version `2025-12-09` or later, review the changes described in this migration guidance that might require you to update your client.

## Changed values for the `protocol` property in security group and network ACL rules
{: #changed-property-values-security-group-network-acl-api-version-2025-12-09}

The values for the `protocol` property in security group rules and network ACL rules have changed for API requests that use a version query parameter of `2025-12-09` or later.

| Old value | New value      |
|-----------|----------------|
| `all`     | `icmp_tcp_udp` |
{: caption="Old and new values for the protocol property in security group and network ACL rules" caption-side="bottom"}

Note that only one of the allowed values for `protocol` differs between the old and new API versions:

| API version             | Allowed values for the `protocol` property              |
|-------------------------|---------------------------------------------------------|
| `2025-12-08` or earlier | `any`, `all`, `icmp`, `tcp`, `udp`, `gre`, ...          |
| `2025-12-09` or later   | `any`, `icmp_tcp_udp`, `icmp`, `tcp`, `udp`, `gre`, ... |
{: caption="Old and new allowed values for the protocol property in security group and network ACL rules" caption-side="bottom"}

When security groups and network ACLs were first introduced, they supported the ICMP, TCP, and UDP protocols only. At that time, the value `all` signified "all supported protocols". Now that [any protocol number](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml){: external} is supported, the value `all` is misleading, but is kept in earlier versions of the API for client compatibility. In later versions of the API, the value `all` is replaced with `icmp_tcp_udp`.
{: note}

### Client migration
{: #client-migration-security-group-network-acl-api-version-2025-12-09}

Before you migrate a client to API version `2025-12-09` or later, review your code that uses the following security group methods to create rules:

- `POST /security_groups` (with `rules` provided in the request body)
- `POST /security_groups/{security-group-id}/rules`

Replace the `protocol` property value `all` with the value `icmp_tcp_udp` if the security group rule is intended to allow these three protocols.

Likewise, check your API client code for the following network ACL methods that create rules with an `action` value of `allow`:

- `POST /network_acls` (with `rules` provided in the request body)
- `POST /network_acls/{network-acl-id}/rules`

Replace the `protocol` property value `all` with the value `icmp_tcp_udp` if the network ACL rule is intended to allow these three protocols.

Check your API client code for  the following network ACL methods that create rules with an `action` value of `deny`:

- `POST /network_acls` (with `rules` provided in the request body)
- `POST /network_acls/{network-acl-id}/rules`

Replace the `protocol` property value `all` with the value `any` if the network ACL rule is intended to deny any protocol.

If your API client code has logic to update the `action` property of network ACL rules, see the [Hazardous migration considerations for updating the `action` property of network ACL rules](#changed-property-values-hazardous-network-acl-deny-api-version-2025-12-09) below.

Review your code for the following security group and network ACL methods that list or retrieve rules:

- `GET /security_groups`
- `GET /security_groups/{id}`
- `GET /security_groups/{security_group_id}/rules`
- `GET /security_groups/{security_group_id}/rules/{id}`
- `GET /network_acls`
- `GET /network_acls/{id}`
- `GET /network_acls/{network_acl_id}/rules`
- `GET /network_acls/{network_acl_id}/rules/{id}`

Update your client code to look for the `protocol` property value `icmp_tcp_udp` instead of the value `all`.

Review the changes that were announced in the [API change log](/docs/vpc?topic=vpc-api-change-log#9-december-2025), and verify that your code adopts these changes in a manner that is appropriate for your programming language.

## Hazardous migration considerations for updating the `action` property of network ACL rules
{: #changed-property-values-hazardous-network-acl-deny-api-version-2025-12-09}

Check your API client code for logic that updates the `action` property of network ACL rules using the patch method:

- `PATCH /network_acls/{network-acl-id}/rules/{id}`

If your existing API client code, using an API version of `2025-12-08` or earlier, updates a network ACL rule to change the `action` property from `allow` to `deny`, then carefully evaluate the following scenarios.

### 1. Updating a network ACL rule from "allow all" to "deny all"
{: #updating-action-to-deny-hazardous-network-acl-deny-api-version-2025-12-09}

The value `all` is no longer supported for the `protocol` property in API version of `2025-12-08` or later. If the rule was created with an earlier API version, the `protocol` value will be `icmp_tcp_udp` in API version `2025-12-09` or later. When a rule's `protocol` is `icmp_tcp_udp`, the `action` property cannot be updated to `deny`.

Evaluate the intent of your API client logic that updates a network ACL rule's `action` property from `allow` to `deny` in this scenario, and consider options such as:

- Delete the rule. This will result in ICMP, TCP, and UDP traffic no longer being allowed unless they are allowed by other rules in the network ACL.
- Delete the rule and create three new rules in its place with an `action` of `deny` and `protocol` values of `icmp`, `tcp`, and `udp`.
- Delete the rule and create a new rule in its place with an `action` of `deny` and a `protocol` of `any`.

### 2. Updating a network ACL rule from "deny all" to "allow all"
{: #updating-action-to-allow-hazardous-network-acl-deny-api-version-2025-12-09}

The value `all` is no longer supported for the `protocol` property in API version of `2025-12-09` or later. In API version `2025-12-08` or earlier, a network ACL rule with an `action` of `deny` and a `protocol` of `all` denies any traffic. If the rule was created with an earlier API version, the `protocol` value will be `any` in API version `2025-12-09` or later.

Evaluate the intent of your API client logic that updates a rule's `action` property from `deny` to `allow` in this scenario:

- If the intent is to allow only ICMP, TCP, and UDP traffic, delete the current rule and create a new rule in its place with an `action` of `allow` and a `protocol` of `icmp_tcp_udp`.
- If the intent is to allow any traffic, the existing API client logic can be used.

## Examples
{: #example-security-group-network-acl-api-version-2025-12-09}

These examples compare differences between before and after the `2025-12-09` versioned change.

### Creating a security group rule to allow ICMP, TCP, and UDP traffic
{: #creating-security-group-rule-icmp-tcp-udp-2025-12-09}

The following example uses API version `2025-12-08` or earlier to create a security group rule that allows ICMP, TCP, and UDP inbound traffic to a local address. The data object specifies a `protocol` value of `all`.

```sh
curl -X POST \
  --url '$vpc_api_endpoint/v1/security_groups/$security_group_id/rules?version=2025-12-08&generation=2' \
  --header 'Authorization: Bearer $iam_token' \
  --header 'Content-Type: application/json' \
  --data '{
      "direction": "inbound",
      "local": {
        "address": "192.168.11.12"
      },
      "protocol": "all"
    }'
```
{: codeblock}

The following example uses API version `2025-12-09` or later to create a security group rule that allows ICMP, TCP, and UDP inbound traffic to a local address. The data object specifies a `protocol` value of `icmp_tcp_udp`.

```sh
curl -X POST \
  --url '$vpc_api_endpoint/v1/security_groups/$security_group_id/rules?version=2025-12-09&generation=2' \
  --header 'Authorization: Bearer $iam_token' \
  --header 'Content-Type: application/json' \
  --data '{
      "direction": "inbound",
      "local": {
        "address": "192.168.11.12"
      },
      "protocol": "icmp_tcp_udp"
    }'
```
{: codeblock}

### Creating a network ACL rule to allow ICMP, TCP, and UDP traffic
{: #creating-network-acl-rule-allow-icmp-tcp-udp-2025-12-09}

The following example uses API version `2025-12-08` or earlier to create a network ACL rule that allows ICMP, TCP, and UDP inbound traffic to a local address. The data object specifies a `protocol` value of `all`.

```sh
curl -X POST \
  --url '$vpc_api_endpoint/v1/network_acls/$network_acl_id/rules?version=2025-12-08&generation=2' \
  --header 'Authorization: Bearer $iam_token' \
  --header 'Content-Type: application/json' \
  --data '{
      "action": "allow",
      "destination": "192.168.11.12",
      "direction": "inbound",
      "protocol": "all",
      "source": "0.0.0.0/0"
    }'
```
{: codeblock}

The following example uses API version `2025-12-09` or later to create a network ACL rule that allows ICMP, TCP, and UDP inbound traffic to a local address. The data object specifies a `protocol` value of `icmp_tcp_udp`.

```sh
curl -X POST \
  --url '$vpc_api_endpoint/v1/network_acls/$network_acl_id/rules?version=2025-12-09&generation=2' \
  --header 'Authorization: Bearer $iam_token' \
  --header 'Content-Type: application/json' \
  --data '{
      "action": "allow",
      "destination": "192.168.11.12",
      "direction": "inbound",
      "protocol": "icmp_tcp_udp",
      "source": "0.0.0.0/0"
    }'
```
{: codeblock}

### Creating a network ACL rule to deny any traffic
{: #creating-network-acl-rule-deby-any-2025-12-09}

The following example uses API version `2025-12-08` or earlier to create a network ACL rule that denies any inbound traffic to a local address. The data object specifies a `protocol` value of `all`.

```sh
curl -X POST \
  --url '$vpc_api_endpoint/v1/network_acls/$network_acl_id/rules?version=2025-12-08&generation=2' \
  --header 'Authorization: Bearer $iam_token' \
  --header 'Content-Type: application/json' \
  --data '{
      "action": "deny",
      "destination": "192.168.11.12",
      "direction": "inbound",
      "protocol": "all",
      "source": "0.0.0.0/0"
    }'
```
{: codeblock}

The following example uses API version `2025-12-09` or later to create a network ACL rule that denies any inbound traffic to a local address. The data object specifies a `protocol` value of `any`.

```sh
curl -X POST \
  --url '$vpc_api_endpoint/v1/network_acls/$network_acl_id/rules?version=2025-12-09&generation=2' \
  --header 'Authorization: Bearer $iam_token' \
  --header 'Content-Type: application/json' \
  --data '{
      "action": "deny",
      "destination": "192.168.11.12",
      "direction": "inbound",
      "protocol": "any",
      "source": "0.0.0.0/0"
    }'
```
{: codeblock}
