---

copyright:
  years: 2022
lastupdated: "2022-06-17"

keywords: vpc, troubleshoot, tips, error, hpcr, API, CLI, problem, debug,  

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:troubleshoot: .troubleshoot}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:support: data-reuse='support'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Troubleshooting your IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}} instance
{: #troubleshooting-hpcr}

This document covers common difficulties you might encounter, and offers some helpful tips.
{: shortdesc}

## Common problems
{: #troubleshoot-hpcr-common}

Here are a few difficulties you might encounter.

### Image or Profile mismatch
{: #hpcr_troubleshoot_imagemismatch}

IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instances can be created from the UI, CLI and any other interface that {{site.data.keyword.vpc_full}} (VPC) supports, and you must select the right image and profile combination. Without the combination of the image and profile, the instance fails to start.  

### Only single data volume
{: #hpcr_troubleshoot_datavol}

Currently only a single data volume is supported. You might not get any explicit errors from VPC if you try to attach additional data volumes, but it could disrupt the functionality. When the IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance boots, it automatically detects the attached data volume and encrypts it. If it finds more than one data volume, it will randomly select one of them and encrypt it. The container workload stores data into this data volume, but when the IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance reboots, it follows the same process of randomly picking up one of the attached data volumes and there's a possibility that it does not pick up the one that has workload data. This leads to a situation where the workload no longer has access to its data.

### Attach data volume within 15 min
{: #hpcr_troubleshoot_volattach}

If you have a volumes section in the contract, the IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance assumes that a data volume will be attached. In such a case, the cloud user has to either attach the volume during the IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance creation, or within 15 minutes after instance creation has been initiated. After this 15 minute wait period, the instance shuts down.

#### Single workload
{: #hpcr_troubleshoot_workload}

Currently, only a single workload is supported. Therefore, only a single workload must be specified as part of the docker-compose file.

#### The contract is mandatory
{: #hpcr_troubleshoot_contract}

If you create an IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_short}} instance without any contract passed in through the "**User Data**" section, the instance will come to running state and then will shutdown eventually.

### Contract format is yaml
{: #hpcr_troubleshoot_contractformat}

The contract must follow the YAML format. If the format is not proper, it will not pass the contract validation check and the instance will shutdown eventually.

###  Contract Schema
{: #hpcr_troubleshoot_contract_schema}

If the contract schema is incorrect, the contract validation fails when the instance is booting and the instance will shut down. You can  check for errors associated with `hpcr-contract log` identifier in IBM Logging instance.

### Contract encryption key
{: #hpcr_troubleshoot_contract_encryptkey}

If you opt to use contract encryption, then you must take the correct key from the contract documentation. The contract is decrypted by the bootloader and if it is not encrypted by the correct key, then you will see decryption failures errors in the serial console of the instance.

### Logging configuration failure
{: #hpcr_troubleshoot_logconfig}

When the instance boots, monitor the serial console to identify if there are any errors logged by the bootloader or from the logging service. If you don't see any logs reaching your private IBM Logging instance, it could be because your logging configuration failed. Failure to configure logging also leads the instance shutting down. If your logging configuration has failed, you should check if the logging hostname, port and ingestion key provided in the contract are correct. If they are, check if you have a floating IP or a public gateway associated with the instance.
