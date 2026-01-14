---

copyright:
  years: 2023, 2026
lastupdated: "2026-01-14"

keywords: sgx, intel sgx, software guard extension, confidential computing, trusted execution environment, TEE, data protection

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Creating a virtual server with SGX or TDX
{: #creating-virtual-server-with-confidential-computing-vpc}

[Select availability]{: tag-green}

You can create one or more virtual server instances with SGX or TDX in your {{site.data.keyword.cloud}} VPC.
{: shortdesc}

Confidential computing profiles, Confidential computing with Intel SGX for VPC, and Confidential computing with Intel TDX for VPC are available in the Dallas (us-south), Washington DC (us-east), and Frankfurt (eu-de) regions. For more information, see [Confidential computing known issues](/docs/vpc?topic=vpc-known-issues#confidential-computing-vpc-known-issues). Confidential computing is only available with select profiles. For more information, see [Confidential computing profiles](/docs/vpc?topic=vpc-profiles&interface=ui#confidential-computing-profiles).
{: preview}

## Creating a virtual server with confidential computing
{: #creating-virtual-server-with-confidential-computing-vpc-steps}

Use the following steps to create a virtual server with confidential computing.

Make sure that you created a VPC.
{: tip}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
1. Click **Create** and select or enter the following information.
1. Select a location.
1. Enter a unique name for your virtual server instance.
1. Select an available image. For more information about SGX- or TDX-supported images, see [Limitations](/docs/vpc?topic=vpc-about-confidential-computing-vpc#limitations-confidential-computing-vpc).
1. Select an SGX- or TDX-supported profile by clicking **Confidential computing**. Keep in mind that only the Balanced _bx3dc_ profiles and Compute _cx3dc_ profiles support SGX or TDX.
1. Click **Save**.
1. SGX is the default value. Keep the SGX confidential computing value or select TDX confidential computing.
1. Secure boot is enabled by default. To disable it, toggle secure boot to **Disabled**.
1. Select an existing public SSH key or click **Create an SSH key** to create a new one. For more information about creating an SSH key, see [Creating your SSH key by using the UI](/docs/vpc?topic=vpc-ssh-keys&interface=ui#generate-ssh-keys-ui).
1. Go to **Networking**, and choose a **VPC** for this virtual server instance.
1. Click **Create a virtual server instance** when you are ready to provision. After your new virtual server with SGX or TDX is provisioned, it's ready to use.
