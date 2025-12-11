---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-11"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Working with subnets in VPC
{: #subnets-configure}

Subnets help your network traffic travel more efficient paths from origin to destination. You can use access control lists (ACLs) with subnets to protect your resources within each subnet.
{: shortdesc}

## Before you begin
{: #subnets-prereqs}

Before you begin, you must be assigned one or more IAM access roles that include the following actions, depending on any listed conditions. You can check your access by going to **[Users](/iam/users) > User > Access**.

* `is.subnet.subnet.create`
* `is.public-gateway.public-gateway.operate`
    Required when `public_gateway` is specified
* `is.network-acl.network-acl.operate`

## Creating a subnet in VPC
{: #subnets-vpc-create}

You can create a subnet in VPC by using the console, CLI, or API.

If you use an IP range outside of the ranges that [RFC 1918](https://datatracker.ietf.org/doc/html/rfc1918){: external} defines (`10.0.0.0/8`, `172.16.0.0/12`, or `192.168.0.0/16`) for a subnet, the instances that are attached to that subnet might be unable to reach parts of the public internet. If you plan to configure VPCs that use both non-RFC-1918 addresses and also have public connectivity (floating IP addresses or public gateways), make sure to use a custom route that contains the Delegate-VPC action.
{: tip}

### Creating subnets in VPC in the console
{: #subnets-create-ui}
{: ui}

To create a subnet in your VPC instance, take the following steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external}.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Subnets**.
1. Click **Create +** on the Subnets for VPC list table.
1. In the Location section, provide the following information:
    * **Geography**: Indicate the general area where you want the subnet to be created.
    * **Region**: Indicate the region where you want the subnet to be created.
    * **Zone**: Indicate the zone where you want the subnet to be created.
1. In the Details section, provide the following information:
    * **Name**: Enter a unique identifier for the subnet, such as `my-subnet`.
    * **Resource group**: Select a resource group for the subnet.
    * **Tags**: Optionally, add any relevant tags to help group your subnets.
    * **Access management tags**: Optionally, add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
    * **Virtual private cloud**: Select the VPC in which you want the subnet to be created.
    * **IP range selection**: The most efficient location for your IP range is calculated automatically. If you want to customize the IP range instead of accepting the default selection, select a different address prefix, change the number of addresses, or enter your IP range manually.
    * **Routing table**: Select which routing table that you want the new subnet to use.
    * **Subnet access control list**: Select which access control list you want the new subnet to use.
    * **Public gateway**: Indicate whether you want the subnet to communicate with the public internet by toggling the switch to **Attached**.

        Attaching a public gateway creates a [Floating IP](/docs/vpc?topic=vpc-fip-about) and incurs a cost.
        {: note}

1. In the Summary panel, click **Create subnet** to create the subnet.

### Attached resources tab
{: #subnets-attached-resources}
{: ui}
 
The **Attached resources** tab lists all the resources that are attached to your subnet, and provides a method of creating and attaching new resources. You can create the following resources from this tab:

* Instances
* Bare metal servers
* Instance templates
* Instance groups
* Load balancers
* VPN gateways
* Virtual endpoint gateways
* VPN servers

Click **Create+** in the resource list table of the resource you want to create. You're taken to the respective create resource workflow where you can begin the process of creating the resource.

### Reserved IPs tab
{: #subnets-reserved-ip}
{: ui}

The **Reserved IPs** tab is a list of all reserved IP addresses that are associated with the subnet. For more information about Reserved IP addresses, see [Managing IP addresses](/docs/vpc?topic=vpc-managing-ip-addresses&interface=ui).

### Creating subnets in VPC from the CLI
{: #subnets-create-cli}
{: cli}

To create a subnet from the CLI, run the following command:

```sh
ibmcloud is subnet-create SUBNET_NAME VPC ((--zone ZONE_NAME --ipv4-address-count ADDR_COUNT) | --ipv4-cidr-block CIDR_BLOCK) [--acl ACL] [--pgw PGW] [--rt RT] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] {} [--output JSON] [-q, --quiet]
```
{: codeblock}


Where:

`SUBNET_NAME`
:    Name of the subnet.

`VPC`
:    ID or name of the VPC.

`--ipv4-cidr-block`
:    The IPv4 range of the subnet. This option is mutually exclusive with `--ipv4-address-count`.

`--ipv4-address-count`
:    The total number of IPv4 addresses required, it must be a power of 2 and the minimum value is 8. This option is mutually exclusive with `--ipv4-cidr-block`.

`--zone`
:    Name of the zone.

`--acl`
:    The ID or name of the network ACL.

`--pgw`
:    The ID or name of the public gateway.

`--rt`
:    The ID or name of the routing table. This option also supports the cloud resource name (CRN) of the routing table.

`--resource-group-id`
:    The ID of the resource group. This ID is mutually exclusive with `--resource-group-name`.

`--resource-group-name`
:    Name of the resource group. This name is mutually exclusive with `--resource-group-id`.

`--output`
:    Specify output format, only JSON is supported. One of: `JSON`.

`-q, --quiet`
:    Suppress verbose output.



### Creating subnets in VPC with the API
{: #subnets-create-api}
{: api}

Follow these instructions to create a subnet in your VPC by using the API:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in API commands.

|Query parameters|Information|
|----------------|-----------|
|**version** \n Required \n `string` | Requests the version of the API as of a date in the format `YYYY-MM-DD`. Any date up to the current date can be provided. Specify the current date to request the most recent version. \n **Possible values:** Value must match regular expression `^[0-9]{4}-[0-9]{2}-[0-9]{2}$`|
|**generation \n Required \n int32 |The infrastructure generation. \n **Possible values:** `2`|
|**Request Body** \n Required |The subnet prototype object.|
|**vpc** \n Required \n `string`|The VPC the subnet will reside in. \n One of: `VPCIdentityById`, `VPCIdentityByCRN`, `VPCIdentityByHref`|
|**ip_version** \n `string`|The IP versions to support for this subnet. **Allowable values:** `[ipv4]`|
|**name** \n Required \n `string` | The name for this subnet. The name must not be used by another subnet in the VPC. If unspecified, the name is going to be a hyphenated list of randomly selected words. \n **Possible values:** 1 ≤ length ≤ 63, Value must match regular expression `^([a-z]&verbar;[a-z][-a-z0-9]*[a-z0-9])$` \n **Example:** `my-subnet`|
|**network_acl** \n `string` | The network ACL to use for this subnet. If unspecified, the default network ACL for the VPC is used. \n One of: `NetworkACLIdentityById`, `NetworkACLIdentityByCRN`, `NetworkACLIdentityHref`|
|**public_gateway** \n `string` |The public gateway to use for internet-bound traffic for this subnet. If unspecified, the subnet is not going to be attached to a public gateway. \n One of: `PublicGatewayIdentityById`, `PublicGatewayIdentityByCRN`, `PublicGatewayIdentityByHref`|
|**resource_group** \n ResourceGroupIdentityById | The resource group to use. If unspecified, the account’s [default resource group](/apidocs/resource-manager#introduction) is used.|
|**routing_table** \n `string` | The routing table to use for this subnet. If unspecified, the default routing table for the VPC is used. The routing table properties `route_direct_link_ingress`, `route_internet_ingress`, `route_transit_gateway_ingress`, and `route_vpc_zone_ingress` must be `false`. \n One of: `RoutingTableIdentityById`, `RoutingTableIdentityByCRN`, `RoutingTableIdentityByHref`|
|**SubnetByTotalCount** \n `int 64` \n or \n **SubnetByCIDR** \n `string` | For `SubnetByTotalCount`, specify the total number of IPv4 addresses required. For `SubnetByCIDR`, specify the IPv4 range of the subnet, expressed in CIDR format. Both options require a `zone` for the subnet to be created in.
{: caption="Parameters for creating a subnet" caption-side="bottom"}

When all the variables are initiated, submit the request.

See the following example:

```sh
curl -X POST
"$vpc_api_endpoint/v1/subnets?version=2022-10-31&generation=2"
-H "Authorization: Bearer $iam_token"
-d '{
      "name": "my-subnet-1",
      "ipv4_cidr_block": "10.0.1.0/24",
      "ip_version": "ipv4",
      "zone": { "name": "us-south-1" },
      "vpc": { "id": "a0819609-0997-4f92-9409-86c95ddf59d3" }
    }'
```
{: codeblock}

The following response shows after you initiate the request:

|Response Body |Details|
|--------------|-------|
|**available_ipv4_address_count** \n Always included \n `int64`| The number of IPv4 addresses in this subnet that are not in-use, and are not reserved by the user or the provider. \n **Example:** `15`|
|**created_at** \n Always included \n `date-time` | The date and time that the subnet was created.|
|**crn** \n Always included \n `string`| The CRN for this subnet. \n **Possible values:** 9 ≤ length ≤ 512 \n **Example:** `crn:v1:bluemix:public:is:us-south-1:a/123456::subnet:7ec86020-1c6e-4889-b3f0-a15f2e50f87e`|
|- **href** \n Always included* \n `string`| The URL for this subnet \n **Possible values:** 10 ≤ length ≤ 8000, Value must match regular expression `^http(s)?:\/\/([^\/?#]*)([^?#]*)(\?([^#]*))?(#(.*))?$` \n **Example:** `https://us-south.iaas.cloud.ibm.com/v1/subnets/7ec86020-1c6e-4889-b3f0-a15f2e50f87e`|
|- **id** \n Always included \n `string`| The unique identifier for this subnet \n **Possible values:** 1 ≤ length ≤ 64, Value must match regular expression `^[-0-9a-z_]+$` \n **Example:** `7ec86020-1c6e-4889-b3f0-a15f2e50f87e`|
|**ip_version** \n Always included \n `string`|The IP version(s) that are supported by this subnet. \n **Possible values:** `[ipv4]` |
|**ipv4_cidr_block** \n Always included \n `string`| The IPv4 range of the subnet, which is expressed in CIDR format. \n **Possible values:** 9 ≤ length ≤ 18, Value must match regular expression `^(([0-9]&verbar;[1-9][0-9]&verbar;1[0-9]{2}&verbar;2[0-4][0-9]&verbar;25[0-5])\.){3}([0-9]&verbar;[1-9][0-9]&verbar;1[0-9]{2}&verbar;2[0-4][0-9]&verbar;25[0-5])(\/(3[0-2]&verbar;[1-2][0-9]&verbar;[0-9]))$` \n **Example:** `10.0.0.0/24`|
|**name** \n Always included \n `string`| The name for this subnet. The name is unique across all subnets in the VPC. \n **Possible values:** 1 ≤ length ≤ 63, Value must match regular expression `^-?([a-z]&verbar;[a-z][-a-z0-9]*[a-z0-9]&verbar;[0-9][-a-z0-9]*([a-z]&verbar;[-a-z][-a-z0-9]*[a-z0-9]))$` \n **Example:** `my-subnet`|
|**network_acl**| \n Always included \n `NetworkACLReference`| The network ACL for this subnet.|
|**resource_group** \n Always included \n `ResourceGroupReference`| The resource group for this subnet.|
|**resource_type** \n Always included \n `string`| The resource type. \n **Possible values:** 1 ≤ length ≤ 128, Value must match regular expression `^[a-z][a-z0-9]*(_[a-z0-9]+)*$` \n **Example:** `subnet` |
|**routing_table** \n Always included \n `RoutingTableReference`| The routing table for this subnet.|
|**status** \n Always included \n `string`|The status of the subnet. \n **Possible values:** `available`,`deleting`,`failed`, `pending`|
|**total_ipv4_address_count** \n Always included \n `int64`|  |
|**vpc** \n Always included \n `VPCReference`| The VPC this subnet resides in. |
|**zone** \n Always included \n `ZoneReference`| The zone this subnet resides in. |
|**public_gateway** \n `PublicGatewayReference` | The public gateway to use for internet-bound traffic for this subnet.|
{: caption="API response example" caption-side="bottom"}

|Status Code| Meaning |
|-----------|---------|
|**201**| The subnet was created successfully.|
|**400**| An invalid subnet prototype object was provided.|
|**409**| The subnet prototype object conflicts with another subnet in the VPC, or specifies a CIDR block outside of the VPC's address prefixes.|
{: caption="Create status codes" caption-side="bottom"}

For more information (including Java, Node, Python and Go examples), see the create a subnet method in the [VPC API reference](/apidocs/vpc/latest#create-subnet).

## Viewing subnets in VPC
{: #subnets-view}

You can view the details of a subnet in your VPC by using the UI, API, or CLI.

### Viewing subnets in VPC in the console
{: #subnets-view-ui}
{: ui}

After the subnet is created, it appears in the Subnets for VPC list.

To view the details about a subnet in the list in the console, click the link of the subnet in the **Name** column. The **Overview** tab shows the information that you used during the subnet creation.

### Viewing subnets in VPC from the CLI
{: #subnets-view-cli}
{: cli}

To view the details about a subnet in VPC from the CLI, run the following command:

```sh
ibmcloud is subnet SUBNET [--vpc VPC] [--show-attached] [--output JSON] [-q, --quiet]
```
{: codeblock}



Where:

`SUBNET`
:    ID or name of the subnet.

`--vpc`
:    ID or name of the VPC. It is only required to specify the unique resource by name inside this VPC.

`--show-attached`
:    View details of resources (instances, load balancers, VPN gateways) attached to the subnet.

`--output`
:    Specify output format, only JSON is supported. One of: JSON.

`-q, --quiet`
:    Suppress verbose output.



### Viewing subnets in VPC with the API
{: #subnets-view-api}
{: api}

To call this method, you must be assigned one or more IAM access roles that include the following action. You can check your access by going to  **[Users](/iam/users) > User > Access**

* `is.subnet.subnet.read`

Follow these instructions to retrieve a subnet in your VPC by using the API:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in API commands.

|Query parameters|Information|
|----------------|-----------|
|**id** \n Required \n `string` |The subnet identifier. \n **Possible values:**  1 ≤ length ≤ 64, Value must match regular expression `^[-0-9a-z_]+$` |
|**version** \n Required \n `string` | Requests the version of the API as of a date in the format `YYYY-MM-DD`. Any date up to the current date can be provided. Specify the current date to request the most recent version. \n **Possible values:** Value must match regular expression `^[0-9]{4}-[0-9]{2}-[0-9]{2}$`|
|**generation** \n Required \n `int32` |The infrastructure generation. \n **Allowable values:** `2`|
{: caption="Parameters for retrieving a subnet" caption-side="bottom"}

1. When all variables are initiated, run the command.

See the following example:

```sh
curl -X GET "$vpc_api_endpoint/v1/subnets/$subnet_id?version=2022-10-31&generation=2"
-H "Authorization: Bearer $iam_token"
```
{: codeblock}

The following response shows after you initiate the request:

|Response Body |Details|
|--------------|-------|
|**available_ipv4_address_count** \n Always included \n `int64`| The number of IPv4 addresses in this subnet that are not in-use, and are not reserved by the user or the provider. \n **Example:** `15`|
|**created_at** \n Always included \n date-time|The date and time that the subnet was created|
|**crn** \n Always included \n `string`|The CRN for this subnet \n **Possible values:** 9 ≤ length ≤ 512 \n **Example:** `crn:v1:bluemix:public:is:us-south-1:a/123456::subnet:7ec86020-1c6e-4889-b3f0-a15f2e50f87e`|
|- **href** \n Always included* \n `string`|The URL for this subnet \n **Possible values:** 10 ≤ length ≤ 8000, Value must match regular expression `^http(s)?:\/\/([^\/?#]*)([^?#]*)(\?([^#]*))?(#(.*))?$` \n **Example:** `https://us-south.iaas.cloud.ibm.com/v1/subnets/7ec86020-1c6e-4889-b3f0-a15f2e50f87e`|
|- **id** \n Always included \n `string`|The unique identifier for this subnet \n **Possible values:** 1 ≤ length ≤ 64, Value must match regular expression `^[-0-9a-z_]+$` \n **Example:** `7ec86020-1c6e-4889-b3f0-a15f2e50f87e`|
|**ip_version** \n Always included \n `string`|The IP versions supported by this subnet. \n **Possible values:** `[ipv4]`|
|**ipv4_cidr_block** \n Always included \n `string` | The IPv4 range of the subnet, expressed in CIDR format. \n **Possible values:** 9 ≤ length ≤ 18, Value must match regular expression `^(([0-9]&verbar;[1-9][0-9]&verbar;1[0-9]{2}&verbar;2[0-4][0-9]&verbar;25[0-5])\.){3}([0-9]&verbar;[1-9][0-9]&verbar;1[0-9]{2}&verbar;2[0-4][0-9]&verbar;25[0-5])(\/(3[0-2]&verbar;[1-2][0-9]&verbar;[0-9]))$` \n **Example:** `10.0.0.0/24`|
|**name** \n Always included \n `string`|The name for this subnet. The name is unique across all subnets in the VPC. \n **Possible values:** 1 ≤ length ≤ 63, Value must match regular expression `^-?([a-z]&verbar;[a-z][-a-z0-9]*[a-z0-9]&verbar;[0-9][-a-z0-9]*([a-z]&verbar;[-a-z][-a-z0-9]*[a-z0-9]))$` \n **Example:** `my-subnet`|
|**network_acl**| \n Always included \n NetworkACLReference|The network ACL for this subnet.|
|**resource_group** \n Always included \n ResourceGroupReference|The resource group for this subnet.|
|**resource_type** \n Always included \n `string`|The resource type. \n **Possible values:** 1 ≤ length ≤ 128, Value must match regular expression `^[a-z][a-z0-9]*(_[a-z0-9]+)*$` \n **Example:** `subnet` |
|**routing_table** \n Always included \n RoutingTableReference|The routing table for this subnet.|
|**status** \n Always included \n `string`|The status of the subnet. \n **Possible values:** `available`,`deleting`,`failed`, `pending`|
|**total_ipv4_address_count** \n Always included \n `int64`|  |
|**vpc** \n Always included \n VPCReference| The VPC this subnet resides in. |
|**zone** \n Always included \n ZoneReference| The zone this subnet resides in. |
|**public_gateway** \n PublicGatewayReference |The public gateway to use for internet-bound traffic for this subnet.|
{: caption="Initiate response retrieve example" caption-side="bottom"}

|Status Code| Meaning|
|-----------|--------|
|**200**| The subnet was retrieved successfully.|
|**404**| A subnet with the specified identifier could not be found.|
{: caption="Retrieve status codes" caption-side="bottom"}

For more information (including Java, Node, Python and Go examples), see "Retrieve a subnet" in the [VPC API reference](/apidocs/vpc/latest#get-subnet).
{: note}
