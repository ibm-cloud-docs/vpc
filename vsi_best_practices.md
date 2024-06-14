---

copyright:
  years: 2018, 2024
lastupdated: "2024-06-14"

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
| Profile | Consider the popular [profile options](/docs/vpc?topic=vpc-profiles#profiles) of vCPU and RAM combinations for your workload. Profiles contain preconfigured instances that are ready to use in a matter of minutes. It's important to make sure that your instances have the necessary resources to keep your workloads and your environment up and running. For {{site.data.keyword.cloud}} Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}} instances and z/OS virtual server instances, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles).  \n  \n Some profiles might not be available because the amount network interfaces in the virtual server exceed profile limits. You can remove network interfaces to select from more profiles. For more information, see [Resizing a virtual server](/docs/vpc?topic=vpc-resizing-an-instance).|
| Operating system | Determine what operating system [image](/docs/vpc?topic=vpc-about-images) to select for your instance. You can choose a stock image, a custom image from your account, or a custom image that was shared with your account from a private catalog. For more information about custom images, see [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images). \n If you plan to use Windows operating systems with SQL Server, see the [About Microsoft SQL on VPC](/docs/microsoft?topic=microsoft-mssql-about). |
| Naming | Make sure that you have a unique name for the instance. The instance name must be unique within an account and region. If you have a method to naming virtual server instances, it's much easier to filter and search on them later. |
| Auxillary storage | Determine the amount of auxiliary storage [volumes](/docs/vpc?topic=vpc-block-storage-about#secondary-data-volumes) that you need. |
| Network interfaces | Determine how many [network interfaces](/docs/vpc?topic=vpc-using-instance-vnics#about-network-interfaces) that you need and which [security group](/docs/vpc?topic=vpc-using-security-groups) to attach to each interface.|
| Placement groups | Determine if you want to use placement groups. If you add an instance to an existing placement group, the instance is placed according to the placement group strategy. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc). |
| Boot volume size | Specify the size of the boot volume. |
| User tags | Decide what [user tags](/docs/account?topic=account-tag&interface=ui) you might want to add during instance provisioning. You can add user tags identify the instance, the boot volume, and any data volumes you create and attach during instance provisioning. |
| Connectivity | For {{site.data.keyword.cloud}} Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}} instances, make sure that you either [enable a public gateway](/docs/vpc?topic=vpc-about-networking-for-vpc#public-gateway-for-external-connectivity) in the subnet, or [reserve a floating IP address](/docs/vpc?topic=vpc-about-networking-for-vpc#floating-ip-for-external-connectivity) and associate it with the network interface of the instance just after instance creation. |
{: caption="Table 1. Checklist for planning to provision a virtual server instance" caption-side="bottom"}

## Next steps
{: #next-create-instance}

When you're ready to get started, see the following tutorials:
 * [Using the {{site.data.keyword.cloud_notm}} console to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console)
 * [Creating VPC resources with CLI and API](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli)
