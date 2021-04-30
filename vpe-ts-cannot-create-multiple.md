---

copyright:
  years: 2018, 2020
lastupdated: "2020-12-30"

keywords: VPE, virtual private endpoint, troubleshooting

subcollection: vpc

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:support: data-reuse='support'}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Why can't I create more than one endpoint gateway for a service instance?
{: #troubleshoot-cannot-create-multiple}
{: troubleshoot}
{: support}

An endpoint gateway can be associated only with service instances that are created in your VPC. In addition, an endpoint gateway has a 1:1 association with a service instance.
{: shortdesc}

For example, if you purchase an instance of KeyProtect with a unique CRN, only one endpoint gateway can be associated with it. You can associate multiple reserved IPs with the
endpoint gateway, one per Availability Zone (AZ) in the region. Your virtual server instance in any AZ in the region has access to the endpoint gateway by using any of the reserved IPs, provided no network ACL rules prevent access.
