---

copyright:
  years: 2022, 2023
lastupdated: "2023-01-27"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring VPC resources
{: #configure-vpc-resources}

Before you start deploying a High Availability (HA) Virtual Network Function (VNF), complete the following tasks:
{: shortdesc}

1. Create a VPC. For more information, see [Creating a VPC and subnet](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console&interface=cli#creating-a-vpc-and-subnet).
1. Create a subnet that is to be shared between the VNF's data traffic interface and the Network Load Balancer (NLB).
1. If the VNF requires a subnet for management traffic, create the subnet. This subnet can be shared between multiple VNFs to support clustering.
1. Create any additional subnets that might be required for the virtual server instance's workloads that are to be routed through the NLB and VNFs.

## Next step
{: #next-step-vnf-configure-resources}

[Configuring security groups](/docs/vpc?topic=vpc-configure-security-groups)
