---

copyright:
  years: 2019, 2026
lastupdated: "2026-02-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Service deprecation: IBM Cloud LinuxONE and Hyper Protect Virtual Server offerings
{: #ichpcs_deprecated_anmt}

IBM announces the deprecation of the following services:

   - [IBM Cloud LinuxONE Virtual Server for VPC (s390x architecture)](/docs/vpc?group=virtual-servers)
   - [IBM Cloud Dedicated Host for VPC (s390x architecture)](/docs/vpc?group=dedicated-hosts)
   - [IBM Cloud Hyper Protect Virtual Server for VPC](https://www.ibm.com/products/hyper-protect-virtual-servers/cloud)
   - [IBM Cloud Wazi as a Service](https://www.ibm.com/products/wazi-as-a-service)

## Important dates
{: #ichpcs_deprecated_dates}

| Milestone | Date | Details |
|----------|------|---------|
| **End of Marketing (EoM)** | **28 February 2026** | IBM will disable new instance provisioning. Instances provisioned before this date remain supported until the EoS date. |
| **End of Service (EoS)** | **20 February 2027** | IBM will terminate all active instances on this date. |
{: caption="Migration dates" caption-side="bottom"}

## Action required
{: #ichpcs_deprecated_action}

Migrate all active instances to the recommended alternatives. 

| Deprecated Service | Recommended Alternative |
|--------------------|-------------------------|
| IBM Cloud LinuxONE Virtual Server for VPC (s390x architecture) | Redeploy workloads on-premises using SUSE Linux Enterprise Server or Ubuntu Linux. For more information, see [Virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui). |
| IBM Cloud Dedicated Host for VPC (s390x architecture) | Migrate to an on-premises deployment model. For more information, see [Creating dedicated hosts and groups](/docs/vpc?group=dedicated-hosts). |
| IBM Cloud Hyper Protect Virtual Servers for VPC | Redeploy workloads by using Hyper Protect Virtual Servers (SLES or Ubuntu) or Hyper Protect Container Runtime for Red Hat Virtualization. For more information, see [Confidential computing with LinuxONE](/docs/vpc?topic=vpc-about-se). |
| IBM Cloud Wazi as a Service | The On-Demand Environment capability in [IBM Test Accelerator for Z](https://www.ibm.com/products/test-accelerator-z) provides replacement software that lets enterprises host z/OS on virtualized or emulated IBM® Z hardware. It runs on Linux on x86-64 or s390x systems and supports mainframe application demonstration, development, testing, and education use cases. For more information, see [Release notes](https://www.ibm.com/docs/en/wazi-aas/1.1.0?topic=release-notes).|
{: caption="Migration information" caption-side="bottom"}

## Support and assistance
{: #ichpcs_deprecated_support}

Contact your **IBM representative** or **Customer Service Manager** for migration planning or technical support. They will coordinate with IBM Cloud Support or the **Z Client Acceleration Team** as needed.
