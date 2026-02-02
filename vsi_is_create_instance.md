---

copyright:
  years: 2018, 2026
lastupdated: "2026-02-02"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating virtual server instances
{: #creating-virtual-servers}

You can create one or more virtual server instances in your {{site.data.keyword.vpc_short}} by using the {{site.data.keyword.cloud_notm}} console, CLI, API, or Terraform.
{: shortdesc}

When you create a virtual server, you specify information such as the location and name for your virtual server. You specify an operating system image, a profile that defines the combination of vCPU and RAM, and SSH keys to securely connect to your virtual server. You can add data volumes in addition to the boot volume. You can also specify the type of network interface that is created for your virtual server. Finally, you can select from advanced options for your virtual server configuration.

## Creating a virtual server instance in the console
{: #creating-virtual-servers-ui}
{: ui}

Use the following steps to create a virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.

1. Click **Create** enter the information in Table 1.

   | Field | Value |
   |-------|-------|
   | Location | Locations are composed of regions (specific geographic areas) and zones (fault-tolerant data centers within a region). Select the location where you want to create your virtual server instance. |
   | Name  | A name is required for your virtual server instance. |
   | Resource group | Select a resource group for the instance. |
   | Tags | You can assign a user tag to the instance so that you can easily filter instance resources in your resource list. For more information, see [Working with tags](/docs/account?topic=account-tag&interface=ui).|
   | Access management tags | Access management tags help you apply flexible access policies on specific resources. For more information, see the [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial) UI tutorial. |
   {: caption="Selections to begin instance provisioning" caption-side="bottom"}

1. Select an image and profile for the instance. To select from all available images, click **Change image**. You can select an image, a snapshot of a boot volume, or an existing boot volume. If the geographic location where you are provisioning an instance supports it, you can select either of the following \n * x86 architecture \n *  s390x architecture. \n Table 2 describes image, snapshot, and existing volume options. Then, select a profile. To select from all available vCPU and RAM combinations, click **Change profile**. Table 3 describes profile selection. {: #select-image-and-profile}

    Allowed-use expressions: The image that you select determines the profiles that are available to create the virtual server instance. During the creation of a virtual server instance with images that use an allowed-use expression, the information that is provided in the allowed-use properties is then evaluated against a potential virtual server instance to determine whether that image can be used to create the virtual server instance. Stock images use defined allowed-use expressions. You must define any allowed-use expressions for custom images. For more information, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui).
    {: note}

   | Field | Value |
   |-------|-------|
   | Stock image | Select from available stock images and click **Save**. \n - For more information about available stock images, see: \n * [x86 virtual server images](/docs/vpc?topic=vpc-about-images) \n *  [s390x virtual server images](/docs/vpc?topic=vpc-vsabout-images) \n All operating system images use cloud-init that you can use to enter user metadata that is associated with the instance for post-provisioning scripts. Metadata isn't supported for {{site.data.keyword.cloud}} Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}} instances and z/OS virtual server instances. \n - If you plan to use Windows operating systems with SQL Server, see the [About Microsoft SQL on VPC](/docs/microsoft?topic=microsoft-mssql-about).  |
   | Custom image  | Select from available custom images and click **Save**. If no custom images are available, click **Create**. \n - A custom image can be an image that you customize and upload to {{site.data.keyword.cos_full_notm}}, which you can then import into {{site.data.keyword.vpc_short}}. For more information about custom images, see [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images). \n - You can also use a custom image that was created from a boot volume. For more information about creating an image from a volume, see [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc). \n - You can also select either an RHEL or Windows custom image and bring your own license (BYOL). For more information about creating BYOL custom images, see [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about). |
   | Catalog image | After you select an available catalog image, click **Select version and pricing plan**, choose the version and pricing plan, and then click **Save**. \n - A catalog image is a custom image that is imported into a private catalog. For more information about catalog images, see [VPC considerations for custom images in a private catalog](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=ui). \n **Note:** If you select a catalog image that belongs to a different account, you have more considerations and limitations to review. See [Using cross-account image references in a private catalog in the console](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=ui#private-catalog-image-reference-vpc-ui). \n - To create a private catalog, see the tutorial [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).|
   | Snapshot | Select either **Import existing snapshot** or **Import snapshot by CRN**. Then, choose a snapshot of a boot volume, and click **Save**. If no snapshots are available, click **Create**. \n - Filter the list of snapshots for [fast restore](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-use-fast-restore). With this option, you can create the boot volume quickly by using a snapshot that is cached in a different zone of your region. For more information about restoring a volume from a snapshot, see [Restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore). \n - If you're using the CRN of a snapshot from another account, make sure that the right [IAM authorizations](/docs/vpc?topic=vpc-block-s2s-auth&interface=ui#block-s2s-auth-xaccountrestore-ui) are in place. |
   | Existing volume | Select an existing boot volume that is not attached to an instance and click **Save**.  |
   {: caption="Instance provisioning image, snapshot, or volume selections" caption-side="bottom"}
   {: #table-select-image-and-profile}

   | Field | Value |
   |-------|-------|
   | Profile |  The profile families are Balanced, Compute, Memory, Very High Memory, Ultra High Memory, GPU, Storage Optimized, Confidential Computing, and Flex. For more information, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles). \n  When you create an {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}} instance, make sure that you select secure execution-enabled profiles, otherwise provisioning fails. For more information, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles). \n \n Some profiles might not be available because the number of network interfaces in the virtual server exceed profile limits. You can remove network interfaces to select from more profiles. For more information, see [Resizing a virtual server](/docs/vpc?topic=vpc-resizing-an-instance).  \n  \n Some profiles might not be available because the image selected contains an allowed-use expression that is not compatible with the profile. In these cases, select an image with an allowed-use expression that is compatible with the wanted profile. For more information, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui). |
   | Spot instances [Select availability]{: tag-green} | Spot instances are highly discounted versions of the standard instances. They are designed to use available compute resources for interruptible or stateless workloads.  \n To convert an instance to a Spot instance, make sure that you select a supported profile and click the **Convert to spot instance** toggle under **Deployment configurations**.|
   | Advanced security selections |  |
   | Secure boot | Click the toggle to enable secure boot. Secure boot is available with only compatible instance profiles. Second-generation boot volumes with the `sdp` volume profile do not support secure boot yet. For more information about secure boot, see [Secure boot for Virtual Servers for VPC](/docs/vpc?topic=vpc-confidential-computing-with-secure-boot-vpc).|
   | Confidential computing [Select availability]{: tag-green} | Confidential computing with Intel® Software Guard Extensions (SGX) and confidential computing with Intel Trusted Domain Extension (TDX) protects your data through hardware-based server security. Your data is protected by using isolated memory regions that are known as encrypted enclaves. Both SGX and TDX are available with only compatible profiles. For more information about confidential computing, see [Confidential computing for x86 Virtual Servers for VPC](/docs/vpc?topic=vpc-about-confidential-computing-vpc). |
   {: caption="Profile selections" caption-side="bottom"}

   Secure boot and confidential computing are available with selected balanced and compute profiles. For more information, see [SGX-compatible profiles](/docs/vpc?topic=vpc-about-confidential-computing-vpc#compatible-profiles-confidential-computing-vpc).
   {: important}

1. Complete SSH keys, storage, and networking details by specifying the information in Table 4.

   | Field | Value |
   |-------|-------|
   | SSH keys | You must select an existing public SSH key or click **Create an SSH key** to create one. For more information about creating an SSH key, see [Creating your SSH key by using the UI](/docs/vpc?topic=vpc-ssh-keys&interface=ui#generate-ssh-keys-ui). SSH keys are used to securely connect to the instance after it's running. \n **Note:** Alpha-numeric combinations are limited to 100 characters. SSH keys can be either RSA or ED25519. You can create only RSA SSH keys. For an ED25519 SSH key, you must upload the key information. ED25519 can be used only if the operating system supports this key type. ED25519 can't be used with Windows or VMware images. \n For more information, see the [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys) topic. |
   | Boot volume | The default boot volume size for most profiles is 100 GB. The default boot volume size for a z/OS virtual server instance is 245 GB. If you're importing a custom image, the boot volume capacity can be 10 - 250 GB, depending on what the image requires. Images that are smaller than 10 GB are rounded up to 10 GB. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to modify the boot volume's attributes in the side panel. \n \n You can change the name of the boot volume by specifying a unique, meaningful name. For example, it can be a name that describes your compute or workload function. The volume name must begin with a lowercase letter. The name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-). Volume names must be unique to the entire VPC infrastructure. You can edit the name later if you want to. \n \n You can toggle the auto-delete option off for the boot volume. If it is enabled, then the volume is deleted when the instance is deleted. If it is disabled, then the volume persists after the instance is deleted. \n \n You can specify optional user tags and access management tags to associate with this volume. For more information about organizing resources with user tags, see [Working with tags](/docs/account?topic=account-tag&interface=ui). \n \n You can select the encryption type. Provider-managed encryption is enabled by default on all volumes. You can also choose to create an envelop encryption with your own root keys. Encryption keys are created and maintained in Key Management Services ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}). \n \n You can select the [Storage profile](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui) that best suits your needs for capacity, IOPS, and bandwidth. \n Note: Second-generation boot volumes with the `sdp` volume profile do not support secure boot yet. If secure-boot is needed, select one of the `tiered` profiles or the `custom` profile. |
   | Data volumes | You can create one or more secondary data volumes to be attached when you provision the instance. Click **Create** in the Data volumes section to open the side panel where you can define the volume. \n \n Specify a unique, meaningful name. The same specifications apply as for the boot volume. \n \n You can toggle the auto-delete option on for the data volume. If it is enabled, then the volume is deleted when the instance is deleted. If it is disabled, then the volume persists after the instance is deleted. \n \n You can specify optional user tags and access management tags to associate with this volume. For more information about organizing resources with user tags, see [Working with tags](/docs/account?topic=account-tag&interface=ui). \n \n You can select the encryption type. Provider-managed encryption is enabled by default on all volumes. You can also choose to create an envelope encryption with your own root keys that are created and maintained in Key Management Services. \n \n You can select the [Storage profile](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui) that best suits your needs for capacity and IOPS. For more information, see [Create and attach a Block Storage volume when you create an instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi).|
   | Virtual private cloud | Specify the IBM Cloud VPC where you want to create your instance. You can use the default VPC, another existing VPC, or you can create a VPC. To create a VPC, click **New VPC**. |
   | Add to cluster network | If you select the H100 GPU profile, `gx3d-160x1792x8h100`, you see the option to **Add to cluster network**. You can set **Add to cluster network** to on to enable the virtual server to access the power of a high-performance network that supports Remote Direct Memory Access (RDMA). When **Add to cluster network** is set to on and a cluster network is available, the {{site.data.keyword.cloud_notm}} console includes default selections for configuring the virtual server for the cluster network. If no cluster network is available, you can click **Create cluster network**. When a cluster network is selected, only the VPC where the cluster network is provisioned is displayed in the **Virtual private cloud** drop-down menu. For more information, see [About cluster networks](/docs/vpc?topic=vpc-about-cluster-network&interface=ui). |
   | Network interfaces | By default the virtual server instance is created with a single primary network interface. You can click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to edit the details of the network interface, for example, the subnet or security group that's associated with the interface. To include extra secondary network interfaces, click **Create**. You can create and assign up to 15 network interfaces for your virtual server instance, depending on the vCPU count that is included in the instance profile. For more information, see [About network interfaces](/docs/vpc?topic=vpc-using-instance-vnics#about-network-interfaces). \n \n With the virtual network interface feature, you can select the type of network interface that you want to use. You can select the new option **Network attachment with a virtual network interface** or the traditional option **Instance network interface**. Whichever type of network interface option that you select when you provision the virtual server persists through the lifecycle of the virtual server. You can click **Attach** to create a network attachment with an existing virtual network interface. For more information, see [About virtual network interfaces](/docs/vpc?topic=vpc-vni-about). |
   {: caption="Selections to complete instance provisioning" caption-side="bottom"}

