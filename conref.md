---

copyright:
  years: 2015, 2023

lastupdated: "2023-06-29"

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

All custom images must meet the following requirements：
- Contain a single file or volume
- Is in qcow2 or vhd format
- Is cloud-init enabled
- The operating system is supported as a [stock image](/docs/vpc?topic=vpc-about-images#stock-images)
- Size doesn't exceed 250 GB
- The minimum size is 10 GB. For any image that is less than 10 GB, the size is rounded up to 10 GB.
{: #custom-image-requirements-list}

For more information about custom images, see [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images).
{: #custom-image-information-link}

To import a a custom image into a private catalog, see [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).
{: #access-custom-image-private-catalog}

When you want to delete an {{site.data.keyword.vpc_short}} custom image that is part of a private catalog offering, you must first remove that image from the associated version in the private catalog offering. Then, you can delete the custom image from {{site.data.keyword.vpc_short}}.  To delete the custom image from the private catalog, see [Deprecating a private product](/docs/account?topic=account-deprecate-product&interface=ui).
{: #delete-custom-image-private-catalog}

# Content references for instance template
{: #instance-template-conrefs}

| Field | Value |
|-------|-------|
| Architecture | Select the processor architecture that your instance is created with. *x86* means x86_64 bit processor, and *s390x* is z Systems or LinuxONE (s390x processor architecture). |
| Hosting type | A **Public** virtual server instance, created in a multi-tenant environment, is the default selection for a new instance. You can also select a **Dedicated** virtual server instance to create the instance in a single-tenant space. To provision a dedicated instance, you must have a dedicated host available or [create one](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances). z/OS virtual server instances do not support the dedicated type.   |
| Location | Locations are composed of regions (specific geographic areas) and zones (fault tolerant data centers within a region). Select the geography, region, and zone where you want your virtual server instance to be created. |
| Name  | A name is required for your virtual server instance. |
| Resource group | Select a resource group for the instance. |
| Operating system | Select a a stock image, a custom image from your account, or a custom image that was shared with your account from a private catalog. For more information about stock images, see [x86 virtual server images](/docs/vpc?topic=vpc-about-images&interface=ui). \n * A `Custom image` is an image that you create externally and import to {{site.data.keyword.cloud}}, which you can then import into {{site.data.keyword.vpc_short}}. \n * A `Catalog image` is a custom image that is imported into a private catalog. \n For more information about catalog images, see [Custom images in a private catalog](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#custom-image-cloud-private-catalog). \n  **Note** If you select a catalog image that belongs to a different account, there are additional considerations and limitations to review. See [Using cross-account image references in a private catalog in the UI](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#private-catalog-image-reference-vpc-ui). To create a private catalog, see the tutorial [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui). |
| Profile |  Select from popular profiles or all available vCPU and RAM combinations. The profile families are Balanced, Compute, Memory, and GPU. For more information, see [Profiles](/docs/vpc?topic=vpc-profiles). |
| SSH Key | You must select an existing public SSH key or click **Create an SSH key** to create a new one. You can create only RSA SSH keys. For Ed25519 SSH keys, you must upload the key information. For more information on creating a SSH key, see [Creating your SSH key using the UI](/docs/vpc?topic=vpc-ssh-keys&interface=ui#generate-ssh-keys-ui). SSH keys are used to securely connect to the instance after it's running. \n  \n  **Note:** Alpha-numeric combinations are limited to 100 characters. SSH keys can either be RSA or Ed25519. Ed25519 can only be used if the operating system supports this key type. Ed25519 can't be used with Windows or VMware images. \n  \n For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| Boot volume | The default boot volume size for all profiles is 100 GB. You can specify a larger boot volume capacity, up to 250 GB, depending on what the image allows. You can also specify user tags. |
| Data volumes | You can add one or more secondary data volumes to be included when you provision the instance. To add volumes, click **Create**. You can specify customer-managed encryption and user tags for the volume. |
| Virtual private cloud | Specify the IBM Cloud VPC where you want to create your instance. You can use an existing VPC or you can create a new VPC. To create a new VPC, click **New VPC**. |
| Network interfaces | Defines the networking connection into the IBM Cloud VPC.  |
| User data | You can add user data that automatically performs common configuration tasks or runs scripts. For more information, see [User data](/docs/vpc?topic=vpc-user-data). |
| Metadata | Disabled by default, lets instances created from this template gather metadata about itself. Click the toggle to turn the metadata service on. For more information, see [About Instance Metadata for VPC](/docs/vpc?topic=vpc-imd-about). |
| Host failure auto restart | This setting is enabled by default. To disable host failure auto restart, click the toggle. For more information, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=ui). |
| Add instance to placement group | You have the option to select a placement group for the instance. To enable placement groups, click the toggle. Then, select or create a placement group for the instance. If you add a placement group, the instance is placed according to the placement group strategy. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc). |
{: caption="Table 1. Instance template selections" caption-side="bottom"}
{: #create-instance-template-table}

# Content references for VPC resource attributes
{: #vpc-resource-attributes-conrefs}

|   Resource     | Resource Attribute |
| ------- | ------ |
| Auto Scale for VPC | `instanceGroupId:<instance-group-id>` |
| Backup service | `backupPolicyId: <backup-policy-id>`|
| Block Storage for VPC | `volumeId: <volume-id>` |
| Baremetal server | `bareMetalServerId: <bare-metal-server-id>` |
| Dedicated Host for VPC | `dedicatedHostId:<dedicated-host-id>` <!--(staging)--> |
| File Storage | `shareId: <share-id>` | 
| Floating IP for VPC | `floatingIpId: <fip-id>` |
| Flow Logs for VPC | `flowLogCollectorId: <flc-id>` |
| Image Service for VPC | `imageId:<image-id>` |
| Load Balancer for VPC | `loadBalancerId: <load-balancer-id>` |
| Network ACL | `networkAclId: <nacl-id>` |
| Placement Group for VPC | `placementGroupId: <placement-group-id>` |
| Public Gateway for VPC | `publicGatewayId: <pgw-id>` |
| Security Group for VPC | `securityGroupId: <default-sec-grp-id>` |
| Snapshots | `snapshotId: <snapshot-id>`|
| SSH Key for VPC | `keyId:<key-id>` |
| Subnet | `subnetId: <subnet-id>` |
| Virtual Private Endpoint for VPC | `endpointGatewayId:<endpoint-gateway-id>`<!--(staging)--> |
| Virtual Private Cloud |  `vpcId: <vpc-id>`  |   
| Virtual Server for VPC | `instanceId: <instance-id>` |   
| {{site.data.keyword.vpn_vpc_short}} | `vpnGatewayID: <vpn-gateway-id>` |
{: caption="Table 1. VPC resource attributes" caption-side="bottom"}
{: #vpc-resource-attributes-table}
