---

copyright:
  years: 2022, 2024
lastupdated: "2024-06-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring security groups
{: #configure-security-groups}

The Virtual Network Function (VNF) data network interface is attached to a VPC security group. Ensure that the security group has inbound rules that allow traffic on the pool health port that is set up between the NLB and the VNF. For example, if the health check is set up for TCP on port 80 (HTTP), then create an inbound rule for that security group. Additionally, you can create rules to allow or restrict data traffic.
{: shortdesc}

## Next step
{: #next-step-deploy-vnf}

[Deploying a supported VNF](/docs/vpc?topic=vpc-deploy-vnf)
