---

copyright:
  years: 2019, 2021
lastupdated: "2021-08-02"

keywords: check point peer

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:important: .important}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:download: .download}


# Connecting to a Check Point Security Gateway peer
{: #check-point-config}

You can use {{site.data.keyword.cloud}} {{site.data.keyword.vpn_vpc_short}} to securely connect your VPC to an on-premises network through a VPN tunnel. This topic provides guidance about how to configure your Check Point Security Gateway to connect to {{site.data.keyword.vpn_vpc_short}}.
{: shortdesc}

These instructions are based on Check Point Security Gateway, Software Release [R80.40]. Earlier versions of Check Point software are not supported.

Read [VPN gateway limitations](/docs/vpc?topic=vpc-vpn-limitations) before continuing to connect to your on-premises peer. 
{:note}

Because Check Point Security Gateway uses IKEv1 by default, you must create a custom IKE and IPsec policy to replace the default auto-negotiation policy for the VPN in your VPC.
{: important}

To support these functions, the following general configuration steps must be performed on the Check Point Security Gateway.

## Connecting an IBM policy-based VPN to a Check Point Security Gateway peer
{: #check-point-config-policy-based}

1. Configure the security gateways that are internally managed:
  * Go to **SmartConsole \> Security & Services** and click the name of security gateway to open the Security Gateway configuration page.
  * Go to the **Network Management** page to define the Topology.
  * Go to the **Network Management \> VPN Domain** page to define the VPN domain.
1. Create an `Interoperable Device` on the Check Point SmartConsole:
  * Go to Object Explorer and click **New \> Network Object \> More \> Interoperable Device** to open the new interoperable device page.
  * Go to the **General Properties** page and enter the IBM VPN gateway name and public IP address.
  * Go to the **Topology page** and add the IBM VPN gateway public IP address and IBM VPC subnets. The IBM VPN public IP address should be added as an external network with netmask `255.255.255.255`. The IBM VPC subnets should be added as an internal network.
1. Add the VPN Community. These instructions are based on type `Star Community`, but type `Meshed Community` is also an option.
  * Go to **SmartConsole \> Security Policies \> Access Tools \> VPN Communities**, click `Star Community` to open the new VPN community page.
  * Enter the new Community name.
  * Go to the **Gateways \> Center Gateways** page, click the `+` icon, and add the Check Point Security Gateway.
  * Go to the **Gateways \> Satellite Gateways** page, click the `+` icon, and add the IBM VPN gateway.
  * Go to the **Encryption** page and use the default `Encryption Method` and `Encryption Suite`.
  * Go to the **Tunnel Management** page and select `One VPN tunnel per subnet pair`.
  * Go to the **Shared Secret** page and set the pre-shared key.
  * Click **OK** and publish the changes.
1. Add relevant access rules in the Security Policy page.
  * Add the Community in the VPN column, the services in the Service & Applications column, the desired Action, and the appropriate Track option.
  * Install the Access Control Policy.

## Creating a custom IKE policy for a Check Point Security Gateway
{: #custom-ike-policy-with-cp}

By default, Check Point Security Gateway uses IKEv1; therefore, you must create a custom IKE policy to replace the default policy for the VPN in your VPC.

To use a custom IKE policy in {{site.data.keyword.vpn_vpc_short}}:
1. On the {{site.data.keyword.vpn_vpc_short}} page in the IBM Cloud console, select the **IKE policies** tab.
1. Click **New IKE policy** and specify the following values:
  * For the **IKE Version** field, select **1**.
  * For the **Authentication** field, select **sha1**.
  * For the **Encryption** field, select **aes256**.
  * For the **DH Group** field, select **2**.
  * For the **Key lifetime** field, specify **86400**.
1. When you create the VPN connection in your VPC, select this custom IKE policy.

## Creating a custom IPsec policy for Check Point Security Gateway
{: #custom-ipsec-policy-with-cp}

To use a custom IPsec policy in {{site.data.keyword.vpn_vpc_short}}:
1. On the {{site.data.keyword.vpn_vpc_short}} page in the IBM Cloud console, select the **IPsec policies** tab.
1. Click **New IPsec policy** and specify the following values:
  * For the **Authentication** field, select **sha1**.
  * For the **Encryption** field, select **aes128**.
  * For the **Key lifetime** field, specify **3600**.
1. When you create the VPN connection in your VPC, select this custom IPsec policy.

## Ensuring NAT-T is always on
{: #checkpoint-defaults}

Make sure that the NAT-T feature is enabled on your on-premises VPN device. The following list shows the default behaviors:

  * NAT-T is enabled when a NAT device is detected.
  * **offer_nat_t_initator** is set to `false` (initiator sends NAT-T traffic).
  * **offer_nat_t_responder_for_known_gw** is set to `true` (responder accepts NAT-T traffic from known gateways).
  * **force_nat_t** is set to `false` (forces NAT-T, even if there is no NAT-T device).

It is recommended to change these default settings to the following:
  * Enable NAT-T
  * Set **offer_nat_t_initator** to `true` 
  * If you know that there's no NAT device in the environment, then set **force_nat_t** to `true`.
 
**Note**: You can view and change these variables using the GuiDBedit Tool. (Refer to the Check Point documentation for your particular version to confirm these steps.)

   1. In the upper-left pane, click **TABLE > Network Objects > network_objects**.
   1. In the upper-right pane, select the applicable Security Gateway object.
   1. In the lower pane, see the VPN section.
   1. To save the changes, click **File > Save All**.
   1. In SmartConsole, install the Access Control Policy on this Security Gateway object.

   For more information, see [NAT-T Compatibility With Check Point Devices ](https://support.cohesive.net/support/solutions/articles/31000156433-nat-t-compatibility-with-check-point-devices){:external}.

