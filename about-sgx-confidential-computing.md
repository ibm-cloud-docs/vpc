---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-19"

keywords: sgx, intel sgx, software guard extension, confidential computing, trusted execution environment, TEE, data protection

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Confidential computing for x86 Virtual Servers for VPC
{: #about-confidential-computing-vpc}

[Select availability]{: tag-green}

Confidential computing is a new technology that offers technical assurances that workloads and data are confidential and protected from everyone, including the Cloud Service Provider (CSP).
{: shortdesc}

Confidential computing profiles are available in the Dallas (us-south), Washington DC (us-east), and Frankfurt (eu-de) regions. Confidential computing with Intel SGX for VPC is Dallas (us-south), Washington DC (us-east), and Frankfurt (eu-de). Confidential computing with Intel TDX for VPC is available only in the Washington DC (us-east) and Frankfurt (eu-de) regions. If you want to create a virtual server instance with a confidential computing profile and TDX, you can create that virtual server instance only in the Washington DC (us-east) Frankfurt (eu-de) regions. You can’t create a virtual server instance with TDX in any other region, including Dallas (us-south). For more information, see [Confidential computing known issues](/docs/vpc?topic=vpc-known-issues#confidential-computing-vpc-known-issues). Confidential computing is only available with select profiles. For more information, see [Confidential computing profiles](/docs/vpc?topic=vpc-profiles&interface=ui#confidential-computing-profiles).
{: preview}

## Confidential computing with Intel Trusted Domain Extension (TDX)
{: #confidential-computing-vpc-with-tdx}

Confidential computing with Intel Trust Domain Extensions(TDX) offers confidentiality to virtual machines by providing CPU enhancements that are leveraged by the firmware and hardware to provide confidentiality and integrity. Everything within the virtual machine is confidential and can't be eavesdropped. Also, everything within the virtual machine is integrity protected and can't be tampered. For more information about TDX, see [Intel Trust Domain Extensions](https://www.intel.com/content/www/us/en/developer/tools/trust-domain-extensions/overview.html){: external}.

## Confidential computing with Intel Software Guard Extensions (SGX)
{: #confidential-computing-vpc-with-sgx}

Confidential computing with Intel&reg; Software Guard Extensions (SGX) protects your data through hardware-based server security by using isolated memory regions that are known as encrypted enclaves. This hardware-based computation helps protect your data from disclosure or modification. Which means that your sensitive data is encrypted while it is in virtual server instance memory by allowing applications to run in private memory space. To use SGX, you must install the platform software on SGX-capable worker nodes. Then, design your app to run in an SGX environment. While your sensitive data is inside an encrypted enclave, your data is split into trusted and untrusted parts. While the trusted parts are used in the encrypted enclave, the CPU denies all other access to the enclave regardless of access privileges. The data is guarded from internal and external threats and can't be stolen or sabotaged.

When you use confidential computing with SGX, your data is protected through the entire compute lifecycle. Which means that your data is accessible only to authorized code and is invisible to anyone or anything else, including the operating system and {{site.data.keyword.cloud}}.

### SGX and TDX are trusted execution environments (TEE)
{: #trusted-execution-environments-vpc}

Both SGX and TDX use a trusted execution environment (TEE). TEE is a secure area of the main processor that provides a higher level of data security for trusted data and applications.

A TEE sets up an isolated, secure area of the main processor on a device that is dedicated to processing and storing sensitive data. The secure environment is protected from unauthorized access - even if the system software is compromised.

So, all Intel SGXs and TDxs are TEEs, but not all TEEs are Intel SGXs or TDXs.

## Attestation
{: #attestation-confidential-computing-vpc}

When you develop a confidential computing SGX application, you must design it so you can segment the information that needs confidentiality. At run time, the segmented information is kept in encrypted enclaves. The confidential information is loaded into the encrypted enclaves, only after the encrypted enclaves proves its authenticity through a process called attestation. For more information about attestation with Intel SGX and TDX, see [Attestation with Intel SGX or TDX and Data Center Attestation Primitives (DCAP)](/docs/vpc?topic=vpc-about-confidential-computing-attestation-dcap-vpc).

## Confidential computing with SGX and TDX use cases
{: #confidential-computing-scenarios-vpc}

The following are some of the use cases for confidential computing with SGX and TDX.

* **Confidentiality and Privacy of Workloads and Applications** within a Confidential Computing environment make sure that data privacy and security applications are always protected.

* **Confidential AI and Analytics** enable data and business analytics applications, machine learning models, and applications within secure enclaves. It includes SMPC applications that also help gain data insights.

* **Secure Multi-party Compute** enables distributed SMPC that helps make sure that participant data and insights are protected even when calculated outside their direct control.

* **Digital Assets** is the trusted platform for digital custody solutions, for storing and transferring high-value digital assets in highly secure wallets, reliable at scale.

## SGX and TDX compatible profiles
{: #compatible-profiles-confidential-computing-vpc}

The following profiles support SGX.

* All Balanced _bx3dc_ profiles
* All Compute _cx3dc_ profiles

The following profiles support TDX.

* All Balanced _bx3dc_ profiles with less than 160 GB memory
* All Compute _cx3dc_ profiles with less than 160 GB memory

SGX and TDX profiles might experience slightly longer start times, approximately in the range of 180-240 seconds, depending on profile memory size.
{: note}

For more information about profiles, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles).

## Limitations
{: #limitations-confidential-computing-vpc}

Keep the following limitations in mind if you want to use SGX or TDX.

* Available on only third-generation Sapphire Rapids-based virtual servers.
* SGX doesn't protect against side-channel attacks.
* Only the following images support SGX and TDX. Keep in mind that images with kernel versions 5.11 and earlier don't support SGX and images with kernel version 6.5 and earlier don't support TDX.

   - SGX
      - Red Hat 8.6, 8.8, 9.0, 9.2
      - Ubuntu 20.04, 22.04
      - CentOS Stream 8, 9
      - Rocky Linux 8.8, 9.2
      - SLES 15 SP4, SP5
   - TDX
      - Ubuntu 24.04
      - Red Hat 9.4
      - CentOS Stream 9
      - Rocky Linux 9.2, 9.4
      - SLES 15.6

* TDX limitations
   - When you force a reboot of a TDX-enabled virtual server, the virtual server shuts down. You must then restart this virtual server.
   - VNC-console is not supported.
   - TDX doesn't support the `vsock` interface for Quote Generation. For Quote Generation, you must use `tdvmcal`.
