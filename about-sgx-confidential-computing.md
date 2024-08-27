---

copyright:
  years: 2023, 2024
lastupdated: "2024-08-26"

keywords: sgx, intel sgx, software guard extension, confidential computing, trusted execution environment, TEE, data protection

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Confidential computing with Intel Software Guard Extensions (SGX) for Virtual Servers for VPC
{: #about-sgx-vpc}

[Select availability]{: tag-green}

Confidential computing with Intel&reg; Software Guard Extensions (SGX) protects your data through hardware-based server security by using isolated memory regions that are known as encrypted enclaves. This hardware-based computation helps protect your data from disclosure or modification. Which means that your sensitive data is encrypted while it is in virtual server instance memory by allowing applications to run in private memory space. To use SGX, you must install the SGX drivers and platform software on SGX-capable worker nodes. Then, design your app to run in an SGX environment. For more information about SGX, see [Intel Software Guard Extensions](https://www.intel.com/content/www/us/en/developer/tools/software-guard-extensions/overview.html).
{: shortdesc}

Confidential computing with Intel SGX for VPC is available only in the US-South (Dallas) region.
{: note}

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

### Confidential computing with SGX use cases
{: #scenarios-sgx-vpc}

The following are some of the use cases for confidential computing with SGX.

* **Confidentiality and Privacy of Workloads and Applications** within a Confidential Computing environment make sure that data privacy and security applications are always protected.

* **Confidential AI and Analytics** enable data and business analytics applications, machine learning models, and applications within secure enclaves. Includes SMPC applications that also help gain data insights.

* **Secure Multi-party Compute** enables distributed SMPC that helps make sure that participant data and insights are protected even when calculated outside their direct control.

* **Digital Assets** is the trusted platform for digital custody solutions, for storing and transferring high-value digital assets in highly secure wallets, reliable at scale.

## SGX-compatible profiles
{: #compatible-profiles-confidential-computing-vpc-sgx}

The following profiles support SGX.

* All Balanced _bx3dc_ profiles
* All Compute _cx3dc_ profiles

For Gen3 profiles, you can enable and disable secure boot. But when you toggle between enable and disable the machine type changes. So, make sure that you create a snapshot before you change this setting.

SGX profiles might experience slightly longer start times, approximately in the range of 180-240 seconds, depending on profile EPC size.
{: note}

For more information about profiles, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles).

## Limitations
{: #limitations-confidential-computing-vpc-sgx}

Keep the following limitations in mind if you want to use SGX.

* Available on only third-generation Sapphire Rapids-based virtual servers.
* SGX doesn't protect against side-channel attacks.
* Only the following images support SGX. Keep in mind that images with kernel versions 5.11 and prior don't support SGX.
   - Red Hat 8.6, 8.8, 9.0, 9.2
   - Ubuntu 20.04, 22.04
   - CentOS Stream 8, 9
   - Rocky Linux 8.8, 9.2
   - SLES 15 SP4, SP5

If you resize a virtual server that is secure boot-enabled to a profile that is secure boot-disabled (and vice-versa), the topology of PCIe devices change. Depending on the operating system, this topology change can rename devices. The I/O performance can also change.
