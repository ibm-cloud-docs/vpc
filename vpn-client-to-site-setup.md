---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Setting up a client VPN environment and connecting to a VPN server
{: #vpn-client-environment-setup}

After you create the VPN server by using the newly created certificate, you can set up and configure your clients' VPN environment to connect to the VPN server. Depending on the client authentication that you selected during VPN server provisioning, users can connect to the VPN server by using a client certificate, a user ID with passcode, or both.
{: shortdesc}

1. Open the details page of the VPN server and click the **Clients** tab. You can:
   * Download the client profile template (`<vpn_server>.ovpn`) and add your certificate and private key.

   Or
   * For private certificates only, you can either select **All client profiles** to download a client profile with a merged private certificate and key for all certificates. Or you can select one or more certificates and then download the client profile with merged private certificate and key for selected certificates. This way, the client profiles are ready for use with the OpenVPN client for users, avoiding having to add keys or distribute certificates manually.

      Only an administrator with VPN server and Secrets Manager permissions can download client profiles.
      {: important}

1. Distribute the client profile file to the VPN client users and instruct users to complete the following steps:

   A step-by-step tutorial for VPN client users client is provided in [Setting up a VPN client](/docs/vpc?topic=vpc-setting-up-vpn-client).
   {: note}

   * Download and install an OpenVPN client. For a list of supported OpenVPN clients, see [Supported VPN client software](/docs/vpc?topic=vpc-client-to-site-vpn-planning#vpn-client-software).
   * If you're using user ID-based authentication, ask users to get an IAM passcode. For instructions, see [Configuring user IDs and passcodes](/docs/vpc?topic=vpc-client-to-site-authentication#client-to-site-configuration-passcode).
   * If you're using certificate-based authentication, choose one of the following options:
      * If you are using a private certificate and downloaded the `vpn-server-name.zip` file, extract the compressed file. Make sure that the `.ovpn` file contains the merged private certificate and key. Then, follow the instructions in the `.txt` file. VPN client user does not need to modify the client profile manually.
      * Otherwise, generate and distribute the client certificate to the VPN client users safely. VPN client users must edit the client profile to add the client certificate into the file.

         The client certificate can be added to the end of the client profile in two ways.

         * Option 1

         ```sh
         cert /path/client_public_key.crt
         key /path/client_private_key.key
         ```

         * Option 2

         ```sh
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

      If the VPN server certificate is ordered from a public CA, you must update the `<ca>` section with the public CA certificate.
      {: important}

1. Connect to the VPN server by using the OpenVPN client and configuration file.
1. To verify that a client connected successfully, open the details page of the VPN server. Then, click the Clients tab to view all connected VPN clients in the last 5 minutes.

   You can click the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions") to disconnect or delete clients.
   {: tip}
