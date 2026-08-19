---

copyright:
  years: 2021, 2026
lastupdated: "2026-08-19"

keywords: virtual private network, VPN, VPN server, troubleshooting, client-to-site, authentication, certificate, connection failure

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I establish or maintain a connection to my VPN server?
{: #troubleshoot-c2s-connection-establishment}
{: troubleshoot}
{: support}

Client-to-site VPN connection issues can prevent users from connecting to a VPN server or cause established sessions to disconnect unexpectedly. These issues commonly occur due to authentication failures, certificate validation problems, TLS handshake errors, network connectivity issues, or configuration mismatches.
{: shortdesc}

You might experience one or more of the following issues:
{: tsSymptoms}

- The VPN connection fails immediately after entering credentials.
- The VPN client displays `AUTH_FAILED` or `User authentication failed`.
- The VPN client disconnects during certificate validation or TLS negotiation.
- The VPN connection disconnects intermittently.
- Connectivity stops working after configuration changes.

Client-to-site VPN connection failures can occur because of several interconnected factors:
{: tsCauses}

**Authentication and authorization issues**
:   Occur when invalid IAM credentials are used, one-time passcodes expire, or users don't have the required IAM permissions to access the VPN server.

**Certificate and TLS validation failures**
:   Occur when the VPN server can't validate the client certificate chain, the client certificate fails verification, or the VPN profile contains outdated certificate authority (CA) information.

**VPN server certificate configuration issues**
:   Occur when VPN server certificates are inaccessible, incorrectly referenced, or improperly generated.

**Network connectivity and configuration issues**
:   Occur because of security group restrictions, transport interruptions, service provider issues, or outdated VPN client configuration profiles.

Follow these steps to diagnose and resolve VPN connection issues:
{: tsResolve}

## Diagnosing VPN connection issues
{: #diagnose-c2s-connection-issues}

Before you modify the VPN server or client configuration, determine where the failure occurs.

1. Verify that the VPN server is provisioned successfully and available to accept client connections. For more information, see [VPN server states and descriptions](/docs/vpc?topic=vpc-vpn-server-states&interface=ui).

1. Review the VPN client logs for authentication, certificate, TLS handshake, or transport-related errors. For more information, see [Diagnosing VPN server health](/docs/vpc?topic=vpc-vpn-server-health&interface=ui).

1. Verify connectivity to the VPN endpoint by confirming that DNS resolution and connectivity to the VPN server are working correctly.

### Resolve network connectivity and configuration issues
{: #resolve-network-connectivity}

Network interruptions, DNS resolution failures, and configuration changes can prevent successful VPN connections even when authentication succeeds.

#### Verify configuration changes
{: #verify-config-changes}

Changes to VPN authentication methods or network configuration can invalidate existing VPN client profiles.

1. Verify that the VPN client profile contains all required certificate and authentication settings.

1. If the VPN client authentication configuration changes, download a new VPN client profile.

   For example, if you update the authentication method from certificate only to both certificate and user ID + passcode authentication, you must download the client profile again and reconfigure the client certificates.

1. If the VPN client certificate in the Secrets Manager is changed, make sure that the VPN server is configured with the correct client certificate.

1. Verify that security groups and network ACLs allow the required VPN traffic and that the VPN ports and protocols are added to the inbound and outbound rules.

1. Retry the connection by using the updated configuration.

#### Troubleshoot transport and connectivity errors
{: #troubleshoot-network-eof}

Unexpected transport interruptions can cause VPN connection failures.

You might observe log messages similar to the following message:

   ```sh
   TCP recv EOF
   Transport Error: NETWORK_EOF_ERROR
   ```
   {: screen}

1. Verify that the VPN hostname resolves correctly by using DNS lookup tools such as `nslookup` or `dig`.

1. Confirm that the VPN endpoint is reachable by following these steps:

   - For TCP, enter the following command:

   ```sh
   nc -zv <vpn-hostname> 443
   ```
   {: codeblock}

   - For UDP, enter the following command:

   ```sh
   nc -zvu <vpn-hostname> 443
   ```
   {: codeblock}

1. Verify that your client is using valid and reachable DNS servers.

1. Flush the DNS cache or restart the network service on the client system.

1. If the issue occurs only on a specific system, review local firewall, DNS, and operating system network settings.

1. Retry the VPN connection after confirming that hostname resolution and connectivity are working correctly.

## Resolving authentication and authorization issues
{: #resolve-authentication-issues}

Authentication failures occur when users provide incorrect credentials or don't have sufficient permissions to connect to the VPN server.

### Use a valid IAM passcode
{: #use-valid-passcode}

If the client-to-site VPN uses a username and passcode based authentication, the IAM passcode is a time-based one-time passcode (TOTP) and must be regenerated periodically.

You might observe log messages similar to the following examples:

   ```sh
   AUTH_FAILED

   SIGUSR1[soft,auth-failure] received, process restarting
   ```
   {: screen}

1. Go to [https://iam.cloud.ibm.com/identity/passcode](https://iam.cloud.ibm.com/identity/passcode){: external} and generate a new passcode.

1. Use your IAM username and the newly generated passcode when connecting to the VPN server.

   The passcode is a Time-based One-time passcode (TOTP) and cannot be reused.
   {: note}

1. Retry the connection.

### Verify user permissions and access groups
{: #verify-user-permissions}

The VPN client can successfully reach the VPN server but fails authentication when the user doesn't have the required IAM roles or access policies.

You might observe an error similar to the following message:

   ```sh
   User authentication failed
   ```
   {: screen}

1. Verify that the client that's being connected is assigned the correct IAM access group.

1. Confirm that the access group contains policies that grant the client access to the VPN server. For more information, see [Creating an access group](/docs/vpc?topic=vpc-create-iam-access-group).

1. Verify that the **Users of the VPN Server need this role to connect to the VPN server** option is enabled in the VPN Client service access settings.

1. Retry the VPN connection after the IAM permissions are updated.

## Resolving VPN client connection failures
{: #resolve-c2s-client-connection-failures}

If the VPN client hangs during connection establishment and reports TLS negotiation errors, verify that the VPN server is reachable and that client authentication components are valid.

### Verify network connectivity to the VPN server
{: #verify-c2s-network-connectivity}

You might see an error similar to the following message:

   ```text
   TLS Error: TLS key negotiation failed to occur within 60 seconds (check your network connectivity)
   ```
   {: screen}

Follow these steps to resolve the error:

1. Verify that the security group that is attached to the VPN server allows the configured VPN protocol and port.

1. Verify that the subnet ACL associated with the VPN server permits the required inbound and outbound VPN traffic.

1. Confirm that no corporate firewall, ISP, or intermediate network device is blocking the VPN protocol or port.

1. If network restrictions exist, consider configuring the VPN server to use a commonly allowed port such as TCP `443`.

### Verify certificates and CRL configuration
{: #verify-c2s-certificates}

Authentication can fail if client certificates or certificate revocation information are no longer valid.

1. Check the client certificate expiration date.

   For Linux clients, use the following command:

   ```sh
   openssl x509 -enddate -noout -in client.crt
   ```
   {: pre}

1. Replace expired client certificates.

1. Verify that the certificate revocation list (CRL) is current and uploaded to the VPN server.

1. Update the CRL if it is expired or outdated. For more information, see [client certificate revocation lists](/docs/vpc?topic=vpc-client-to-site-vpn-planning#client-certificate-revocation-lists).

## Resolving certificate and TLS validation issues
{: #resolve-certificate-validation}

Certificate and TLS validation failures can prevent the VPN client from verifying the VPN server identity or client certificates.

### Validate the VPN server certificate chain
{: #validate-server-cert-chain}

The VPN client must be able to validate the complete certificate chain of the VPN server.

You might observe log messages similar to the following examples:

   ```sh
   SESSION is ACTIVE
   Sending PUSH_REQUEST to server...
   AUTH_FAILED
   EVENT: AUTH_FAILED
   EVENT: DISCONNECTED
   ```
   {: screen}

This issue commonly occurs when the VPN client profile contains an outdated or incorrect intermediate certificate authority (CA) certificate.

1. Identify the certificate that is associated with the VPN server.

1. Determine the intermediate CA that issued the server certificate (for example, a Let's Encrypt R‑series intermediate).

1. Open the VPN client profile (`.ovpn`) file that is used to connect to the VPN server.

1. Verify that the profile contains the correct intermediate and root CA certificates that correspond to the VPN server certificate.

1. Replace any outdated CA certificates with the current certificates that are provided by the certificate issuer. For more information, see [Managing VPN server and client certificates](/docs/vpc?topic=vpc-client-to-site-authentication&interface=ui#creating-cert-manager-instance-import).

1. Save the updated VPN client profile and reconnect to the VPN server.

1. If the connection issue persists, regenerate the VPN client profile and make sure that the correct intermediate and root CA certificates are included before you retry the connection.

If you use an older version of OpenVPN client, make sure you update it to the latest version.
{: note}

### Resolve certificate not found errors
{: #resolve-certificate-not-found}

When you create a client-to-site VPN server by using API, CLI, or Terraform, the VPN creation fails and returns an error.

   ```text
   "code": "vpn_server_certificate_not_found",

   "message": "The certificate could not be found. Please try again with a correct certificate CRN."
   ```
   {: screen}

This issue can occur when the specified certificate CRN is incorrect or invalid, or when the VPN service doesn't have permission to access certificates that are stored in Secrets Manager.

1. Verify that the certificate CRN is correct.

1. Confirm that the certificate exists in IBM Cloud Secrets Manager.

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.

1. Create an IAM service-to-service authorization. For more information, see [Creating an IAM service-to-service authorization](/docs/vpc?topic=vpc-client-to-site-authentication&interface=ui#creating-iam-service-to-service).

1. After you create the authorization, the VPN service can access the certificate in Secrets Manager, and the VPN provisioning completes successfully.

#### Resolve parent CA URL missing errors
{: #resolve-parent-ca-url-missing}

When you create or update a client-to-site VPN server, certificate validation can fail and return an error similar to the following:

   ```sh
   The parent CA's URL does not encode in CA of CA chain of certificate <certificate_crn>
   ```
   {: screen}

This error occurs when the certificate chain does not contain the required CA Issuer URI information. The VPN service uses the CA Issuer URI to validate the complete certificate chain. If the root CA or intermediate CA certificates were created without embedding the CA chain URL, any certificates that are issued from that CA hierarchy can't be validated by the VPN service.

Follow these steps to resolve the issue:

1. Verify that the root CA and intermediate CA certificates are created with CRL building enabled.
1. Ensure that the Encode URL option is selected when you create the CA certificates.
1. Re-create the root CA and intermediate CA certificates if the CA Issuer URI is missing.
1. Generate new client and server certificates from the updated CA hierarchy.
1. Store the new certificates in Secrets Manager and update the VPN server configuration with the new certificate CRNs.

After the certificates are re-created with the embedded CA chain URL information, the VPN server can successfully validate the certificate chain, and the update operation completes successfully. For more information, see [Setting up client-to-site authentication](/docs/vpc?topic=vpc-client-to-site-authentication#using-private-certificate).

Existing certificates that contain valid CA Issuer URI information continue to function normally. Certificate renewal alone does not resolve this issue if the newly generated certificates are missing the embedded CA chain URL information.
{: note}

### Resolve Easy-RSA certificate generation failures
{: #resolve-easyrsa-errors}

When you generate certificates with Easy-RSA, certificate creation can fail if the Easy-RSA public key infrastructure (PKI) isn't initialized correctly.

You might observe errors similar to the following messages:

   ```text
   Easy-RSA error:
   sign_req - Randomize Serial number failed

   system library:fopen: No such file or directory
   BIO routines:BIO_new_file:no such file
   ```
   {: screen}

1. Download or clone the Easy-RSA repository into a clean directory.

1. Change to the Easy‑RSA directory and initialize the PKI by running the following command:

   ```sh
   ./easyrsa init-pki
   ```
   {: pre}

1. Create or rebuild the certificate authority before you generate the VPN server certificate:

   ```sh
   ./easyrsa build-ca nopass
   ```
   {: pre}

1. Generate the VPN server certificate again.

1. If the issue persists, create a new private certificate by using IBM Cloud Secrets Manager and associate it with the VPN server instead of generating the certificate locally with Easy‑RSA. For more information, see [Setting up client-to-site authentication](/docs/vpc?topic=vpc-client-to-site-authentication).

## Resolving connectivity issues from office networks
{: #resolve-c2s-office-network-connectivity}

Corporate networks often enforce MTU restrictions through firewalls, proxies, and routers. When VPN traffic exceeds the supported MTU, packets can be fragmented or dropped, which causes intermittent connectivity or connection failures.

### Reduce packet size
{: #reduce-c2s-packet-size}

Follow these steps to resolve the issue:

1. Add the following setting to the OpenVPN client configuration:

   ```text
   tun-mtu 1320
   ```
   {: codeblock}

1. If the issue persists, add one of the following settings to the configuration file:

   ```text
   fragment 1400
   mssfix
   ```
   {: codeblock}

   Or use a fixed value:

   ```text
   mssfix 1360
   ```
   {: codeblock}

These settings help accommodate network devices that enforce lower MTU values:

`tun-mtu`
:   Defines the maximum tunnel packet size.

`fragment`
:   Enables fragmentation before encryption.

`mssfix`
:   Adjusts the TCP maximum segment size to reduce fragmentation.

After you update the client configuration, reconnect the VPN client and verify that the connection remains stable.

## Related links
{: #c2s-connection-establishment-related}

- [Creating an IAM access group and granting the role to connect to the VPN server](/docs/vpc?topic=vpc-create-iam-access-group)
- [Setting up client-to-site authentication](/docs/vpc?topic=vpc-client-to-site-authentication)
- [Monitoring VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-monitoring&interface=ui)
