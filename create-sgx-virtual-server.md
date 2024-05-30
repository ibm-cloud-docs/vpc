---

copyright:
  years: 2023, 2024
lastupdated: "2024-05-29"

keywords: sgx, intel sgx, software guard extension, confidential computing, trusted execution environment, TEE, data protection

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Creating a virtual server with SGX
{: #creating-virtual-servers-with-sgx-vpc}

[Select availability]{: tag-green}

You can create one or more virtual server instances with SGX in your {{site.data.keyword.cloud}} VPC.
{: shortdesc}

Confidential computing with Intel SGX for VPC is available only in US-South (Dallas) region.
{: note}

## Creating a virtual server with SGX
{: #creating-a virtual-server-with-sgx-vpc}

Use the following steps to create a virtual server with SGX.

Make sure that you created a VPC.
{: tip}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
1. Click **Create** and select or enter the following information.
1. Select a location. Location must be **North America** > **Dallas**.
1. Enter a unique name for your virtual server instance.
1. Select from the available images. For more information about SGX-supported images, see [Limitations](/docs/vpc?topic=vpc-about-sgx-vpc#limitations-confidential-computing-vpc-sgx).
1. Select an SGX-supported profile by clicking **Confidential Computing**. Keep in mind that only the Balanced _bx3dc_ profiles and Compute _cx3dc_ profiles support SGX.
1. Click **Save**.
1. Enable confidential computing by using the SGX toggle button. Secure boot is automatically enabled. Keep in mind that you can enable secure boot without enabling SGX, but you can't enable SGX without enabling secure-boot.
1. Select an existing public SSH key or click **Create an SSH key** to create a new one. For more information about creating an SSH key, see [Creating your SSH key by using the UI](/docs/vpc?topic=vpc-ssh-keys&interface=ui#generate-ssh-keys-ui).
1. Go to **Networking**, and choose a **VPC** for this virtual server instance.
1. Click **Create a virtual server instance** when you are ready to provision. After your new virtual server with SGX is provisioned, it's ready to use.

For Gen3 profiles, you can enable and disable secure boot. But when you toggle between enable and disable the machine type changes. So, make sure that you create a snapshot before change this setting.
{: important}
