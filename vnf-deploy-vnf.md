---

copyright:
  years: 2022, 2023
lastupdated: "2023-01-27"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deploying a VNF
{: #deploy-vnf}

Before you start deploying VNF, meet the following requirements:

1. Ensure that the VNF data interface is on the same shared subnet as the NLB, which is created in [Creating a network load balancer with routing mode](/docs/vpc?topic=vpc-deploy-nlb).
1. Ensure that the VNF data interface (shared subnet with NLB) shows `Allow IP Spoofing` as enabled. You can enable this function through the [Virtual server instances for VPC](https://cloud.ibm.com/vpc-ext){: external} UI in the **Network interfaces** section of a virtual server's details page.

   For more information about IP spoofing, see [About IP spoofing checks](/docs/vpc?topic=vpc-ip-spoofing-about).
   {: note}

1. Enable health checks from the NLB for your VNF configuration. HTTP and TCP are supported.

   To enable the VNF interfaces and policies to accept the NLB health checks, you must first retrieve the NLB IP addresses.

   Keep in mind:

   * The NLB is deployed as an active/passive cluster. Each node has a distinct IP address.
   * You must use the active IP address in the custom routes that are created later in this process.

      To retrieve the IP addresses, go to the [Load balancers for VPC](/vpc-ext/network/loadBalancers){: external} page in the [{{site.data.keyword.cloud_notm}} console](/login){: external}, and select the name of the NLB in the table to show its details. In the Load balancer details section (Overview tab), the Private IP addresses are shown. The first IP is the primary IP address.

      Because NLB failovers can occur, you must configure the VNF to accept health check requests on both IP addresses.
      {: note}

## Available IBM Cloud VNF catalog offerings
{: #available-vnf-offerings}

Currently, the following IBM Cloud VNF catalog offerings are available:

* [Check Point Security Gateway](https://cloud.ibm.com/catalog/content/checkpoint-iaas-gw-ibm-vpc-1.0.7-9ed8dbde-2931-45f5-a7a7-0c90ce0d2686-global){: external}
* [Fortinet FortiGate Firewall A/P HA](https://cloud.ibm.com/catalog/content/ibm-fortigate-AP-HA-terraform-deploy-5dd3e4ba-c94b-43ab-b416-c1c313479cec-global){: external}
* [Fortinet FortiGate Next-Generation Firewall - Single VM](https://cloud.ibm.com/catalog/content/ibm-fortigate-terraform-deploy-1f878ca9-069f-42ca-9ed9-5b461d4d5231-global){: external}
* [F5 Big-IP](https://cloud.ibm.com/catalog/content/ibmcloud_schematics_bigip_multinic_declared-1.0-d33f1544-e938-478a-b0dd-d883370f08d0-global){: external}
* [IaaS Security Management](https://cloud.ibm.com/catalog/content/checkpoint-iaas-mgmt-ibm-vpc-1.0.7-9ea27a83-e5e7-43d9-9e8f-df8dd1befc5a-global){: external}
* [Juniper vSRX](https://cloud.ibm.com/catalog/content/juniper-vsrx-catalog-deploy-1.4-dc1e707c-33dd-4321-b2a5-c22dbf0dd0ee-global){: external}
* [Palo Alto VM-Series](https://cloud.ibm.com/catalog/content/ibmcloud-vmseries-1.9-6470816d-562d-4627-86a5-fe3ad4e94b30-global){: external}

## Next step
{: #next-step-create-nlb}

[Creating a network load balancer with routing mode](/docs/vpc?topic=vpc-deploy-nlb)
