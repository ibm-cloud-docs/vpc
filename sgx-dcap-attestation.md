---

copyright:
  years: 2023, 2025
lastupdated: "2025-01-14"

keywords: sgx, intel sgx, software guard extension, confidential computing, attestation, DCAP, data center attestation primitives

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attestation with Intel SGX or TDX for Virtual Servers for VPC
{: #about-attestation-sgx-dcap-vpc}

[Select availability]{: tag-green}

Attestation is a process that validates that a runtime environment is instantiated in an encrypted SGX or TDX enclave on a system with a known security configuration. Data Center Attestation Primitives (DCAP) from Intel facilitates Attestation.
{: shortdesc}

Confidential computing with Intel SGX for VPC is available only in the Dallas (us-south) and Frankfurt (eu-de) regions. Confidential computing with Intel TDX for VPC is available for select customers. Contact IBM Sales if you are interested in being allowlisted and using this offering.  Confidential computing with Intel TDX for VPC is available only in the Washington DC (us-east) region. Confidential computing is only available with select profiles. For more information, see [SGX-compatible profiles](/docs/vpc?topic=vpc-about-sgx-vpc&interface=ui#compatible-profiles-confidential-computing-vpc-sgx).
{: preview}

Intel SGX helps protect data in use through application isolation technology. Intel TDX helps protect data in use through virtual machine isolation technology. By using these features, developers can protect the integrity and confidentiality of their code and data.

## Enabling attestation in SGX VSI
{: #configuring-sgx-dcap-for-attestation}

The PCK certificate is readily made available in the SGX VSI, thus
eliminating the need to procure from a PCCS service. This certificate is at `/root/.dcap-qcnl/*`.

Install DCAP and QCNL packages; as specified by Intel.

Install DCAP version 1.19 or greater since previous versions do not support locally cached certificates.
{: note}

Reconfigure AESM to use the locally cached PCK certificate and restart the
service as shown in the following example.

```sh
 Configure  /etc/sgx_default_qcnl.conf
 "use_secure_cert": false
 "local_cache_only": true

 and restart aesmd

 systemctl restart aesmd
```
{: codeblock}

Non-root user must copy the `/root/.dcap-qcnl/*` directory to their `$HOME` directory to use DCAP.
{: note}

## Enabling attestation in TDX VSI
{: #configuring-tdx-dcap-for-attestation}

TDX supports vSOCK-based attestation which is based on DCAP SDK version 1.21 or greater.

Install DCAP package; as specified by Intel.

Configure the `qgsd` vSOCK port in the TDX virtual server instance. In the TDX virtual server instance, create a file named `/etc/tdx-attest.conf` and add the following line.

```sh
 port=4050
```
{: codeblock}

## SGX and TDX documentation from Intel
{: #sgx-tdx-documentation-intel}

For more information about SGX, see the following links.

* For DCAP, see [Intel® SGX Data Center AttestationPrimitives Intel® SGX DCAP](https://www.intel.com/content/dam/develop/public/us/en/documents/intel-sgx-dcap-ecdsa-orientation.pdf){: external}.

* For attestation by using DCAP, see [Quote Generation, Verification, and Attestation with Intel® Software Guard Extensions Data Center Attestation Primitives (Intel® SGX DCAP)](https://www.intel.com/content/www/us/en/developer/articles/technical/quote-verification-attestation-with-intel-sgx-dcap.html){: external}.

* For DCAP installation, see [Intel® Software Guard Extensions Data Center Attestation Primitives (Intel® SGX DCAP) - A Quick Install Guide](https://www.intel.com/content/www/us/en/developer/articles/guide/intel-software-guard-extensions-data-center-attestation-primitives-quick-install-guide.html){: external}.

* For the Attestation verification service, see [Intel® Trust Authority](https://docs.trustauthority.intel.com/main/articles/introduction.html){: external}.

For more information about TDX, see [Intel TDX Remote Attestation](https://cc-enabling.trustedservices.intel.com/intel-tdx-enabling-guide/02/infrastructure_setup/){: external}
