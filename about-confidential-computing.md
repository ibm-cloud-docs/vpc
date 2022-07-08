---

copyright:
  years: 2022
lastupdated: "2022-06-23"

keywords: confidential computing, enclave, secure execution, hpcr, hyper protect virtual server for vpc

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Confidential computing with LinuxONE
{: #about-se}

Confidential computing is enabled on LinuxONE (s390x processor architecture) by using the [IBM Secure Execution for Linux](https://www.ibm.com/docs/en/linux-on-systems?topic=overview-introducing-secure-execution-linux) technology. This technology is part of the hardware of IBM z15 (z15) and IBM LinuxONE III generation systems. With IBM Secure Execution for Linux, you can securely deploy workloads in the cloud. It ensures the integrity and confidentiality of boot images, and server authenticity. Applications are isolated from the operating system, thus providing more privacy and security for the workload.
{: shortdesc}

By using IBM Secure Execution for Linux, you can create encrypted Linux images that can run on a public, private, or hybrid cloud with their in-use memory protected. The workload or data is protected from external and insider threats.

A new operating system called as IBM Hyper Protect is now available and the associated image that is used to create the instance is called IBM Hyper Protect Container Runtime image. On the create virtual server page, under **Confidential computing**, click the **Run your workload with an OS and a profile protected by Secure Execution** toggle to enable secure execution. A virtual server instance that is provisioned by using the [IBM Hyper Protect Container Runtime image](/docs/vpc?topic=vpc-vsabout-images#hyper-protect-runtime) is called as an IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}} instance. A new set of secure execution profiles are also available when you select IBM Hyper Protect as the operating system. For more information about profiles, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles). You would also need a valid contract, which is used to provide container details at instance creation. For more information, see [About the contract](/docs/vpc?topic=vpc-about-contract_se).

## IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}}
{: #about_hyperprotect_vs_forvpc}

The IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} takes advantage of the IBM Secure Execution for Linux to provide a boundary around each instance and provides the following benefits:

* Managed Container Runtime service.
* Secure Execution boundary for protection from internal and external threats.
* Built on the Virtual Private Cloud (VPC) infrastructure for extra network security.

To create an IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance on LinuxONE (s390x processor architecture), see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers), and [IBM Hyper Protect Container Runtime image](/docs/vpc?topic=vpc-vsabout-images#hyper-protect-runtime). A valid contract is required for creating an instance.   

## Overview
{: #hpcr_overview}

You must pass a valid contract as part of the user data, in the **User Data** field. If no contract is passed, then the instance eventually shuts down. The IBM Hyper Protect Container Runtime image consists of different components or services that decrypts the contract (if it is encrypted), validates the contract schema, checks for contract signature, creates the passphrase to encrypt the disk device, and brings up the containers that are specified in the contract.

Currently, the instance supports only one data volume and this volume is encrypted by default with the seed or passphrase that you provide, which ensures that your workload data is protected. Even though you can add multiple data volumes, they are ignored and only one of them is encrypted. It is recommended that you attach a data volume to the instance during instance creation so that data from the container is stored in the data volume. It is also recommended that you take a snapshot of the data volume so that you can revert to it in case you face any issues in the future.

You can use two types of logging to view the IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance logs:
* Serial console logs - After you select the operating system and proceed with the deployment, you can view the status of the deployment and also view the logs that are displayed on the serial console to confirm whether the instance being deployed is an IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance. You can also use the information from this log to troubleshoot provisioning issues. For more information, see [Accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console&interface=ui).
* LogDNA logs - The LogDNA configuration that is passed as part of the contract works only if the instance has either a public gateway or a floating IP associated with it. If logging is not configured successfully, the instance shuts down automatically. For more details, see [Setting up logging for IBM Cloud Hyper Protect Virtual Server for VPC deployment](#hpcr_setup_logging).

The ports that are associated with the containers or workloads must be explicitly opened up through security groups for them to be accessible. Therefore, you must create a security group based on the ports that are associated with the containers and attach it to the corresponding VPC that is used with the IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance.

## Recover or upgrade an IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance by using {{site.data.keyword.vpc_short}} Snapshots.
{: #hpcr_snapshots}

If your IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} fails for any reason, you can create a new instance and attach the data volume that was attached to the failed instance (within 15 minutes of creating the new instance). Ensure that you use the same contract that was used originally to create the instance.

Alternatively, [create a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-create&interface) of the data volume that was attached to the failed instance, then you create a new instance and when you attach the data volume, choose [restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface) and select the snapshot that you took earlier to restore the volume from. Ensure that you use the same contract that was used originally to create the instance.

When you want to use a new Hyper Protect Container Runtime image whenever IBM makes it available, you can follow either of the procedures that are mentioned that uses snapshots. You must use the same contract that you used for creating the original instance.

## Setting up logging for IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} provisioning
{: #hpcr_setup_logging}

A logging endpoint from IBM Cloud platform logging is needed for the successful launch of an IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance. If an incorrect log endpoint is configured, then the IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance shuts down as soon it starts.

To get the logging details after you bring up an IBM Logging instance, complete the following steps:

1. [Log in to your IBM Cloud account](https://cloud.ibm.com/login).
2. [Provision an IBM Logging instance](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-provision). Choose a plan according to your requirements.  
3. After you create the logging instance, click **Open dashboard** of the instance.
4. Click the "?" icon at the lower left of the page. On the add a log source page, under **Via syslog**, click **rsyslog**.
5. Make a note of the ingestion key value that is displayed at the upper right of the page, and the endpoint value as shown in the example:
   ```sh
   ### START LogDNA rsyslog logging directives ###

   ## TCP TLS only ##
   $DefaultNetstreamDriverCAFile /etc/ld-root-ca.crt # trust these CAs
   $ActionSendStreamDriver gtls # use gtls netstream driver
   $ActionSendStreamDriverMode 1 # require TLS
   $ActionSendStreamDriverAuthMode x509/name # authenticate by hostname
   $ActionSendStreamDriverPermittedPeer *.au-syd.logging.cloud.ibm.com
   ## End TCP TLS only ##

   $template LogDNAFormat,"<%PRI%>%protocol-version% %timestamp:::date-rfc3339% %HOSTNAME% %app-name% %procid% %msgid% [logdna@48950 key=\"bc8a5ba9aa5c0c12b26c6c45089228a4\"] %msg%"

   # Send messages to LogDNA over TCP using the template.
   *.* @@syslog-u.au-syd.logging.cloud.ibm.com:6514;LogDNAFormat

   ### END LogDNA rsyslog logging directives ###
   ```
   {: codeblock}

   In this example, the value of the endpoint is `syslog-u.au-syd.logging.cloud.ibm.com`
6. Add these values in the `env` `logging` subsection of the contract. For more information, see [`logging` subsection](/docs/vpc?topic=vpc-about-contract_se&interface=ui#hpcr_contract_env_log).

   When the IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance boots, the IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} logging instance extracts the logDNA information from the contract and configures it, so that all the logs are routed to the endpoint specified. Then, you can open the logging dashboard and view the logs from within the IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance.

## Deploying images that are built by using {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}}

You can securely build your own image on {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}}, and use that image to provision an IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance. For more information on securely building your own image, see [Protecting your image build](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-imagebuild).

Ensure that you use the latest Secure Build CLI code and also use the latest `secure_build.asc` file. For more information, see [Deploying the Secure Build Server as a Hyper Protect Virtual Server](/docs/hp-virtual-servers?topic=hp-virtual-servers-imagebuild#deploysecurebuild)
You can get the public key by following these [instructions](https://github.com/ibm-hyper-protect/secure-build-cli#how-to-extract-public-key-used-for-signing-container-image-inside-sbs). You can now create the contract and provision an IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance. For more information, see [About the contract](/docs/vpc?topic=vpc-about-contract_se), and [Creating an instance](/docs/vpc?topic=vpc-creating-virtual-servers).

## Next steps
{: #next-steps-se}

After you choose a profile, it's time to plan for and create an instance.
* [Planning for instances](/docs/vpc?topic=vpc-vsi_best_practices)
* [Creating an instance by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers)
* [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli)
