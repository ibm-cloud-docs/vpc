---

copyright:
  years: 2023
lastupdated: "2023-10-10"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# `2023-10-10` API migration (VPN)
{: #2023-10-10-migration-vpn}

As described in the [VPC API](/apidocs/vpc/latest) reference [versioning](/apidocs/vpc#api-versioning) policy, most changes to the VPC APIs are fully backward compatible and are made available to all clients, regardless of the API version the client requests. However, the `2023-10-10` release of the VPC API necessitated incompatible changes in support of the health diagnostics feature for VPN gateways.

Before you adopt release version `2023-10-10` or later, be aware of the following change, which might require you to update your client:

The `status` and `members[].status` properties are not returned on version `2023-10-10` or later versions of [VPN gateways](/apidocs/vpc/latest#list-vpn-gateways).

## Action needed
{: #action-needed-migration-vpn}

Before specifying version query parameter of `2023-10-10` or later, follow these actions to avoid regressions in client functionality.

If your clients continue to specify version `2023-10-09` or earlier, no changes are required.

### Migrating the `status` and `members[].status` properties
{: #status-migration-vpn}

Before you migrate a client to API version `2023-10-10` or later, review your code for use of the `status` and `members[].status` properties in the VPN gateway response. Change all VPN gateway `status` and `members[].status` to `lifecycle_state` or `health_state` in the manner appropriate for your programming language. Changes are required in all response JSON field name formats. For more information, see the [API change log](/docs/vpc?topic=vpc-api-change-log#10-october-2023).

The following table maps the old property values for `status` and `members[].status` to the new properties and values for `lifecycle_state`, `members[].lifecycle_state`, `health_state`, and `members[].health_state` for API requests [listing all VPN gateways](/apidocs/vpc/latest#list-vpn-gateways) and [retrieving a VPN gateway](/apidocs/vpc/latest#get-vpn-gateway) using a `version` query parameter of `2023-10-10` or later.

| `status`, `members[].status` | `lifecycle_state`, `members[].lifecycle_state` | `health_state`, `members[].health_state` |
|------------------------------|------------------------------------------------|------------------------------------------|
| `available`                  | `stable`                                       | `ok/degraded`                            |
| `failed`                     | `stable`                                       | `faulted`                                |
| `deleted`                    | `deleting`                                     | `inapplicable`                           |
| `pending`                    | `pending`                                      | `inapplicable`                           |
{: caption="Table 1: VPN property migration path" caption-side="bottom"}

## Examples
{: #example-api-migration-vpn}

The following examples compare how to make a [`GET /vpn_gateways/{id}`](/apidocs/vpc/latest#get-vpn-gateway) request before and after the `2023-10-10` versioned change.

Specifying API version `2023-10-09` or earlier:

```sh
curl -X GET \
  "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id?version=2023-10-09&generation=2" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

The response includes the `status` property in the `"members": []` array:

```sh
{
    "connections": [],
    "created_at": "2023-10-02T23:35:11.079589Z",
    "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::vpn:0717-66787b0d-53db-4e20-abb6-d6302fe7c26e",
    "health_reasons": [],
    "health_state": "ok",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/0717-66787b0d-53db-4e20-abb6-d6302fe7c26e",
    "id": "0717-66787b0d-53db-4e20-abb6-d6302fe7c26e",
    "lifecycle_reasons": [],
    "lifecycle_state": "stable",
    "members": [
        {
            "health_reasons": [],
            "health_state": "ok",
            "lifecycle_reasons": [],
            "lifecycle_state": "stable",
            "private_ip": {
                "address": "172.18.0.62",
                "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/0717-5948e25c-73ae-4580-9ff8-2e283874fa37/reserved_ips/0717-a3bc7de9-1dc8-4b66-8c9f-248051325166",
                "id": "0717-a3bc7de9-1dc8-4b66-8c9f-248051325166",
                "name": "ragged-icing-sizing-regalia",
                "resource_type": "subnet_reserved_ip"
            },
            "public_ip": {
                "address": "169.59.215.48"
            },
            "role": "active",
            "status": "available"
        },
        {
            "health_reasons": [],
            "health_state": "ok",
            "lifecycle_reasons": [],
            "lifecycle_state": "stable",
            "private_ip": {
                "address": "172.18.0.63",
                "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/0717-5948e25c-73ae-4580-9ff8-2e283874fa37/reserved_ips/0717-8ebc0927-4de5-4f4c-92da-e2e2a8e2dd09",
                "id": "0717-8ebc0927-4de5-4f4c-92da-e2e2a8e2dd09",
                "name": "barley-overhead-aggregate-truffle",
                "resource_type": "subnet_reserved_ip"
            },
            "public_ip": {
                "address": "0.0.0.0"
            },
            "role": "standby",
            "status": "available"
        }
    ],
    "mode": "policy",
    "name": "demo",
    "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/b863652e11af49d891859d182d42c712",
        "id": "b863652e11af49d891859d182d42c712",
        "name": "Default"
    },
    "resource_type": "vpn_gateway",
    "status": "available",
    "subnet": {
        "crn": "crn:v1:bluemix:public:is:us-south-2:a/a1234567::subnet:0717-5948e25c-73ae-4580-9ff8-2e283874fa37",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/0717-5948e25c-73ae-4580-9ff8-2e283874fa37",
        "id": "0717-5948e25c-73ae-4580-9ff8-2e283874fa37",
        "name": "demo",
        "resource_type": "subnet"
    },
    "vpc": {
        "crn": "crn:v1:bluemix:public:is:us-south:a/1a1234567::vpc:r006-67221f43-3542-4790-9c2a-7f98a65ee02c",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/r006-67221f43-3542-4790-9c2a-7f98a65ee02c",
        "id": "r006-67221f43-3542-4790-9c2a-7f98a65ee02c",
        "name": "demo",
        "resource_type": "vpc"
    }
}

```
{: codeblock}

Specifying API version `2003-10-10` or later:

```sh
curl -X GET \
  "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id?version=2023-10-10&generation=2" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

The response does not include the `status` property in the `"members": []` array:

```sh
{
    "connections": [],
    "created_at": "2023-10-02T23:35:11.079589Z",
    "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::vpn:0717-66787b0d-53db-4e20-abb6-d6302fe7c26e",
    "health_reasons": [],
    "health_state": "ok",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/0717-66787b0d-53db-4e20-abb6-d6302fe7c26e",
    "id": "0717-66787b0d-53db-4e20-abb6-d6302fe7c26e",
    "lifecycle_reasons": [],
    "lifecycle_state": "stable",
    "members": [
        {
            "health_reasons": [],
            "health_state": "ok",
            "lifecycle_reasons": [],
            "lifecycle_state": "stable",
            "private_ip": {
                "address": "172.18.0.62",
                "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/0717-5948e25c-73ae-4580-9ff8-2e283874fa37/reserved_ips/0717-a3bc7de9-1dc8-4b66-8c9f-248051325166",
                "id": "0717-a3bc7de9-1dc8-4b66-8c9f-248051325166",
                "name": "ragged-icing-sizing-regalia",
                "resource_type": "subnet_reserved_ip"
            },
            "public_ip": {
                "address": "169.59.215.48"
            },
            "role": "active"
        },
        {
            "health_reasons": [],
            "health_state": "ok",
            "lifecycle_reasons": [],
            "lifecycle_state": "stable",
            "private_ip": {
                "address": "172.18.0.63",
                "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/0717-5948e25c-73ae-4580-9ff8-2e283874fa37/reserved_ips/0717-8ebc0927-4de5-4f4c-92da-e2e2a8e2dd09",
                "id": "0717-8ebc0927-4de5-4f4c-92da-e2e2a8e2dd09",
                "name": "barley-overhead-aggregate-truffle",
                "resource_type": "subnet_reserved_ip"
            },
            "public_ip": {
                "address": "0.0.0.0"
            },
            "role": "standby"
        }
    ],
    "mode": "policy",
    "name": "demo",
    "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/b863652e11af49d891859d182d42c712",
        "id": "b863652e11af49d891859d182d42c712",
        "name": "Default"
    },
    "resource_type": "vpn_gateway",
    "subnet": {
        "crn": "crn:v1:bluemix:public:is:us-south-2:a/a1234567::subnet:0717-5948e25c-73ae-4580-9ff8-2e283874fa37",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/0717-5948e25c-73ae-4580-9ff8-2e283874fa37",
        "id": "0717-5948e25c-73ae-4580-9ff8-2e283874fa37",
        "name": "demo",
        "resource_type": "subnet"
    },
    "vpc": {
        "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::vpc:r006-67221f43-3542-4790-9c2a-7f98a65ee02c",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/r006-67221f43-3542-4790-9c2a-7f98a65ee02c",
        "id": "r006-67221f43-3542-4790-9c2a-7f98a65ee02c",
        "name": "demo",
        "resource_type": "vpc"
    }
}
```
{: codeblock}
