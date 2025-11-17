---

copyright:
  years: 2018, 2025
lastupdated: "2025-11-17"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning for virtual server instances
{: #vsi_best_practices}

When you're planning to provision virtual server instances for {{site.data.keyword.vpc_short}}, use the following table to help you get the most out of your deployment.
{: shortdesc}

## Planning to provision virtual server instances
{: #planning-for-provisioning-instances}

After a VPC is available, consider the following before you provision your instance.

The default VPC is selected automatically. If another VPC is not selected, the default VPC is attributed to the virtual server instance.
{: tip}

| Item | Considerations |
|------| ----- |
| Required IAM permissions | Make sure that your account has the required user permissions. If you have authorization as an editor or admin on a VPC resource, then you also inherit authorization to create, delete, and modify virtual server instances within that virtual private cloud.|
| Account limits | Check your [account limits](/docs/vpc?topic=vpc-quotas) for concurrent instances.|
| SSH key | Make sure that your [SSH key](/docs/vpc?topic=vpc-ssh-keys#ssh-keys) is available.|
| Location | Determine what region and zone to select.|
| Subnet | Determine which subnets that you want the instance to connect to.|
| Profile | Consider the popular [profile options](/docs/vpc?topic=vpc-profiles#profiles) of vCPU and RAM combinations for your workload. Profiles contain preconfigured instances that are ready to use in a matter of minutes. It's important to make sure that your instances have the necessary resources to keep your workloads and your environment up and running. For {{site.data.keyword.cloud}} Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}} instances and z/OS virtual server instances, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles).Some profiles might not be available because of one of the following reasons:  \n  \n - The number of network interfaces in the virtual server exceeds profile limits. You can remove network interfaces to select from more profiles. For more information, see [Resizing a virtual server](/docs/vpc?topic=vpc-resizing-an-instance). \n  \n - The image selected contains an allowed-use expression that is not compatible with the profile. In these cases, select an image with an allowed-use expression that is compatible with the desired profile. For more infomation, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui). |
| Operating system | Determine what operating system image to select for your instance. You can select a stock image, custom image, catalog image, snapshot, or an existing volume.  \n * **Stock images**: You can select from available stock images. For more information, see [x86 virtual server images](/docs/vpc?topic=vpc-about-images&interface=ui) and [s390x virtual server images](/docs/vpc?topic=vpc-vsabout-images). \n * **Custom images**: A custom image can be an image that you customize and upload to {{site.data.keyword.cos_full_notm}}, which you can then import into {{site.data.keyword.vpc_short}}. You can also use a custom image that was created from a boot volume. For more information about custom images, see [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images). \n * **Catalog images**: A catalog image is a custom image that is imported into a private catalog. For more information about catalog images, see [VPC considerations when using custom images in a private catalog](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=ui). \n * **Snapshot**: You can select from available snapshots. For more information, see [About Block Storage Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about). \n * **Existing volume**: You can select from existing volumes. The specified volume must be unattached, and must have an operating system with the same architecture as the instance profile.|
| Profile | Consider the popular [profile options](/docs/vpc?topic=vpc-profiles#profiles) of vCPU and RAM combinations for your workload. Profiles contain preconfigured instances that are ready to use in a matter of minutes. It's important to make sure that your instances have the necessary resources to keep your workloads and your environment up and running. For {{site.data.keyword.cloud}} Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}} instances and z/OS virtual server instances, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles).  \n  \n Some profiles might not be available because the number of network interfaces in the virtual server exceeds profile limits. You can remove network interfaces to select from more profiles. For more information, see [Resizing a virtual server](/docs/vpc?topic=vpc-resizing-an-instance). \n  \n Some profiles might not be available because the image selected contains an allowed-use expression that is not compatible with the profile. In these cases, select an image with an allowed-use expression that is compatible with the wanted profile. For more information, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui). |
| Operating system | Determine what operating system [image](/docs/vpc?topic=vpc-about-images) to select for your instance. You can choose a stock image, a custom image from your account, or a custom image that was shared with your account from a private catalog. For more information about custom images, see [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images). \n  \n  If you plan to use Windows operating systems with SQL Server, see the [About Microsoft SQL on VPC](/docs/microsoft?topic=microsoft-mssql-about).  \n  \n You can use an allowed-use expression with your custom image to define the capabilities and restrictions of an image and help you find compatible image and profile combinations during server creation. To use allowed-use expressions with your custom images, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui).  \n \n  |
| Naming | Make sure that you have a unique name for the instance. The instance name must be unique within an account and region. If you have a method to naming virtual server instances, it's much easier to filter and search on them later. |
| Network interfaces | Determine how many [network interfaces](/docs/vpc?topic=vpc-using-instance-vnics#about-network-interfaces) that you need and which [security group](/docs/vpc?topic=vpc-using-security-groups) to attach to each interface.|
| Placement groups | Determine whether you want to use placement groups. If you add an instance to an existing placement group, the instance is placed according to the placement group strategy. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc). |
| Boot volume size | Specify the size of the boot volume. The default boot volume size for most profiles is 100 GB. The boot volume size can be increased up to 250 GB.|
| Auxiliary storage | Determine the number of auxiliary storage [volumes](/docs/vpc?topic=vpc-block-storage-about#secondary-data-volumes) that you need and their capacity. You can attach up to 12 data volumes to your instance. |
| Bandwidth allocation | Instance bandwidth is distributed between networking and storage resources. If you do not specify the initial volume and network bandwidth allocation, then 25% of total instance bandwidth is allocated to volume bandwidth and 75% is allocated to network bandwidth. The network bandwidth allocation is distributed evenly across network interfaces, and each network interface has a cap of 25 Gbps. The storage bandwidth is divided between the boot volume and the attached data volumes. To help ensure reasonable boot times, a minimum of 393 MBps is allocated to the primary boot volume. The remaining bandwidth is divided proportionally between the attached data volumes based on their provisioned size and IOPS setting. The storage bandwidth allocation changes only if and when a data volume is attached or detached. \n [New]{: tag-new} Select [compute profiles](/docs/vpc?group=profile-details) support dynamic bandwidth allocation for data volumes, so a volume that is maximizing its I/O capability can use unused capacity from other volume attachments. For more information, see [Pooled volume bandwidth allocation](/docs/vpc?topic=vpc-block-storage-bandwidth#pooled-vol-bandwidth).|
| User tags | Decide what [user tags](/docs/account?topic=account-tag&interface=ui) you might want to add during instance provisioning. You can add user tags to help identify the instance, the boot volume, and any data volumes you create and attach during instance provisioning. |
| Connectivity | For {{site.data.keyword.cloud}} Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}} instances, make sure that you either [enable a public gateway](/docs/vpc?topic=vpc-about-networking-for-vpc#public-gateway-for-external-connectivity) in the subnet, or [reserve a floating IP address](/docs/vpc?topic=vpc-about-networking-for-vpc#floating-ip-for-external-connectivity) and associate it with the network interface of the instance just after instance creation. |
{: caption="Checklist for planning to provision a virtual server instance" caption-side="bottom"}

## Next steps
{: #next-create-instance}

When you're ready to get started, see the following tutorials:

* [Using the {{site.data.keyword.cloud_notm}} console to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console)
* [Creating VPC resources with CLI and API](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli)
