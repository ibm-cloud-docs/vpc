---

copyright:
  years: 2023, 2025
lastupdated: "2025-05-23"

keywords: sgx, intel sgx, software guard extension, confidential computing, attestation, DCAP, data center attestation primitives

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About attestation with Intel SGX or TDX for Virtual Servers for VPC
{: #about-confidential-computing-attestation-dcap-vpc}

[Select availability]{: tag-green}

Attestation is a validation process that that makes sure that a runtime environment is instantiated in an encrypted SGX or TDX enclave on a system with a known security configuration. Data Center Attestation Primitives (DCAP) from Intel facilitates Attestation.
{: shortdesc}

Confidential computing profiles are available in the Dallas (us-south), Washington DC (us-east), and Frankfurt (eu-de) regions. Confidential computing with Intel SGX for VPC is Dallas (us-south), Washington DC (us-east), and Frankfurt (eu-de). Confidential computing with Intel TDX for VPC is available only in the Washington DC (us-east) region. If you want to create a virtual server instance with a confidential computing profile and TDX, you can create that virtual server instance only in the Washington DC (us-east) region. You can’t create a virtual server instance with TDX in any other region, including Dallas (us-south) and Frankfurt (eu-de). For more information, see [Confidential computing known issues](/docs/vpc?topic=vpc-known-issues#confidential-computing-vpc-known-issues). Confidential computing is only available with select profiles. For more information, see [Confidential computing profiles](/docs/vpc?topic=vpc-profiles&interface=ui#confidential-computing-profiles).
{: preview}

Intel SGX helps protect data that is in use through application isolation technology. Intel TDX helps protect data that is in use through virtual machine isolation technology. By using these features, developers can protect the integrity and confidentiality of their code and data.

## Enabling attestation for SGX
{: #enabling-attestation-dcap-for-sgx}

The PCK certificate is available in the SGX virtual server, so you don't need to procure it from a PCCS service. This certificate is at `/root/.dcap-qcnl/*`. A non-root user must copy the `/root/.dcap-qcnl/*` directory to their `$HOME` directory to use DCAP.

Install DCAP and QCNL packages as specified by Intel.

Install DCAP version 1.19 or greater since previous versions do not support locally cached certificates.
{: note}

Reconfigure AESM to use the locally cached PCK certificate and restart the service as shown in the following example.

1. Configure  /etc/sgx_default_qcnl.conf

   ```sh
    "use_secure_cert": false
    "local_cache_only": true
   ```
   {: codeblock}

 1. Restart `aesmd`

   ```sh
    systemctl restart aesmd
   ```
   {: codeblock}

## Enabling attestation for TDX
{: #enabling-attestation-dcap-for-tdx}

To enable attestation for TDX, follow the Intel instructions [Trust Domain at Runtime](https://cc-enabling.trustedservices.intel.com/intel-tdx-enabling-guide/07/trust_domain_at_runtime/){: external}.

TDX doesn't support `vsock` interface for Quote Generation. For Quote Generation, you must use `tdvmcal`.
{: note}

## SGX and TDX documentation from Intel
{: #sgx-tdx-documentation-intel}

For more information about SGX and TDX, see the following links.

### SGX
{: #sgx-documentation-intel}

* For DCAP, see [Intel® SGX Data Center AttestationPrimitives Intel® SGX DCAP](https://www.intel.com/content/dam/develop/public/us/en/documents/intel-sgx-dcap-ecdsa-orientation.pdf){: external}.
* For attestation by using DCAP, see [Quote Generation, Verification, and Attestation with Intel® Software Guard Extensions Data Center Attestation Primitives (Intel® SGX DCAP)](https://www.intel.com/content/www/us/en/developer/articles/technical/quote-verification-attestation-with-intel-sgx-dcap.html){: external}.
* For DCAP installation, see [Intel® Confidential Computing Documentation](https://cc-enabling.trustedservices.intel.com)){: external}.
* For the Attestation verification service, see [Intel® Trust Authority](https://docs.trustauthority.intel.com/main/articles/introduction.html){: external}.

### TDX
{: #tdx-documentation-intel}

* For TDX, see [Intel TDX Enabling Guide](https://cc-enabling.trustedservices.intel.com/intel-tdx-enabling-guide/01/introduction/){: external}.
* For TDX remote attestation, see [Intel TDX Remote Attestation](https://cc-enabling.trustedservices.intel.com/intel-tdx-enabling-guide/02/infrastructure_setup/){: external}.
