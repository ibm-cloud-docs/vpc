---

copyright:
  years: 2022, 2023
lastupdated: "2023-04-11"

keywords: linuxone bare metal license, linux license, byol, bring your own license, s390x bare metal byol, s390x license

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# s390x bare metal server images
{: #s390x-bare-metal-images}

s390x Bare Metal Servers for VPC is available for customers with special approval to preview this service in the Washington DC (us-east), London (eu-gb), Tokyo (jp-tok), Toronto (ca-tor), and São Paulo (br-sao) regions.
{: preview}


When you provision an s390x bare metal server on your VPC, you need to select an image to determine the operating system for the server.
{: shortdesc}

## Supported s390x operating system images
{: #s390x-bare-metal-supported-os-images}

| Image | Architecture |
|---------|---------|
|  SUSE Linux Enterprise Server (SLES) 15 SP3 | s390x |
|  Red Hat Enterprise Linux 8.4 | s390x |
{: caption="Table 1. Supported s390x image operating systems" caption-side="bottom"}

You can also use your own license for Red Hat Enterprise Linux or SUSE Linux Enterprise Server with LinuxONE Bare Metal Servers. You can create a custom image with the supported Linux operating system and your own license to provision a server. For more information, see [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about).

For more information about registering your own Linux server license, see [How to register and subscribe a RHEL system to the Red Hat Customer Portal by using Red Hat Subscription-Manager](https://documentation.suse.com/sles/15-SP1/html/SLES-all/cha-register-sle.html#sec-register-sle-system-suseconnect){: external}.

The Red Hat Enterprise Linux 8.4 image that is provided by s390x Bare Metal Servers for VPC is now Red Hat image certified. For more information, see [Cloud images](https://catalog.redhat.com/cloud/detail/218647).

### Special considerations for Red Hat Enterprise Linux 8.4
{: #s390x-bare-metal-images-rhel-special-considerations}

By default, the release lock feature for Red Hat Enterprise Linux 8.4 is disabled. To prevent Red Hat Enterprise Linux from going beyond version 8.4 when you update, run the following commands from the command line:

   ```text
   # subscription-manager release --set=8.4
   ```
   {: codeblock}

   ```text
   # yum clean all
   ```
   {: codeblock}
