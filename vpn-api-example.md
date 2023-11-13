---



copyright:
  years: 2019, 2020
lastupdated: "2020-11-13"

keywords: VPN, network, encryption, authentication, algorithm, IKE, IPsec, policies

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# API example - Connecting two VPCs using VPN
{: #vpn-example}
{: api}

The following API example describes how to connect two VPCs by creating a VPN gateway in each VPC. A VPN gateway can connect all the subnets in the zone where it is deployed. Therefore, having a VPN gateway in each zone that you want to interconnect makes the subnets in both zones act as if they were a single network. The IP addresses of the subnets in the two VPCs must not overlap.
{: shortdesc}

You can use a VPN gateway to connect two VPCs. However, it is recommended to use IBM Cloud Transit Gateway for VPC-to-VPC connectivity. For more information, see [Getting started with IBM Cloud Transit Gateway](/docs/transit-gateway).
{: important}

The following diagram shows how to interconnect three VPCs; the following example shows you how to connect only the first two. You can repeat the steps to connect more VPCs.

![Interconnecting VPCs by using VPN gateways](images/vpc-vpn.svg){: caption="Figure 1: Interconnecting VPCs by using VPN gateways" caption-side="bottom"}

The following example assumes that:

1. You already created VPCs, subnets, and virtual server instances. For more information about creating VPC resources, see [Getting started with Virtual Private Cloud (VPC)](/docs/vpc?topic=vpc-getting-started).
1. You set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) and initialized all the variables needed.

