---

copyright:
  years: 2018, 2025
lastupdated: "2025-03-05"

keywords: connecting, linux

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Connecting to Linux instances
{: #vsi_is_connecting_linux}

After you created your Linux instance, you can connect to it by completing these steps.
{: shortdesc}

## Locating floating IP address
{: #locating-floating-ip-address}

If you haven’t associated a floating IP to your instance, follow Step 3 and Step 4
in [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#create-instance-cli) to request a floating IP address to associate to your instance.
{: note}

If you need to locate your floating IP address for the instance to which you want to connect, complete the following steps. If you already know your floating IP address, skip to [Getting connected](#getting-connected-linux).

1. You need to identify your floating IP address. Run the following command to identify your floating IP address:

   ```sh
   ibmcloud is instance-network-interfaces <INSTANCE_ID> <NETWORK_INTERFACE_ID> --json
   ```
   {: pre}

   For this example, you'd see a response similar to the following output:

   ```json
   "floating_ips": [
           {
               "address": "123.45.678.90"
               "crn:v1:mydomain:public:vpc:us-south:a/c4cxxxc10xx54xxx9e2xxx59xxx3fa0f::floating_ip:12345x67-8901-234x-5678-9xx01xx23x4x",
               "href": "https://us-south.myaccount.cloud.ibm.com/v1/floating_ips/12345x67-8901-234x-5678-9xx01xx23x4x",
               "id": "0738-12345x67-8901-234x-5678-9xx01xx23x4x",
               "name": “my-instance”
           }
       ]
   ```
   {: codeblock}

2. Now that you have your floating IP ID, you can locate your floating IP address by running the following command.

   ```sh
   ibmcloud is ip <FLOATING_IP_ID>
   ```
   {: pre}

   For this example, you'd see a response similar to the following output:

   ```json
   ID               0738-12345x67-8901-234x-5678-9xx01xx23x4x
   Address          123.45.678.90
   Name             my-instance
   Target           primary(1xx2x34x-.)
   Target Type      intf
   Target IP        12.345.6.78
   Created          1 week ago
   Status           available
   Zone             us-south-1
   Resource Group   -
   Tags             -
   ```
   {: codeblock}

Optionally, you can locate the floating IP address that is associated to the instance to which you want to connect through the {{site.data.keyword.cloud_notm}} console.
{: tip}

## Determining the default user account
{: #determining-default-user-account}

The system-generated default user account is typically associated with managing access and operations. The default user can be used to log into the virtual server instance using SSH. The SSH keys are configured for the default user in the image.

The name of the default user account depends on the operating system image being used. See the following table for the operating system and its corresponding default user account.

| Operating system | Default user account |
|-----------------|----------------|
| Red Hat Enterprise Linux (RHEL) | `vpcuser` |
| SUSE Linux Enterprise Server (SLES) | `vpcuser` |
| Debian | `vpcuser` |
| Rocky Linux | `vpcuser` |
|  Ubuntu | `ubuntu` |
| Fedora Core | `core` |
{: caption="Default user account for Linux images" caption-side="bottom"}

## Getting connected
{: #getting-connected-linux}

1. To connect to your instance, use your private key and run the following command:

   ```sh
   ssh -i <path to your private key file> <default-user-account>@<floating ip address>
   ```
   {: pre}

   You receive a response similar to the following example. When prompted to continue connecting, type `yes`.
   ```json
   The authenticity of host 'xxx.xxx.xxx.xxx (xxx.xxx.xxx.xxx)' can't be established.
   ECDSA key fingerprint is SHA256:abcdef1Gh/aBCd1EFG1H8iJkLMnOP21qr1s/8a3a8aa.
   Are you sure you want to continue connecting (yes/no)? yes
   Warning: Permanently added 'xxx.xxx.xxx.xxx' (ECDSA) to the list of known hosts.
   ```
   {: codeblock}

   You are now accessing your server.

2. When you are ready to end your connection, run the following command:

   ```sh
   exit
   ```
   {: pre}
