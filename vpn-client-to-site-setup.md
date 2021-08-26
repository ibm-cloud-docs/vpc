---

copyright:
  years: 2021
lastupdated: "2021-08-26"

keywords:

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:beta: .beta}
{:important: .important}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:download: .download}

# Setting up a client VPN environment and connecting to a VPN server (Beta)
{: #vpn-client-environment-setup}

Client VPN for VPC is available to all IBM Cloud users. After the Beta period ends, you will be given a time period to migrate your VPN servers to the standard pricing plan to avoid disruption of service.
{: beta}

After you create the VPN server using the newly created certificate, you can set up and configure your clients' VPN environment to connect to the VPN server. Depending on the client authentication you selected during VPN server provisioning, users can connect to the VPN server using a client certificate, a user ID with passcode, or both.
{: shortdesc}

1. Open the details page of the VPN server, then click **Download client profile** to download the client configuration file (`<vpn_server>.ovpn`).

   ![Download client profile](images/vpn-download-client-profile.png "Download client profile")

1. Distribute the client profile file to the VPN client users.
1. Instruct VPN client users to do the following:

   * Download and install an OpenVPN client. For a list of supported OpenVPN clients, see [Supported VPN client software](/docs/vpc?topic=vpc-client-to-site-vpn-planning#vpn-client-software).
   * If using user ID-based authentication, ask users to get an IAM passcode. For instructions, see [Configuring user IDs and passcodes](/docs/vpc?topic=vpc-client-to-site-authentication#client-to-site-configuration-passcode).
   * If using certificate-based authentication, generate and distribute the client certificate to the VPN client users safely. Also, VPN client users must edit the client profile to add the client certificate into the file. 
      
      There are two approaches to add the client certificate into the file:
   
      * Option 1: Add the following information to the end of the client profile:
      
         ```
         cert /path/client_public_key.crt
         key /path/client_private_key.key
         ```
         {: pre}
      
      * Option 2: Add the following information to the end of the client profile:
      
         ```
         <cert>
         -----BEGIN CERTIFICATE-----
         place your VPN client certificate
         -----END CERTIFICATE-----
         </cert>
         <key>
         -----BEGIN PRIVATE KEY-----
         place your VPN client private key
         -----END PRIVATE KEY-----
         </key>
         ```
         {: pre}

      If the VPN server certificate is ordered from a public CA, you must update the `<ca>` section with the public CA certificate. For more information, see [Ordering a certificate using the certificate manager](/docs/vpc?topic=vpc-client-to-site-authentication#order-certificate).
      {: important}

1. Connect to the VPN server using the OpenVPN client and configuration file.
1. To verify that a client connected successfully, open the details page of the VPN server. Then, scroll to the Clients section to view all connected VPN clients in the last hour.

   ![VPN client connections](images/vpn-client-section.png "VPN client connections")

   You can click the overflow menu ![overflow menu](images/overflow.png) to disconnect or delete clients.
   {: tip}
