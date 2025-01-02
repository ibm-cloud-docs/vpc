---

copyright:
  years: 2021, 2025
lastupdated: "2024-06-06"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Network booting your own operating system with Bare Metal Servers on VPC
{: #network-boot-bare-metal-servers}

When you create a bare metal server on {{site.data.keyword.vpc_full}} (VPC), you can select to network boot an operating system over the network. The operating system image can be hosted on your own server or on a public server. You can install the booted operating system to a disk or you can run the operating system without a disk.

Before you use the network boot option, you must make sure that your server's security groups allow the server to access any remote iPXE script and either your target operating system's remote image or installation files.

Currently, a known timing issue exists when you use iPXE to network boot a bare metal server. For more information, see [iPXE network boot known timing issue](/docs/vpc?topic=vpc-known-issues#ipxe-network-boot-known-issue).
{: important}

When you create a bare metal server on {{site.data.keyword.vpc_short}}, the user data that you provide determines what operating system boots. The user data must contain either a single URL that points to a remotely hosted iPXE script or the text of an iPXE script (including a first line of `#!ipxe`.) For more information about creating iPXE scripts, see [iPXE - open source boot firmware](https://ipxe.org/scripting).

Use security best practices to limit access to any files that you expose on the internet.
{: important}

## Network booting your own operating system with Bare Metal Servers on VPC by using the UI
{: #network-boot-bare-metal-servers-ui}
{: ui}

To network boot a bare metal server from the UI, use the following steps.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**.
1. Click **Create**.
1. In the **Image and profile** section, click the **Network boot** tab.
1. Either copy and paste the **IPXE script** into the text box or you can import the IPXE script.
1. Review the configuration **Summary** and click **Create bare metal server**.
1. Verify that your server's Security Group allows egress to the server that is hosting your network boot artifacts.
1. Verify that your subnet gateway is configured to allow network routing to the server that hosts your network boot artifacts.

For connecting to servers that use HTTPS, the iPXE image recognizes server certificates that are signed by common public certificate authorities. Verify the server that is hosting the network boot artifacts presents such a certificate. Otherwise, you might see a `Permission denied` error.
{: important}

## Network booting your own operating system with Bare Metal Servers on VPC by using the CLI
{: #network-boot-bare-metal-servers-cli}
{: cli}

To network boot a bare metal server from the CLI, use the following information when you create your bare metal server.

For the image, you must specify the ID of a stock image that has the `user_data_format` property that is set to `ipxe` in the region where you want to create the server and provide the appropriate user data. For more information, see [Creating Bare Metal Servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers&interface=cli) and [User data](/docs/vpc?topic=vpc-user-data).

## Network booting your own operating system with Bare Metal Servers on VPC by using the API
{: #network-boot-bare-metal-servers-api}
{: api}

To network boot a bare metal server from the API, use the following information when you create your bare metal server.

For the image, you must specify the ID of a stock image that has the `user_data_format` property that is set to `ipxe` in the region where you want to create the server and provide the appropriate user data. For more information, see [Creating Bare Metal Servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers&interface=api) and [User data](/docs/vpc?topic=vpc-user-data).

## Example of using the CentOS installer to network boot a bare metal server
{: #generic-os-network-boot-centos-installer}

This example is just one example of how to network boot an operating system. In this example, a bare metal server is created which boots to the Centos installer. You provide the script as the user data when you create the server.

```screen
#!ipxe

dhcp
ntp pool.ntp.org

# Set source URI
set mirror http://mirror.stream.centos.org/9-stream

# Detect CPU architecture and calculate repository URI
cpuid --ext 29 && set arch x86_64 || set arch i386
set repo ${mirror}/BaseOS/${arch}/os

# Start installer
kernel ${repo}/images/pxeboot/vmlinuz initrd=initrd.img inst.repo=${repo}
initrd ${repo}/images/pxeboot/initrd.img
boot
```
{: codeblock}
