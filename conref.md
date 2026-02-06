---

copyright:
  years: 2015, 2026
lastupdated: "2026-02-06"

keywords:

subcollection: vpc

content-type: conref

---

{{site.data.keyword.attribute-definition-list}}

# Content references for VPC
{: #vpc-conrefs}

# Content references for load balancer
{: #load-balancer-conrefs}

You must also create a service authorization to allow your application load balancer instance to access the {{site.data.keyword.secrets-manager_full_notm}} instance that contains your SSL certificate.

You can create an authorization through [IAM Authorizations](/iam/authorizations){: external}. Make sure to choose
**VPC Infrastructure Services** as the Source service, then select **Specific resources**. Click **Select an attribute** and choose **Resource type** from the list. Select **Load Balancer for VPC** as the resource type and click **Next**. For the Target service, select **Secrets Manager**. Set the Target service instance access to **All instances** or to your specific {{site.data.keyword.secrets-manager_full_notm}} instance. Assign the **Writer** service access role. For more information, see [Granting access between services](/docs/account?topic=account-serviceauth&interface=ui#create-auth).
{: #load-balancer-grant-service-auth}

# Content referenced for x86 instance profiles
{: #x86-instance-profile-conrefs}

The profile families are Balanced, Compute, Memory, Ultra High Memory, Very High Memory, and GPU.
{: #x86-profile-families}

# Content referenced for custom images
{: #custom-image-conrefs}

All custom images must meet the following requirements：
- Contain a single file or volume.
- Be in qcow2 or vhd format.
- Be cloud-init enabled or bootable by using ESXi kickstart.
- Size doesn't exceed 250 GB.
- The minimum size is 10 GB. For any image that is less than 10 GB, the size is rounded up to 10 GB.
{: #custom-image-requirements-list}

For more information about custom images, see [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images).
{: #custom-image-information-link}

To import a custom image into a private catalog, see [Importing software to your private catalogOnboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).
{: #access-custom-image-private-catalog}

When you want to delete an {{site.data.keyword.vpc_short}} custom image that is part of a private catalog offering, you must first remove that image from the associated version in the private catalog offering. Then, you can delete the custom image from {{site.data.keyword.vpc_short}}. To delete the custom image from the private catalog, see [Deprecating a private product](/docs/account?topic=account-deprecate-product&interface=ui).
{: #delete-custom-image-private-catalog}

# Content references for instance template
{: #instance-template-conrefs}

| Field | Value |
|-------|-------|
| Location | Locations are composed of regions (specific geographic areas) and zones (fault tolerant data centers within a region). Select the geography, region, and zone where you want your virtual server instance to be created. |
| Name  | A name is required for your virtual server instance. |
| Resource group | Select a resource group for the instance. |
| Image | Click **Change image** to select an image. On the Select an image page, you can select a stock image, custom image, catalog image, snapshot, or an existing volume. If the geographic location where you are provisioning an instance supports it, you can select one of the following:  \n  \n * x86 architecture  \n  \n *  s390x architecture  \n  \n After you select your image, click **Save**. \n * **Stock images**: You can select from available stock images. For more information, see:  \n  \n  * [x86 virtual server images](/docs/vpc?topic=vpc-about-images&interface=ui)  \n  \n  *  [s390x virtual server images](/docs/vpc?topic=vpc-vsabout-images) \n * **Custom images**: A custom image can be an image that you customize and upload to {{site.data.keyword.cos_full_notm}}, which you can then import into {{site.data.keyword.vpc_short}}. You can also use a custom image that was created from a boot volume. For more information about custom images, see [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images). \n * **Catalog images**: A catalog image is a custom image that is imported into a private catalog. For more information about catalog images, see [VPC considerations when using custom images in a private catalog](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=ui). \n * **Snapshot**: You can select from available snapshots. For more information, see [About Block Storage Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about). \n * **Existing volume**: You can select from existing volumes. The specified volume must be unattached, and must have an operating system with the same architecture as the instance profile. \n  |
| Profile |  Click **Change profile** to select from all available vCPU and RAM combinations. The profile families are Flex, Balanced, Compute, Memory, Ultra High Memory, Very High Memory, GPU, and Confidential Compute. For more information, see [Profiles](/docs/vpc?topic=vpc-profiles). |
| SSH Key | You must select an existing public SSH key or click **Create an SSH key** to create a new one. You can create only RSA SSH keys. For ED25519 SSH keys, you must upload the key information. For more information about creating an SSH key, see [Creating your SSH key by using the UI](/docs/vpc?topic=vpc-ssh-keys&interface=ui#generate-ssh-keys-ui). SSH keys are used to securely connect to the instance after it's running. \n \n Note:** Alpha-numeric combinations are limited to 100 characters. SSH keys can be either RSA or ED25519. ED25519 can be used only if the operating system supports this key type. ED25519 can't be used with Windows or VMware images. \n \n For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| Boot volume | The default boot volume size for all profiles is 100 GB. You can specify a larger boot volume capacity, up to 250 GB, depending on what the image allows. You can also specify user tags. |
| Data volumes | You can add one or more secondary data volumes to be included when you provision the instance. To add volumes, click **Create**. You can specify customer-managed encryption and user tags for the volume. |
| Virtual private cloud | Specify the IBM Cloud VPC where you want to create your instance. You can use an existing VPC or you can create a new VPC. To create a new VPC, click **New VPC**. |
| Network interfaces | Defines the networking connection into the IBM Cloud VPC. By default, the new option **Network attachment with a virtual network interface** is selected for your instance template. Alternatively, you can select the legacy option **Instance network interface**. Whichever type of network interface option that you select when you provision the virtual server persists through the lifecycle of the virtual server. After you selected your network interface type, you can click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to update the details of the network interface, for example, the subnet or the associated security group. |
| User data | You can add user data that automatically performs common configuration tasks or runs scripts. For more information, see [User data](/docs/vpc?topic=vpc-user-data). |
| Metadata | Disabled by default, lets the instance that is created from this template gather metadata about itself. Click the toggle to turn on the metadata service. For more information, see [About Metadata for VPC](/docs/vpc?topic=vpc-imd-about). |
| Add to dedicated host | You can add the virtual server instance to a dedicated host, creating the instance in a single-tenant space. To provision a dedicated instance, you must have a dedicated host available or [create one](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances).  |
| Add to placement group | You can select a placement group for the instance. To enable placement groups, click the toggle. Then, select or create a placement group for the instance. If you add a placement group, the instance is placed according to the placement group strategy. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc). |
| Dynamic volume bandwidth allocation [New]{: tag-new} | Click the toggle to enable [Pooled volume bandwidth allocation](/docs/vpc?topic=vpc-block-storage-bandwidth#pooled-vol-bandwidth) for attached data volumes. This feature is supported for select [compute profiles](/docs/vpc?group=profile-details). |
| Host failure auto restart | This setting is enabled by default. To disable host failure auto restart, click the toggle. For more information, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=ui). |
{: caption="Instance template selections" caption-side="bottom"}
{: #create-instance-template-table}

# Content references for VPC resource attributes
{: #vpc-resource-attributes-conrefs}

|   Resource     | Resource Attribute |
| ------- | ------ |
| Auto Scale for VPC | `instanceGroupId:<instance-group-id>` |
| Backup service | `backupPolicyId: <backup-policy-id>`|
| Block Storage for VPC | `volumeId: <volume-id>` |
| Bare metal server | `bareMetalServerId: <bare-metal-server-id>` |
| Cluster networks for VPC | `clusterNetworkId: <cluster-network-id>` |
| Dedicated Host for VPC | `dedicatedHostId:<dedicated-host-id>` |
| File Storage | `shareId: <share-id>` |
| Floating IP for VPC | `floatingIpId: <fip-id>` |
| Flow Logs for VPC | `flowLogCollectorId: <flc-id>` |
| Image Service for VPC | `imageId:<image-id>` |
| Load Balancer for VPC | `loadBalancerId: <load-balancer-id>` |
| Network ACL | `networkAclId: <nacl-id>` |
| Placement Group for VPC | `placementGroupId: <placement-group-id>` |
| Private Path services for VPC | `privatePathServiceGatewayId: <private-path-service-gateway-id>` |
| Public Address Range for VPC | `publicAddressRangeId: <public-address-range-id>` |
| Public Gateway for VPC | `publicGatewayId: <pgw-id>` |
| Reservations for VPC | `reservationId: <reservation-id>` |
| Security Group for VPC | `securityGroupId: <default-sec-grp-id>` |
| Snapshots | `snapshotId: <snapshot-id>`|
| SSH Key for VPC | `keyId:<key-id>` |
| Subnet | `subnetId: <subnet-id>` |
| Virtual Network Interface | `virtualNetworkInterfaceId:<virtual-network-interface-id>` |
| Virtual Private Endpoint for VPC | `endpointGatewayId:<endpoint-gateway-id>` |
| Virtual Private Cloud |  `vpcId: <vpc-id>`  |
| Virtual Server for VPC | `instanceId: <instance-id>` |
| {{site.data.keyword.vpn_vpc_short}} | `vpnGatewayID: <vpn-gateway-id>` |
{: caption="VPC resource attributes" caption-side="bottom"}
{: #vpc-resource-attributes-table}
