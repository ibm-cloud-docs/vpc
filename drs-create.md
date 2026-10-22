---

copyright:
  years: 2026
lastupdated: "2026-10-22"

keywords: dynamic route server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a dynamic route server
{: #drs-create}

Dynamic Route Server for VPC is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

You can create a dynamic route server to enable policy-driven, dynamic routing between your VPC and connected networks by exchanging routes with external peers by using BGP. You can add members to a dynamic route server to deploy routing appliances across availability zones, providing high availability and resilience for BGP route exchange.
{: shortdesc}

A dynamic route server instance contains one or more members and defines the routing scope for peers and policies. A member is a routing appliance that is deployed as a virtual server instance and runs the BGP routing stack. Members establish BGP sessions with peers. A dynamic route server instance can have one to three members for high availability.

## Before you begin
{: #create-drs-prerequisites}

Before you create a dynamic route server:

* Make sure that you have the required level of access to create VPC services. For more information, see [Getting started with Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started) and [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started).
* Review [Planning considerations for dynamic route servers](/docs/vpc?topic=vpc-planning-drs).
* Review [Known issues for dynamic route servers](/docs/vpc?topic=vpc-known-issues).

## Creating a dynamic route server in the console
{: #drs-create-ui}
{: ui}

To create a dynamic route server in the console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), and then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Dynamic route servers**.
1. On the Dynamic route servers for VPC page, click **Create**.
1. Specify the following information:
   - **Location** - Select the location where the dynamic route server is provisioned.
   - **Name** - Enter a name for the dynamic route server, such as `my-dynamic-route-server`.
   - **Resource group** - Select a resource group for the dynamic route server.
   - **Tags** - Optionally, add tags to identify this dynamic route server.
   - **Access management tags** - Optionally, add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
   - **Virtual private cloud** - Select the VPC for the dynamic route server.
   - **ASN** - Enter an ASN value. An ASN is a numeric identifier for the dynamic route server, which identifies your local network for BGP peering and is used to set up a BGP session with other peers.
1. Create a dynamic route server member and click **Create dynamic route server** in the Summary panel.

### Creating dynamic route server members
{: #drs-create-members-ui}
{: ui}

To create a dynamic route server member in the console, follow these steps:

1. In the dynamic route server members section, click **Create**. Dynamic route server members require a virtual network interface (VNI) to establish BGP sessions. You can attach an existing VNI or create a new one. For more information, see [About virtual network interfaces](/docs/vpc?topic=vpc-vni-about).
1. Enter a name for the dynamic route server member, such as `my-drs-mem`.
1. Select the location where the member is created.
1. If you already have a VNI, select one from the list. If you don't have one, follow these steps:
   - In the **Details** section, click **Create** and enter a name for the virtual network interface.
     - Select the resource group to which it belongs.
     - Select the subnet that you want to use for the member.
     - Add the Network and IP information
     - Click **Next**
   - In the **Network configuration** section, complete the following information:
      - **Allow IP spoofing**: Select the switch to disable IP spoofing.
      - **Infrastructure NAT**: Select the switch to enable infrastructure NAT.

         Infrastructure NAT is enabled when IP spoofing is enabled and must be enabled for the dynamic route server member.
         {: note}

      - [**Protocol state filtering mode**](/docs/vpc?topic=vpc-vni-about&interface=ui#protocol-state-filtering): Select a radio button to set the mode. For the dynamic route server, it must be set to **Auto** or **Enabled**.
      - **Security groups**: Select at least one and at most five security groups to control traffic at the networking level. You can select security groups from the list, or create one by clicking Create. For more information about creating a security group, see [Creating security groups](/docs/vpc?topic=vpc-using-security-groups).
   - In the IP section, keep the default setting. A dynamic route server member VNI must have one primary IP address.
   - Review the information in the Summary panel, and click **Create**.

## Creating a dynamic route server from the CLI
{: #drs-create-cli}
{: cli}

To create a dynamic route server from the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your CLI environment. After you enter the password, the system prompts which account and region that you want to use:

   ```sh
   ibmcloud login --sso
   ```
   {: pre}

1. Enable the following feature flag:

   ```sh
   export IBMCLOUD_IS_FEATURE_DYNAMIC_ROUTE_SERVER=true
   ```
   {: pre}

1. Run the following command:

   ```sh
   ibmcloud is dynamic-route-server-create --local-asn LOCAL_ASN (--members MEMBERS_JSON | @MEMBERS_JSON_FILE) [--name NAME] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
   ```
   {: codeblock}

Where:

`--local-asn`
    : The local autonomous system number (ASN).

`--members`
    : A JSON payload that defines the dynamic route server members. You can specify a minimum of one and a maximum of three members. For the data schema, see the `members` property in the [API documentation](/apidocs/vpc#create-dynamic-route-server).

`@MEMBERS_JSON_FILE`
    : A file that contains the members JSON payload.

`--name`
    : The name of the dynamic route server.

`--resource-group-id`
    : The ID of the resource group. This option is mutually exclusive with `--resource-group-name`.

`--resource-group-name`
    : The name of the resource group. This option is mutually exclusive with `--resource-group-id`.

`--output`
    : Output in JSON format.

`-q, --quiet`
    : Suppresses verbose output.

To create a dynamic route server member from the CLI, run the following command:

```sh
ibmcloud is dynamic-route-server-member-create DYNAMIC_ROUTE_SERVER (--vni VNI | (--vni-auto-delete true | false [--vni-psfm auto | enabled | disabled] --vni-name VNI_NAME [--vni-rip VNI_RIP | (--vni-rip-address VNI_RIP_ADDRESS --vni-rip-auto-delete true | false --vni-rip-name VNI_RIP_NAME)] --vni-subnet VNI_SUBNET [--vpc VPC] --vni-sgs VNI_SGS --vni-ais false | true --vni-ein true | false --vni-ips VNI_RESERVED_IPS_JSON | @VNI_RESERVED_IPS_JSON_FILE --resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME)) [--name NAME] [--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

`DYNAMIC_ROUTE_SERVER`
    : The dynamic route server identifier.

`--vni`
    : The ID or name of the VNI.

`--vni-auto-delete`
    : Indicates whether the VNI is automatically deleted when its target is deleted. One of: `true` or `false`.

`--vni-psfm`
    : The protocol state filtering mode for the VNI. If `auto`, protocol state packet filtering is enabled or disabled based on the `target` resource type of the VNI. One of: `auto`, `enabled`, or `disabled`. The default is `auto`.

`--vni-name`
    : The name of the VNI.

`--vni-rip`
    : The ID or name of the reserved IP to bind to the VNI. Required if you don't specify a subnet.

`--vni-rip-address`
    : The IP address of the reserved IP to bind to the VNI. Requires a subnet.

`--vni-rip-auto-delete`
    : Indicates whether the reserved IP is automatically deleted when its target is deleted or when the reserved IP is unbound. One of: `true` or `false`.

`--vni-rip-name`
    : The name of the reserved IP to bind to the VNI.

`--vni-subnet`
    : The ID or name of the subnet. If you specify a name, use `--vpc` to specify the VPC for name resolution.

`--vpc`
    : The ID or name of the VPC. Required for subnet name resolution.

`--vni-sgs`
    : A comma-separated list of the IDs or names of the security groups for the VNI.

`--vni-ais`
    : Allow IP spoofing. Indicates whether source IP spoofing is allowed on the VNI. One of: `false` or `true`. The default is `false`.

`--vni-ein`
    : Enable infrastructure NAT. Indicates whether the VPC infrastructure performs any needed NAT operations. One of: `true` or `false`. The default is `true`.

`--vni-ips`
    : A JSON payload that defines the secondary reserved IP addresses to bind to the VNI.

`@VNI_RESERVED_IPS_JSON_FILE`
    : A file that contains the secondary reserved IP addresses JSON payload.

`--resource-group-id`
    : The ID of the resource group. This option is mutually exclusive with `--resource-group-name`.

`--resource-group-name`
    : The name of the resource group. This option is mutually exclusive with `--resource-group-id`.

`--name`
    : The name of the dynamic route server member.

`--output`
    : Output in JSON format.

`-q, --quiet`
    : Suppresses verbose output.

### Command examples
{: #cli-cmd-examples-drs-create}

* To create a dynamic route server with local ASN and one member, run the following command:

   ```sh
   ibmcloud is dynamic-route-server-create --local-asn 64514 --members '[{"name":"drs-member-1","virtual_network_interfaces":[{"id":"0717-3c8a9f81-b1a0-4c92-9a38-1d9c2f9d1234"}]}]' --name my-dynamic-route-server --resource-group-name default --output JSON
   ```
   {: pre}

* To create a dynamic route server member, run the following command:

   ```sh
   ibmcloud is dynamic-route-server-member-create 0717-drs-12345678 --vni-auto-delete false --vni-psfm auto --vni-name drs-member-vni-1 --vni-rip-address 10.0.1.5 --vni-rip-auto-delete false --vni-rip-name drs-member-rip-1 --vni-subnet 0717-subnet-abc12345 --vpc 0717-vpc-9f8e7d6c --vni-sgs 0717-sg-111aaa22,0717-sg-333bbb44 --vni-ais false --vni-ein true --resource-group-name default --name drs-member-2 --output JSON
   ```
   {: pre}

## Creating a dynamic route server with the API
{: #drs-create-api}
{: api}

To create a dynamic route server with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the required variables.
1. Store any additional variables to use in the API commands. For example:

   - `ResourceGroupId` - Find the resource group ID by using the `get resource groups` command and then populate the variable:

    ```sh
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

1. When all variables are set, create the dynamic route server:

   ```sh
   curl -X POST \
     "$vpc_api_endpoint/v1/dynamic_route_servers?version=$api_version&generation=2" \
     -H "Authorization: Bearer $iam_token" \
     -H "Content-Type: application/json" \
     -d '{
       "name": "my-drs",
       "local_asn": 64514,
       "members": [
         {
           "name": "my-drs-member",
           "virtual_network_interfaces": [
             {
               "id": "VNI_ID"
             }
           ]
         }
       ],
       "resource_group": {
         "id": "abcd1234-5678"
       }
     }'
   ```
   {: codeblock}



## Next steps
{: #drs-create-next-steps}

Now that your dynamic route server is provisioned, consider these next steps:

- [Create a peer group](/docs/vpc?topic=vpc-drs-create-peer-group) to group and add similar peers.
