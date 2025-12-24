---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-24"

keywords:
subcollection: vpc

content-type: tutorial
account-plan: paid
completion-time: 20m

---

{{site.data.keyword.attribute-definition-list}}

# Setting up a VPN client
{: #setting-up-vpn-client}
{: toc-content-type="tutorial"}
{: toc-completion-time="20m"}

In this tutorial, you learn how to set up your VPN client and connect to the VPN server.
{: shortdesc}

## Before you begin
{: #cd-kube-prereqs}

Obtain the following information from your VPN server administrator:

* Client profile file (`<vpn_server>.ovpn`)
* The type of authentication that was configured for the VPN server

   Choices are client certificate, user ID and passcode, or both. If client certificate-based authentication was implemented, make sure that you also received the client certificate.
* If the VPN server certificate was ordered from a public Certificate Authority (CA), make sure that you have the CA certificate number.

## Install a VPN client
{: #install-vpn-client}
{: step}

First, you must download and install an OpenVPN client. The following client software versions are supported.

* For macOS Catalina and later: [OpenVPN Connect v3](https://openvpn.net/client/#tab-macos){: external}, OpenVPN Connect v2, and Tunnelblick 3.8.4
* Windows 8 and later: [OpenVPN Connect v3](https://openvpn.net/client/#tab-windows){: external}, OpenVPN Connect v2
* RHEL 7.x and later: [OpenVPN Connect v3](https://openvpn.net/client/#tab-linux){: external}, OpenVPN Connect v2, and OpenVPN command-line client (version 2.4.4 and later)
* Ubuntu 18.04 and later: [OpenVPN Connect v3](https://openvpn.net/client/#tab-linux){: external}, OpenVPN Connect v2, and OpenVPN command-line client (version 2.4.10 and later)

You can choose other OpenVPN-2.4-compatible client software. However, software that is not listed is not guaranteed to work.
{: tip}

## Set up client authentication
{: #set-up-client-authentication}
{: step}

Your VPN server administrator can choose to set up a client certificate, a user ID and passcode, or both types of authentication. Depending on what your administrator instructs you to do, complete one or both of the following procedures.

### Configure a client certificate
{: #configure-client-certificate}
{: step}

If your administrator provided a client certificate for certificate-based authentication, you must edit the client profile file (`<vpn_server>.ovpn`), and then add the client certificate into the file by using any ASCII editor.

If your administrator used a private certificate, they might have provided you with a client profile that already includes your client certificate and private key. If so, save the client profile file on your system and skip to step 4 to open the OpenVPN client UI and import the file.
{: important}

1. Save the client profile (`<vpn_server>.ovpn`) on your system.
1. Open the file in an ASCII editor and add the client certificate to the end of the client profile. For example, use one of the following methods:

   * Enter the paths to both the client public key and the client private key:

      ```sh
      cert /path/client_public_key.crt
      key /path/client_private_key.key
      ```

   OR

   * Wrap `client_public_key.crt` between the `<cert></certs>` tags and enclose `client_private_key.key` between `<key></key>` as shown.

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

1. If the VPN server certificate was ordered from a public Certificate Authority (CA), you must also update the `<ca>` section with the public CA certificate.

   ```sh
   <ca>
   ----BEGIN CERTIFICATE-----
   MIID......................
   ..........................
   ..........................
   ..........................
   ----END CERTIFICATE-----
   </ca>
   ```

1. Open the OpenVPN client UI and import the `.ovpn` profile file. Depending on the OpenVPN client you installed, the instructions vary. OpenVPN Connect and Tunnelblick examples are provided.

   _OpenVPN Connect example:_
   * Click the Plus icon on the lower right of the window.
      ![Profiles window](images/vpn-c2s-profile.png "Profiles window"){: caption="Profiles window" caption-side="bottom"}
   * Click **Browse** to select and import the `.ovpn` file (client profile).
      ![Import Profile window](images/vpn-c2s-import.png "Import Profile window"){: caption="Import Profile window" caption-side="bottom"}

   _Tunnelblick example:_
   * Click **VPN Details**.
      ![Tunnelblick UI](images/vpn-c2s-vpn-details.png "Tunnelblick UI"){: caption="Tunnelblick UI" caption-side="bottom"}
   * Drag and drop the `.ovpn` file in the Configurations section.
      ![Configurations window](images/vpn-c2s-configurations.png "Configurations window"){: caption="Configurations window" caption-side="bottom"}
   * Click **OK**.
      ![Add a Configuration window](images/vpn-c2s-ok.png "Add a Configuration window"){: caption="Add a Configuration window" caption-side="bottom"}
   * Select **Only Me** and provide your system username and system password. The expected values are typically your IBMid and password. Then, click **OK**.
      ![Install Configuration For All Users window](images/vpn-c2s-only-me.png "Install Configuration For All Users window"){: caption="Install Configuration For All Users window" caption-side="bottom"}

1. If you are configuring only a client certificate, skip to [Step 3: Connect to the VPN server](/docs/vpc?topic=vpc-setting-up-vpn-client#connect-vpn-server). If you are also configuring a user ID and passcode, complete the steps in the next section.

### Configure a user ID and passcode
{: #cd-configure-user-id-passcode}
{: step}

If your administrator configured user ID and passcode authentication on the VPN server, follow these steps:

Your VPN server administrator invites you to the account that the VPN server resides in so that you can connect to the VPN server. Your administrator already assigned your ID an {{site.data.keyword.iamlong}} (IAM) permission.
{: note}

1. Go to this website to generate a passcode for your user ID:

   ```sh
   https://iam.cloud.ibm.com/identity/passcode
   ```

1. If a VPN server certificate was ordered from a public Certificate Authority (CA), save the client profile (`<vpn_server>.ovpn`) on your system, and then update the `<ca>` section with the public CA certificate. _If you configured a client certificate, skip this step._

   See the following example:

   ```sh
   <ca>
   ----BEGIN CERTIFICATE-----
   MIID......................
   ..........................
   ..........................
   ..........................
   ----END CERTIFICATE-----
   </ca>
   ```

1. Open the OpenVPN UI and import the file.

   Do not select the **Save password** option.
   {: note}

   _OpenVPN Connect example:_

   * In the OpenVPN Connect client, enter your IAM username, then click **CONNECT**.
      ![Imported Profile window](images/vpn-c2s-imported-profile.png "Imported Profile window"){: caption="Imported Profile window" caption-side="bottom"}


   * Add the one-time passcode generated in step 1, then click **OK**.
      ![Enter password window](images/vpn-c2s-enter-password.png "Configurations window"){: caption="Enter password window" caption-side="bottom"}

   _Tunnelblick example:_

   * After you import the `.ovpn` file, click **Connect**.
      ![Configurations window](images/vpn-c2s-config.png "Configurations window"){: caption="Configurations window" caption-side="bottom"}

   * In the pop-up window, enter your IAM username and the one-time passcode that is generated by using the link in step 1. Then, click **OK**.
      ![Login window](images/vpn-c2s-login.png "Login window"){: caption="Login window" caption-side="bottom"}

1. Continue to the next section.

## Connect to the VPN server
{: #connect-vpn-server}
{: step}

When ready to connect to the VPN server, click **Connect**.

_OpenVPN Connect example:_
![Imported Profile window](images/vpn-c2s-imported.png "Imported Profile window"){: caption="Imported Profile window" caption-side="bottom"}
_Tunnelblick example:_
![Configurations window](images/vpn-c2s-config.png "Configurations window"){: caption="Configurations window" caption-side="bottom"}

If you encounter a warning message, such as one of the following, select not to be warned again. Then, click **Continue** or **OK** depending on your OpenVPN client.

* `Missing external certificate`
* `This computer's apparent public IP address was not different after connecting to xxx...`
* `One or more possible problems with DNS were found:`

You are now connected to the client-to-site VPN server.

   _OpenVPN Connect example:_
   ![Connected status](images/vpn-c2s-connected.png "Connected status"){: caption="Connected status" caption-side="bottom"}
   _Tunnelblick example:_
   ![Connected status](images/vpn-c2s-tunnelblick.png "Connected status"){: caption="Connected status" caption-side="bottom"}
