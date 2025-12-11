---
copyright:
  years: 2022
lastupdated: "2025-12-11"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# How do I fix my public ingress route configuration problem?
{: #troubleshoot-pi-configuration-problem}
{: troubleshoot}
{: support}

Correct public ingress route configuration errors as a way to resolve traffic routing errors.
{: shortdesc}

You attempt to configure a public ingress route, but run into issues. For example, the packets continue to be routed to the destination instead of being routed to the next hop IP. No error occurs, but the packets are received in the wrong location.
{: tsSymptoms}

Your routing table or route was not set up correctly.
{: tsCauses}

Make sure that your routing table and route are configured properly.
{: tsResolve}

Confirm that the routing table **Traffic type** is `Ingress` and **Ingress properties** is set to `Public internet`. The following diagram shows this route configuration in the IBM Cloud console.

![Routing table traffic type and ingress property options](images/ts-pi-config.svg "Routing table traffic type and ingress property options"){: caption="Routing table traffic type and ingress property options" caption-side="bottom"}

The custom route **Destination** CIDR matches the public IP of the destination floating IP. The route's **Action** is `Deliver` and the **Next hop** IP matches the virtual server instance's next hop private IP. The following diagram shows this route configuration.

![Route action and next hop IP options](images/ts-pi-config-2.svg "Route action and next hop IP options"){: caption="Route action and next hop IP options" caption-side="bottom"}

Make sure that the routing table and route API settings are configured properly.

At the routing table API level:

- `route_internet_ingress` is set to `true`.
- route_table_id: `<routing_table_id>`
- traffic_source: `if`

At the route API level:

- The **Action** must be `Deliver`.
- The **Destination** must be floating-IP **public IP**.
- The **Next_hop** must be `private IP`.
- The **floating-IP** (attached port) and the **Next_hop** must be defined on the same zone.

See the following example:

- **Action**: `Deliver`
- **cidr**: `99.74.80.0/28` # fip public ip
- **target_ipv4_addr**: `192.168.100.101` # private ip
