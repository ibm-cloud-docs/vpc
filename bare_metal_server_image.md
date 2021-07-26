---

copyright:
  years: 2021
lastupdated: "2021-07-20"

keywords: bare metal servers, esxi license, byol

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

# Bare Metal Servers for VPC (beta) images
{: #bare-metal-image}

You can license the ESXi hypervisor that is installed on a bare metal server with your own license (bring-your-own-license), or let IBM handle the licensing for you.

You can specify how a bare metal server is licensed by selecting from different ESXi image options: "ESXi 7.x BYOL" or "ESXi 7.x".

* The "ESXi 7.x BYOL" option provides ESXi in evaluation mode. The evaluation period is 60 days and begins at the time of provisioning. At any time during the 60-day evaluation period, you can convert from evaluation mode to licensed mode with your appropriate customer provided license.

* The "ESXi 7.x" option provides ESXi in licensed mode and is activated during the provisioning process. Billing will apply for IBM rented licenses. 

For information about how to license ESXi, see [Licensing ESXi Hosts]( https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.esxi.install.doc/GUID-28D25806-748B-49C0-97A1-E7DE5CB335A9.html){: external}.
