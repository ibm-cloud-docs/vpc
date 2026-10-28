---

copyright:
  years: 2026
lastupdated: "2026-10-28"

keywords: VPN, VPN gateway, regional VPN, VPN member, update member, move zone

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

[Regional VPN start]{: tag-purple}

# Updating a regional VPN gateway member subnet
{: #vpn-regional-member-subnet-update}

You can update the subnet of a regional route-based VPN gateway member to move it to a different subnet within the same zone, or to a subnet in a different zone. You do not need to delete and re-create the gateway.
{: shortdesc}

Updating a member's subnet re-creates the VPN appliance in the new subnet. During this operation, the member's lifecycle state changes to `Updating`, and VPN connections through that member experience a brief disruption. For regional VPN gateways, traffic automatically fails over to the other member during the update.
{: important}

## Before you begin
{: #vpn-update-member-prereqs}

* Confirm that the gateway is a **regional** route-based VPN gateway. This operation is not available for zonal gateways or policy-based gateways.
* Ensure that the target subnet has enough space for the VPN appliance and two reserved IP addresses.
* If you move the member to a different zone and have static ingress routes configured, plan to update those routes after the move. See [Ingress routing for a regional static route-based VPN gateway](/docs/vpc?topic=vpc-advertise-routes-s2s#ingress-routing-regional-vpn).
* Ensure that you have the required IAM permissions. VPN member operations require the `vpn.read` and `vpn.update` permissions, which are the same permissions that VPN gateway operations require.

The following table lists the IAM action and Activity Tracker event for each member API operation:

| API | IAM action | Activity Tracker event |
| --- | --- | --- |
| List members (`GET /members`) | `vpn.read` | `is.vpn.vpn-member.list` |
| Get member (`GET /members/{id}`) | `vpn.read` | `is.vpn.vpn-member.read` |
| Update member (`PUT /members/{id}`) | `vpn.update` | `is.vpn.vpn-member.update` |
{: caption="API, IAM action, and Activity Tracker events" caption-side="top"}

Activity Tracker generates the following events for member operations: `is.vpn.vpn-member.read`, `is.vpn.vpn-member.list`, and `is.vpn.vpn-member.update`. For more information, see [VPN gateway events](/docs/vpc?topic=vpc-at_events#events-vpns).
{: note}

## What happens during an update
{: #vpn-update-member-behavior}

The behavior depends on whether you move the member within the same zone or to a different zone:

- Same-zone move: The appliance is re-created in the new subnet within the same zone. The member gets a new private IP address, and its floating IP address is retained.
- Cross-zone move: The appliance is re-created in the new subnet in the new zone. The member gets a new private IP address and a new floating IP address for the new zone.

In both cases, any VPN connections through the updated member experience a brief disruption. For a regional gateway, the other member continues handling traffic during the update.

If a dynamic (BGP) regional VPN gateway is connected to a transit gateway, the VPN service automatically notifies the transit gateway of the zone or IP change. The transit gateway updates its generic routing encapsulation (GRE) tunnels to reflect the new appliance location.
{: note}

If you move a member to a different zone and have static ingress routes configured, update those routes to point to the appliance in its new zone. Routes for the previous zone are no longer valid.
{: important}

## Updating a member subnet in the console
{: #vpn-update-member-subnet-ui}
{: ui}

To update a regional VPN gateway member in the console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **VPNs**.
1. Select the **Site-to-site gateways > VPN gateways** tabs.
1. Click the name of the regional VPN gateway that you want to update.
1. On the gateway details page, click the **Members** tab.
1. Identify the member that you want to update and click the **Edit** icon ![Edit icon](../icons/edit-tagging.svg "Edit").
1. Select a new subnet for the member. The subnet can be in the same zone as the current member, or in a different zone within the same region.
1. Click **Save**. The member lifecycle state changes to `updating` while the appliance is re-created in the new subnet.

## Updating a member subnet from the CLI
{: #vpn-update-member-subnet-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To update a regional VPN gateway member's subnet from the CLI, run the following command:

```sh
ibmcloud is vpn-gateway-member-replace VPN_GATEWAY --member MEMBER_ID --private-ip-subnet SUBNET_ID
```
{: pre}

Where:

VPN_GATEWAY
:   The name or ID of the regional VPN gateway.

--member MEMBER_ID
:   The ID of the VPN gateway member to update.

--private-ip-subnet SUBNET_ID
:   The ID of the new subnet for the member. The subnet can be in the same zone or a different zone within the same region.

### Command examples
{: #vpn-update-member-subnet-cli-examples}
{: cli}

List members of a VPN gateway to find member IDs:

```sh
ibmcloud is vpn-gateway-members my-regional-vpn
```
{: pre}

Retrieve details of a specific member:

```sh
ibmcloud is vpn-gateway-member my-regional-vpn --member 0717-6d7cc5fb-8f26-4847-9b3a-e2c5e1b4c5a1
```
{: pre}

Move a member to a new subnet in a different zone:

```sh
ibmcloud is vpn-gateway-member-replace my-regional-vpn --member 0717-6d7cc5fb-8f26-4847-9b3a-e2c5e1b4c5a1 --private-ip-subnet 0717-e120cc2a-0ff8-4348-abaa-a03d8934ca3f
```
{: pre}

## Updating a member subnet with the API
{: #vpn-update-member-subnet-api}
{: api}

To update a VPN gateway member's subnet with the API, follow these steps:

1. Set up your API environment with the required variables.

1. Store the required variables:

   ```sh
   export vpn_gateway_id=<your_vpn_gateway_id>
   export member_id=<your_member_id>
   export new_subnet_id=<your_new_subnet_id>
   ```
   {: pre}

1. Run the following command to update the member's subnet:

   ```sh
   curl -X PUT "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/members/$member_id?version=$api_version&generation=2" \
     -H "Authorization: Bearer $iam_token" \
     -H "Content-Type: application/json" \
     -d '{
       "subnet": {
         "id": "'"$new_subnet_id"'"
       }
     }'
   ```
   {: pre}

A successful request returns an HTTP `204 No Content` response. The member's `lifecycle_state` changes to `updating` while the appliance is re-created in the new subnet.

[Regional VPN end]{: tag-purple}
