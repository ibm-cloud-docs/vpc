---

copyright:
  years: 2018, 2026
lastupdated: "2026-03-28"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I reach network destinations when multiple CIDRs are configured for my VPN connection?
{: #troubleshoot-multi-cidr-oneway-traffic}
{: troubleshoot}
{: support}

This issue occurs when the IBM Cloud VPN connection is configured with multiple CIDRs on the local side, remote side, or both, but the on-premises peer gateway negotiates only one CIDR pair at a time. As a result, only the selected CIDR pair forms a valid IPsec tunnel, and traffic from other CIDRs is dropped.
{: shortdesc}

When traffic is sent from a virtual server instance (for example, `192.168.2.10`) to an on‑premises host such as `10.0.1.1`, packets reach the VPN gateway but are not encrypted or forwarded through the IPsec tunnel. Connectivity from other IP addresses (for example, `192.168.1.5`) might still work because the peer gateway successfully negotiates a tunnel for those specific CIDR or host pairs.
{: tsSymptoms}

The following configuration example illustrates the negotiation scenario:

   * IBM VPN configuration:

      * Local CIDRs: `192.168.1.0/24`, `192.168.2.0/24`
      * Remote CIDRs: `10.0.1.0/24`, `10.0.2.0/24`

   * Peer device supports only one CIDR pair per tunnel

   * Negotiation result:

      * Selected pair: `192.168.1.0/24` and `10.0.1.0/24`

   * Outcome:

      * Traffic between `192.168.1.5` and `10.0.1.1` works
      * Traffic between `192.168.2.10` and `10.0.1.1` fails

The following log file shows the negotiation:

   ```sh
      selecting traffic selectors for us:
      config: 192.168.1.0/24, received: 192.168.1.5/32 => match: 192.168.1.5/32
      config: 192.168.2.0/24, received: 192.168.1.5/32 => no match

      selecting traffic selectors for other:
      config: 10.0.1.0/24, received: 10.0.1.1/32 => match
      config: 10.0.2.0/24, received: 10.0.1.1/32 => no match
      config: 10.0.3.0/24, received: 10.0.1.1/32 => no match

      CHILD_SA peer established with TS 192.168.1.5/32 === 10.0.1.1/32
   ```

* This issue occurs when the IBM Cloud VPN gateway sends multiple local and remote CIDRs to the peer gateway in a single request. However, the peer responds with a single CIDR pair during IKE negotiation.
* When using IKEv2, the IBM Cloud VPN includes all configured CIDRs in its traffic selector proposal. However, some peer devices narrow the proposal and respond only with one acceptable CIDR pair. This behavior is known as traffic selector narrowing.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Review your IBM VPN connection configuration and list all local and remote CIDRs.
1. Check IKE and IPsec logs on the IBM VPN appliance to verify which CIDR pairs the peer successfully negotiated. If a specific IP address is not present, the tunnel does not exist for that IP.
1. Consult your peer gateway documentation for configuration options that are related to tunnel sharing, traffic selector limits, and multiple CIDR support per tunnel.
1. If supported by the peer gateway, modify its configuration to allow multiple CIDRs per tunnel.
1. If the peer gateway cannot negotiate multiple CIDRs in a single request, split the CIDRs into separate VPN connections, one per required CIDR pair to mirror the peer tunnel requirement.
1. If peer allows only a single subnet or CIDR per tunnel, then configure single subnet or CIDR per IBM side connection.
1. Where possible, consolidate multiple smaller CIDRs into a larger network CIDR to reduce negotiation complexity. For example, use `10.243.0.0/24` instead of 8 different IP addresses.
1. If traffic must originate from the on‑premises host, generate traffic from the peer towards the VPN gateway. Some VPN peer devices establish IPsec tunnels only when traffic is initiated from their side. If traffic is initiated from the IBM Cloud side, the peer might not negotiate for all configured CIDR pairs, which can result in missing tunnels.
1. After you apply the changes, verify from IKE and IPsec logs that tunnels are established for all required CIDR pairs.
