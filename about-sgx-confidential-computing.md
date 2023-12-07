---

copyright:
  years: 2023
lastupdated: "2023-12-07"

keywords: sgx, intel sgx, software guard extension, confidential computing, trusted execution environment, TEE, data protection

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Confidential computing with Intel Software Guard Extensions (SGX) for Virtual Servers for VPC
{: #about-sgx-vpc}

[Select availability Beta]{: tag-blue}

Confidential computing with Intel&reg; Software Guard Extensions (SGX) protects your data through hardware-based server security by using isolated memory regions that are known as encrypted enclaves. This hardware-based computation helps protect your data from disclosure or modification. Which means that your sensitive data is encrypted while it is in virtual server instance memory by allowing applications to run in private memory space. To use SGX, you must install the SGX drivers and platform software on SGX-capable worker nodes. Then, design your app to run in an SGX environment. For more information about SGX, see [Intel Software Guard Extensions](https://www.intel.com/content/www/us/en/developer/tools/software-guard-extensions/overview.html).
{: shortdesc}

[Confidential computing with Intel SGX for VPC is available to select users in the US-South/Dallas region.]{: tag-green}

## Confidential computing
{: #confidential-computing-vpc-sgx}

When you use confidential computing with SGX, your data is protected through the entire compute lifecycle. Which means that your data is accessible only to authorized code and is invisible to anyone or anything else, including the operating system and even {{site.data.keyword.cloud}}.

### SGX is a trusted execution environment (TEE)
{: #tee-sgx-vpc}

SGX uses a trusted execution environment (TEE). TEE is a secure area of the main processor that provides a higher level of data security for trusted data and applications.

A TEE sets up an isolated, secure area of the main processor on a device that is dedicated to processing and storing sensitive data. The secure environment is protected from unauthorized access - even if the operating system is compromised. While your sensitive data is inside an encrypted enclave, your data is split into trusted and untrusted parts. The trusted parts are used in the encrypted enclave while the CPU denies all other access to the enclave regardless of access privileges. The data is inaccessible to internal and external threats and can't be removed or modified.

So, all Intel SGXs are TEEs, but not all TEEs are Intel SGXs.

### Attestation
{: #attestation-sgx-vpc}

When you develop a confidential computing application, you must design it so you can segment the information that needs encryption. At run time, the segmented information is kept confidential through attestation. When a request for information from the segmented code or app data is received, the encrypted enclave verifies that the request comes from the part of the application that exists outside of the enclave within the same application before it shares any information. Through the attestation process, information is kept confidential and data leakage is prevented. For more information about attestation with Intel SGX, see [Attestation with Intel SGX and Data Center Attestation Primitives (DCAP)](/docs/vpc?topic=vpc-about-attestation-sgx-dcap-vpc).

<!--Attestation in SGX is the process of demonstrating that an operation is instantiated. SGX attestation confirms that the intended software or code is running within an encrypted enclave. In other words, attestation provides evidence that you are running in an SGX platform that is inside a properly instantiated encrypted enclave, on a system that has a known security configuration.

Attestation is signed and verified when you provision a server with ECDSA signed collateral that is then saved in the caching service. While SGX helps uphold the integrity and confidentiality of your data (thanks to the encrypted enclaves), SGX doesn't protect against all attack types - such as side-channel attacks.-->

### Confidential computing with SGX use cases
{: #scenarios-sgx-vpc}

The following are some of the use cases for confidential computing with SGX.

* **Confidentiality and Privacy of Workloads and Appliations** within a Confidential Computing environment make sure that data privacy and security applications are always protected.

* **Confidential AI and Analytics** enables data and business analytics applications, machine learning models, and applications within secure enclaves. Includes SMPC applications that also help gain data insights.

* **Secure Multi-party Compute** enable distributed SMPC, where participants are ensured that their data and insights are protected even when calculated outside their direct control.

* **Digital Assets** is the trusted platform for digital custody solutions, for storing and transferring high value digital assets in highly secure wallets, reliable at scale.

## Requirements
{: #requirements-sgx-vpc}

* A VPC

## SGX-compatible profiles
{: #compatible-profiles-confidential-computing-vpc-sgx}

The following profiles support SGX.

* All Balanced _bx3d_ profiles
* All Compute _cx3d_ profiles

For more information about profiles, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles).

## Limitations
{: #limitations-confidential-computing-vpc-sgx}

Keep the following limitations in mind if you want to use SGX.

* Available on only third-generation Sapphire Rapids-based virtual servers.
* SGX doesn't protect against side-channel attacks.
* Hot plugging of components isn't supported. If you want to add more storage or network attachments, you need to restart your server after the hot-plug operation.
* Virtual servers can't be resized to disable SGX or secure boot. You need to create a new virtual server to disable SGX or secure boot. However, you can resize a server to enable SGX or secure boot.
* You can't resize an SGX-enabled virtual server from a profile lower than bx3d-64x320 to a higher RAM or CPU profile.
* Virtual servers that are provisioned with profiles that don't support secure boot can't be resized to a profile that supports secure boot.
* Only the following images support SGX. Keep in mind that images with kernel versions 5.11 or lower don't support SGX.
   - Red Hat 8.6, 8.8, 9.0, 9.2
   - Ubuntu 20.04, 22.04
   - CentOS Stream 8
   - Rocky Linux 8.8, 9.2
   - Debian 11, 12
   - SLES 15 SP4, SP5

If you resize a virtual server that is secure boot enabled to a profile that is secure boot disabled (and vice-versa), the topology of PCIe devices change. Depending on the operating system, this topology change can rename devices. The I/O performance can also change.
{: note}