You can also create a VPN gateway and add VPN connections by using the UI. For instructions, see [Creating a VPN gateway using the UI](/docs/vpc?topic=vpc-vpn-create-gateway#vpn-create-ui).
{: tip}

## Step 1. Create a VPN gateway in the first VPC
{: #step-1-create-a-vpn-gateway-in-your-vpc-subnet}

The VPN gateway is created in the zone that is associated with the subnet you select. For best performance, create the VPN gateway in a subnet without any other VPC resources to ensure that enough private IPs are available for the gateway. A VPN gateway needs 8 private IP addresses to accommodate high availability and rolling upgrades.

The VPN gateway can connect only to virtual server instances in the zone it is deployed. Instances in other zones can't use this VPN gateway to communicate with the other network. For zone fault tolerance, deploy one VPN gateway per zone.
{: tip}

Find the Subnet ID by using the **get subnet** command then populate the variable:

```sh
export SubnetId1=<your_subnet_id>
```
{: pre}

The following command deploys the VPN gateway in the `Default` resource group.

```bash
curl -X POST "$vpc_api_endpoint/v1/vpn_gateways?version=$api_version&generation=2" \
    -H "Authorization: $iam_token" \
    -d '{
            "name": "vpn-gateway-1",
            "subnet": {
                "id": "'$SubnetId1'"
            }
        }'
```
{: codeblock}

Sample output:

```sh
{
    "id": "0738-7fd72524-6e2d-49a6-b975-0071efccd89a",
    "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567:vpn:0738-7fd72524-6e2d-49a6-b975-0071efccd89a",
    "name": "vpn-gateway-1",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/0738-7fd72524-6e2d-49a6-b975-0071efccd89a",
    "created_at": "2018-07-06T19:19:28.694388Z",
    "status": "pending",
    "public_ip": {
        "address": "169.61.161.167"
    },
    "subnet": {
        "id": "0738-f45ee0be-cf3f-41ca-a279-23139110aa58",
        "name": "subnet-1",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/0738-f45ee0be-cf3f-41ca-a279-23139110aa58"
    },
    "resource_group": {
        "id": "d28a2jsiw1pl2g22q8462tyr321416z2",
        "href": "https://resource-manager.cloud.ibm.com/v1/resource_groups/d28a2jsiw1pl2g22q8462tyr321416z2"
    }
}
```
{: screen}

Save the `id` of the first VPN gateway, for example:

```sh
export gwid1=<your_vpngateway_id>
```
{: pre}

The gateway status appears as `pending` while the VPN gateway is being created, and the status becomes `available` when creation is complete. Creation can take a few minutes.

You can check the gateway's status with the following command:

```bash
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$gwid1?version=$api_version&generation=2" \
     -H "Authorization: $iam_token"
```
{: codeblock}

Save the `public_ip.address` of the first VPN gateway, for example:

```sh
export gwaddress1=<your_vpngateway_public_ip>
```
{: pre}

## Step 2. Create a VPN gateway in the second VPC
{: #step-2-create-a-second-vpn-gateway-on-a-different-vpc}

Find the Subnet ID of the second VPC by using the **get subnet** command then populate the variable:

```sh
export SubnetId2=<your_subnet_id>
```
{: pre}

The following command deploys the VPN gateway in the `Default` resource group. If the second VPC belongs to a different region,
make sure to update the variable `vpc_api_endpoint`. See the list of [API endpoints](/apidocs/vpc/latest#endpoint-url).

```bash
curl -X POST  "$vpc_api_endpoint/v1/vpn_gateways?version=$api_version&generation=2" \
    -H "Authorization: $iam_token"  \
    -d '{
            "name": "vpn-gateway-2",
            "subnet": {
                "id": "'$SubnetId2'"
            }
        }'
```
{: codeblock}


Sample output:

```sh
{
    "id": "0738-f72559a3-2fac-4958-b937-54474e6a8a8d",
    "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::vpn:0738-f72559a3-2fac-4958-b937-54474e6a8a8d",
    "name": "vpn-gateway-2",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/0738-f72559a3-2fac-4958-b937-54474e6a8a8d",
    "created_at": "2018-07-06T19:33:23.789675Z",
    "status": "pending",
    "public_ip": {
        "address": "169.61.161.150"
    },
    "subnet": {
        "id": "0738-f72c7f7c-0fa5-42d1-9bdc-9e0acad53cb4",
        "name": "subnet-2",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/0738-f72c7f7c-0fa5-42d1-9bdc-9e0acad53cb4"
    },
    "resource_group": {
        "id": "d28a2jsiw1pl2g22q8462tyr321416z2",
        "href": "https://resource-manager.cloud.ibm.com/v1/resource_groups/d28a2jsiw1pl2g22q8462tyr321416z2"
    }
}
```
{: screen}

Save the `id` of the second VPN gateway, for example:

```sh
export gwid2=<your_vpngateway_id>
```
{: pre}

The gateway status appears as `pending` while the VPN gateway is being created, and the status becomes `available` when creation is complete. Creation can take a few minutes.

You can check the gateway's status with the following command:

```bash
curl -X GET "$vpc_api_endpoint/v1/vpn_gateways/$gwid2?version=$api_version&generation=2" \
     -H "Authorization: $iam_token"
```
{: codeblock}

Save the `public_ip.address` of the second VPN gateway, for example:

```sh
export gwaddress2=<your_vpngateway_public_ip>
```
{: pre}


## Step 3. Create a VPN connection from the first VPN gateway to the second VPN gateway
{: #step-3-create-a-vpc-connection-from-the-first-vpn-gateway-to-the-second-vpn-gateway}

When you create the connection for the VPN gateway of VPC 1, set `local_cidrs` to the subnets on VPC 1 and set `peer_cidrs` to the subnets on VPC 2. Separate multiple CIDRs with commas. Remember to update the variable `vpc_api_endpoint` depending on the region the VPN gateway is in.

```sh
export Vpc1Subnets=<your_vpc1_subnets>
export Vpc2Subnets=<your_vpc2_subnets>
```
{: codeblock}

```bash
curl -X POST "$vpc_api_endpoint/v1/vpn_gateways/$gwid1/connections?version=$api_version&generation=2" \
        -H "Authorization: $iam_token"   \
        -d '{
                "name": "vpn-connection-to-vpn-gateway-2",
                "peer_address": "'$gwaddress2'",
                "psk": "VPNDemoPassword",
                "local_cidrs": [ "'$Vpc1Subnets'" ],
                "peer_cidrs": [ "'$Vpc2Subnets'" ]
            }'
```
{: codeblock}

Sample output:

```sh
{
    "id": "0738-a252d380-0784-45ff-8fc0-c2b58e446b4d",
    "name": "vpn-connection-to-vpn-gateway-2",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/0738-7fd72524-6e2d-49a6-b975-0071efccd89a/connections/0738-a252d380-0784-45ff-8fc0-c2b58e446b4d",
    "local_cidrs": [
        "192.168.100.0/24"
    ],
    "peer_cidrs": [
        "192.168.0.0/24"
    ],
    "peer_address": "169.61.161.150",
    "admin_state_up": true,
    "psk": "VPNDemoPassword",
    "dead_peer_detection": {
        "action": "none",
        "interval": 30,
        "timeout": 120
    },
    "created_at": "2018-07-06T19:50:49.252072Z",
    "route_mode": "policy",
    "authentication_mode": "psk",
    "status": "down"
}
```
{: screen}

## Step 4. Create a VPN connection from the second VPN gateway to the first VPN gateway
{: #step-4-create-a-vpn-connection-from-the-second-vpn-gateway-to-the-first-vpn-gateway}

When you create the connection for the VPN gateway of VPC 2, set `local_cidrs` to the subnets on VPC 2 and `peer_cidrs` to the subnets on VPC 1. Remember to update the variable `vpc_api_endpoint` depending on the region the VPN gateway is in.

```bash
curl -X POST "$vpc_api_endpoint/v1/vpn_gateways/$gwid2/connections?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
                "name": "vpn-connection-to-vpn-gateway-1",
                "peer_address": "'$gwaddress1'",
                "psk": "VPNDemoPassword",
                "local_cidrs": [ "'$Vpc2Subnets'" ],
                "peer_cidrs": [ "'$Vpc1Subnets'" ]
            }'
```
{: codeblock}

Sample output:

```sh
{
    "id": "0738-1d4dbacq-673d-2qed-hf68-858961739gf0",
    "name": "vpn-connection-to-vpn-gateway-1",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/0738-f72559a3-2fac-4958-b937-54474e6a8a8d/connections/0738-1d4dbacq-673d-2qed-hf68-858961739gf0",
    "local_cidrs": [
        "192.168.100.0/24"
    ],
    "peer_cidrs": [
        "192.168.100.0/24"
    ],
    "peer_address": "169.61.161.167",
    "admin_state_up": true,
    "psk": "VPNDemoPassword",
    "dead_peer_detection": {
        "action": "none",
        "interval": 30,
        "timeout": 120
    },
    "created_at": "2018-07-06T19:54:14.961597Z",
    "route_mode": "policy",
    "authentication_mode": "psk",
    "status": "down"
}
```
{: screen}

## Step 5. Verify connectivity
{: #step-5-verify-connectivity}



You can check the status of the VPN connection as follows:

```bash
curl  -X GET "$vpc_api_endpoint/v1/vpn_gateways/$gwid1/connections?version=$api_version&generation=2" \
      -H "Authorization: $iam_token"
```
{: codeblock}

Sample output:

```sh
{
    "first": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/0738-7fd72524-6e2d-49a6-b975-0071efccd89a/connections?limit=10"
    },
    "limit": 10,
    "connections": [
        {
            "id": "0738-a252d380-0784-45ff-8fc0-c2b58e446b4d",
            "name": "vpn-connection-to-vpn-gateway-2",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/vpn_gateways/0738-7fd72524-6e2d-49a6-b975-0071efccd89a/connections/0738-a252d380-0784-45ff-8fc0-c2b58e446b4d",
            "local_cidrs": [
                "192.168.100.0/24"
            ],
            "peer_cidrs": [
                "192.168.0.0/24"
            ],
            "peer_address": "169.61.161.150",
            "admin_state_up": true,
            "psk": "VPNDemoPassword",
            "dead_peer_detection": {
                "action": "none",
                "interval": 30,
                "timeout": 120
            },
            "created_at": "2018-07-06T19:50:49.252072Z",
            "route_mode": "policy",
            "authentication_mode": "psk",
            "status": "up"
        }
    ]
}
```
{: screen}

After the VPN connection is established, you can reach your virtual server instances from the subnets in VPC 1 to the ones in VPC 2, and vice versa.
