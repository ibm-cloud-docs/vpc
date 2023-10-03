---

copyright:
  years: 2022, 2023
lastupdated: "2023-02-07"

keywords: troubleshoot, troubleshoot hyper protect virtual servers for vpc, debug hyper protect virtual servers for vpc, questions about hyper protect virtual servers for vpc, hyper protect virtual server functionality disrupted

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my virtual server functionality disrupted?
{: #hyper-protect-virtual-server-disrupt-functionality}
{: troubleshoot}
{: support}

Your virtual server functionality is disrupted.
{: tsSymptoms}

Multiple data volume or workloads are attached.
{: tsCauses}

The [contract](/docs/vpc?topic=vpc-about-contract_se) is mandatory. Attach only a single data volume and workload.
{: tsResolve}

- Single data volume

   Currently, only a single data volume is supported. You might not get any explicit errors from VPC if you try to attach more data volumes, but it might disrupt the functionality. When the {{site.data.keyword.hpvs}} for VPC instance boots, it automatically detects the attached data volume and encrypts it. If it finds more than one data volume, it randomly selects one of them and encrypts it. The container workload stores data into this data volume. When the {{site.data.keyword.hpvs}} for VPC instance reboots, it follows the same process of randomly picking up one of the attached data volumes and it's possible that it doesn't pick up the one that has workload data. This leads to a situation where the workload no longer has access to its data.

- Single workload

   Currently, only a single workload is supported. Therefore, only a single workload must be specified as part of the docker-compose file.
