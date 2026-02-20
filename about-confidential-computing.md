---

copyright:
  years: 2022, 2026
lastupdated: "2026-02-20"

keywords: confidential computing, enclave, secure execution, hpcr, hyper protect virtual server for vpc

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Confidential computing with LinuxONE
{: #about-se}

The {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for VPC is deprecated. As of 28 February 2026, you can't create new instances. Existing instances are supported until 20 February 2027. Any instances that still exist on that date will be deleted. You can redeploy your workloads by using [IBM Confidential Computing Container Runtime (formerly known as Hyper Protect Virtual Servers)](https://www.ibm.com/docs/en/hpvs) or [IBM Confidential Computing Container Runtime for Red Hat Virtualization Solutions (formerly known as Hyper Protect Container Runtime for Red Hat Virtualization Solutions)](https://www.ibm.com/docs/en/hpcr/1.1.x). For information about data migration, see the [Migration guide](/docs/vpc?topic=vpc-migration_guide). For more information, see the [Service deprecation announcement](/docs/vpc?topic=vpc-ichpcs_deprecated_anmt).
{: deprecated}

Confidential computing is enabled on LinuxONE (s390x processor architecture) by using the [IBM Secure Execution for Linux](https://www.ibm.com/docs/en/linux-on-systems?topic=management-secure-execution){: external} technology. This technology is part of the hardware of IBMz17, IBM z16, IBM z15, IBM LinuxONE Emperor 4,IBM LinuxONE Emperor 5, and IBM LinuxONE III systems. With IBM Secure Execution for Linux, you can securely deploy workloads in the cloud. It helps ensure the integrity and confidentiality of boot images, and server authenticity. Applications are isolated from the operating system, thus providing more privacy and security for the workload.
{: shortdesc}

By using IBM Secure Execution for Linux, you can create encrypted Linux images that can run on a public, private, or hybrid cloud with their in-use memory protected. The workload or data is protected from external and insider threats.

A new operating system that uses the IBM Secure Execution for Linux technology is now available as **IBM Hyper Protect**. The associated image that is used to create the instance is called the [IBM Hyper Protect Container Runtime (HPCR) image](/docs/vpc?topic=vpc-vsabout-images#hyper-protect-runtime). A virtual server instance that's provisioned by using this image is called an **{{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for VPC** (Virtual Private Cloud) instance.

For a technical deep dive into the IBM Hyper Protect Platform, see the white paper [The Second Generation of IBM Hyper Protect Platform](https://www.ibm.com/downloads/documents/us-en/107a02e95c48f700){: external}.

Check out the [tutorial](/docs/vpc?topic=vpc-financial-transaction-confidential-computing-on-hyper-protect-virtual-server-for-vpc) and the [video](https://mediacenter.ibm.com/media/Confidential+Computing+for+a+financial+transaction+using+Hyper+Protect+Virtual+Server+for+VPC/1_vv3j2oo6){: external} on Confidential Computing for a financial transaction by using Hyper Protect Virtual Server for VPC. You can learn about how to protect Personally Identifiable Information and credit card information that is entered into web forms by using Confidential Computing on Hyper Protect Virtual Server for VPC.

For the on-prem version of {{site.data.keyword.hpvs}}, see [IBM Hyper Protect Virtual Servers v2.1](https://community.ibm.com/community/user/blogs/nicolas-mding/2022/10/06/ibm-hyper-protect-virtual-servers-v21){: external}.

## {{site.data.keyword.hpvs}} for VPC
{: #about-hyper-protect-virtual-servers-for-vpc}

The {{site.data.keyword.hpvs}} for VPC takes advantage of the IBM Secure Execution for Linux to provide a boundary around each instance and provides the following benefits: 

- Select Availability

   Instance profiles for the Hyper Protect Virtual Server instances are available in the US South (Dallas), US East (Washington, DC), Canada (Toronto), Brazil (São Paulo), United Kingdom (London), Germany (Frankfurt), Spain (Madrid), and Japan (Tokyo) regions.

- Secure Execution boundary for protection from internal and external threats

   {{site.data.keyword.hpvs}} for VPC provides technical assurance that unauthorized users, including {{site.data.keyword.cloud_notm}} admins, do not have access to the application. Workloads are locked down by individual, instance-level secure boundaries.

- Multiparty contract and attestation of deployment

   Apply zero trust principles from workload development through deployment. As multiple personas and legal entities collaborate, it’s essential to separate duty and access. {{site.data.keyword.hpvs}} for VPC is based on a newly introduced [encrypted contract](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt) concept. It enables each persona to provide its contribution and be ensured through encryption that none of the other personas can access this data or intellectual property. The deployment can be validated by an auditor persona through an [attestation record](/docs/vpc?topic=vpc-about-attestation), which is signed and encrypted to ensure that only the auditor has this level of insight.

- Malware protections

   {{site.data.keyword.hpvs}} for VPC uses Secure Build to set up a verification process to ensure that only authorized code is running in an application. It deploys only container versions that are validated at deployment through explicit [digest or are signed](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_images), and it can be pulled only from a [private Container Registry with authentication](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_auths).

- Bring your own OCI image and use managed Container Runtime service

   Use any [open-container initiative (OCI)](https://opencontainers.org/){: external} image and gain the benefits of a confidential computing solution for extra levels of protection. Ensure through a [signed contract](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_sign), that only a specific combination of your workload and environment is deployed.


- Built on the Virtual Private Cloud (VPC) infrastructure for extra network security

   Choose from various profile sizes and grow as needed to protect containerized applications and pay-as-you-go on an hourly basis.

   Leverage your existing or common [network security groups](/docs/vpc?topic=vpc-security-in-your-vpc) and [logging infrastructure](/docs/vpc?topic=vpc-logging-for-hyper-protect-virtual-servers-for-vpc).

Complete the following steps to get started with {{site.data.keyword.hpvs}} for VPC. Most importantly, a valid [contract](/docs/vpc?topic=vpc-about-contract_se) is required for creating an instance.

## Before you begin
{: #before-you-begin}

It's important to read the following information and complete the required preparations before you create a {{site.data.keyword.hpvs}} for VPC instance.

- [Planning](/docs/vpc?topic=vpc-vsi_best_practices)

   Consider aspects such as profiles and operating system images. To create a Hyper Protect Virtual Servers for VPC instance with a stock image, choose the [IBM Hyper Protect Container Runtime image](/docs/vpc?topic=vpc-vsabout-images#hyper-protect-runtime).

- Choose to build your own image with {{site.data.keyword.hpvs}}

   Apart from using the stock image, you can choose to securely build your own image with {{site.data.keyword.hpvs}} and use that image to provision a {{site.data.keyword.hpvs}} for VPC instance. For more information, see [Hyper Protect Secure Build](/docs/vpc?topic=vpc-about-hpsb).

- [Prepare a contract](/docs/vpc?topic=vpc-about-contract_se)

   When you create an instance, you must pass a **valid contract** in the **User Data** field. The IBM Hyper Protect Container Runtime image consists of different components or services that decrypts the contract (if it's encrypted), validates the contract schema, checks for contract signature, creates the passphrase to encrypt the disk device, and brings up the container that's specified in the contract.

   If no contract is passed, the instance eventually shuts down.
   {: important}

- [Set up logging](/docs/vpc?topic=vpc-logging-for-hyper-protect-virtual-servers-for-vpc)

   To launch a {{site.data.keyword.hpvs}} for VPC instance, you need to set up logging first by **adding the logging configuration in the contract**. After deployment, you can use two types of logging to view the {{site.data.keyword.hpvs}} for VPC instance logs.

   - Logging service that you set up

      See [Logging for {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-logging-for-hyper-protect-virtual-servers-for-vpc).

      If logging isn't configured successfully, the instance shuts down automatically.
      {: important}

   - Serial console

      After you select the operating system and proceed with the deployment, you can view the status of the deployment and also view the logs on the serial console to confirm whether the instance is a {{site.data.keyword.hpvs}} for VPC instance. You can also use the information from this log to troubleshoot provisioning issues. For more information, see [Accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console&interface=ui).

- Data volume

   The instance supports multiple data volumes that are [encrypted by default with the seed or passphrase that you provide](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_volumes), which helps ensure that your workload data is protected. It is recommended that you attach all the required data volumes to the instance during the instance creation so that data from the container is stored in the data volumes. It is also recommended that you take a snapshot of the data volumes so that you can revert to it in case you face any issues in future.

   Starting from the HPCR image version `ibm-hyper-protect-container-runtime-1-0-s390x-9`, for new {{site.data.keyword.hpvs}} for VPC instances, the data volume is partitioned into two parts. The first partition (100Mib) is reserved for internal metadata; the second partition remains as the data volume for workload. Only new volumes are partitioned, and you can't use the partitioned volume with an older version of the HPCR image. Provisioning with an existing encrypted volume also works. The difference is that the existing volume does not get partitioned, and you can also go back to an older image with this volume.

   When you create a {{site.data.keyword.hpvs}} for VPC instance with an attached data volume, detaching that data volume causes the workload running on the instance to fail. It's recommended that you do not detach the data volume.

- Security group for ports

   The ports that are associated with the container or workload must be explicitly opened up through security groups for them to be accessible. Therefore, you must create a security group based on the ports that are associated with the container and attach it to the corresponding VPC that's used with the {{site.data.keyword.hpvs}} for VPC instance.

## Creating a {{site.data.keyword.hpvs}} for VPC instance
{: #create-hyper-protect-virtual-servers-for-vpc-instance}

After you finish planning and have the contract ready, you can create a {{site.data.keyword.hpvs}} for VPC instance.

- [Creating an instance by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui). On the [provisioning page](https://cloud.ibm.com/infrastructure/provision/vs?architecture=s390x&secureExecution=true){: external}, make sure that you select **IBM Z, LinuxONE** for **Architecture**, and turn on the **Run your workload with an OS and a profile protected by Secure Execution** toggle under **Confidential computing**.
- [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli)

See the [troubleshooting documentation](/docs/vpc?topic=vpc-hyper-protect-virtual-server-shutdown) or [get support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc&interface=cli) if you have any problems with the instance.

You can use [Terraform](/docs/vpc?topic=vpc-terraform-for-hyper-protect-virtual-servers-for-vpc) to automate operations with {{site.data.keyword.hpvs}} for VPC.

You can use your {{site.data.keyword.hpvs}} for VPC instance in **private-only** network configurations, in which the VPC doesn't have a public gateway, and the virtual server instance doesn't have a floating IP. You can connect to private endpoints of other services, including Container Registry and [IBM Log Analysis](/docs/vpc?topic=vpc-logging-for-hyper-protect-virtual-servers-for-vpc). The prerequisite is to have a DNS server that is attached to your virtual server instance. You don't need to do any additional configurations.


## Recovering or upgrading a {{site.data.keyword.hpvs}} for VPC instance by using {{site.data.keyword.vpc_short}} Snapshots.
{: #recover-upgrade-hyper-protect-virtual-servers-for-vpc-using-snapshots}

If your {{site.data.keyword.hpvs}} for VPC fails for any reason, you can create a new instance and attach the data volume that was attached to the failed instance (within 15 minutes of creating the new instance). Ensure that you use the same contract that was used originally to create the instance.

Alternatively, [create a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-create&interface) of the data volume that was attached to the failed instance. Then create a new instance. When you attach the data volume, choose [restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface) and select the snapshot that you took earlier to restore the volume from. Ensure that you use the same contract that was used originally to create the instance.

When you want to use a new Hyper Protect Container Runtime image whenever IBM makes it available, you can follow either of the procedures that are mentioned that uses snapshots. You must use the same contract that you used for creating the original instance.
