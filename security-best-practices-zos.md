---

copyright:
  years: 2022, 2024
lastupdated: "2024-02-21"

keywords: zos, security, virtual server instance

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Security best practices for z/OS virtual server instances
{: #security-best-practices-zos}

To secure your z/OS virtual server instance and identify any security vulnerabilities, you must refer to the following security information and industry standard security reports.
{: shortdesc}

Products and services in the z/OS stock images are periodically reviewed and updated. IBM continues to follow the standard security guidance and provides differentiating technologies in security and data privacy, focusing on but not limited to the following areas:
* Security patch management
* Firmware currency
* Setup of suggested products and services
* Configuration of hardware and software (operating systems, middleware, third-party applications, open source, network cards, and so on)
* Integration and monitoring of software, hardware, and end-points
* Cybersecurity:
    * Least access privilege
    * Separation of duties
    * Defense-in-depth
    * Authentication strength
    * End-to-end encryption

For this release, you need to follow the security best practices for your z/OS virtual server instance:

* If you want to apply the latest service for your instance, it is suggested that you wait until the next month’s refresh stock image to provision a new z/OS virtual server instance.

* Ensure that you follow the password policy, see [Configuring the password](/docs/vpc?topic=vpc-vsi_is_connecting_zos#configure-password).

* For more information about various security topics, see the following references:

    * [What is zero trust?](https://www.ibm.com/topics/zero-trust){: external}

    * [NIST Special Publication Security & Privacy Controls for Information Systems & Organizations](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53r5.pdf){: external}

    * [IBM X-Force Threat Intelligence Report](https://www.ibm.com/reports/threat-intelligence){: external}

    * [IBM on Enterprise Security](https://www.ibm.com/z/security){: external}

    * [PCI DSS v4.0](https://docs-prv.pcisecuritystandards.org/PCI%20DSS/Standard/PCI-DSS-v4_0.pdf){: external}

    * [CIS Benchmarks for IBM Z](https://workbench.cisecurity.org/files/3877){: external}
