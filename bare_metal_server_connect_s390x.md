---

copyright:
  years: 2022
lastupdated: "2022-11-07"

keywords: connecting to s390x bare metal server, floating IP, serial console, vnc console

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to s390x bare metal servers 
{: #connect-to-s390x-bare-metal-servers}

s390x Bare Metal Servers for VPC is available for customers with special approval to preview this service in the Washington DC (us-east) and São Paulo (br-sao) regions. 
{: preview}

After your s390x bare metal server is running, you can connect to the server by using your private SSH key through a floating IP.
{: shortdesc}

You can't use a VNC console or a serial console to connect to s390x bare metal servers.
{: note}

## Accessing the s390x bare metal server by using a floating IP 
{: #access-s390x-bm-using-fip}

You can access the s390x bare metal server through a floating IP.

### Attaching a floating IP to an s390x bare metal server network interface
{: #attaching-fip-to-nic}

Before you can access the bare metal server through the public internet, you need to attach a floating IP to its primary HiperSocket network interface. You can attach a floating IP by using the UI, [CLI](#attaching-fip-to-nic-cli), or [API](#attaching-fip-to-nic-api).

### Attaching a floating IP by using the UI
{: #attaching-fip-to-nic-ui}

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Bare metal servers**.

2. Click the name of the server that you want to attach a floating IP.

3. On the **Bare metal server details** page, scroll to the **Network interfaces** section and click **Edit**.

4. On **Edit network interface**, select a reserved floating IP or **Reserve a new floating IP** and click **Save**.

### Attaching a floating IP by using the CLI
{: #attaching-fip-to-nic-cli}

You need the following information to attach a floating IP to a bare metal server:

* Server ID
* Network interface ID 
   - You can use the [List the network interfaces](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#bare-metal-server-network-interfaces) command to find the ID of the network interface.
* Floating IP ID 
   - You can use the [ibmcloud is floating-ips](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#floating-ips) command to find the reserved floating IP IDs. Or, use the [ibmcloud is floating-ip-reserve](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#floating-ip-reserve) command to reserve a new one.

After you collect all the necessary information, use the following command to attach the floating IP to the s390x bare metal server:

```sh
ibmcloud is bare-metal-server-network-interface-floating-ip-add $bare_metal_server_id $network_interface_id $floating_ip_id
```
{: pre}

### Attaching a floating IP by using the API
{: #attaching-fip-to-nic-api}

You need the following information when you attach a floating IP to a bare metal server network interface:

* Server ID
* Network interface ID
* Floating IP ID

Use the [List that all network interfaces](/apidocs/vpc#list-bare-metal-server-network-interfaces) command to find the ID of the network interface. Use the [List all floating IP](/apidocs/vpc#list-floating-ips) command to find the reserved floating IP IDs, or, use the [Reserve a floating IP](/apidocs/vpc#create-floating-ip) command to reserve a new one.
{: tip}

After you collect all the required information, use the following API request to attach the floating IP to the bare metal server:

```sh
curl -X PUT "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/network_interfaces/$network_interface_id/floating_ips/$floating_ip_id?version=2022-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

## Accessing the s390x bare metal server
{: #login-s390x-bm}

1. To connect to your s390x bare metal server, use your private key and run the following command:

   ```sh
   ssh -i <path to your private key file> root@<floating ip address>
   ```
   {: pre}

   You receive a response similar to the following example. When prompted to continue, type `yes`.

   ```text
   The authenticity of host 'xxx.xxx.xxx.xxx (xxx.xxx.xxx.xxx)' can't be established.
   ECDSA key fingerprint is SHA256:abcdef1Gh/aBCd1EFG1H8iJkLMnOP21qr1s/8a3a8aa.
   Are you sure you want to continue connecting (yes/no)? yes
   Warning: Permanently added 'xxx.xxx.xxx.xxx' (ECDSA) to the list of known hosts.
   ```
   {: screen}

2. When you are ready to end your connection, run the following command:

   ```sh
   exit
   ```
   {: pre}
