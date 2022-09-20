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

# s390x Bare Metal servers images
{: #s390x-bare-metal-image}

You can install IBM provided Linux operating system images on your LinuxONE Bare metal server.
{: shortdesc}


## IBM provided Linux (s390x processor architecture) operating system images
{: #s390x-supported-os}

| Image | Architectures |
|---------|---------|
|  SUSE Linux Enterprise server (SLES) 15 SP3 | s390x |
|  Red Hat Enterprise Linux 8.x | s390x |
{: caption="Table 1. Supported s390x stock image operating systems" caption-side="bottom"}

In addition to IBM provided Linux distributions, you can use your own license for Red Hat Enterprise Linux or SUSE Linux Enterprise Server with LinuxONE Bare Metal servers. You can create a custom image with your own license, and then provision the instance. For more information, see [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about).

To register your own license for the LinuxONE Bare Metal server, see [How to register and subscribe a RHEL system to the Red Hat Customer Portal using Red Hat Subscription-Manager? ![External link icon](../icons/launch-glyph.svg "External link icon")](https://access.redhat.com/solutions/253273) and [Registering SUSE Linux Enterprise and Managing Modules/Extensions ![External link icon](../icons/launch-glyph.svg "External link icon")](https://documentation.suse.com/sles/15-SP1/html/SLES-all/cha-register-sle.html#sec-register-sle-system-suseconnect).

### Special considerations for Red Hat Enterprise Linux 8.4
{: #bare-metal-images-rhel-considerations}

By default, the release lock feature for Red Hat Enterprise Linux 8.4 is disabled. To prevent the Red Hat Enterprise Linux from going beyond version 8.4 when you run an update, run the following commands from the command line:

   ```text
   # subscription-manager release --set=8.4
   ```
   {: codeblock}

   ```text
   # yum clean all
   ```
   {: codeblock}