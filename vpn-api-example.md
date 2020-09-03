---



copyright:
  years: 2019, 2020
lastupdated: "2020-08-12"

keywords: VPN, network, encryption, authentication, algorithm, IKE, IPsec, policies, gateway, api example

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:important: .important}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# API example: Connecting two VPCs using VPN
{: #vpn-example}

The following API example describes how to connect two VPCs by creating a VPN gateway in each VPC. Because you create the VPN gateways in zone 1 of each VPC, you connect the subnets and associated virtual server instances in VPC 1 zone 1 to the subnets and associated instances in VPC 2 zone 1 as if they were a single network. The IP addresses of the subnets in the two VPCs must not overlap.
{: shortdesc}

You can use VPN to connect two VPCs. However, it is recommended to use IBM Cloud Transit Gateway for VPC-to-VPC connectivity. For more information, see [Getting Started with IBM Cloud Transit Gateway](https://test.cloud.ibm.com/docs/transit-gateway).
{: important}

Here's what the scenario looks like:

![Figure showing a scenario where you connect two VPCs using VPN](images/vpc-vpn.svg){: caption="Figure 1: Connecting two VPCs using VPN" caption-side="top"} 

The following example assumes that you already created VPCs, subnets, and virtual server instances. For more information about creating VPC resources, see [Getting started](/docs/vpc?topic=vpc-getting-started).

You can also create a VPN gateway using the UI. For instructions, see [Using the {{site.data.keyword.cloud_notm}} console to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#vpn-ui).
{: tip}

## Step 1. Create a VPN gateway in a subnet of your VPC
{: #step-1-create-a-vpn-gateway-in-your-vpc-subnet}

For best performance, create the VPN gateway in a subnet without any other VPC resources to ensure that there are enough available private IPs for the gateway. A VPN gateway needs 8 private IP addresses to accommodate high availability and rolling upgrades.

The VPN gateway is created in the zone that is associated with the subnet you select. Because the VPN gateway can connect to virtual server instances in this zone only, instances in other zones can't use this VPN gateway to communicate with the other network. For zone fault tolerance, deploy one VPN gateway per zone.
{: tip}


```bash
curl -H "Authorization: $iam_token" -X POST "$api_endpoint/v1/vpn_gateways?version=$api_version&generation=2" \
    -d '{
            "name": "vpn-gateway-1",
            "subnet": {"id": $subnet1}
        }'
```
{: codeblock}

Sample output:
```
{
    "id": "0738-7fd72524-6e2d-49a6-b975-0071efccd89a",
    "crn": "crn:v1:staging:public:is:us-south:a/b668aa2600ac21c890aef16a6210b2fd::vpn:0738-7fd72524-6e2d-49a6-b975-0071efccd89a",
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

Save the following fields for subsequent steps.
* `id`. The ID of the VPN gateway in VPC 1, referred to in this example as `$gwid1`.
* `address`. The public IP address of the VPN gateway in VPC 1, referred to in this example as `$gwaddress1`.

The gateway status appears as `pending` while the VPN gateway is being created, and the status becomes `available` when creation is complete. Creation might take some time.

You can check the gateway's status with the following command:

```bash
curl -H "Authorization: $iam_token" -X GET "$api_endpoint/v1/vpn_gateways/$gwid1?version=$api_version&generation=2"
```
{: codeblock}

## Step 2. Create a second VPN gateway in a different VPC
{: #step-2-create-a-second-vpn-gateway-on-a-different-vpc}

```bash
curl -H "Authorization: $iam_token" -X POST "$api_endpoint/v1/vpn_gateways?version=$api_version&generation=2" \
        -d '{
                "name": "vpn-gateway-2",
                "subnet": {"id": $subnet2}
            }'
```
{: codeblock}

Sample output:
```
{
    "id": "0738-f72559a3-2fac-4958-b937-54474e6a8a8d",
    "crn": "crn:v1:staging:public:is:us-south:a/b668aa2600ac21c890aef16a6210b2fd::vpn:0738-f72559a3-2fac-4958-b937-54474e6a8a8d",
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

Save the following fields for subsequent steps.
* `id`. The ID of the VPN gateway in VPC 2, referred to in this example as `$gwid2`.
* `address`. The public IP address of the VPN gateway in VPC 2, referred to in this example as `$gwaddress2`.


## Step 3. Create a VPN connection from the first VPN gateway to the second VPN gateway
{: #step-3-create-a-vpc-connection-from-the-first-vpn-gateway-to-the-second-vpn-gateway}

When you create the connection for the VPN gateway of VPC 1, set `local_cidrs` to the subnets on VPC 1 and set `peer_cidrs` to the subnets on VPC 2.

```bash
curl -H "Authorization: $iam_token" -X POST "$api_endpoint/v1/vpn_gateways/$gwid1/connections?version=$api_version&generation=2" \
        -d '{
                "name": "vpn-connection-to-vpn-gateway-2",
                "peer_address": $gwaddress2,
                "psk": "VPNDemoPassword",
                "local_cidrs": [ "<LOCAL-CIDR>" ],
                "peer_cidrs": [ "<PEER-CIDR>" ]
            }'
```
{: codeblock}

Sample output:

```
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

When you create the connection for the VPN gateway of VPC 2, set `local_cidrs` to the subnets on VPC 2 and `peer_cidrs` to the subnets on VPC 1.

```bash
curl -H "Authorization: $iam_token" -X POST "$api_endpoint/v1/vpn_gateways/$gwid2/connections?version=$api_version&generation=2" \
        -d '{
                "name": "vpn-connection-to-vpn-gateway-1",
                "peer_address": $gwaddress1,
                "psk": "VPNDemoPassword",
                "local_cidrs": [ "<LOCAL-CIDR>" ],
                "peer_cidrs": [ "<PEER-CIDR>" ]
            }'
```
{: codeblock}

Sample output:
```
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

After the VPN connection is established, you can reach your virtual server instances on subnet 2 from subnet 1, and vice versa.

You can check the status of the VPN connection as follows:
```bash
curl -H "Authorization: $iam_token" -X GET "$api_endpoint/v1/vpn_gateways/$gwid1/connections?version=$api_version&generation=2"
```
{: codeblock}

Sample output:
```
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
{: codeblock}
