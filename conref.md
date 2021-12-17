---

copyright:
  years: 2015, 2021

lastupdated: "2021-12-17"

keywords: 

subcollection: vpc

content-type: conref

---

{{site.data.keyword.attribute-definition-list}}

# Content references for VPC
{: #vpc-conrefs}

# Content references for load balancer
{: #load-balancer-conrefs}

You must also create a service authorization to allow your application load balancer instance to access the Certificate Manager instance that contains your SSL certificate. 
You can create an authorization through [IAM Authorizations](https://{DomainName}/iam/authorizations){: external}. Make sure to choose 
**VPC Infrastructure Services** as the Source service, select **Resources based on selected attributes**, and click the **Resource type** checkbox to expose 
the dropdown menu. Select **Load Balancer for VPC** as the resource type from the dropdown. For the Target service, select **Certificate Manager**. Set the Target 
service instance access to **All instances** or to your specific Certificate Manager instance. Assign the **Writer** service access role. For more information, see 
[Granting access between services](/docs/account?topic=account-serviceauth#create-auth).
{: #load-balancer-grant-service-auth}
