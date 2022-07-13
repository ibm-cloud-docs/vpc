---

copyright:
  years: 2015, 2022

lastupdated: "2022-07-13"

keywords: 

subcollection: vpc

content-type: conref

---

{{site.data.keyword.attribute-definition-list}}

# Content references for VPC
{: #vpc-conrefs}

# Content references for load balancer
{: #load-balancer-conrefs}

You must also create a service authorization to allow your application load balancer instance to access the Secrets Manager instance that contains your SSL certificate. 

You can create an authorization through [IAM Authorizations](https://{DomainName}/iam/authorizations){: external}. Make sure to choose 
**VPC Infrastructure Services** as the Source service, select **Resources based on selected attributes**, and click the **Resource type** checkbox to expose 
the dropdown menu. Select **Load Balancer for VPC** as the resource type from the dropdown. For the Target service, select **Secrets Manager**. Set the Target 
service instance access to **All instances** or to your specific Secrets Manager instance. Assign the **Writer** service access role. For more information, see 
[Granting access between services](/docs/account?topic=account-serviceauth#create-auth).
{: #load-balancer-grant-service-auth}

# Content referenced for  x86 instance profiles
{: #x86-instance-profile-conrefs}

The profile families are Balanced, Compute, Memory, Ultra High Memory, Very High Memory, and GPU.
{: #x86-profile-families}

# Content referenced for custom images
{: #custom-image-conrefs}

All custom images must meet the following requirements:
- Contain a single file or volume
- Is in qcow2 or vhd format
- Is cloud-init enabled
- The operating system is supported as a [stock image](/docs/vpc?topic=vpc-about-images#stock-images)
- Size doesn't exceed 250 GB
- The minimum size is 10 GB. For any image that is less than 10 GB, the size is rounded up to 10 GB.
{: #custom-image-requirements-list}

For more information about custom images, see [Getting started with custom images](/docs/vpc?topic=vpc-about-custom-images).
{: #custom-image-information-link}
