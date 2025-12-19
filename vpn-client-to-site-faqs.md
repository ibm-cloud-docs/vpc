---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-19"

keywords: VPN server, faq, faqs, frequently asked questions, vpn, VPN

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for client-to-site VPN servers
{: #faqs-vpn-server}

You might encounter these frequently asked questions when you use {{site.data.keyword.cloud}} {{site.data.keyword.vpn_vpc_short}}.
{: shortdesc}
{: #vpn-faq}

## What does a VPN health status indicate?
{: #faq-vpn-server-health-status}
{: faq}
{: support}

Status descriptions are as follows:

* `Healthy` - Indicates that your VPN server is operating correctly.
* `Degraded` - Indicates that your VPN server has compromised performance, capacity, or connectivity.
* `Faulted` - Indicates that your VPN server is completely unreachable and inoperative.
* `Inapplicable` - Indicates that the health state does not apply because of the current lifecycle state. A resource with a lifecycle state of `failed` or `deleting` will have a health state of inapplicable. A `Pending` resource might also have this state.

## Why can't I save the password/passcode in the VPN client?
{: #faq-vpn-server-save-passcode}
{: faq}
{: support}

The passcode generated from [IBM IAM](https://iam.cloud.ibm.com/identity/passcode){: external} is a Time-based One-Time Passcode (TOTP), which cannot be reused. You must regenerate it each time.

## If I update a VPN server with connected VPN clients, what happens to the VPN clients?
{: #faq-vpn-server-update-VPN-server}
{: faq}
{: support}

The VPN clients are disconnected along with the VPN server. All VPN clients need to re-connect with the VPN server again. When you use the user ID and passcode authentication, you have to retrieve the passcode and initiate the connection from your VPN client.

## Why is the VPN client disconnected?
{: #faq-vpn-server-client-disconnected}
{: faq}
{: support}

The VPN server administrator can specify an idle time of the VPN client. When there is no traffic from the VPN client during the idle time window, the VPN server disconnects the client automatically. You must reinitiate the VPN session from your VPN client if it is disconnected.

## If I delete a VPN server with connected VPN clients, what happens to the VPN clients?
{: #faq-vpn-server-delete-security-group}
{: faq}
{: support}

The VPN clients are disconnected along with the VPN server.

## What happens to a VPN server if I try to delete the subnet that the VPN server is on?
{: #faq-vpn-server-delete-subnet}
{: faq}
{: support}

You cannot delete the subnet if any VPN servers are present.

## How do I update the security group that my VPN server is attached to?
{: #faq-vpn-server-update-sg}
{: faq}
{: support}

1. In the navigation pane, click **VPNs > Client-to-site servers**, and select the VPN server you want to update.
1. Click **Attached security groups > Attach**, and select the security group you want to attach.

## Can I create routes and use the private IPs of the VPN server as next hop?
{: #create-routes-and-use-private-ips}

No, you cannot create routes and use the private IPs. The private IPs are not static and can change at any time. The VPN for VPC service updates routes in the VPC routing table automatically. If you create a VPC routing table and you want the VPN for VPC service to inject routes into the new routing table, you have to add the VPN server to the `accept routes from` flag. You must also remove the VPN server from the `accept routes from` flag if you do not want the VPN server to inject routes into the routing table. For more information, see [Configuring route propagation for VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-route-propagation).

## What happens to a VPN server if I try to delete the security group that the VPN server is attached to?
{: #faq-vpn-server-delete-sg}
{: faq}
{: support}

You cannot delete a security group if any VPN servers are present.


## How do I switch a client-to-site VPN from full tunnel mode to split tunnel mode?
{: #faq-change-configuration-ft-to-st}
{: faq}
{: support}

Follow the steps:

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> **Network** > VPNs**.
1. Click **Client-to-site servers**, and click the name of the VPN server that you want to update.
1. In VPN server details, click **Edit** and select **Split Tunnel** in the Tunnel mode menu.
1. Enter the specific IP addresses or address ranges of the networks that you want to access through the VPN.
1. Click **Save**.

## Why must I choose subnets during VPN server provisioning?
{: #faq-vpn-server-choose-subnet}
{: faq}
{: support}

The server resides in the VPN subnet that you choose. A VPN server needs two available private IP addresses in each subnet to provide high availability and automatic maintenance. It is best if you use the dedicated subnet for the VPN server of size 8, where the length of the subnet prefix is shorter or equal to 29. With dedicated subnets, you can customize the security group and ACL for greater VPN server flexibility.

## Does VPN server support high-availability configurations?
{: #faq-vpn-server-ha}
{: faq}
{: support}

Yes, it supports high availability in an Active/Active configuration. You must choose two subnets if you want to deploy a high availability VPN server with two fault domains. You can also upgrade the stand-alone VPN server to high-availability mode, and downgrade the high-availability VPN server to stand-alone mode.

## Are there any caps on throughput for VPN server?
{: #faq-vpn-server-caps}
{: faq}
{: support}

Up to 600 Mbps of aggregation throughput is supported with a stand-alone VPN server. A maximum of 1200 Mbps of aggregation throughput is supported with a high availability VPN server. Up to 150 Mbps of throughput for a single client connection (applicable for both stand-alone and high availability VPN servers).

## Can I use a VPN server for IBM Cloud classic infrastructure?
{: #faq-vpn-server-with-classic}
{: faq}
{: support}

Yes, you can use a VPN server for IBM Cloud classic infrastructure. However, you must also enable either Classic Access on the VPC, or configure IBM Cloud Transit Gateway to connect the VPC where the VPN server resides.

## What VPN client is supported?
{: #faq-vpn-server-client-version}
{: faq}
{: support}

See [Supported client software](/docs/vpc?topic=vpc-client-to-site-vpn-planning#vpn-client-software) for details.

## What protocol and port can I use for the VPN server?
{: #faq-vpn-server-protocol-port}
{: faq}
{: support}

You can use UDP or TCP and any port number to run the VPN server. UDP is recommended because it provides better performance; port 443 is recommended because other ports might be blocked by internet service providers. If you cannot connect to the VPN server from your VPN client, you can try TCP/443 because it is open on almost all internet service providers.

## What route action should I use?
{: #faq-vpn-server-route-action}
{: faq}
{: support}

The action of the VPN route depends on the route destination:

* If the route destination is in the VPC, or an on-premises private subnet connected using VPN gateway, the route action can be `deliver`; otherwise, it is `translate`.
* If you want to block traffic from the client, use the `drop` route action to forward unwanted or undesirable network traffic to a null or "black hole" route.
* When the route action is `translate`, the source IP is translated to the VPN server private IP before it is sent out from the VPN server. This means that your VPN client IP is invisible from the destination devices.

## What DNS server IP addresses should I use?
{: #faq-vpn-server-dns-server-ip}
{: faq}
{: support}

DNS server IP addresses are optional when you provision a VPN server. You should use `161.26.0.10` and `161.26.0.11` IP addresses if you want to access service endpoints and IaaS endpoints from your client. See [Service endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc#service-endpoints) and [IaaS endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc#infrastructure-as-a-service-iaas-endpoints) for details.

   Use `161.26.0.7` and `161.26.0.8` if you need to resolve private DNS names from your client. See [About DNS Services](/docs/dns-svcs?topic=dns-svcs-about-dns-services) for details.
   {: note}

## How do I update the certificate for the VPN server?
{: #faq-vpn-server-update-certificate}
{: faq}
{: support}

The VPN server is not aware of updates made to a certificate in Secrets Manager. You must re-import the certificate with a different CRN, and then update the VPN server with the new certificate CRN.

## Can I use a customized hostname for the VPN server?
{: #faq-vpn-server-customized-hostname}
{: faq}
{: support}

Yes, you can. You must create a CNAME DNS record and point it to the VPN server hostname in your DNS provider. After that, edit the client profile by replacing direct `remote 445df6c234345.us-south.vpn-server.appdomain.cloud` with `remote your-customized-hostname.com`.

`445df6c234345.us-south.vpn-server.appdomain.cloud` is an example VPN server hostname.
{: note}

If you are using [IBM Cloud Internet Services](/docs/cis?topic=cis-getting-started) as your DNS provider, refer to [CNAME Type record](/docs/cis?topic=cis-set-up-your-dns-for-cis#cname-type-record) for information about how to add a CNAME DNS record.
{: note}

## What information should I provide in an IBM Support case if I need help?
{: #faq-vpn-server-support-info}
{: faq}
{: support}

Supply the following content in your [IBM Support case](/docs/account?topic=account-open-case):

1. Your VPN server ID.
1. Your VPN client and operating system version.
1. The logs from your VPN client.
1. The time range when you encountered the problem.
1. If user-ID-based authentication is used, supply the username.
1. If certificate-based authentication is used, supply the common name of your client certificate.

   To view the common name of your client certificate, use the OpenSSL command `openssl x509 -noout -text -in your_client_certificate_file` in the `subject` section.
   {: tip}

## How do I assign the `VPN client` role using IAM access management tags in the API?
{: #faq-vpn-server-access-tag-mgmt}
{: faq}
{: support}

When you manage user access using access management tags on a client-to-site VPN and enable the `UserId and Passcode` mode for `Client Authentication`, you must attach the `VPN Client` role with an access tag. Otherwise, the VPN client cannot connect to the VPN server. For more information, see [Granting users access to tag IAM-enabled resources by using the API](/docs/account?topic=account-access&interface=api#iam-managed-api) and set the `role_id` to `crn:v1:bluemix:public:is::::serviceRole:VPNClient` to grant access.
