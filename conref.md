---

copyright:
  years: 2015, 2022

lastupdated: "2022-08-01"

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

To import a a custom image into a private catalog, see [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).
{: #access-custom-image-private-catalog}

When you want to delete an {{site.data.keyword.vpc_short}} custom image that is part of a private catalog offering, you must first remove that image from the associated version in the private catalog offering. Then, you can delete the custom image from {{site.data.keyword.vpc_short}}.  To delete the custom image from the private catalog, see [Deprecating a private product](/docs/account?topic=account-deprecate-product&interface=ui).
{: #delete-custom-image-private-catalog}

# Content references for instance template
{: #instance-template-conrefs}

| Field | Value |
|-------|-------|
| Name  | A name is required for your virtual server instance. |
| Virtual private cloud | Specify the IBM Cloud VPC where you want to create your instance. |
| Resource group | Select a resource group for the instance. |
| Tags |  You can assign a label to this resource so that you can easily filter resources in your resource list. |
| Location | Locations are composed of regions (specific geographic areas) and zones (fault tolerant data centers within a region). Select the location where you want your virtual server instance to be created. |
| Processor architecture |  Select the processor architecture that your instance is created with. *x86* means x86_64 bit processor, and *s390x* is LinuxONE (s390x processor architecture). |
| Operating system | Select a a stock image, a custom image from your account, or an image that was shared with your account from a private catalog. For more information about stock images, see [x86 virtual server images](/docs/vpc?topic=vpc-about-images&interface=ui). \n * A `Custom image` is an image that you create externally and import to {{site.data.keyword.cloud}}, which you can then import into {{site.data.keyword.vpc_short}}. \n * A `Catalog image` is a custom image that is imported into a private catalog. \n For more information about custom and private catalog images, see [Getting started with custom images](/docs/vpc?topic=vpc-about-custom-images). |
| Profile |  Select from popular profiles or all available vCPU and RAM combinations. The profile families are Balanced, Compute, Memory, and GPU. For more information, see [Profiles](/docs/vpc?topic=vpc-profiles). |
| SSH Key | You must select an existing SSH key or upload a new SSH key to use before you can create the instance. SSH keys are used to securely connect to the instance after it's running. \n **Note:** Alpha-numeric combinations are limited to 100 characters. \n For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| Metadata | Disabled by default, lets instances created from this template gather metadata about itself. Click the toggle to turn the metadata service on. For more information, see [About Instance Metadata for VPC](/docs/vpc?topic=vpc-imd-about). |
| User data | You can add user data that automatically performs common configuration tasks or runs scripts. For more information, see [User data](/docs/vpc?topic=vpc-user-data). |
| Placement group | Select a placement group for the instance. If you add a placement group, the instance is placed according to the placement group strategy. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc). |
| Boot volume | The default boot volume size for all profiles is 100 GB. You can specify a larger boot volume capacity, up to 250 GB, depending on what the image allows. You can also specify user tags. |
| Data volumes | You can add one or more secondary data volumes to be included when you provision the instance. To add volumes, click **Create**. You can specify customer-managed encryption and user tags for the volume. |
| Virtual private cloud | Specify the IBM Cloud VPC where you want to create your instance. You can use an existing VPC or you can create a new VPC. To create a new VPC, click **New VPC**. |
| Network interfaces | Defines the networking connection into the IBM Cloud VPC.  |
{: caption="Table 1. Instance template selections" caption-side="bottom"}
{: #create-instance-template-table}
