---

copyright:
  years: 2021
lastupdated: "2022-01-11"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring VPC resources
{: #configure-vpc-resources}

To get started deploying a High Availability (HA) Virtual Network Function (VNF), make sure to complete the following tasks:
{: shortdesc}

1. Create a VPC. For more information, see [Creating a VPC and subnet](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console&interface=cli#creating-a-vpc-and-subnet).
1. Create a subnet that will be shared between the VNF's data traffic interface and the Network Load Balancer (NLB).
1. If the VNF requires a subnet for management traffic, create the subnet. This subnet can be shared between multiple VNFs to support clustering.
1. Create any additional subnets required for the virtual server instance's workloads that will be routed through the NLB and VNFs.

## Next step
{: #next-step-vnf-configure-resources}

[Configuring security groups](/docs/vpc?topic=vpc-configure-security-groups)
