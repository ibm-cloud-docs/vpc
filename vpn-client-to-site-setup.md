---

copyright:
  years: 2021, 2022
lastupdated: "2022-06-26"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Setting up a client VPN environment and connecting to a VPN server (Beta)
{: #vpn-client-environment-setup}

Client VPN for VPC is available to all IBM Cloud users. After the Beta period ends, you will be given a time period to migrate your VPN servers to the standard pricing plan to avoid disruption of service.
{: beta}

After you create the VPN server using the newly created certificate, you can set up and configure your clients' VPN environment to connect to the VPN server. Depending on the client authentication you selected during VPN server provisioning, users can connect to the VPN server using a client certificate, a user ID with passcode, or both.
{: shortdesc}

1. Open the details page of the VPN server and click the **Clients** tab. Then, click **Download client profile** to download the client configuration file (`<vpn_server>.ovpn`).

   ![Download client profile](images/vpn-download-client-profile.png){: caption="Download client profile" caption-side="bottom"}

1. Distribute the client profile file to the VPN client users and instruct users to do the following: 

   A step-by-step tutorial for VPN client users client is provided in [Setting up a VPN client](/docs/vpc?topic=vpc-setting-up-vpn-client).
   {: note}

   * Download and install an OpenVPN client. For a list of supported OpenVPN clients, see [Supported VPN client software](/docs/vpc?topic=vpc-client-to-site-vpn-planning#vpn-client-software).
   * If using user ID-based authentication, ask users to get an IAM passcode. For instructions, see [Configuring user IDs and passcodes](/docs/vpc?topic=vpc-client-to-site-authentication#client-to-site-configuration-passcode).
   * If using certificate-based authentication, generate and distribute the client certificate to the VPN client users safely. Also, VPN client users must edit the client profile to add the client certificate into the file. 
      
      There are two approaches to adding the client certificate to the end of the client profile.
   
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

1. Connect to the VPN server using the OpenVPN client and configuration file.
1. To verify that a client connected successfully, open the details page of the VPN server. Then, click the Clients tab to view all connected VPN clients in the last 5 minutes. 

   ![VPN clients](images/vpn-clients.png){: caption="VPN clients" caption-side="bottom"}

   You can click the Actions menu ![Actions menu](images/overflow.png) to disconnect or delete clients.
   {: tip}
