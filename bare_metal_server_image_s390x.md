---

copyright:
  years: 2022
lastupdated: "2022-09-27"

keywords: linuxone bare metal license, linux license, byol, bring your own license

subcollection: vpc

---

{:beta: .beta}
{:codeblock: .codeblock}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:preview: .preview}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# s390x bare metal server images
{: #s390x-bare-metal-image}

When you provision a s390x bare metal server on your VPC, you need to select an image to determine the operating system for the server.
{: shortdesc}


## Supported s390x operating system images
{: #s390x-supported-os}

| Image | Architectures |
|---------|---------|
|  SUSE Linux Enterprise Server (SLES) 15 SP3 | s390x |
|  Red Hat Enterprise Linux 8.x | s390x |
{: caption="Table 1. Supported s390x stock image operating systems" caption-side="bottom"}

You can also use your own license for Red Hat Enterprise Linux or SUSE Linux Enterprise Server with LinuxONE Bare Metal Servers. You can create a custom image with the supported Linux operating system and your own license, and then provision the instance. For more information, see [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about).

For more information about registering your own Linux server license, see [How to register and subscribe a RHEL system to the Red Hat Customer Portal by using Red Hat Subscription-Manager](https://documentation.suse.com/sles/15-SP1/html/SLES-all/cha-register-sle.html#sec-register-sle-system-suseconnect){: external}.

### Special considerations for Red Hat Enterprise Linux 8.4
{: #bare-metal-images-rhel-considerations}

By default, the release lock feature for Red Hat Enterprise Linux 8.4 is disabled. To prevent Red Hat Enterprise Linux from going beyond version 8.4 when you update, run the following commands from the command line:

   ```text
   # subscription-manager release --set=8.4
   ```
   {: codeblock}

   ```text
   # yum clean all
   ```
   {: codeblock}
