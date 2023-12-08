---

copyright:
  years: 2023
lastupdated: "2023-12-07"

keywords: sgx, intel sgx, software guard extension, confidential computing, attestation, DCAP, data center attestation primitives

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attestation with Intel SGX and Data Center Attestation Primitives (DCAP) for Virtual Servers for VPC
{: #about-attestation-sgx-dcap-vpc}

[Select availability Beta]{: tag-blue}

Attestation with SGX confirms that the intended software or code is running within an encrypted enclave. In other words, attestation with SGX provides evidence that you are running inside a properly instantiated encrypted enclave, on a system that has a known security configuration. With Intel SGX DCAP, you can use remote attestation to check that software is properly running in a secure enclave.
{: shortdesc}

Confidential computing with Intel SGX for VPC is available only to select users in the US-South/Dallas region.
{: note}

When you develop a confidential computing application, you must design it so you can segment the information that needs encryption. At run time, the segmented information is kept confidential through attestation. When a request for information from the segmented code or app data is received, the encrypted enclave verifies that the request comes from the part of the application that exists outside of the enclave within the same application before it share any information. Through the attestation process, information is kept confidential and data leakage is prevented.

Attestation is signed and verified when you provision a server with Elliptic Curve Digital Signature Algorithm (ECDSA)-signed collateral that is then saved in the caching service. While SGX DCAP helps uphold the integrity and confidentiality of your data (thanks to the encrypted enclaves), SGX DCAP doesn't protect against all attack types - such as side-channel attacks.

For more information about DCAP, see [Intel® SGX Data Center Attestation
Primitives Intel® SGX DCAP](https://www.intel.com/content/dam/develop/public/us/en/documents/intel-sgx-dcap-ecdsa-orientation.pdf){: external}.

For more information about installing DCAP, see [Intel® Software Guard Extensions Data Center Attestation Primitives (Intel® SGX DCAP): A Quick Install Guide](https://www.intel.com/content/www/us/en/developer/articles/guide/intel-software-guard-extensions-data-center-attestation-primitives-quick-install-guide.html){: external}.

## Performing attestation with SGX DCAP
{: #perform-attestation-with-sgx-dcap-vpc}

Use the following command to perform attestation with DCAP. For more information about performing attestation with SGX DCAP, see [Quote Generation, Verification, and Attestation with Intel® Software Guard Extensions Data Center Attestation Primitives (Intel® SGX DCAP)](https://www.intel.com/content/www/us/en/developer/articles/technical/quote-verification-attestation-with-intel-sgx-dcap.html){: external}.

Make sure that you use DCAP version 1.19 or greater. Previous DCAP versions don't support offline cache-based attestation.
{: important}

   ```sh
   Configure  /etc/sgx_default_qcnl.conf
   "use_secure_cert": false
   "local_cache_only": true

   and restart aesmd

   systemctl restart aesmd
   ```
   {: pre}
