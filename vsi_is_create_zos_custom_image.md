---

copyright:
  years: 2022
lastupdated: "2022-09-19"

keywords: z/OS custom image, wazi image builder

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}

# Creating a z/OS Wazi aaS custom image
{: #create-zos-custom-image}

You can use IBM Wazi Image Builder to create your own custom z/OS-based {{site.data.keyword.waziaas_full_notm}} (Wazi aaS) image and import the custom image into {{site.data.keyword.vpc_full}}. You can then use the z/OS Wazi aaS custom image to create a virtual server instance in the {{site.data.keyword.vpc_short}} infrastructure.
{: shortdesc}
 
IBM Wazi Image Builder is a separately orderable product from [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/){: external}. There are additional requirements to order and use Wazi Image Builder. The image cost is the premium applied to cover the cost of technologies that allows for z/OS dev and test images to run on IBM Z hardware in IBM’s cloud infrastructure as a service layer. 

The z/OS Wazi aaS custom image must meet the following requirements: 
* Is in qcow2 format
* The operating system is z/OS 2.4 or z/OS 2.5
* The zHYPaaS host control program creates a virtual machine (VM) runtime environment for each guest operating system (OS) and can host many guests at the same time. For required program temporary fixes (PTFs) on z/OS and other IBM software products, see [Hardware and software requirements](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=builder-hardware-software-requirements){: external}. 

{{site.data.keyword.waziaas_full_notm}} (Wazi aaS) is available in the Japan (Tokyo), Brazil (São Paulo), Canada (Toronto), United Kingdom (London), and US East (Washington DC) regions. For more information, see [{{site.data.keyword.waziaas_full_notm}} documentation](https://www.ibm.com/docs/en/wazi-aas/1.0.0){: external}.
{: note}

IBM Wazi Image Builder is licensed only for development, testing, employee education, or demonstration of your applications that run on z/OS. This product must not be used for production workloads of any kind, nor robust development workloads, production module builds, preproduction testing, stress testing, or performance testing.
{: important}

For details about getting IBM Wazi Image Builder license keys and activation kits, go to the [IBM Support Licensing](https://www.ibm.com/support/pages/ibm-support-licensing-start-page){: external} page. To download the software, follow the instructions on [IBM Passport Advantage&reg;](https://www.ibm.com/software/passportadvantage/){: external}. 

For details about installing, configuring, and using IBM Wazi Image Builder, see [Bringing your own image with Wazi Image Builder](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=bringing-your-own-image-wazi-image-builder){: external}.

