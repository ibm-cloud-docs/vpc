---

copyright:
  years: 2022, 2025
lastupdated: "2025-06-27"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Securing your environment with VPN server
{: #client-to-site-vpn-securing-environment}

A client-to-site VPN server can help you secure your environment and meet compliance requirements.
{: shortdesc}

## Using security groups and ACLs for boundary protection
{: #vpn-server-boundary-protection}

VPN servers support integration with VPC security groups and Access Control Lists (ACLs). For more information, see [About security groups](/docs/vpc?topic=vpc-using-security-groups) and [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls).

To protect your environment against unexpected or unauthorized communication, configure your ACL and security group rules to deny all traffic by default and allow only expected protocols, CIDRs, or ports. [Configuring security groups and ACLs for use with VPN](/docs/vpc?topic=vpc-vpn-client-to-site-security-groups) provides examples of rules that are needed for the VPN server to function properly. You can use additional rules to restrict access with a VPN server port to specific networks instead of `Any` or `0.0.0.0/0`.

## Access control with VPN client authentication
{: #vpn-server-access-control}

Client-to-site VPN servers support certificate authentication and user/passcode authentication. See [Setting up client-to-site authentication](/docs/vpc?topic=vpc-client-to-site-authentication) and [VPN client authentication](/docs/vpc?topic=vpc-client-to-site-vpn-planning#client-authentication) for more information on the configuration required with each method.

You can use both authentication methods together to authenticate client access. Use the following guidelines for best results.

### Authenticating with a username/passcode
{: #vpn-server-authenticate-with-username-passcode}

To authenticate with a username or passcode, be aware of the following:

* This option requires VPN client users to authenticate with your wanted MFA method before a valid passcode can be acquired and used for a VPN connection. It can be configured from the IAM Authentication section (**Manage > Access (IAM) > Settings > Authentication**). For more information, see [Enabling multifactor authentication](/docs/account?topic=account-enablemfa).
* Username/passcode authentication requires a user to have the IAM VPN server user role. For more information, see [Creating an IAM access group and granting the role to connect to the VPN server](/docs/vpc?topic=vpc-create-iam-access-group). You must configure IAM access groups or access policies to enable this role only for users that require VPN access.

### Authenticating with client certificates
{: #vpn-server-authenticate-with-client-certs}

To authenticate with client certificates, be aware of the following:

* Use unique client certificates. The VPN server configuration of client certificate authentication requires a client certificate as input. However, unique client certificates signed by the same Certificate Authority (CA) can be created separately and used for different users.
* Use a Certificate Revocation List (CRL) to revoke client certificates. Client-to-site VPN servers support CRL to revoke client certificates. For more information, see [Creating a VPN server](/docs/vpc?topic=vpc-vpn-create-server).
* Do not use a client certificate signed with a public CA, such as LetsEncrypt certificates created through Secrets Manager. Using public-CA-signed client certificates means that anyone can provision a client certificate and authenticate successfully. Also, certificates that are created with a public CA cannot be easily tracked and revoked with a CRL.

## Additional VPN server configurations
{: #vpn-server-additional-configurations}

Client-to-site VPN servers support a number of additional options that you can configure for improved security and to meet compliance requirements. For more information, see [Creating a VPN server](/docs/vpc?topic=vpc-vpn-create-server) and [Planning considerations for VPN servers](/docs/vpc?topic=vpc-client-to-site-vpn-planning).

You should be aware of the following issues with VPN server configurations:

* VPN servers support client idle timeout. By default, a VPN client connection disconnects after 10 minutes without active traffic. You can edit the timeout to add your required value.
* Enable full-tunnel mode on VPN servers. Full-tunnel mode is more secure and preferred, especially if connecting from an untrusted network. In full-tunnel mode, all traffic from a VPN client is routed to the VPN server.
* Configure the transport protocol and VPN ports. In some cases, you might not want to leave well-known ports, such as TCP/443, open. Edit this option to use a non-default transport protocol and VPN ports.
* Configure {{site.data.keyword.atracker_full_notm}}  to receive and analyze VPN server logs. For more information, see [Activity tracking events](/docs/vpc?topic=vpc-at_events&interface=ui#events-vpn-server).
