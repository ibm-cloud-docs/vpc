---

copyright:
  years: 2022, 2024
lastupdated: "2024-07-17"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to the `2022-03-29` version (network interfaces, security groups)
{: #2022-03-29-migration}

As described in [Versioning](/apidocs/vpc/latest#api-versioning){: external}, most changes to the VPC APIs are fully compatible with earlier versions and therefore are made available to all clients, regardless of the API version the client requests. However, the `2022-03-29` release of the VPC API necessitated incompatible changes in support of the reserved IP addresses feature:

- IP addresses are now modeled as objects (resources), rather than strings.
- Security groups must now be associated with targets rather than network interfaces.

If your clients continue to specify version `2022-03-28` or earlier of the VPC API, no changes are required, but those clients remain unable to fully benefit from reserved IP addresses. For more information about reserved addresses, see [Managing IP addresses](/docs/vpc?topic=vpc-managing-ip-addresses).

Even if you are not planning to use reserved IP addresses, to avoid regressions in client functionality, read, and follow the [action needed](#action-needed) section before your client specifies a version `2022-03-29` or later.
{: important}

The [VPC Instance Metadata API](/apidocs/vpc-metadata) does not yet support reserved IP addresses. Clients that use the metadata API do not require changes to use version `2022-03-29` or later.
{: note}

## Action needed
{: #action-needed}

Before the client specifies a version `2022-03-29` or later, follow these actions to avoid regressions in client functionality.

### Migrating use of IP addresses
{: #migrate-ip-addresses}

Before you migrate a client to an API version `2022-03-29` or later, review your code for any use of the `primary_ipv4_address` property. As shown in the [examples](#examples), as of version `2022-03-29`, the `primary_ipv4_address` string property is replaced by the `primary_ip` object property, which includes an `address` subproperty. Therefore, you need to change all instances of `primary_ipv4_address` to `primary_ip.address` in the manner appropriate for your programming language.

While `primary_ipv4_address` appears only in the `NetworkInterface`, `NetworkInterfaceReference`, and `NetworkInterfacePrototype` schemas (and their closely related `BareMetalNetworkInterface` schemas), those schemas are used extensively in the VPC API. Therefore, even though the incompatible change is limited to the `primary_ipv4_address` property, many operations were affected. See [Changed operations](#changed-operations) for a complete list.

### Migrating use of security group associations
{: #migrate-security-group-assoc}

As part of the [`2021-02-23` release](/docs/vpc?topic=vpc-api-change-log#23-february-2021), security group target methods were made available, and the original methods specific to network interfaces were deprecated. API version `2022-03-29` removes support for those deprecated methods. As shown in
the [examples](#examples), the security group target methods include the capabilities of the deprecated methods. Security group targets also allow security groups to be associated with other VPC resources, such as endpoint gateways and load balancers.

The deprecated `GET` and `PUT` methods returned `NetworkInterface` response schemas, but the new `GET` and `PUT` methods return `NetworkInterfaceReference` response schemas. If your client was accessing properties beyond the properties in `NetworkInterfaceReference`, your client needs to call `GET
/instances/{instance_id}/network_interfaces/{id}` to retrieve those properties separately.

Additionally, if your client was previously accessing the `network_interfaces[]` property that is returned by `GET /security_groups`, it needs to access the `targets[]` property instead. If your client was accessing properties in `network_interfaces[]` beyond the properties in `NetworkInterfaceReference`, your client needs to call `GET /instances/{instance_id}/network_interfaces/{id}` to retrieve those properties separately.
{: important}

## Examples
{: #examples}

These examples compare differences between before and after the `2022-03-29` versioned change.

### IP address examples
{: #ip-address-examples}

The following examples compare how to make a `POST /instances` call to provision an instance with a specific IP address on its primary network interface before and after the `2022-03-29` versioned change, and how to retrieve that IP address.

Specifying API version `2022-03-28` or earlier:

```sh
curl -X POST \
  "$vpc_api_endpoint/v1/instances?version=2022-03-28&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
        "image": {"id": "'"$image_id"'"},
        "keys": [{"id": "'"$key_id"'"}],
        "primary_network_interface": {
          "primary_ipv4_address": "10.240.0.5",
          "subnet": {"id": "'"$subnet_id"'"}
        },
        "zone": {"name": "'"$zone"'"}
      }'
```
{: pre}

To retrieve the address:

```sh
curl -X GET \
  "$vpc_api_endpoint/v1/instances/$instance_id?version=2022-03-28&generation=2" \
  -H "Authorization: Bearer $iam_token" | jq '.primary_network_interface.primary_ipv4_address'
```
{: pre}

Specifying API version `2022-03-29` or later:

```sh
curl -X POST \
  "$vpc_api_endpoint/v1/instances?version=2022-03-29&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
        "image": {"id": "'"$image_id"'"},
        "keys": [{"id": "'"$key_id"'"}],
        "primary_network_interface": {
          "primary_ip": {
            "address": "10.240.0.5"
          },
          "subnet": {"id": "'"$subnet_id"'"}
        },
        "zone": {"name": "'"$zone"'"}
      }'
```
{: pre}

To retrieve the address:

```sh
curl -X GET \
  "$vpc_api_endpoint/v1/instances/$instance_id?version=2022-03-29&generation=2" \
  -H "Authorization: Bearer $iam_token" | jq '.primary_network_interface.primary_ip.address'
```
{: pre}

The following examples compare how to make a `POST /instances/{id}/network_interface` call to create a new network interface on an existing instance with a specific IP address before and after the `2022-03-29` versioned change, and how to retrieve the address.

Specifying API version `2022-03-28` or earlier:

```sh
curl -X POST \
  "$vpc_api_endpoint/v1/instances/$instance_id/network_interfaces?version=2022-03-28&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
        "primary_ipv4_address": "10.240.0.5",
        "subnet": {"id": "'"$subnet_id"'"}
      }'
```
{: pre}

To retrieve the address:

```sh
curl -X GET \
  "$vpc_api_endpoint/v1/instances/$instance_id/network_interfaces/$network_interface_id?version=2022-03-28&generation=2" \
  -H "Authorization: Bearer $iam_token" | jq '.primary_ipv4_address'
```
{: pre}

Specifying API version `2022-03-29` or later:

```sh
curl -X POST \
  "$vpc_api_endpoint/v1/instances/$instance_id/network_interfaces?version=2022-03-29&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
        "primary_ip": {
          "address": "10.240.0.5"
        },
        "subnet": {"id": "'"$subnet_id"'"}
      }'
```
{: pre}

To retrieve the address:

```sh
curl -X GET \
  "$vpc_api_endpoint/v1/instances/$instance_id/network_interfaces/$network_interface_id?version=2022-03-29&generation=2" \
  -H "Authorization: Bearer $iam_token" | jq '.primary_ip.address'
```
{: pre}

### Security group association examples
{: #security-group-assoc-examples}

The following examples compare how to place a network interface into a security group before and after the `2022-03-29` versioned change.

Specifying API version `2022-03-28` or earlier:

```sh
curl -X PUT \
  "$vpc_api_endpoint/v1/security_groups/$security_group_id/network_interfaces/$id?version=2022-03-28&generation=2" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

Specifying API version `2022-03-29` or later:

```sh
curl -X PUT \
  "$vpc_api_endpoint/v1/security_groups/$security_group_id/targets/$id?version=2022-03-29&generation=2" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}


The following examples compare how to remove a network interface from a security group before and after the `2022-03-29` versioned change.

Specifying API version `2022-03-28` or earlier:

```sh
curl -X DELETE \
  "$vpc_api_endpoint/v1/security_groups/$security_group_id/network_interfaces/$id?version=2022-03-28&generation=2" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

Specifying API version `2022-03-29` or later:

```sh
curl -X DELETE \
  "$vpc_api_endpoint/v1/security_groups/$security_group_id/targets/$id?version=2022-03-29&generation=2" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

## Changed operations
{: #changed-operations}

### IP addresses
{: #ip-address-changes}

The following table lists the operations in which the `primary_ipv4_address` changed to `primary_ip` in the request bodies, response bodies, or both. A period (`.`) indicates that `primary_ipv4_address` was changed to `primary_ip` in the top-level response schema.


| Method   | Path                                                                                        | Changed request properties                        | Changed response properties                                            |
|----------|---------------------------------------------------------------------------------------------|---------------------------------------------------|------------------------------------------------------------------------|
| `GET`    | `/bare_metal_servers`                                                                       |                                                   | `.bare_metal_servers[].network_interfaces[]`, `.bare_metal_servers[].primary_network_interface` |
| `POST`   | `/bare_metal_servers`                                                                       | `.network_interfaces[]`, `primary_network_interface` | `.network_interfaces[]`, `.primary_network_interface` |
| `GET`    | `/bare_metal_servers/{id}`                                                                  |                                                   | `.network_interfaces[]`, `.primary_network_interface` |
| `PATCH`  | `/bare_metal_servers/{id}`                                                                  |                                                   | `.network_interfaces[]`, `.primary_network_interface` |
| `GET`    | `/bare_metal_servers/{id}/network_interfaces`                                               |                                                   | `.network_interfaces[]` |
| `POST`   | `/bare_metal_servers/{id}/network_interfaces`                                               | `.`                                               | `.` |
| `GET`    | `/bare_metal_servers/{bare_metal_server_id}/network_interfaces/{id}`                        |                                                   | `.` |
| `GET`    | `/bare_metal_servers/{bare_metal_server_id}/network_interfaces/{id}/floating_ips`           |                                                   | `.floating_ips[].target` |
| `GET`    | `/bare_metal_servers/{bare_metal_server_id}/network_interfaces/{network_interface_id}/floating_ips/{id}` |                                      | `.target` |
| `PUT`    | `/bare_metal_servers/{bare_metal_server_id}/network_interfaces/{network_interface_id}/floating_ips/{id}` |                                      | `.target` |
| `GET`    | `/floating_ips`                                                                             |                                                   | `.floating_ips[].target` |
| `POST`   | `/floating_ips`                                                                             |                                                   | `.target` |
| `GET`    | `/floating_ips/{id}`                                                                        |                                                   | `.` |
| `PATCH`  | `/floating_ips/{id}`                                                                        |                                                   | `.` |
| `GET`    | `/instance/templates`                                                                       |                                                   | `.templates[].network_interfaces[]`, `.templates[].primary_network_interface` |
| `POST`   | `/instance/templates`                                                                       | `.network_interfaces[]`, `primary_network_interface` | `.network_interfaces[]`, `.primary_network_interface` |
| `GET`    | `/instance/templates/{id}`                                                                  |                                                   | `.network_interfaces[]`, `.primary_network_interface` |
| `PATCH`  | `/instance/templates/{id}`                                                                  |                                                   | `.network_interfaces[]`, `.primary_network_interface` |
| `GET`    | `/instances`                                                                                |                                                   | `.instances[].network_interfaces[]`, `.instances[].primary_network_interface` |
| `POST`   | `/instances`                                                                                | `.network_interfaces[]`, `primary_network_interface` | `.network_interfaces[]`, `.primary_network_interface` |
| `GET`    | `/instances/{id}`                                                                           |                                                   | `.network_interfaces[]`, `.primary_network_interface` |
| `PATCH`  | `/instances/{id}`                                                                           |                                                   | `.network_interfaces[]`, `.primary_network_interface` |
| `GET`    | `/instances/{id}/network_interfaces`                                                        |                                                   | `.network_interfaces[]` |
| `POST`   | `/instances/{id}/network_interfaces`                                                        | `.`                                               | `.` |
| `GET`    | `/instances/{instance_id}/network_interfaces/{id}`                                          |                                                   | `.` |
| `GET`    | `/instances/{instance_id}/network_interfaces/{id}/floating_ips`                             |                                                   | `.floating_ips[].target` |
| `GET`    | `/instances/{instance_id}/network_interfaces/{network_interface_id}/floating_ips/{id}`      |                                                   | `.target` |
| `PUT`    | `/instances/{instance_id}/network_interfaces/{network_interface_id}/floating_ips/{id}`      |                                                   | `.target` |
| `GET`    | `/vpcs/{id}/default_security_group`                                                         |                                                   | `.network_interfaces[]` |
{: caption="Table 1. List of operations that have changed properties." caption-side="bottom"}

### Security group associations
{: #sg-assoc-changes}

The following table lists the methods or properties that were removed as part of completing the migration to security group targets.

| Method   | Path                                                                                        | Changed request properties                        | Changed response properties                                            |
|----------|---------------------------------------------------------------------------------------------|---------------------------------------------------|------------------------------------------------------------------------|
| `GET`    | `/security_groups`                                                                          |                                                   | ~~`.security_groups[].network_interfaces`~~ |
| `GET`    | ~~`/security_groups/{id}/network_interfaces`~~                                              |                                                   | |
| `DELETE` | ~~`/security_groups/{security_group_id}/network_interfaces/{id}`~~                          |                                                   | |
| `GET`    | ~~`/security_groups/{security_group_id}/network_interfaces/{id}`~~                          |                                                   | |
| `PUT`    | ~~`/security_groups/{security_group_id}/network_interfaces/{id}`~~                          |                                                   | |
{: caption="Table 2. List of methods or properties that have been removed." caption-side="bottom"}
