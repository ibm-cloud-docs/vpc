---

copyright:
  years: 2023, 2026
lastupdated: "2026-02-20"

keywords: confidential computing, enclave, secure execution, hpcr, contract, security requirement, additional security, env, workload, encryption

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# Additional security responsibilities for {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for VPC
{: #hpvs-security-req}

The {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for VPC is deprecated. As of 28 February 2026, you can't create new instances. Existing instances are supported until 20 February 2027. Any instances that still exist on that date will be deleted. You can redeploy your workloads by using [IBM Confidential Computing Container Runtime (formerly known as Hyper Protect Virtual Servers)](https://www.ibm.com/docs/en/hpvs) or [IBM Confidential Computing Container Runtime for Red Hat Virtualization Solutions (formerly known as Hyper Protect Container Runtime for Red Hat Virtualization Solutions)](https://www.ibm.com/docs/en/hpcr/1.1.x). For information about data migration, see the [Migration guide](/docs/vpc?topic=vpc-migration_guide). For more information, see the [Service deprecation announcement](/docs/vpc?topic=vpc-ichpcs_deprecated_anmt).
{: deprecated}

Learn about the security related responsibilities that you must observe when you use {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for VPC.

You must observe the following security best practices that help in maintaining a more secure environment:
* When the current HPVS image is deprecated, you must upgrade to the latest version as soon as possible to ensure continued support and compatibility.
* Ensure that you update the environment regularly to the latest available images when they are made available.
* Take the required actions on regular security notifications from IBM.
* Ensure that only required ports are opened and the ports are secured (TLS enabled). If you want to open up any port on the virtual server instance, ensure that you follow the security best practices. IBM is not responsible for any security incidents that arise from the usage of the port.
* Ensure only trusted or known users are allowed access to the environment and virtual servers.
* Employ the principle of least privilege where it is essential for minimizing security risks in your Docker environment. Avoid running containers as non-root users, or as privileged containers.
* The AppArmor Linux kernel security module is enabled on virtual server instance. For more information, see [Using AppArmor](https://documentation.ubuntu.com/server/apparmor/){: external}.
* If an image is deprecated, it is recommended to upgrade to the latest available version. Once the image is obsolete, the vsi creation or start will fail with error as image is no longer valid.
* You must observe the following best practices for the contract:
   - It is recommended that all sections of contract are encrypted. For more information, see [Contract encryption](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt).
   - To ensure the integrity of the contract, it is recommended that you sign the contract. For more information, see [Contract signature](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_sign).
   - The container images can be signed. For more information, see [IBM Hyper Protect Container Runtime images](/docs/vpc?topic=vpc-vsabout-images#hyper-protect-runtime).
   - It is your responsibility to keep the copy of the contract that you created safe to prevent inadvertent security risks because you won't be able to retrieve it once it is lost.
   - Input data can be validated by using the attestation record. For more information, see [Attestation](/docs/vpc?topic=vpc-about-attestation).
   - The attestation records can be encrypted by using the [attestationPublicKey](/docs/vpc?topic=vpc-about-attestation#attest_pubkey).
   - You can validate the certificates that you download for contract encryption and attestation. For more information, see [Validating the certificates](/docs/vpc?topic=vpc-cert_validate).
   - Ensure that the seeds you use in the contract are not easy to guess or crack.
   - Ensure that all software you define in the contract are from trusted sources.
