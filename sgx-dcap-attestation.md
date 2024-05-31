---

copyright:
  years: 2023, 2024
lastupdated: "2024-05-31"

keywords: sgx, intel sgx, software guard extension, confidential computing, attestation, DCAP, data center attestation primitives

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attestation with Intel SGX and Data Center Attestation Primitives (DCAP) for Virtual Servers for VPC
{: #about-attestation-sgx-dcap-vpc}

Attestation is a process that validates that a runtime environment is instantiated in an encrypted SGX enclave on a system with a known security configuration. Intel SGX DCAP facilitates Attestation.
{: shortdesc}

Confidential computing with Intel SGX for VPC is available in the US-South/Dallas region.
{: note}

Intel SGX helps protect data in use through application isolation technology. By protecting the integrity and confidentiality of selected code and data, developers can partition their application into hardened enclaves or trusted execution modules to help increase application security.

## Installing attestation with SGX DCAP
{: #configuring-dcap-for-attestation}

The PCK certificate is readily made available in the SGX VSI, thus
eliminating the need to procure from a PCCS service. This certificate is at `/root/.dcap-qcnl/*`.

Install all the required SGX DCAP and QCNL packages; as specified by Intel.

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

## SGX documentation from Intel
{: #sgx-documentation-intel}

For more information about SGX, see the following links.

* For DCAP, see [Intel® SGX Data Center AttestationPrimitives Intel® SGX DCAP](https://www.intel.com/content/dam/develop/public/us/en/documents/intel-sgx-dcap-ecdsa-orientation.pdf){: external}.

* For attestation by using DCAP, see [Quote Generation, Verification, and Attestation with Intel® Software Guard Extensions Data Center Attestation Primitives (Intel® SGX DCAP)](https://www.intel.com/content/www/us/en/developer/articles/technical/quote-verification-attestation-with-intel-sgx-dcap.html){: external}.

* For DCAP installation, see [Intel® Software Guard Extensions Data Center Attestation Primitives (Intel® SGX DCAP) - A Quick Install Guide](https://www.intel.com/content/www/us/en/developer/articles/guide/intel-software-guard-extensions-data-center-attestation-primitives-quick-install-guide.html){: external}.

* For the Attestation verification service, see [Intel® Trust Authority](https://docs.trustauthority.intel.com/){: external}.
