---

copyright:
  years: 2021
lastupdated: "2022-01-11"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a network load balancer using routing mode
{: #deploy-nlb}

Traffic destined for servers in an IBM Cloud VPC must be delivered to healthy VNF devices; otherwise, traffic disruption occurs. You can use Network Load Balancers (NLB) with routing mode enabled to perform health checks, and to ensure that workloads only travel through healthy VNF devices. Because of this, network load balancers with routing mode support only VNF devices as back-end targets.

After you deploy the VNF, you must create an NLB and grant a service authorization for your IBM Cloud account to allow the NLB to modify custom routes if an NLB failover occurs.

To create an NLB and a service-to-service authentication policy for your NLB, follow these steps:

1. From your browser, log in to Identity and Access Management (IAM):

   * Click **Authorizations**, then click **Create**.
   * For the source service, select **VPC Infrastructure Services**. For scope access, select **Resources based on selected attributes > Resource Type > Load Balancer for VPC**.
   * For the target service, select **VPC Infrastructure Services**. For scope access, select **Resources based on selected attributes > Resource Type > Virtual Private Cloud**.
   
   This IAM authorization is required only one time per account.
   {: note}

1. To create a NLB with routing mode enabled, follow instructions in [Creating a network load balancer for VPC](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer).

   To enable routing mode, select the **Private** type, then click the toggle button to enable routing mode. 
   
   You cannot modify this configuration after your NLB finishes provisioning. 
   
   Currently, the NLB routing mode feature is available only with a private IP and only supports TCP data traffic.  
   {: note}
  
## Next step
{: #next-step-nlb}

[Configuring custom routes](/docs/vpc?topic=vpc-config-custom-routes&interface=ui)
