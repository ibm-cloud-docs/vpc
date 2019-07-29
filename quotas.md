---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-29"

keywords: quotas, vpc, resources, limited, early access 

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Quotas
{: #quotas}

This document covers quotas and limits for the {{site.data.keyword.vpc_full}} early access release and the resources available within it.
{:shortdesc}


## VPCs
{: #vpc-quotas}

|   Resource     | Maximum Number |
| ------- | :------: |
| Virtual private clouds | 10 per account per region|
| Subnets | 15 per VPC |
| Public Gateways | 15 per VPC |
| Address prefixes | 5 per account per VPC |

## Virtual server instances
{: #vsi-quotas}

|   Resource     | Default Quota |
| ------- | :------: |
| vCPU |  20 per region  |
| RAM | 80 per region* |
| Network interfaces | 5 per instance |
| Floating IP addresses | 20 per zone |
| SSH keys | unlimited |

*based on host configuration ratio of 1:8 vCPU:RAM

## Security groups
{: #security-group-quotas}

|Resource|Maximum Number|
|--------|-----|
|Security groups|5 per instance|
|Rules|50 per security group|
|Remote rules|5 per security group|
|Network interfaces|10 per security group|



