---



copyright:
  years: 2019, 2020
lastupdated: "2020-02-14"

keywords: VPN, network, encryption, authentication, algorithm, IKE, IPsec, policies, gateway, auto-negotiation, vpc, vpc network

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

# Using VPN 
{: #using-vpn}
[comment]: # (linked help topic)

You can use the {{site.data.keyword.cloud}} VPN for VPC service to securely connect your VPC to another private network. You can use VPN to set up an IPsec site-to-site tunnel between your VPC and your on-premises private network or another VPC.
{:shortdesc}

Currently, only policy-based routing is supported.

## Beta participants
{: #beta-participants}
14 February 2020: This service is now generally available. You must change to a standard plan to continue using instances that you created during the beta. Any instances that continue to use the beta plan for this service beyond 30 days notice of general availability will be subject to deletion.

## Features
{: #vpn-features}

* IKEv1 and IKEv2
* Authentication algorithms: `md5`, `sha1`, `sha256`
* Encryption algorithms: `3des`, `aes128`, `aes256`
* Diffie-Hellman (DH) groups: 2, 5, 14
* IKE negotiation mode: main
* IPsec transform protocol: ESP
* IPsec encapsulation mode: tunnel
* Perfect Forward Secrecy (PFS)
* Dead Peer Detection
* Routing: Policy-based
* Authentication mode: Pre-shared key
* High availability support in active/standby mode only

## APIs available
{: #apis-available}

You can use the following APIs to create and manage VPNs in your VPC. For more information, see the [VPC REST API reference](https://{DomainName}/apidocs/vpc#list-all-ike-policies){: external}.

### VPN gateways and VPN connections
{: #vpn-gateways-and-vpn-connections}

Table 1 describes API methods for VPN gateways and connections.

| Description | API |
|----------------------------|-------------|
| Create a VPN gateway | `POST /vpn_gateways` |
| Retrieve VPN gateways | `GET /vpn_gateways` |
| Retrieve a VPN gateway | `GET /vpn_gateways/ID` |
| Delete a VPN gateway | `DELETE /vpn_gateways/ID` |
| Update a VPN gateway | `PATCH /vpn_gateways/ID` |
| Create a VPN connection | `POST /vpn_gateways/VPN_GATEWAY_ID/connections` |
| Retrieve VPN connections | `GET /vpn_gateways/VPN_GATEWAY_ID/connections` |
| Retrieve a VPN connection | `GET /vpn_gateways/VPN_GATEWAY_ID/connections/ID` |
| Delete a VPN connection | `DELETE /vpn_gateways/VPN_GATEWAY_ID/connections/ID` |
| Update a VPN connection | `PATCH /vpn_gateways/VPN_GATEWAY_ID/connections/ID` |
| Retrieve all local CIDRs for a VPN connection | `GET /vpn_gateways/VPN_GATEWAY_ID/connections/ID/local_cidrs` |
| Delete a local CIDR from a VPN connection | `DELETE /vpn_gateways/VPN_GATEWAY_ID/connections/ID/local_cidrs/PREFIX_ADDRESS/PREFIX_LENGTH` |
| Check if a specific local CIDR exists on a VPN connection | `GET /vpn_gateways/VPN_GATEWAY_ID/connections/ID/local_cidrs/PREFIX_ADDRESS/PREFIX_LENGTH` |
| Sets a local CIDR on a VPN connection | `PUT /vpn_gateways/VPN_GATEWAY_ID/connections/ID/local_cidrs/PREFIX_ADDRESS/PREFIX_LENGTH` |
| Retrieve all peer CIDRs for a VPN connection | `GET /vpn_gateways/VPN_GATEWAY_ID/connections/ID/peer_cidrs` |
| Delete a peer CIDR from a VPN connection | `DELETE /vpn_gateways/VPN_GATEWAY_ID/connections/ID/peer_cidrs/PREFIX_ADDRESS/PREFIX_LENGTH` |
| Checks if a specific peer CIDR exists on a VPN connection | `GET /vpn_gateways/VPN_GATEWAY_ID/connections/ID/peer_cidrs/PREFIX_ADDRESS/PREFIX_LENGTH` |
| Sets a peer CIDR on a VPN connection | `PUT /vpn_gateways/VPN_GATEWAY_ID/connections/ID/peer_cidrs/PREFIX_ADDRESS/PREFIX_LENGTH` |
{: caption="Table 1. Description of APIs for VPN gateways and connections" caption-side="top"}

### IKE policies
{: #ike-policies}

| Description | API |
|-----------------------------|--------------|
| Retrieve all IKE policies | `GET /ike_policies` |
| Create an IKE policy | `POST /ike_policies` |
| Delete an IKE policy | `DELETE /ike_policies/ID` |
| Retrieve an IKE policy | `GET /ike_policies/ID` |
| Update an IKE policy | `PATCH /ike_policies/ID` |
| Retrieve all the connections that use the specified IKE policy | `GET /ike_policies/ID/connections` |
{: caption="Table 2. Description of APIs for IKE policies" caption-side="top"}

### IPsec policies
{: #ipsec-policies}

| Description | API |
|---------------------|-------------|
| Retrieve all IPsec policies | `GET /ipsec_policies` |
| Create an IPsec policy | `POST /ipsec_policies` |
| Delete an IPsec policy | `DELETE /ipsec_policies/ID` |
| Retrieve an IPsec policy | `GET /ipsec_policies/ID` |
| Update an IPsec policy | `PATCH /ipsec_policies/ID` |
| Retrieve all the connections that use the specified IPsec policy | `GET /ipsec_policies/ID/connections` |
| Retrieve all the connections that use the specified IKE policy | `GET /ike_policies/ID/connections` |
{: caption="Table 3. Description of APIs for IPsec policies" caption-side="top"}

## API example: Connecting two VPCs using VPN
{: #vpn-example}

In the following API example, you connect two VPCs by creating a VPN gateway in each VPC. Because you create the VPN gateways in zone 1 of each VPC, you connect the subnets and associated virtual server instances in VPC 1 zone 1 to the subnets and associated instances in VPC 2 zone 1 as if they were a single network.

The IP addresses of the subnets in the two VPCs must not overlap.
{: important}

Here's what the scenario looks like:

![Figure showing a scenario where you connect two VPCs using VPN](images/vpc-vpn.svg){: caption="Figure 1: Connecting two VPCs using VPN" caption-side="top"}

The following example assumes that you already created VPCs, subnets, and virtual server instances. For more information about creating VPC resources, see [Getting started](/docs/vpc?topic=vpc-getting-started).

You can also create a VPN gateway using the UI. For instructions, see [Creating a VPC using the IBM Cloud console](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#vpn).
{: tip}

### Step 1. Create a VPN gateway in a subnet of your VPC
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

#### Step 2. Create a second VPN gateway in a different VPC
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


#### Step 3. Create a VPN connection from the first VPN gateway to the second VPN gateway
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

#### Step 4. Create a VPN connection from the second VPN gateway to the first VPN gateway
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

#### Step 5. Verify connectivity
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

## Configuring ACLs and security groups for use with VPN
{: #acls-security-groups-vpn}

If you configure access control lists (ACLs) on the subnet in which the VPN gateway is deployed, make sure that the following rules are in place to allow management traffic and VPN tunnel traffic:

* **Inbound rules**
    - Allow protocol TCP source port 9091
    - Allow protocol TCP source port 10514
    - Allow protocol TCP source port 443
    - Allow protocol TCP source port 80
    - Allow protocol TCP source port 53
    - Allow protocol UDP source port 53
    - Allow protocol ALL source IP is VPN peer gateway public IP
    - Allow protocol TCP destination port 443
    - Allow protocol TCP destination port 56500
    - Allow traffic between virtual server instances in your VPC and your on-premises private network
    - Allow ICMP traffic

* **Outbound rules**
   - Allow all traffic

If you use ACLs or security groups on the subnets that need to communicate over the VPN tunnel, make sure that ACL rules or security group rules are in place to allow traffic between virtual server instances in your VPC and the other network.

## Quotas
{: #see-vpn-quotas}

For information about VPN quotas, see [Quotas](/docs/vpc?topic=vpc-quotas#vpn-quotas).

## Policy auto-negotiation
{: #policy-auto-negotiation}

The use of IKE and IPsec policies to configure a VPN connection is optional. When no policy is selected, default proposals are chosen automatically through a process known as _auto-negotiation_.

Because IBM Cloud auto-negotiation uses **IKEv2**, the on-premises device must also use **IKEv2**. Use a customized IKE policy if your on-premises device does not support **IKEv2**.
{: tip}

### IKE auto-negotiation (Phase 1)
{: #ike-auto-negotiation-phase-1}

The following encryption, authentication, and Diffie-Hellman Group options detailed in Table 4 might be used in any combination:

|    | Encryption | Authentication | DH Group |
|----|------------|----------------|----------|
| 1  | aes128 | sha1   | 2  |
| 2  | aes256 | sha256 | 5  |
| 3  | 3des   | md5    | 14 |
{: caption="Table 4. Encryption, authentication, and DH Group options for IPsec auto-negpotion phase 1" caption-side="top"}

### IPsec auto-negotiation (Phase 2)
{: #ipsec-auto-negotiation-phase-2}

The following encryption and authentication options detailed in Table 5 might be used in any combination:

|    | Encryption | Authentication | DH Group |
|----|------------|----------------|----------|
| 1  | aes128 | sha1   | Disabled  |
| 2  | aes256 | sha256 | Disabled  |
| 3  | 3des   | md5    | Disabled  |
{: caption="Table 5. Encryption and authentication options for IPsec auto-negoation phase 2" caption-side="top"}

## Access service endpoints through VPN
{: #build-se-connectivity-using-vpn}

VPC allows access to a service endpoint from an on-premises network through a VPN.

To set up access to a service endpoint:

1. Get the IP of the service endpoint. IBM Cloud supports two types of service endpoints: Infrastructure as a Service (IaaS) endpoints and IBM Cloud service endpoints. The IaaS endpoints are hosted in the IP address ranges 161.26.0.0/16; cloud service endpoints are hosted in the IP address ranges 166.9.0.0/14. For more information about endpoints, see [Endpoints available for VPC](/docs/vpc?topic=vpc-service-endpoints-for-vpc).
2. Create a VPN gateway and a VPN connection. For the VPN connection, the local subnets should include the range 166.9.0.0/14 for IBM Cloud service endpoints or 161.26.0.0/16 for IaaS endpoints.

## Limitations
{: #vpn-limitations}

The VPN gateway resides in the zone associated with the subnet that you chose during provisioning. The VPN gateway serves only the virtual server instances in the same zone of the VPC. Therefore, instances in other zones can't use the VPN gateway to communicate with an on-premises private network. For zone fault tolerance, you must deploy one VPN gateway per zone.

## FAQs
{: #vpn-faq}

### When I create a VPN gateway, can I create VPN connections at the same time?
{: #faq-vpn-0}
{: faq}

If you use the API or CLI, VPN connections must be created after the VPN gateway is created. In the IBM Cloud console, you can create the gateway and a connection at the same time.

### If I delete a VPN gateway that has VPN connections attached, what happens to the connections?
{: #faq-vpn-1}
{: faq}

The VPN connections are deleted along with the VPN gateway.

### Are IKE or IPsec policies deleted if I delete a VPN gateway or VPN connection?
{: #faq-vpn-2}
{: faq}

No, IKE and IPsec policies can apply to multiple connections.

### What happens to a VPN gateway if I try to delete the subnet that the gateway is located on?
{: #faq-vpn-3}
{: faq}

The subnet cannot be deleted if any virtual server instances are present, including the VPN gateway.

### Are there default IKE and IPsec policies?
{: #faq-vpn-4}
{: faq}

When you create a VPN connection without referencing a policy ID (IKE or IPsec), auto-negotiation is used.

### Why do I need to choose a subnet during VPN gateway provisioning?
{: #faq-vpn-5}
{: faq}

The subnet connects the VPN gateway with other resources in your VPC. The best practice is to create a dedicated subnet for the VPN gateway with no virtual server instances on this subnet to ensure that there are enough free private IPs in the subnet. A VPN gateway needs 8 private IP addresses to accommodate HA and rolling upgrades.


### What should I do if I am using ACLs on the subnet that is used to deploy the VPN gateway?
{: #faq-vpn-6}
{: faq}

Make sure the following ACL rules are in place to allow management traffic and VPN tunnel traffic:

* **Inbound rules**
    - Allow protocol TCP source port 9091
    - Allow protocol TCP source port 10514
    - Allow protocol TCP source port 443
    - Allow protocol TCP source port 80
    - Allow protocol TCP source port 53
    - Allow protocol UDP source port 53
    - Allow protocol ALL source IP is VPN peer gateway public IP
    - Allow protocol TCP destination port 443
    - Allow protocol TCP destination port 56500
    - Allow traffic between virtual server instances in your VPC and your on-premises private network
    - Allow ICMP traffic

* **Outbound rules**
   - Allow all traffic

### What should I do if I am using ACLs on the subnets that need to communicate with on-premises private network?
{: #faq-vpn-7}
{: faq}

Make sure that ACL rules rules are in place to allow traffic between virtual server instances in your VPC and your on-premises private network.

### Does VPN for VPC support HA configurations?
{: #faq-vpn-8}
{: faq}

Yes, it supports HA in an Active / Standby configuration.

### Are there plans to support SSL VPN?
{: #faq-vpn-9}
{: faq}

No, only IPsec site-to-site is supported.

### Are there any caps on throughput for site-to-site VPNaaS?
{: #faq-vpn-10}
{: faq}

Up to 650 Mbps of throughput is supported.

### Is PSK and certificate-based IKE authentication supported for VPNaaS?
{: #faq-vpn-11}
{: faq}

Only PSK authentication is supported.

### Can you use VPN for VPC as a VPN gateway for your IBM Cloud classic infrastructure?
{: #faq-vpn-12}
{: faq}

No. To set up a VPN gateway in your classic environment, you must use the [IPsec VPN](https://{DomainName}/catalog/infrastructure/ipsec-vpn){: external}.

### What {{site.data.keyword.cloud_notm}} infrastructure classic resources can be accessed with VPN for VPC?
{: #faq-vpn-13}
{: faq}

Virtual server instances are the only classic resources that can be accessed with VPN for VPC along with Classic Access VPC.

### What will rekey collision cause?
{: #faq-vpn-14}
{: faq}

If you use IKEv1, rekey collision deletes the IKE/IPsec SA. To re-create the IKE/IPsec SA, set the connection admin state to `down` and then `up` again. You can use IKEv2 to minimize rekey collisions.

### Is it possible to view logs from the VPN gateway for debugging purposes?
{: #faq-vpn-15}
{: faq}

Yes. You can find more information in [Using LogDNA to view VPN logs](/docs/vpc?topic=vpc-using-logdna-to-view-vpn-logs).
