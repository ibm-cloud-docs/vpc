---

copyright:
  years: 2022, 2023
lastupdated: "2023-02-07"

keywords: troubleshoot, troubleshoot hyper protect virtual servers for vpc, debug hyper protect virtual servers for vpc, questions about hyper protect virtual servers for vpc, hyper protect virtual server shut down

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my instance shut down or not start after provisioning?
{: #hyper-protect-virtual-server-shutdown}
{: troubleshoot}
{: support}

Your {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} for VPC instance shuts down or doesn't start after provisioning.
{: tsSymptoms}

Causes vary.
{: tsCauses}

Check whether you have performed the following actions correctly.
{: tsResolve}

- Attach data volume within 15 min

   If you have a volumes section in the contract, the IBM Cloud {{site.data.keyword.hpvs}} for VPC instance assumes that a data volume will be attached. The cloud user must either attach the volume during the instance creation, or within 15 minutes after instance creation starts. After this 15-minute wait period, the instance shuts down.

- Contract is mandatory

   If you create a {{site.data.keyword.hpvs}} for VPC instance without any contract that's passed in through the "**User Data**" section, the instance starts running, and then shuts down eventually.

- Contract format is yaml

   The contract must follow the YAML format. If the format is not proper, it fails the contract validation check, and the instance shuts down eventually.

- Contract schema

   If the contract schema is incorrect, the contract validation fails when the instance is booting and the instance shuts down. You can check for errors that are associated with `hpcr-contract log` identifier in your IBM Log Analysis instance.

- Image or Profile mismatch

   {{site.data.keyword.hpvs}} for VPC instances can be created from the UI, CLI, and any other interface that {{site.data.keyword.vpc_full}} (VPC) supports. You must select the right image and profile combination. Without the right combination of the image and profile, the instance fails to start.

- Logging configuration failure

   When the instance boots, monitor the serial console to identify any errors that are logged by the bootloader or from the logging service. If you don't see any logs in your private IBM Log Analysis instance, it might be because your logging configuration failed. Failure to configure logging also leads to the instance shutting down. If your logging configuration fails, check whether the logging hostname, port, and ingestion key that are provided in the contract are correct.