5. For Advanced options, you can choose to complete more instance configurations.

   | Field | Value |
   |-------|-------|
   | User data | You can add user data that automatically performs common configuration tasks or runs scripts. For more information, see [User data](/docs/vpc?topic=vpc-user-data). For more information about using a contract to specify user data when you create an {{site.data.keyword.cloud}} Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}} instance, see [About the contract](/docs/vpc?topic=vpc-about-contract_se). User data is not supported for z/OS virtual server instances. |
   | Metadata | Disabled by default. Click the toggle to enable. This setting informs the instance to collect the instance configuration information and user data. For more information, see [About metadata for VPC](/docs/vpc?topic=vpc-imd-about). Metadata isn't supported for {{site.data.keyword.cloud}} Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}} instances and z/OS virtual server instances. |
   | Trusted profile (optional) | If you enable the metadata service, you can select a trusted profile and link it to this instance. Click **Select a trusted profile**. In the side panel, select a trusted profile and click **Select trusted profile** to link it to the instance. A message displays if none exist or if you don't have access to link it. For more information, see [Create a trusted profile](/docs/account?topic=account-trustedprofile-compute-tutorial#trusted-profile-compute-create). For more information about acquiring access, see [IAM authorizations for linking trusted profiles](/docs/vpc?topic=vpc-imd-trusted-profile-metadata&interface=ui#imd-iam-auth). |
   | Add to dedicated host | This selection is disabled by default. To create the virtual server instance in a single-tenant space, click the toggle to enable the dedicated host. To provision a dedicated instance, you must have a dedicated host available or [create one](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances). |
   | Add to placement group | Placement groups are disabled by default. Click the toggle to enable placement groups. Then, select or create a placement group for the instance. If you add a placement group, the instance is placed according to the placement group policy. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc). |
   | Dynamic volume bandwidth allocation [New]{: tag-new} | Click the toggle to enable [Pooled volume bandwidth allocation](/docs/vpc?topic=vpc-block-storage-bandwidth#pooled-vol-bandwidth) for attached data volumes. This feature is supported for select [compute profiles](/docs/vpc?group=profile-details).|
   | Add to reservation | If you have an active reservation, click the toggle to add the virtual server instance to that reservation. For more information, see [About Reservations for VPC](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc). |
   | Host failure auto restart | This setting is enabled by default. To disable host failure auto restart, click the toggle. For more information, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=ui) |
   | Cloud security posture management | When you select this option, a workload protection instance is created with the configurations to provide CSPM to all the resources. If a workload protection instance exists, this option is not available. For more information, see [About IBM Cloud Security Posture Management (CSPM)](/docs/workload-protection?topic=workload-protection-about&interface=ui). |
   {: caption="Instance provisioning advanced options selections" caption-side="bottom"}

6. Click **Create a virtual server instance** when you are ready to provision.

