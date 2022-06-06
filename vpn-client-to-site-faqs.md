---

copyright:
  years: 2021, 2022
lastupdated: "2022-06-06"

keywords: VPN server, faq, faqs, frequently asked questions, vpn, VPN

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for client-to-site VPN servers
{: #faqs-vpn-server}

You might encounter these frequently asked questions when you use {{site.data.keyword.cloud}} {{site.data.keyword.vpn_vpc_short}}.
{: shortdesc}
{: #vpn-faq}

## What does a VPN health status indicate?
{: #faq-vpn-server-health-status}
{: faq}
{: support}

Status descriptions are as follows:

* `Heathy` - Indicates that your VPN server is operating correctly.
* `Degraded` - Indicates that your VPN server has compromised performance, capacity, or connectivity.
* `Faulted` - Indicates that your VPN server is completely unreachable and inoperative.
* `Inapplicable` - Indicates that the health state does not apply because of the current lifecycle state. A resource with a lifecycle state of `failed` or `deleting` will have a health state of inapplicable. A `Pending` resource might also have this state.

## Why can't I save the password/passcode in the VPN client?
{: #faq-vpn-server-save-passcode}
{: faq}
{: support}

The passcode generated from [IBM IAM](https://iam.cloud.ibm.com/identity/passcode) is a Time-based One-Time Passcode (TOTP), which cannot be reused. You must regenerate it each time.

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

You cannot update the security group attached to the VPN server after the server is provisioned. However, you can change the rules of the security group. The rules take effect immediately on the VPN server.

## Can I create routes and use the private IPs?
{: #create-routes-and-use-private-ips}

No, you cannot create routes and use the private IPs because the private IPs are not static. They can change at any time.

## What happens to a VPN server if I try to delete the security group that the VPN server is attached to?
{: #faq-vpn-server-delete-sg}
{: faq}
{: support}

You cannot delete a security group if any VPN servers are present.

## Why must I choose subnets during VPN server provisioning?
{: #faq-vpn-server-choose-subnet}
{: faq}
{: support}

The server resides in the VPN subnet that you choose. A VPN server needs two available private IP addresses in each subnet to provide high availability and automatic maintenance. It is best if you use the dedicated subnet for the VPN server of size 8, where the length of the subnet prefix is shorter or equal to 29. With dedicated subnets, you can customize the security group and ACL for greater VPN server flexibility.

## Does VPN server support high-availability configurations?
{: #faq-vpn-server-ha}
{: faq}
{: support}

Yes, it supports high availability in an Active/Active configuration. You must choose two subnets if you want to deploy a high availability VPN server with two fault domains.

## Are there any caps on throughput for VPN server?
{: #faq-vpn-server-caps}
{: faq}
{: support}

Up to 600 Mbps of throughput is supported with a stand-alone VPN server. A maximum of 1200 Mbps of throughput is supported with a high availability VPN server.

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

DNS server IP addresses are optional when you provision a VPN server. You should use `161.26.0.10` and `161.26.0.11` IP addresses if you want to access service endpoints and IaaS endpoints from your client. See [Service endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc#cloud-service-endpoints) and [IaaS endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc#infrastructure-as-a-service-iaas-endpoints) for details.

   Use `161.26.0.7` and `161.26.0.8` if you need to resolve private DNS names from your client. See [About DNS Services](/docs/dns-svcs?topic=dns-svcs-about-dns-services) for details.
   {: note}

## How do I update the certificate for the VPN server?
{: #faq-vpn-server-update-certificate}
{: faq}
{: support}

The VPN server is not aware of updates made to a certificate in the certificate manager. You must re-import the certificate with a different CRN, and then update the VPN server with the new certificate CRN.

## What information should I provide in an IBM Support case if I need help?
{: #faq-vpn-server-support-info}
{: faq}
{: support}

What information should I provide if I need support?
{: shortdesc}

Supply the following content in your [IBM Support case](/docs/get-support?topic=get-support-open-case):
{: tsResolve}

1. Your VPN server ID.
1. Your VPN client and operating system version.
1. The logs from your VPN client.
1. The time range when you encountered the problem.
1. If user-ID-based authentication is used, supply the username.  
1. If certificate-based authentication is used, supply the common name of your client certificate.

   To view the common name of your client certificate, use the OpenSSL command `openssl x509 -noout -text -in your_client_certificate_file` in the `subject` section.
   {: tip}