## Next steps after an instance is created in the console
{: #next-steps-after-creating-virtual-servers-ui}
{: ui}



After the instance is created, you need to [associate a floating IP address to the instance](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#reserving-a-floating-ip-address). Then, you can connect to your instance. For more information, see [Connecting to your Linux instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux), [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows), or [Connecting to your z/OS instance](/docs/vpc?topic=vpc-vsi_is_connecting_zos).

If you have an existing instance with a floating IP address, it isn't necessary to assign a second floating IP to another instance. You can connect to the first instance with a floating IP, then SSH to the second instance by using the private subnet IP address that is automatically assigned to it.

## Creating a virtual server instance from the CLI
{: #creating-virtual-servers-cli}
{: cli}

You can create instances by using the command-line interface (CLI). If you would like to use user tags or access management tags to manage your resources, see [Working with tags](/docs/account?topic=account-tag&interface=cli).

 {{site.data.keyword.cloud_notm}} CLI is not supported on LinuxONE (s390x processor architecture). However, you can install the CLI on another supported platform and use it with LinuxONE (s390x processor architecture) virtual server instances.
{: note}

### Before you begin
{: #before-creating-virtual-servers-cli}
{: cli}

* Download, install, and initialize the following CLI plug-ins.
   - {{site.data.keyword.cloud_notm}} CLI
   - The infrastructure-service plug-in

   For more information, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

* Make sure that you [created a VPC](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli).

### Gathering information to create an instance from the CLI
{: #gather-info-to-create-virtual-servers-cli}
{: cli}

Ready to create an instance? Before you can run the `ibmcloud is instance-create` command, you need to know the details about the instance, such as what profile or image that you want to use.

Gather the following information by using the associated commands.

|    Instance details   |  Listing options                | VPC CLI reference documentation |
| --------------------- | --------------------------------|----------------------------|
| Image | `ibmcloud is image` | [List all images](/docs/vpc?topic=vpc-vpc-reference#images-list)|
| Boot volume | `ibmcloud is volumes` | [List all volumes](/docs/vpc?topic=vpc-vpc-reference#volumes-list) |
| Profile | `ibmcloud is instances` | [List all virtual server instances](/docs/vpc?topic=vpc-vpc-reference#instance-profiles-list) |
| Profile | `ibmcloud is image-instance-profiles` | [List instance profiles that are compatible with an image](/docs/vpc?topic=vpc-vpc-reference#image-instance-profiles-list)|
| Profile | `ibmcloud is volume-instance-profiles` | [List instance profiles that are compatible with a volume.](/docs/vpc?topic=vpc-vpc-reference#volume-instance-profiles-list) |
| Profile | `ibmcloud is snapshot-instance-profiles` | [List instance profiles that are compatible with a snapshot](/docs/vpc?topic=vpc-vpc-reference#snapshot-instance-profiles-list) |
| Keys | `ibmcloud is keys` | [List all keys](/docs/vpc?topic=vpc-vpc-reference#keys) \n \n If you don't have any available SSH keys, use [Create a key](/docs/vpc?topic=vpc-vpc-reference#key-create) to create one. \n \n **Note:** RSA and ED25519 are the two types of SSH keys that you can use. However, you can't use the ED25519 SSH key type with Windows or VMware images. You can use only RSA SSH keys for these images. \n For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| VPC | `ibmcloud is vpcs` | [List all VPCs](/docs/vpc?topic=vpc-vpc-reference#vpcs-list) |
| Subnet | `ibmcloud is subnets` | [List all subnets](/docs/vpc?topic=vpc-vpc-reference#subnets-list) |
| Zone | `ibmcloud is zones` | [List all regions](/docs/vpc?topic=vpc-vpc-reference#zones-list) |
| Placement groups | `ibmcloud is placement-groups` | [List all placement groups](/docs/vpc?topic=vpc-vpc-reference#placement-groups-list) |
{: caption="Required instance details" caption-side="bottom"}

Use the following commands to determine the required information for creating a new instance.

#### 1. List the regions associated with your account
{: #list-regions-associated-account-cli}

List the regions associated with your account.

```sh
ibmcloud is regions
```
{: pre}

See the following example.

```text
$ ibmcloud is regions
Listing regions under account Test Account as user test.user@ibm.com...
Name       Endpoint                              Status
au-syd     https://au-syd.iaas.cloud.ibm.com     available
br-sao     https://br-sao.iaas.cloud.ibm.com     available
ca-tor     https://ca-tor.iaas.cloud.ibm.com     available
eu-de      https://eu-de.iaas.cloud.ibm.com      available
eu-es      https://eu-es.iaas.cloud.ibm.com      available
eu-gb      https://eu-gb.iaas.cloud.ibm.com      available
jp-osa     https://jp-osa.iaas.cloud.ibm.com     available
jp-tok     https://jp-tok.iaas.cloud.ibm.com     available
us-east    https://us-east.iaas.cloud.ibm.com    available
us-south   https://us-south.iaas.cloud.ibm.com   available
```
{: screen}

#### 2. Switch to your target region
{: #switch-target-region-cli}

Switch to your target region.

```sh
ibmcloud target -r <region-name>
```
{: pre}

#### 3. List the zones associated with the target region
{: #list-zones-target-region-cli}

List the zones associated with the target region.

```sh
ibmcloud is zones
```
{: pre}

In the following example, the command is run in the `us-south` region and the output shows the available zones in the region.

```text
$ ibmcloud is zones
Listing zones in target region us-south under account Test Account as user test.user@ibm.com...
Name         Region     Status
us-south-1   us-south   available
us-south-2   us-south   available
us-south-3   us-south   available
```
{: screen}

#### 4. List {{site.data.keyword.vpc_short}}s associated with your account
{: #list-vpcs-account-cli}

List the {{site.data.keyword.vpc_short}}s that are associated with your account.

```sh
ibmcloud is vpcs
```
{: pre}

For this example, you see a response that is similar to the following output.

```text
ID                                        Name       Status     Classic access   Default network ACL              Default security group        Resource group
r006-35b9cf35-616e-462e-a145-cf8db4062fcf my-vpc     available  false            immortality-casing-extoll-exit   enhance-corsage-managing-jinx Default
```
{: screen}

If you do not have an available VPC, you can create one by using the **`ibmcloud is vpc-create`** command. For more information about creating a VPC, see the [ibmcloud is vpc-create](/docs/vpc?topic=vpc-vpc-reference#vpc-create).

#### 5. List the subnets that are associated with the {{site.data.keyword.vpc_short}}
{: #list-subnets-associated-vpc-cli}

List the subnets that are associated with the {{site.data.keyword.vpc_short}}.

```sh
ibmcloud is subnets
```
{: pre}

For this example, you see a response that is similar to the following output.

```text
ID                                          Name            Status      Subnet CIDR      Addresses   ACL                              Public Gateway   VPC
Zone         Resource group
0717-198db988-3b9b-4cfa-9dec-0206420d37d0   my-subnet       available   10.240.64.0/28   7/16        immortality-casing-extoll-exit   -               my-vpc
us-south-2   Default
```
{: screen}

If you do not have a subnet available, you can create one by using the **`ibmcloud is subnet-create`** command. For more information about creating a subnet, see [ibmcloud is subnet-create](/docs/vpc?topic=vpc-vpc-reference#subnet-create).

#### 6. List the available images
{: #list-available-images-cli}

You can use various image options with {{site.data.keyword.vpc_short}} included stock images, custom images, images that are shared with your account from a private catalog, boot volumes, and snapshots. Use one of the following links to see how to list resources that are available for creating your instance.

- [List all the available stock or custom images](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#list-available-stock-custom-images-cli)
- [List all available images shared from a private catalog](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#list-available-shared-private-catalog-images-cli)
- [List all available boot volumes](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#list-available-boot-volumes-images-cli)
- [List all available snapshots](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#list-snapshots-images-cli)

You can use allowed-use expressions with images source resources to define the capabilities and restrictions with stock images, custom images, images that are shared with your account from a private catalog, boot volumes, and snapshots and help you find compatible image and profile combinations during server creation. During the creation of a virtual server instance with images that have an allowed-use expression, the information that is provided in the allowed-use properties is then evaluated against a potential virtual server instance. The information is used to determine whether can image can be used to create the virtual server instance. Allowed-used expressions are already set for stock images. You must define them for custom images. For more information, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui).

Depending on the image that you are using, you can use one of the following commands to verify whether a profile is compatible with an image, volume, or snapshot.

- `ibmcloud is image-instance-profiles IMAGE`
- `ibmcloud is volume-instance-profiles VOLUME`
- `ibmcloud is snapshot-instance-profiles SNAPSHOT`

For a full list of command options, see [ibmcloud is images](/docs/vpc?topic=vpc-vpc-reference#images-list).

Deprecated images do not include the most current support. For more information, see [End of support for operating system considerations](/docs/vpc?topic=vpc-eos-os-considerations-intro).
{: tip}

##### List all available stock or custom images
{: #list-available-stock-custom-images-cli}

List the available stock images, custom images, or images that are shared with your account from a private catalog for creating your instance. If you are creating an instance from an existing boot volume, skip this step.

To list all available stock or custom images, run the following command.

```sh
ibmcloud is images
```
{: pre}

For this example, you see a response that is similar to the following output.

```text
ID                                          Name                               Status       Arch    OS name                   OS version       File size(GB)
Visibility   Encryption   Resource group   Catalog Offering User Data Format   Remote Account ID
r006-24d856e2-6aec-41c2-8f36-5a8a3766f0d6   my-test-image                      available    amd64   centos-7-amd64            7.x - Minimal Install  1             private      none         Default          -                cloud_init
r006-9768bb7f-c75d-4408-ba34-61015632f907   ibm-debian-10-13-minimal-amd64-2   available    amd64   debian-10-amd64           10.x Buster/Stable     1             public       none         Default          -                cloud_init       811f8abfbd32425597dc7ba40da98fa6
r006-f83ce520-00b5-40c5-9938-a5c82a273f91   ibm-debian-11-3-minimal-amd64-4    available    amd64   debian-11-amd64           11.x Bullseye/Stable   1             public       none         Default          -                cloud_init       811f8abfbd32425597dc7ba40da98fa6
```
{: screen}

##### List all available images that are shared from a private catalog
{: #list-available-shared-private-catalog-images-cli}

To list all available images shared from a private catalog, run the following commands:

If you select a catalog image that belongs to a different account, you have extra considerations and limitations to review. See [Using cross-account image references in a private catalog in the CLI](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=cli#private-catalog-image-reference-vpc-cli)
{: note}

   - To list all available private catalog image offerings, run the following command.

      ```sh
      ibmcloud is catalog-image-offerings
      ```
      {: pre}

      This command returns the identifier of each image offering and the identifier of the private catalog where the image is. Save the `offering_id` and `catalog_id` in variables, which are used later to provision an instance.

      ```sh
      offering_id=6bf79f7b-de48-4ce8-8cae-866b376f2889
      catalog_id=71306253-8444-4cae-a45d-64d35e5393ec
      ```
      {: pre}

   - To get the `offering_crn` for the offering and the `offering_version_crn` for each version in the offering, run the following command.

      ```sh
      ibmcloud is catalog-image-offering $catalog_id $offering_id
      ```
      {: pre}

   When you provision an instance, you can either provision the instance from the private catalog-managed image in the most recent version in a catalog product offering by using the `offering_crn` value or from the specific version in the catalog product offering by using the `offering_version_crn` value.

      Save the `offering_crn` and `offering_version_crn`in variables, which are used later to provision an instance.

      ```sh
       offering_crn="crn:v1:bluemix:public:globalcatalog-collection:global:a/a1234567:0b322820-dafd-4b5e-b694-6465da6f008a:offering:136559f6-4588-4af2-8585-f3c625eee09d"
       offering_version_crn="crn:v1:bluemix:public:globalcatalog-collection:global:a/a1234567:0b322820-dafd-4b5e-b694-6465da6f008a:version:136559f6-4588-4af2-8585-f3c625eee09d/8ae92879-e253-4a7c-b09f-8d30af12e518"
       ```
       {: pre}

##### List all available boot volumes
{: #list-available-boot-volumes-images-cli}

List the available boot volumes for creating your instance. If you are creating an instance from an image, skip this step To create an instance from an existing volume, you must use a volume that is compatible with the instance options chosen previously. A compatible volume is in the same zone as the instance that is being provisioned, is unattached, and has an OS compatible with the profile that is selected in step 5. Use the `volumes` subcommand to see the compatible volumes. For example, to see unattached volumes with an x64 operating system architecture in `us-south-1`:

```sh
ibmcloud is volumes --attachment-state unattached --operating-system-architecture amd64 --zone us-south-1
```
{: pre}

Second-generation boot volumes with the `sdp` volume profile do not support secure boot yet. If secure boot is needed, select an available boot volume with the `general-purpose` profile.
{: note}

##### List available snapshots
{: #list-snapshots-images-cli}

You can optionally [create a boot volume from a bootable snapshot](#create-instance-bootable-snapshot-cli) and use that for your image. To list all snapshots for a volume, see [View all snapshots that were created from the Block Storage for VPC volume](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#view-snapshots-for-volume).

If you plan to use a snapshot from another account, make sure that the right [IAM authorizations](/docs/vpc?topic=vpc-block-s2s-auth&interface=cli#block-s2s-auth-xaccountrestore-cli) are in place first. Then, contact the snapshot's owner for the CRN of the snapshot.

#### 7. List the available profiles when you create an instance
{: #list-available-profiles-cli}

List the available profiles for creating your instance.

```sh
ibmcloud is instance-profiles
```
{: pre}

For this example, you see a response that is similar to the following output.

```text
Name                         vCPU Manufacturer   Architecture   Family              vCPUs   Memory(GiB)   Bandwidth(Mbps)   Volume bandwidth(Mbps)   GPUs          Storage(GB)   Min NIC Count   Max NIC Count
bx2-2x8                      intel               amd64          balanced            2       8             4000              1000                     -      -                    1               5
bx2a-2x8                     amd                 amd64          balanced            2       8             2000              500                      -      -                    1               5
bx2d-2x8                     intel               amd64          balanced            2       8             4000              1000                     -            1x75          1               5
bx2-4x16                     intel               amd64          balanced            4       16            8000              2000                     -      -                    1               5
bx2a-4x16                    amd                 amd64          balanced            4       16            4000              1000                     -      -                    1               5
bx2d-4x16                    intel               amd64          balanced            4       16            8000              2000                     -            1x150         1               5
```
{: screen}

For more information about available profiles, see: \n * [x86 instance profiles](/docs/vpc?topic=vpc-profiles) \n *  [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles)

Secure execution-enabled profiles are now available and are identified by the fourth character of the profile name that is an "e", such as *bz2e*. For more information, see [Confidential computing with LinuxONE](/docs/vpc?topic=vpc-about-se).

The secure execution-enabled profiles are available for Balanced, Compute, and Memory families. Make sure that you use secure-enabled profiles when you use the IBM Hyper Protect Container Runtime image. Any profile mismatch during profile validation results in an error message that is similar to the following example.

```text
FAILED
Response HTTP Status Code: 400
Error code: bad_field
Error message: Image OS IBM Hyper Protect is not supported by the instance profile <profile_name>
Error target name: profile, type: field
```
{: screen}

#### 8. List the available SSH keys that you can associate with your instance
{: #list-available-ssh-keys-cli}

List the available SSH keys that you can associate with your instance.

```sh
ibmcloud is keys
```
{: pre}

For this example, you see a response that is similar to the following output.

```text
ID                                          Name     Type   Length   FingerPrint          Resource group
r006-89ec781c-9630-4f76-b9c4-a7d204828d61   my-key   rsa    4096     gtnf+pdX2PYI9Ofq..   Default
```
{: screen}

If you do not have an SSH key available, you can create an SSH key by using the [ibmcloud is key-create](/docs/vpc?topic=vpc-vpc-reference#key-create) command. For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys).

#### 9. List all the available placement groups that you can associate with your instance.
{: #list-available-placement-grouops-cli}

List all the available placement groups that you can associate with your instance.

```sh
ibmcloud is placement-groups
```
{: pre}

For this example, you see a response that is similar to the following output.

```text
ID                                            Name                             State    Strategy       Resource Group
c5f1f366-b92a-4080-991a-aa5c2e33d96b          placement-group-region-us-south   stable   power_spread  Default
```
{: screen}

### Creating an instance from the CLI
{: #create-instance-cli}
{: cli}

Use the following information to create an instance with the CLI.

### Provision from a stock or custom image
{: #instance-create-from-image-cli}
{: cli}

After you know the needed values, use them to run the **`ibmcloud is instance-create`** command. You also need to specify a unique name for the instance.

Use the following steps to create a basic virtual server instance from a stock image from the CLI. By default, a boot volume is attached to the instance when the instance is created. For most virtual server instances, the default boot volume size is 100 GB. The default boot volume size for a z/OS virtual server instance is 250 GB.

1. Create an instance by using the following command.

   ```sh
   ibmcloud is instance-create \
       INSTANCE_NAME \
       VPC \
       ZONE_NAME \
       PROFILE_NAME \
       SUBNET \
       --image IMAGE \
       --keys KEYS \
   ```
   {: pre}

   For example, the following `instance-create` command uses the sample values that are found in the [Gathering information](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#gather-info-to-create-virtual-servers-cli) section.

   ```sh
   ibmcloud is instance-create \
       my-instance \
       r006-35b9cf35-616e-462e-a145-cf8db4062fcf \
       us-south-2 \
       bx2-2x8 \
       0717-198db988-3b9b-4cfa-9dec-0206420d37d0 \
       --image r006-f83ce520-00b5-40c5-9938-a5c82a273f91 \
       --keys r006-89ec781c-9630-4f76-b9c4-a7d204828d61 \
   ```
   {: pre}

   Where the following argument and option values are used

      * INSTANCE_NAME: `my-instance`
      * VPC: `r006-35b9cf35-616e-462e-a145-cf8db4062fcf`
      * ZONE_NAME: `us-south-2`
      * PROFILE_NAME: `bx2-2x8`
      * SUBNET: `0717-198db988-3b9b-4cfa-9dec-0206420d37d0`
      * IMAGE: Debian 11 image `r006-f83ce520-00b5-40c5-9938-a5c82a273f91`
      * KEYS: `r006-89ec781c-9630-4f76-b9c4-a7d204828d61`

   The response varies depending on the option values that you use.
   {: note}

   ```text
   ID                                    0726_67b1179a-8b25-4ac9-8bc0-7f3027466ed0
   Name                                  my-instance
   CRN                                   crn:v1:public:is:us-south-2:a/a1234567::instance:0726_67b1179a-8b25-4ac9-8bc0-7f3027466ed0
   Status                                pending
   Availability policy on host failure   restart
   Startable                             true
   Profile                               bx2-2x8
   Architecture                          amd64
   vCPU Manufacturer                     intel
   vCPUs                                 2
   Memory(GiB)                           8
   Bandwidth(Mbps)                       4000
   Volume bandwidth(Mbps)                1000
   Network bandwidth(Mbps)               3000
   Lifecycle Reasons                     Code   Message
                                          -      -

   Lifecycle State                       pending

   Metadata service                      Enabled   Protocol   Response hop limit
                                         false     http       1

   Image                                 ID                                          Name
                                         r006-f83ce520-00b5-40c5-9938-a5c82a273f91   ibm-debian-11-3-minimal-amd64-4

   VPC                                   ID                                          Name
                                         r006-35b9cf35-616e-462e-a145-cf8db4062fcf   my-vpc

   Zone                                  us-south-2
   Resource group                        ID                                 Name
                                         cdc21b72d4e647b195de988b175e3d82   Default

   Created                               2023-03-23T21:50:24+00:00
   Boot volume                           ID   Name   Attachment ID                               Attachment name
                                         -    -      0717-7ccd4284-e59d-45d8-932a-9e52f62f187a   landing-faucet-prankish-sprout
   ```
   {: screen}

   Information about the network interfaces that are created for the new instance aren't returned after the instance is created. You can view the information by using the `ibmcloud is instance INSTANCE` command as described in the following step. The status displays *pending* until the instance is created.

   For more information about some additional features that you can include as command options on the `instance-create` command, see the following topics: [Create a volume attachment JSON](/docs/vpc?topic=vpc-attaching-block-storage&interface=cli#volume_attachment_json), [Enable or disable the metadata service](/docs/vpc?topic=vpc-imd-configure-service&interface=cli#imd-enable-on-instance-cli), and [Creating a placement group](/docs/vpc?topic=vpc-managing-placement-group&interface=cli#creating-placement-group-CLI).
   {: tip}

   For a full list of command options, see [ibmcloud is instance-create](/docs/vpc?topic=vpc-vpc-reference#instance-create).

2. Next, run the following `instance` details command to verify that you can see your new instance and view the network interfaces that were created for the new instance. `0726_67b1179a-8b25-4ac9-8bc0-7f3027466ed0` is the virtual server instance ID that was assigned when the instance was created in the previous step.

   ```sh
   ibmcloud is instance 0726_67b1179a-8b25-4ac9-8bc0-7f3027466ed0
   ```
   {: pre}

   For this example, you see the following response. The status now shows *running*. Check the Network Interfaces section to locate the ID of the network interface.

   ```text
   ID                                    0726_67b1179a-8b25-4ac9-8bc0-7f3027466ed0
   Name                                  my-instance
   CRN                                   crn:v1:public:is:us-south-2:a/a1234567::instance:0726_67b1179a-8b25-4ac9-8bc0-7f3027466ed0
   Status                                running
   Availability policy on host failure   restart
   Startable                             true
   Profile                               bx2-2x8
   Architecture                          amd64
   vCPU Manufacturer                     intel
   vCPUs                                 2
   Memory(GiB)                           8
   Bandwidth(Mbps)                       4000
   Volume bandwidth(Mbps)                1000
   Network bandwidth(Mbps)               3000
   Lifecycle Reasons                     Code   Message
                                          -      -

   Lifecycle State                       stable

   Metadata service                      Enabled   Protocol   Response hop limit
                                         false     http       1

   Image                                 ID                                          Name
                                         r006-f83ce520-00b5-40c5-9938-a5c82a273f91   ibm-debian-11-3-minimal-amd64-4

   VPC                                   ID                                          Name
                                         r006-35b9cf35-616e-462e-a145-cf8db4062fcf   my-vpc

   Zone                                  us-south-2
   Resource group                        ID                                 Name
                                         cdc21b72d4e647b195de988b175e3d82   Default

   Created                               2023-03-23T21:50:24+00:00
   Network Interfaces                    Interface   Name      ID                                          Subnet            Subnet ID                                   Floating IP   Security Groups                 Allow source IP spoofing   Reserved IP
                                         Primary     primary   0717-4db768bb-65c3-4045-8712-523e62eeabd2   my-subnet   0717-198db988-3b9b-4cfa-9dec-0206420d37d0         -             enhance-corsage-managing-jinx   false                      10.240.64.10

   Boot volume                           ID                                          Name                           Attachment ID                                    Attachment name
                                         r006-7a1d72d1-56ac-438e-bf85-6c0173e3f9a6   expend-anger-whiff-jackknife   0717-7ccd4284-e59d-45d8-932a-9e52f62f187a        landing-faucet-prankish-sprout
   ```
   {: screen}

3. Request a floating IP address to associate to your instance by using the following command. The name that is specified for the floating IP is `my-floatingip`. `0717-4db768bb-65c3-4045-8712-523e62eeabd2` is the ID of the network interface for the virtual server instance that displayed in the previous step.

   ```sh
   ibmcloud is floating-ip-reserve \
       my-floatingip \
       --nic 0717-4db768bb-65c3-4045-8712-523e62eeabd2
   ```
   {: pre}

   For this example, you see a response that is similar to the following output.

   ```text
   ID               r006-9b79b9bc-a2dc-4337-865a-57d9b9198b76
   Address          169.59.214.164
   Name             my-floatingip
   CRN              crn:v1:public:is:us-south-2:a/a1234567::floating-ip:r006-9b79b9bc-a2dc-4337-865a-57d9b9198b76
   Status           available
   Zone             us-south-2
   Created          2023-03-23T22:13:07+00:00
   Target           ID                                          Target type         Instance ID                                 Target interface name   Target interface private IP
                    0717-4db768bb-65c3-4045-8712-523e62eeabd2   network_interface   0726_67b1179a-8b25-4ac9-8bc0-7f3027466ed0   primary                 -


   Resource group   ID                                 Name
                    cdc21b72d4e647b195de988b175e3d82   Default
   ```
   {: screen}

    Record the floating IP `Address` to use later.

    For a full list of command options, see [ibmcloud is floating-ip-reserve](/docs/vpc?topic=vpc-vpc-reference#floating-ip-reserve).

Need more help? You can always run `ibmcloud is instance-create --help` to display help for creating an instance.
{: tip}

### Provision from a private catalog image
{: #instance-create-from-private-catalog-image-cli}
{: cli}

After you know the needed values, use them to run the **`ibmcloud is instance-create`** command. You also need to specify a unique name for the instance.

Use the following steps to create a virtual server instance from a private catalog offering or a catalog offering version from the CLI.

1. Create an instance by using the following command.

   ```sh
   ibmcloud is instance-create \
       INSTANCE_NAME \
       VPC \
       ZONE_NAME \
       PROFILE_NAME \
       SUBNET \
       --catalog-offering <CRN for the IBM Cloud catalog offering> or --catalog-offering-version <The CRN for the version of an IBM Cloud catalog offering> \
       --keys KEYS \
       --placement-group PLACEMENT_GROUP_NAME \
   ```
   {: pre}

   For example, if you create an instance that is called *my-instance* in *us-south-2* and use the *bx2-2x8* profile and the catalog offering, your `instance-create` command looks similar to the following example.

   ```sh
   ibmcloud is instance-create\
       my-instance\
       r006-35b9cf35-616e-462e-a145-cf8db4062fcf\
       us-south-2\
       bx2-2x8\
       0717-198db988-3b9b-4cfa-9dec-0206420d37d0\
       --catalog-offering crn:v1:public:globalcatalog-collection:global:a/efe5afc483594adaa8325e2b4d1290df:0b322820-dafd-4b5e-b694-6465da6f008a:offering:136559f6-4588-4af2-8585-f3c625eee09d
       --keys r006-89ec781c-9630-4f76-b9c4-a7d204828d61\
       --placement-group c5f1f366-b92a-4080-991a-aa5c2e33d96b\
   ```
   {: pre}

   Where the following argument and option values are used.

      * INSTANCE_NAME: `my-instance`
      * VPC: `r006-35b9cf35-616e-462e-a145-cf8db4062fcf`
      * ZONE_NAME: `us-south-2`
      * PROFILE_NAME: `bx2-2x8`
      * SUBNET: `0717-198db988-3b9b-4cfa-9dec-0206420d37d0`
      * CATALOG-OFFERING: is `crn:v1:public:globalcatalog-collection:global:a/efe5afc483594adaa8325e2b4d1290df:0b322820-dafd-4b5e-b694-6465da6f008a:offering:136559f6-4588-4af2-8585-f3c625eee09d`
      * KEYS: `r006-89ec781c-9630-4f76-b9c4-a7d204828d61`
      * PLACEMENT_GROUP: `c5f1f366-b92a-4080-991a-aa5c2e33d96b`

   Information about the network interfaces that are created for the new instance aren't returned after the instance is created. You can view the information by using the `ibmcloud is instance INSTANCE` command as described in the following step.

   The status of the virtual server instance displays *pending* until the instance is created.
   {: note}

   For a full list of command options, see [ibmcloud is instance-create](/docs/vpc?topic=vpc-vpc-reference#instance-create).

2. Next, run the following `instance` details command to verify that you can see your new instance and view the network interfaces that were created for the new instance. For `INSTANCE`, specify the ID that was assigned to your new virtual server instance in the previous step.

   ```sh
   ibmcloud is instance INSTANCE
   ```
   {: pre}

   The status now shows *running*. Check the Network Interfaces section to locate the ID of the network interface.

3. Request a floating IP address to associate to your instance by using the following command. For `FLOATING_IP_NAME` specify a name for your floating IP, and for `TARGET_INTERFACE` specify the ID of the network interface that you identified in the previous step.

   ```sh
   ibmcloud is floating-ip-reserve \
       FLOATING_IP_NAME \
       --nic TARGET_INTERFACE
   ```
   {: pre}

    Record the floating IP `Address` to use later.

    For a full list of command options, see [ibmcloud is floating-ip-reserve](/docs/vpc?topic=vpc-vpc-reference#floating-ip-reserve).

Need more help? You can always run `ibmcloud is instance-create --help` to display help for creating an instance.
{: tip}

### Provision from an existing volume
{: #create-instance-volume-cli}
{: cli}

After you know the needed values, use them to run the `instance-create` command. You also need to specify a unique name for the instance.

Use the following steps to create a virtual server instance from a bootable volume, and that includes a volume attachment.

1. Create an instance by using the following command.

   ```sh
   ibmcloud is instance-create \
       INSTANCE_NAME \
       VPC \
       ZONE_NAME \
       PROFILE_NAME \
       SUBNET \
       --boot-volume BOOT_VOLUME_JSON | @BOOT_VOLUME_JSON_FILE \
       --keys KEYS \
       --volume-attach VOLUME_ATTACH_JSON \
   ```
   {: pre}

   For example, if you create an instance that is called *my-instance* in *us-south-1* and use the *bx2-2x8* profile and an existing boot volume, your `instance-create` command looks similar to the following example.

   ```sh
   ibmcloud is instance-create\
       my-instance\
       r006-35b9cf35-616e-462e-a145-cf8db4062fcf\
       us-south-1\
       bx2-2x8\
       0717-198db988-3b9b-4cfa-9dec-0206420d37d0\
       --boot-volume '{"name": "boot-vol-attachment-name", "volume": {"id": "r006-feec3e99-995e-4e8f-896b-48b42c7d05a7"}}'\
       --keys r006-89ec781c-9630-4f76-b9c4-a7d204828d61\
       --volume-attach @/Users/myname/myvolume-attachment_create.json\
   ```
   {: pre}

   For an example volume attachment JSON file, see [Create a volume attachment JSON](/docs/vpc?topic=vpc-attaching-block-storage&interface=cli#volume_attachment_json). You can also include [user tags for the volumes](/docs/vpc?topic=vpc-creating-block-storage&interface=cli#create-instance-vol-cli) in the volume attachment.

   Information about the network interfaces that are created for the new instance aren't returned after the instance is created. You can view the information by using the `ibmcloud is instance INSTANCE` command as described in the next step.

   The status displays *pending* until the instance is created.
   {: note}

   For a full list of command options, see [ibmcloud is instance-create](/docs/vpc?topic=vpc-vpc-reference#instance-create).

2. Next, run the following `instance` details command to verify that you can see your new instance and view the network interfaces that were created for the new instance. For `INSTANCE`, specify the ID that was assigned to your new virtual server instance in the previous step.

   ```sh
   ibmcloud is instance INSTANCE
   ```
   {: pre}

   The status now shows *running*. Check the Network Interfaces section to locate the ID of the network interface.

3. Request a floating IP address to associate to your instance by using the following command. For `FLOATING_IP_NAME` specify a name for your floating IP, and for `TARGET_INTERFACE` specify the ID of the network interface that you identified in the previous step.

   ```sh
   ibmcloud is floating-ip-reserve \
       FLOATING_IP_NAME \
       --nic TARGET_INTERFACE
   ```
   {: pre}

   Record the floating IP `Address` to use later.

   For a full list of command options, see [ibmcloud is floating-ip-reserve](/docs/vpc?topic=vpc-vpc-reference#floating-ip-reserve).

   Need more help? You can always run `ibmcloud is instance-create --help` to display help for creating an instance.
   {: tip}

### Creating a boot volume from a snapshot and use it to provision a new instance from the CLI
{: #create-instance-bootable-snapshot-cli}

You can create a boot volume from a bootable [snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-restore-concepts) and use that for your image. When you run the `ibmcloud is instance-create` command, specify the `source_snapshot` subproperty in the boot volume JSON and the ID, name, or CRN of a bootable snapshot. For an example, see [Create a boot volume from a snapshot for a new instance from the CLI](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=cli#snapshots-vpc-restore-boot-CLI).

### Creating an instance with multiple data volumes and dynamic volume bandwidth allocation from the CLI
{: #create-instance-with-dynamic-storage-qos-cli}
{: cli}

Select [compute profiles](/docs/vpc?group=profile-details) support pooled bandwidth allocation for data volumes. For most compute profiles, the `--volume-bandwidth-qos-mode` property defaults to `weighted`. When `weighted` is used, the volume bandwidth is proportionally allocated between the data volumes. By specifying the `pooled` value, you enable dynamic bandwidth allocation.

The following example uses the `ibmcloud is instance-create` command to create the virtual server instance and specifies the option `--volume-bandwidth-qos-mode` with the value `pooled`.

```sh
ibmcloud is instance-create my-virtual-server my-vpc us-south-2 cx3d-8x20 my-subnet --image r006-534ef2ac-6158-45b3-9657-57629fa85305 --keys r006-68f8333a-1169-42da-ba01-75268bac8362 --volume-attach @/Users/myname/myvolume-attachment.json --volume-bandwidth-qos-mode pooled
```
{: pre}

The `myvolume-attachments.json` file can include up to 12 data volumes that are to be attached to the virtual server. See the following example:

```json
[
  {"volume": {
        "name": "my-data-volume-1",
        "capacity": 3000,
        "profile": {"name": "5iops-tier"},
        "user_tags": []},
   "delete_volume_on_instance_delete": false},
  {"volume": {
        "name": "my-data-volume-2",
        "capacity": 3000,
        "profile": {"name": "5iops-tier"},
        "user_tags": []},
   "delete_volume_on_instance_delete": false},
  {"volume": {
        "name": "my-data-volume-3",
        "capacity": 3000,
        "profile": {"name": "5iops-tier"},
        "user_tags": []},
   "delete_volume_on_instance_delete": false}
]
```
{: codeblock}

### Create an instance with confidential compute
{: #create-instance-confidential-compute-cli}
{: cli}

[Select availability]{: tag-green}

Confidential computing with Intel SGX for VPC and Confidential computing with Intel TDX for VPC are available in the Dallas (us-south), Washington DC (us-east), and Frankfurt (eu-de) regions.
{: preview}

Second-generation boot volumes with the `sdp` volume profile do not support secure boot yet. If secure-boot is required, use a boot volume with a first-generation volume profile.
{: note}

After you know the needed values, use them to run the `ibmcloud is instance-create` command. You also need to specify a unique name for the instance.

For `confidential-compute-mode`, you need to specify either `sgx` or `tdx` for the option.

Use the following steps to create a basic virtual server instance that enables confidential compute.

Create an instance by using the following command.

```sh
   ibmcloud is instance-create \
       INSTANCE_NAME \
       VPC \
       ZONE_NAME \
       PROFILE_NAME \
       SUBNET \
       --image IMAGE \
       --confidential-compute-mode sgx \
       --keys KEYS \
   ```
   {: pre}

For example, the following `instance-create` command uses the sample values that are found in the [Gathering information](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#gather-info-to-create-virtual-servers-cli) section.

```sh
   ibmcloud is instance-create \
       my-instance \
       r006-35b9cf35-616e-462e-a145-cf8db4062fcf \
       us-south-2 \
       bx2-2x8 \
       0717-198db988-3b9b-4cfa-9dec-0206420d37d0 \
       --image r006-f83ce520-00b5-40c5-9938-a5c82a273f91 \
       --confidential-compute-mode sgx \
       --keys r006-89ec781c-9630-4f76-b9c4-a7d204828d61 \
   ```
   {: pre}

   Where the following argument and option values are used

      * INSTANCE_NAME: `my-instance`
      * VPC: `r006-35b9cf35-616e-462e-a145-cf8db4062fcf`
      * ZONE_NAME: `us-south-2`
      * PROFILE_NAME: `bx2-2x8`
      * SUBNET: `0717-198db988-3b9b-4cfa-9dec-0206420d37d0`
      * IMAGE: Debian 11 image `r006-f83ce520-00b5-40c5-9938-a5c82a273f91`
      * KEYS: `r006-89ec781c-9630-4f76-b9c4-a7d204828d61`



### Create an instance with secure boot
{: #create-instance-secure-boot-cli}
{: cli}

[Select availability]{: tag-green}

Confidential computing with Intel SGX for VPC is available only in the Dallas (us-south) and Frankfurt (eu-de) regions. Second-generation boot volumes with the `sdp` volume profile do not support secure boot yet. If secure-boot is required, use a boot volume with a first-generation volume profile.
{: note}

After you know the needed values, use them to run the `ibmcloud is instance-create` command. You also need to specify a unique name for the instance.

For `enable-secure-boot`, you need to specify either `true` or `false`. The default value is `false`.

Use the following steps to create a basic virtual server instance that enables secure boot.

Create an instance by using the following command.

```sh
   ibmcloud is instance-create \
       INSTANCE_NAME \
       VPC \
       ZONE_NAME \
       PROFILE_NAME \
       SUBNET \
       --image IMAGE \
       --enable-secure-boot true \
       --keys KEYS \
   ```
   {: pre}

For example, the following `instance-create` command uses the sample values that are found in the [Gathering information](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#gather-info-to-create-virtual-servers-cli) section.

```sh
   ibmcloud is instance-create \
       my-instance \
       r006-35b9cf35-616e-462e-a145-cf8db4062fcf \
       us-south-2 \
       bx2-2x8 \
       0717-198db988-3b9b-4cfa-9dec-0206420d37d0 \
       --image r006-f83ce520-00b5-40c5-9938-a5c82a273f91 \
       --enable-secure-boot true \
       --keys r006-89ec781c-9630-4f76-b9c4-a7d204828d61 \
   ```
   {: pre}

   Where the following argument and option values are used

      * INSTANCE_NAME: `my-instance`
      * VPC: `r006-35b9cf35-616e-462e-a145-cf8db4062fcf`
      * ZONE_NAME: `us-south-2`
      * PROFILE_NAME: `bx2-2x8`
      * SUBNET: `0717-198db988-3b9b-4cfa-9dec-0206420d37d0`
      * IMAGE: Debian 11 image `r006-f83ce520-00b5-40c5-9938-a5c82a273f91`
      * KEYS: `r006-89ec781c-9630-4f76-b9c4-a7d204828d61`



## Next steps after an instance is created from the CLI
{: #next-step-after-creating-virtual-servers-cli}
{: cli}

A series of emails is sent to your administrator: Acknowledgment of the virtual server instance order, order approval and processing, and a message that the instance is created.

If you choose a GPU profile, see [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus).

After the instance is created, associate a floating IP address to the instance. Then, you can connect to your instance. For more information, see [Connecting to your Linux instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux) or [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows).

## Creating a virtual server instance with the API
{: #create-instance-api}
{: api}

You can create instances with the API.

### Before you begin
{: #before-you-begin-create-instance-api}
{: api}

Make sure that you have the required access. To call these methods, you must be assigned one or more IAM access roles that include the following actions, depending on any listed conditions. You can check your access by going to the **Users** page of [{{site.data.keyword.iamshort}} dashboard](https://cloud.ibm.com/iam/overview){: external}.

### Gathering information to create an instance with the API
{: #gather-info-to-create-virtual-servers-api}
{: api}

Before you can create an instance, you need to know the details about the instance, such as the instance profile or the image that you want to use. Gather information by making the following API calls:

|    Instance details   |  Listing options                | API spec documentation |
|-----------------------|---------------------------------| --------------------------------------------|
| Image                 | `GET /images`                   | [List all images](/apidocs/vpc/latest#list-images)|
| Profile               | `GET /instance/profiles`  \n  \n `GET /images/<id>/instance_profiles` | [List all instance profiles](/apidocs/vpc/latest#list-instance-profiles)  \n  \n [List all instance profiles compatible with a specific image](/apidocs/vpc/latest#list-image-instance-profiles)|
| Key                   | `GET /keys`                     | [List all keys](/apidocs/vpc/latest#list-keys)|
| VPC                   | `GET /vpcs`                     | [List all VPCs](/apidocs/vpc/latest#list-vpcs)|
| Subnet                | `GET /subnets`                  | [List all subnets](/apidocs/vpc/latest#list-subnets) |
| Zone                  | `GET /regions/<region>/zones`   | [List all zones in a region](/apidocs/vpc/latest#list-region-zones) |
| Placement groups      | `GET /placement_groups`         | [List all placement groups](/apidocs/vpc/latest#list-placement-groups)|
{: caption="Required instance details api" caption-side="bottom"}

If you plan to use a snapshot from another account, make sure that the right [IAM authorizations](/docs/vpc?topic=vpc-block-s2s-auth&interface=api#block-s2s-auth-xaccountrestore-api) are in place first. Then, contact the snapshot's owner for the CRN of the snapshot.

Some profiles might not be available because of one of the following reasons:
   - The number of network interfaces in the virtual server exceeds profile limits. You can remove network interfaces to select from more profiles. For more information, see [Resizing a virtual server](/docs/vpc?topic=vpc-resizing-an-instance).
   - The selected image contains an allowed-use expression that is not compatible with the profile. In these cases, select an image with an allowed-use expression that is compatible with the wanted profile. For more information, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui).


### Creating an instance with the API
{: #create-vsi-api}
{: api}

After you retrieved the information that you need, you can run the [`POST /instances`](/apidocs/vpc/latest#create-instance) method to create an instance.

### Provision an instance from a stock or custom image with the API
{: #create-instance-stock-custom-image-api}
{: api}

You can provision an instance with a stock or custom image by specifying the image's `id` subproperty as the value of the `image` property.

 ```bash
    curl -X POST "$vpc_api_endpoint/v1/instances?version=$api_version&generation=2" \
      -H "Authorization:$iam_token" \
      -d '{
            "name": "my-instance",
            "zone": {
              "name": "us-south-3"
            },
            "vpc": {
              "id": "'$vpc'"
            },
            "primary_network_interface": {
              "subnet": {
                "id": "'$subnet'"
              }
            },
            "keys":[{"id": "'$key'"}],
            "profile": {
              "name": "'$profile_name'"
             },
            "image": {
              "id": "'$image_id'"
             }
            }'
 ```
 {: pre}

### Provision an instance from a private catalog image with the API
{: #create-instance-private-catalog-image-api}
{: api}

You can provision an instance with a private catalog image by specifying the image's `offering_crn` or the `version_crn` subproperty as the value of the `catalog_offering` property.

* Create an instance by using a private catalog image from the most recent version of a catalog product offering.

    ```bash
    curl -X POST "$vpc_api_endpoint/v1/instances?version=$api_version&generation=2" \
      -H "Authorization:$iam_token" \
      -d '{
            "name": "my-instance",
            "zone": {
              "name": "us-south-3"
            },
            "vpc": {
              "id": "'$vpc'"
            },
            "primary_network_interface": {
              "subnet": {
                "id": "'$subnet'"
              }
            },
            "keys":[{"id": "'$key'"}],
            "profile": {
              "name": "'$profile_name'"
             },
            "catalog_offering": {
              "offering": {
                "crn": "'$offering_crn'"
             }
            }'
    ```
    {: pre}

* Create an instance by using a private catalog image from a specific version of a catalog product offering.

    ```bash
    curl -X POST "$vpc_api_endpoint/v1/instances?version=$api_version&generation=2" \
      -H "Authorization:$iam_token" \
      -d '{
            "name": "my-instance",
            "zone": {
              "name": "us-south-3"
            },
            "vpc": {
              "id": "'$vpc'"
            },
            "primary_network_interface": {
              "subnet": {
                "id": "'$subnet'"
              }
            },
            "keys":[{"id": "'$key'"}],
            "profile": {
              "name": "'$profile_name'"
             },
            "catalog_offering": {
              "version": {
                "crn": "'$version_crn'"
             }
            }'
    ```
    {: pre}

### Provision from an existing volume
{: #create-instance-ipbv-api}
{: api}

Reusing an existing, bootable volume is faster than creating a new volume from a snapshot or an image.

You can provision an instance with an existing volume by specifying the existing volume's `id` or `crn` subproperty as the value of the `boot_volume_attachment` property.

The existing bootable volume must be an unattached bootable volume that has the same architecture as the instance profile. Use the [list volumes](/apidocs/vpc/latest#list-volumes) filter and reference the `attachment_state` property and `operating_system` property to view a volume's eligibility.

For example, to see unattached volumes in `us-south-1` with an x86 operating system.

```sh
curl -X GET "$vpc_api_endpoint/v1/volumes?version=2023-02-08&generation=2?attachment_state=unattached&zone.name=us-south-1&operating_system.architecture=amd64"
-H "Authorization: Bearer $iam_token"
```

By default, a boot volume that is created as part of provisioning a virtual server instance is deleted when the instance is deleted. You can change this behavior by setting the `delete_volume_on_instance_delete` property to `false` when you create the instance or update the boot volume attachment.

Use the [`POST /instances`](/apidocs/vpc/latest#create-instance) method to create an instance with the information you gathered. The following call is an example of provisioning an instance by using an existing boot volume.

```sh
curl -X POST "$vpc_api_endpoint/v1/instances?version=2023-02-08&generation=2"
-H "Authorization: Bearer $iam_token"
-d '{
  "boot_volume_attachment": {
    "volume": {
      "id": "r006-feec3e99-995e-4e8f-896b-48b42c7d05a7"
    }
  },
  "keys": [
    {
      "id": "363f6d70-0000-0001-0000-00000013b96c"
    }
  ],
  "name": "my-instance",
  "placement_target": {
    "id": "0787-8c2a09be-ee18-4af2-8ef4-6a6060732221"
  },
  "primary_network_interface": {
    "name": "my-network-interface",
    "subnet": {
      "id": "bea6a632-5e13-42a4-b4b8-31dc877abfe4"
    }
  },
  "profile": {
    "name": "bx2-2x8"
  },
  "volume_attachments": [
    {
      "volume": {
        "capacity": 1000,
        "encryption_key": {
          "crn": "crn:[...]"
        },
        "name": "my-data-volume",
        "profile": {
          "name": "5iops-tier"
        }
      }
    }
  ],
  "vpc": {
    "id": "f0aae929-7047-46d1-92e1-9102b07a7f6f"
  },
  "zone": {
    "name": "us-south-1"
  }
}'
```
{: pre}

Second-generation boot volumes with the `sdp` volume profile do not support secure boot yet. If secure-boot is required, use a boot volume with a first-generation volume profile.
{: note}

For more information, see [Create an instance](/apidocs/vpc/latest#create-instance).

### Restore a boot volume from a snapshot and use it to provision a new instance
{: #create-instance-bootable-snapshot-api}
{: api}

You can [restore](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-restore-concepts) a boot volume from a bootable snapshot and then use that boot volume when you provision an instance. The bootable snapshot must have the same operating system and architecture as the instance profile.

In the `POST /instances` request, specify the `boot_volume_attachment` property and the bootable snapshot ID in the `source_snapshot` subproperty. Alternatively, you can also use the name or the CRN of the snapshot. See the following example.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/instances?version=2023-03-07&generation=2" \
-H "Authorization: $iam_token" \
-H "Content-Type: application/json" \
-d '{
      "boot_volume_attachment": {
        "delete_volume_on_instance_delete": true,
        "volume": {
            "profile": {
                "name": "general-purpose"
            },
            "source_snapshot": {
                "id": "eb373975-4171-4d91-81d2-c49efb033753"
            }
        }
     },
     .
     .
     .
  }'
```
{: codeblock}

For more information about restoring a volume with the API, see [Restore a volume from a snapshot with the API](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-restore-API).

### Creating an instance with multiple data volumes and dynamic volume bandwidth allocation with the API
{: #create-instance-with-dynamic-storage-qos}
{: api}

Select [compute profiles](/docs/vpc?group=profile-details) support pooled bandwidth allocation for data volumes. You can programmatically provision a virtual server instance with multiple data volumes by calling the `instances` method as shown in the following sample request. The example creates a virtual server in *us-south-2* zone with a 250-GB boot volume and three 3-TB data volumes. The `storage_qos_mode` is set to `pooled` to enable dynamic volume bandwidth allocation.

```sh
curl -X POST "$vpc_api_endpoint/v1/instances?version=2024-07-12&generation=2" \
-H "Authorization: $iam_token" \
-H "Content-Type: application/json" \
-d '{
  "zone": {"name": "us-south-2"},
  "resource_group": {"id": "db8e8d865a83e0aae03f25a492c5b39e"},
  "name": "my-gen3-instance",
  "vpc": {"id": "r006-6e8fb140-5668-45b8-b98a-d5cb0e0bf39b"},
  "user_data": "",
  "profile": {"name": "cx3d-8x20"},
  "keys": [{"id": "r006-68f8333a-1169-42da-ba01-75268bac8362"}],
  "volume_attachments": [
    {"volume": {
        "name": "my-data-volume-a",
        "capacity": 3000,
        "profile": {"name": "5iops-tier"},
        "user_tags": []},
      "delete_volume_on_instance_delete": false},
    {"volume": {
        "name": "my-data-volume-b",
        "capacity": 3000,
        "profile": {"name": "5iops-tier"},
        "user_tags": []},
      "delete_volume_on_instance_delete": false
    },
    {"volume": {
        "name": "my-data-volume-c",
        "capacity": 3000,
        "profile": {"name": "5iops-tier"},
        "user_tags": []},
      "delete_volume_on_instance_delete": false
      }],
  "boot_volume_attachment": {
    "volume": {
      "name": "my-gen3-instance-boot-1720810915000",
      "capacity": 250,
      "profile": {"name": "general-purpose"},
      "user_tags": []},
    "delete_volume_on_instance_delete": true
  },
  "metadata_service": {"enabled": false},
  "primary_network_attachment": {
    "name": "eth0",
    "virtual_network_interface": {
      "allow_ip_spoofing": false,
      "auto_delete": true,
      "enable_infrastructure_nat": true,
      "primary_ip": {"auto_delete": true},
      "subnet": {"id": "0726-298acd6c-e71e-4204-a04f-fe4a4dd89805"},
      "security_groups": [{"id": "r006-ccdd1f58-f0f2-4ea0-8774-a24bbe61b5d9"}],
      "protocol_state_filtering_mode": "auto"}
  },
  "network_attachments": [],
  "image": {"id": "r006-534ef2ac-6158-45b3-9657-57629fa85305"},
  "enable_secure_boot": false
  "storage_qos_mode": pooled
}'
```
{: codeblock}

### Creating an instance with confidential compute
{: #create-instance-confidential-compute-api}
{: api}

[Select availability]{: tag-green}

Confidential computing with Intel SGX for VPC and Confidential computing with Intel TDX for VPC are available in the Dallas (us-south), Washington DC (us-east), and Frankfurt (eu-de) regions.
{: preview}

Second-generation boot volumes with the `sdp` volume profile do not support secure boot yet. If secure-boot is required, use a boot volume with a first-generation volume profile.
{: note}

To provision an instance with confidential compute, add the `confidential_compute_mode` property and set it to either `sgx` or `tdx`.

 ```bash
    curl -X POST "$vpc_api_endpoint/v1/instances?version=$api_version&generation=2" \
      -H "Authorization:$iam_token" \
      -d '{
            "name": "my-instance",
            "zone": {
              "name": "us-south-3"
            },
            "vpc": {
              "id": "'$vpc'"
            },
            "primary_network_interface": {
              "subnet": {
                "id": "'$subnet'"
              }
            },
            "keys":[{"id": "'$key'"}],
            "profile": {
              "name": "'$profile_name'"
             },
            "image": {
              "id": "'$image_id'"
             },
             "confidential_compute_mode": "sgx"
            }'
 ```
 {: pre}

### Creating an instance with secure boot
{: #create-instance-secure-boot-api}
{: api}

To provision an instance with secure boot, add the `enable_secure_boot` property and set it to `true`.

 ```bash
    curl -X POST "$vpc_api_endpoint/v1/instances?version=$api_version&generation=2" \
      -H "Authorization:$iam_token" \
      -d '{
            "name": "my-instance",
            "zone": {
              "name": "us-south-3"
            },
            "vpc": {
              "id": "'$vpc'"
            },
            "primary_network_interface": {
              "subnet": {
                "id": "'$subnet'"
              }
            },
            "keys":[{"id": "'$key'"}],
            "profile": {
              "name": "'$profile_name'"
             },
            "image": {
              "id": "'$image_id'"
             },
             "enable_secure_boot": "true"
            }'
 ```
 {: pre}

 Second-generation boot volumes with the `sdp` volume profile do not support secure boot yet. If secure-boot is required, use a boot volume with a first-generation volume profile.
 {: note}

## Creating virtual server instances by using Terraform
{: #create-instance-terraform}
{: terraform}

You can create instances by using Terraform. If you would like to use user tags or access management tags to manage your resources, see [Working with tags](/docs/account?topic=account-tag&interface=terraform).

### Before you begin
{: #before-you-begin-create-instance-terraform}
{: terraform}

Make sure that you set up [Terraform for VPC](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest){: external}.

### Creating a private catalog
{: #terraform-create-private-catalog}
{: terraform}

This step is optional. If you plan to share images from a private catalog, the private catalog must be created first. If you select a catalog image that belongs to a different account, review [Using cross-account image references in a private catalog in Terraform](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=terraform#private-catalog-image-reference-vpc-terraform) for more considerations and limitations.

### Gathering information to create an instance by using Terraform
{: #gather-info-to-create-virtual-servers-terraform}
{: terraform}

Ready to create an instance? Before you can run the `ibm_is_instance` command, you need to know the details about the instance, such as what profile or image that you want to use.

Gather the following information by using `DataSource` command.

1. Gather instance profile details. Run the following command for the profile that you select. See [x86 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#profiles) for a list of available profiles. For more information, see the Terraform documentation on [ibm_is_instance_profiles](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_instance_profiles){: external}. Use an instance profile by referring to the instance profile data source. For more information, see the Terraform documentation on [ibm_is_instance_profile](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_instance_profile){: external}.

   * Gather instance profile details for a specific profile

   ```terraform
   data "ibm_is_instance_profile" "example_profile" {
      name = "bx2-2x8"
   }
   ```
   {: codeblock}

1. List the available images for creating your instance. You can use a stock image, a custom image from your account, or an image that was shared with your account from a private catalog. For more information, see the Terraform documentation on [ibm_is_image](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_image){: external}. If you plan to use an image that was shared from a private catalog, see the Terraform documentation on [ibm_cm_version](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/cm_version){: external} or [ibm_cm_offering_instance](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/cm_offering_instance){: external}.

    Allowed-use expressions: The image that you select determines the profiles that are available to create the virtual server instance. During the creation of a virtual server instance with images that use an allowed-use expression, the information that is provided in the allowed-use properties is then evaluated against a potential virtual server instance to determine whether that image can be used to create the virtual server instance. Stock images use defined allowed-use expressions. You must define any allowed-use expressions for custom images. For more information, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui).
    {: note}

   * Select a stock image or custom image from your account for your instance.

       ```terraform
       data "ibm_is_image" "example_image" {
          name = "ibm-centos-7-6-minimal-amd64-2"
       }
       ```
       {: codeblock}

   * Select an image that is shared from a private catalog for the instance. For more information, see the Terraform documentation on [ibm_is_images](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_images){: external}. You can select an image from the list to create the instance as shown in the section [Go to Creating an instance by using Terraform section](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#create-instance-terraform).

   If you select a catalog image that belongs to a different account, you have more considerations and limitations to review. See [Using cross-account image references in a private catalog in Terraform](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=terraform#private-catalog-image-reference-vpc-terraform).
     {: note}

      - To list all available private catalog image offerings, run the following command.

          ```terraform
          data "ibm_is_images" "example_images" {
          catalog_managed = true
          }
          ```
          {: codeblock}

   * List profiles and compatible images by using allowed-use expressions

      - List instance profiles that are compatible with an image

          ```terraform
          data "ibm_is_image_instance_profiles" "testacc_image_instance_profiles" {
          identifier = "r134-0950e619-325e-446e-b895-e0bdd21dd1ea"
          }
          ```
          {: codeblock}

      - List instance profiles compatible with a volume

          ```terraform
          data "ibm_is_volume_instance_profiles" "testacc_volume_instance_profiles" {
          identifier = "r134-0950e619-325e-446e-b895-e0bdd21dd1ea"
          }
          ```
          {: codeblock}

      - List instance profiles compatible with a snapshot

          ```terraform
          data "ibm_is_snapshot_instance_profiles" "testacc_snapshot_instance_profiles" {
          identifier = "r134-0950e619-325e-446e-b895-e0bdd21dd1ea"
          }
          ```
          {: codeblock}

1. Create a VPC resource or use an existing VPC by referring to the VPC data source. For more information, see the Terraform documentation on [ibm_is_vpc](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_vpc){: external}.

   ```terraform
   resource "ibm_is_vpc" "example_vpc" {
      name = "example-vpc"
   }
   ```
   {: codeblock}

1. Create a subnet resource or use an existing subnet by referring to the subnet data source. For more information, see the Terraform documentation on [ibm_is_subnet](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_subnet){: external}.

   ```terraform
   resource "ibm_is_subnet" "example_subnet" {
      name            = "example-subnet"
      vpc             = ibm_is_vpc.example_vpc.id
      zone            = "us-south-1"
      ipv4_cidr_block = "10.240.0.0/24"
   }
   ```
   {: codeblock}

1. Create a ssh-key resource or use an existing ssh-key by referring to the ssh-key data source. For more information, see the Terraform documentation on [ibm_is_ssh_keys](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_ssh_keys){: external}.

   ```terraform
   resource "ibm_is_ssh_key" "example_sshkey" {
      name       = "example-sshkey"
      type       = "rsa"
      public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCKVmnMOlHKcZK8tpt3MP1lqOLAcqcJzhsvJcjscgVERRN7/9484SOBJ3HSKxxNG5JN8owAjy5f9yYwcUg+JaUVuytn5Pv3aeYROHGGg+5G346xaq3DAwX6Y5ykr2fvjObgncQBnuU5KHWCECO/4h8uWuwh/kfniXPVjFToc+gnkqA+3RKpAecZhFXwfalQ9mMuYGFxn+fwn8cYEApsJbsEmb0iJwPiZ5hjFC8wREuiTlhPHDgkBLOiycd20op2nXzDbHfCHInquEe/gYxEitALONxm0swBOwJZwlTDOB7C6y2dzlrtxr1L59m7pCkWI4EtTRLvleehBoj3u7jB4usR"
   }
   ```
   {: codeblock}

   SSH keys can be either RSA or ED25519. You can generate new RSA key pairs by using the UI. Pre-existing RSA and ED25519 SSH keys can be uploaded. ED25519 can be used only if the operating system supports this key type. ED25519 can't be used with Windows or VMware images.
   {: note}

1. Create a subnet_reserved_ip resource or use an existing subnet_reserved_ip by referring to the subnet_reserved_ip data source. For more information, see the Terraform documentation on [ibm_is_subnet_reserved_ip](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_subnet_reserved_ip){: external}.

   ```terraform
   resource "ibm_is_subnet_reserved_ip" "example_reserved_ip" {
      subnet    = ibm_is_subnet.example_subnet.id
      name      = "example-reserved-ip1"
      address   = "${replace(ibm_is_subnet.example_subnet.ipv4_cidr_block, "0/24", "13")}"
   }
   ```
   {: codeblock}

### Creating an instance by using Terraform
{: #create-instance-using-terraform}
{: terraform}

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

Create the instance by using one of the following examples. For more information, see the Terraform documentation on [ibm_is_instance](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance){: external}.

Allowed-use expressions: The image that you select determines the profiles that are available to create the virtual server instance. During the creation of a virtual server instance with images that use an allowed-use expression, the information that is provided in the allowed-use properties is then evaluated against a potential virtual server instance to determine whether that image can be used to create the virtual server instance. Stock images use defined allowed-use expressions. You must define any allowed-use expressions for custom images. For more information, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui).
{: note}

Run one of the following Terraform commands based on the image that you plan to use.

* Create an instance by using a stock image or custom image from your account for your instance.

   ```terraform
   resource "ibm_is_instance" "example_instance" {
     name    = "example-instance-reserved-ip"
     image   = data.ibm_is_image.example_image.id
     profile = data.ibm_is_instance_profile.example_profile.name

     primary_network_interface {
       name   = "eth0"
       subnet = ibm_is_subnet.example_subnet.id
       primary_ip {
         reserved_ip = ibm_is_subnet_reserved_ip.example_reserved_ip.reserved_ip
       }
     }
     network_interfaces {
       name   = "eth1"
       subnet = ibm_is_subnet.example_subnet.id
       primary_ip {
         name = "example-reserved-ip1"
         auto_delete = true
         address = "${replace(ibm_is_subnet.example_subnet.ipv4_cidr_block, "0/24", "14")}"
       }
     }

     vpc  = ibm_is_vpc.example_vpc.id
     zone = "us-south-1"
     keys = [ibm_is_ssh_key.example_sshkey.id]
   }
   ```
   {: codeblock}

* Create an instance that uses a private catalog-managed image. When you specify the second-generation volume profile for the boot volume, you can specify custom capacity, IOPS, and bandwidth limits.

   ```terraform
   resource "ibm_is_instance" "example_instance" {
     name    = "example-instance-reserved-ip"
     image   = data.ibm_is_image.example_image.id
     profile = data.ibm_is_instance_profile.example_profile.name

     primary_network_interface {
       name   = "eth0"
       subnet = ibm_is_subnet.example_subnet.id
       primary_ip {
         reserved_ip = ibm_is_subnet_reserved_ip.example_reserved_ip.reserved_ip
       }
     }
     network_interfaces {
       name   = "eth1"
       subnet = ibm_is_subnet.example_subnet.id
       primary_ip {
         name = "example-reserved-ip1"
         auto_delete = true
         address = "${replace(ibm_is_subnet.example_subnet.ipv4_cidr_block, "0/24", "14")}"
       }
     }

     vpc  = ibm_is_vpc.example_vpc.id
     zone = "us-south-1"
     keys = [ibm_is_ssh_key.example_sshkey.id]

     boot_volume {
       name           = "example-boot-volume"
       profile        = "sdp"
       size           = 250
       iops           = 3000
       bandwidth      = 1000
       encryption_key = "crn:v1:bluemix:public:kms:us-south:a/a1234567:e4a29d1a-2ef0-42a6-8fd2-350deb1c647e:key:5437653b-c4b1-447f-9646-b2a2a4cd6179"
      }
   }
   ```
   {: codeblock}

   * Create an instance by using a snapshot with an allowed-use expression from your account for your instance.

   ```terraform
   resource "ibm_is_instance" "testacc_instance" {
     name    = "example-instance"
     profile = "cx2-2x4"
     primary_network_interface {
       subnet = ibm_is_subnet.testacc_subnet.id
     }
     vpc  = ibm_is_vpc.testacc_vpc.id
     zone = "us-south-1"
     keys = [ibm_is_ssh_key.testacc_sshkey.id]
     boot_volume {
       name     = "example-boot-volume"
       snapshot = ibm_is_snapshot.testacc_snapshot.id
       allowed_use {
         api_version       = "2025-06-05"
         bare_metal_server = "true"
         instance          = "true"
       }
     }
   }
   ```
   {: codeblock}
