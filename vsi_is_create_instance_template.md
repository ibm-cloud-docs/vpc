---

copyright:
  years: 2020, 2023
lastupdated: "2023-10-24"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an instance template
{: #create-instance-template}

You can create an instance template to define instance details for provisioning one or more virtual servers. When your instance template is created, you can use it to provision single virtual server instances, or you can provision multiple instances at the same time as part of an instance group.

## Creating an instance template with the UI
{: #instance-template-ui}
{: ui}

The instance template defines the details of the virtual server instances that are created from the template. For example, specify the profile (vCPU and memory), image, attached volumes, and network interfaces for the image template.

To create an instance template, complete the following steps.

1. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Auto scale > Instance templates**.
2. Click **New instance template** and enter the information in Table 1.
3. Click **Create instance template** when the information is complete.

When you create an instance template, validation steps are performed that ensure you can use your template to provision a virtual server instance.
{: tip}

<!-- this is the conref file in the vpc repo - this content is shared with vsi_is_creating_auto_scale_group.md and vsi_is_create_instance_template.md -->
{{site.data.content.create-instance-template-table}}

## Creating an instance template with the CLI
{: #solo-instance-template-cli}
{: cli}

You can create one or more instance templates in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI). For more information about creating a virtual server instance with a custom image shared from a private catalog, see [Creating VPC resources with CLI and API](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli).

### Before you begin
{: #before-instance-cli}

Make sure that you completed set up for your [{{site.data.keyword.cloud}} CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup)
and your [{{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli).

### Gathering information to create an instance template
{: #cli-options-instance-template-create}

Ready to create an instance template? Before you can run the `ibmcloud is instance-template-create` command, you need to know the details that you want to include for your instance template and command options. Such as what profile and image that you want to use. Follow these steps to prepare for running the command.

Gather the following required instance template details.

|    Instance template details   |       Listing command           | VPC CLI reference documentation |
| ----------------------------- | -------------------------------- |----------------------------------|
| VPC | `ibmcloud is vpcs` | [List all VPCs](/docs/vpc?topic=vpc-vpc-reference#vpcs-list) |
| Zone | `ibmcloud is zones` | [List all zones in the regions](/docs/vpc?topic=vpc-vpc-reference#zones-list) |
| Profile | `ibmcloud is instances` | [List all virtual server instances](/docs/vpc?topic=vpc-vpc-reference#instances-list) |
| Subnet | `ibmcloud is subnets` | [List all subnets](/docs/vpc?topic=vpc-vpc-reference#subnets-list) |
| Image | `ibmcloud is image` | [List all images](/docs/vpc?topic=vpc-vpc-reference#images-list)|
| Keys | `ibmcloud is keys` | [List all keys](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#keys)  \n  \n If you don't have any available SSH keys, use [Create a key](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#key-create) to create one.  \n  \n **Note:**  SSH keys can either be RSA or Ed25519. You can generate new RSA key pairs using the UI. Pre-existing RSA and Ed25519 SSH keys can be uploaded. Ed25519 can be used only if the operating system supports this key type. Ed25519 can't be used with Windows or VMware images. \n For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| Placement groups | `ibmcloud is placement-groups` | [List all placement groups](/docs/vpc?topic=vpc-vpc-reference#placement-groups-list) |
{: caption="Table 1. Required instance template details" caption-side="bottom"}

Use the following commands to determine the required information for creating a new instance template.

1. List the regions associated with your account.

   ```sh
   ibmcloud is regions
   ```
   {: pre}

   See the following example.

   ```sh
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

1. Switch to your target region.

   ```sh
   ibmcloud target -r <region-name>
   ```
   {: pre}

1. List the zones associated with the target region.

   ```sh
   ibmcloud is zones
   ```
   {: pre}

   In the following example, the command is run in the `us-south` region and the output shows the available zones in the region.

   ```sh
   $ ibmcloud is zones
   Listing zones in target region us-south under account Test Account as user test.user@ibm.com...
   Name         Region     Status   
   us-south-1   us-south   available   
   us-south-2   us-south   available   
   us-south-3   us-south   available   
   ```
   {: screen}

1. List the {{site.data.keyword.vpc_short}}s that are associated with your account.

   ```sh
   ibmcloud is vpcs
   ```
   {: pre}

   For this example, you see a response similar to the following output.

   ```sh
   ID                                          Name                         Status      Classic access   Default network ACL                   Default security group                 Resource group
   r006-0d37163a-701d-4ad6-9ece-a3e34cf28935   cdl                          available   false            acl-test-1                            pedicure-budding-providing-excluded    Default
   r006-212def4b-93f0-4b9a-a331-94178b99d474   cli-test-1                   available   false            surprise-pacifier-cubicle-demystify   foyer-alongside-rug-zen                Default
   ```
   {: screen}

   If you don't have one available, you can create an {{site.data.keyword.vpc_short}} by using the `ibmcloud is vpc-create` command. For more information about creating an {{site.data.keyword.vpc_short}}, see [IBM Cloud VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#vpcs).

1. List the available profiles for creating your instance template.

   ```sh
   ibmcloud is instance-profiles
   ```
   {: pre}

   For this example, you see a response similar to the following output.

   ```sh
   Name               vCPU Manufacturer   Architecture   Family              vCPUs   Memory(GiB)   Bandwidth(Mbps)   Volume bandwidth(Mbps)   GPUs   Storage(GB)
   bx2-2x8            intel               amd64          balanced            2       8             4000              1000                     -      -
   bx2d-2x8           intel               amd64          balanced            2       8             4000              1000                     -      1x75
   bx2-4x16           intel               amd64          balanced            4       16            8000              2000                     -      -
   bx2d-4x16          intel               amd64          balanced            4       16            8000              2000                     -     1x150
   bx2-8x32           intel               amd64          balanced            8       32            16000             4000                     -      -
   bx2d-8x32          intel               amd64          balanced            8       32            16000             4000                     -     1x300
   bx2-16x64          intel               amd64          balanced            16      64            32000             8000                     -      -
   bx2d-16x64         intel               amd64          balanced            16      64            32000             8000                     -      1x600
   ```
   {: screen}

1. List the subnets that are associated with the {{site.data.keyword.vpc_short}}.

   ```sh
   ibmcloud is subnets
   ```
   {: pre}

   For this example, you see a response similar to the following output.

   ```sh
   ID                                          Name                       Status      Subnet CIDR       Addresses   ACL                                   Public Gateway   VPC                     Zone         Resource group
   0717-cd4f74f1-bfae-423a-886b-9163256ec4a9   subnet-south-1             available   10.240.0.0/24     124/256     acl-test-1                            -                cdl                     us-south-1   Default
   0727-9447a2c3-efb8-44d2-a180-115531e0b0d3   cdl                        available   10.240.64.0/24    238/256     acl-test-1                            -                cdl                     us-south-2   Default
   ```
   {: screen}

   For the best performance of an instance group, ensure that you use a subnet size of 32 or greater.

   If you don't have a subnet available, you can create one by using the `ibmcloud is subnet-create` command. For more information about creating a subnet, see [IBM Cloud VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#subnets).

1. List the available images for creating your instance template.

   ```sh
   ibmcloud is images
   ```
   {: pre}

   For this example, you see a response similar to the following output.

   ```sh
   ID                                          Name                                                Status       Arch    OS name                              OS version                                               File size(GB)   Visibility   Owner type   Encryption   Resource group
   r006-3fa3bea4-7f9c-4eeb-8248-ab1f6e03185b   ibm-centos-7-9-minimal-amd64-9                      available    amd64   centos-7-amd64                       7.x - Minimal Install                                    1               public       provider     none         Default
   r006-c55167af-ea8c-4c27-871f-ccc9c868753e   ibm-centos-stream-8-amd64-1                         available    amd64   centos-stream-8-amd64                8                                                        2               public       provider     none         Default
    ```
   {: screen}

1. List all the available placement groups that you can associate with your instance.

    ```sh
    ibmcloud is placement-groups
    ```
    {: pre}

    For this example, you see a response similar to the following output.

    ```sh
    Listing placement groups for generation 2 compute in all resource groups and region us-east under account vpcdemo as user    yaohaif@cn.ibm.com...
    ID                                            Name                             State    Strategy       Resource Group
    c5f1f366-b92a-4080-991a-aa5c2e33d96b          placement-group-region-us-east   stable   power_spread       5018a8564e8120570150b0764d39ebcc
    placement-group-cccc-cccc-cccc-cccccccccccc   vsi-placementGroup1              stable   host_spread    5018a8564e8120570150b0764d39ebcc
    placement-group-bbbb-bbbb-bbbb-bbbbbbbbbbbb   vsi-placementGroup2              stable   power_spread     5018a8564e8120570150b0764d39ebcc
    placement-group-aaaa-aaaa-aaaa-aaaaaaaaaaaa   vsi-placementGroup3              stable   power_spread   1d18e482b282409e80eff354c919c6a2
    ```
    {: screen}

After you know these values, use them to run the `instance-template-create` command. In addition to the information that you gathered, you must specify a name for the instance.

```sh
ibmcloud is instance-template-create INSTANCE_TEMPLATE_NAME VPC ZONE_NAME PROFILE_NAME SUBNET --image IMAGE
```
{: pre}

For example, if you create an instance template that is called _my-instance-template_ in _us-south-3_ and use the _bx2-2x8_ profile, your `instance-template-create` command would look similar to the following sample.

```sh
ibmcloud is instance-template-create my-instance-template 0738-xxx1xx23-4xx5-6789-12x3-456xx7xx123x us-south-3 bx2-2x8 0076-2249dabc-8c71-4a54-bxy7-953701ca3999 --image r008-54e9238a-feaa-4f90-9742-7424cb2b9ff1 --placement-group r134-953db18c-068c-4a11-9b07-645684b444b2
```
{: pre}

Where

- `INSTANCE_TEMPLATE_NAME` is _my-instance-template_
- `VPC` is _0738-xxx1xx23-4xx5-6789-12x3-456xx7xx123x_
- `ZONE_NAME` is _us-south-3_
- `PROFILE_NAME` is _bx2-2x8_
- `SUBNET_ID` is _0076-2249dabc-8c71-4a54-bxy7-953701ca3999_
- `IMAGE` is _r008-54e9238a-feaa-4f90-9742-7424cb2b9ff1_
- `PLACEMENT_GROUP_ID` is _r134-953db18c-068c-4a11-9b07-645684b444b2

For this example, you see a response similar to the following output.

The following response varies depending on what values that you use.
{: note}

```sh
ID                          0727-0fdac0fb-ad59-4d7b-9f03-604ddc1db002
Name                        my-instance-template
CRN                         crn:v1:bluemix:public:is:us-south-2:a/7f75c7b025e54bc5635f754b2f888665::instance-template:0727-0fdac0fb-ad59-4d7b-9f03-604ddc1db002
Resource group              Default
Image                       r006-3fa3bea4-7f9c-4eeb-8248-ab1f6e03185b
VPC                         r006-212def4b-93f0-4b9a-a331-94178b99d474
Zone                        us-south-2
Profile                     bx2-2x8
Boot volume                 Name   Capacity   Profile           IOPS   Attachment name                      Auto delete   Tags
                            -      100        general-purpose   0      illude-education-spotted-reanalyze   true          -

Primary Network Interface   Name      Subnet ID                                   Security Groups                             Allow source IP spoofing   Reserved IP Address   Reserved IP ID   Reserved IP Name   Reserved IP Auto Delete
                            primary   0727-944126da-e46d-4104-8ba4-ab5a5832864b   r006-2295f00e-8af0-4736-be11-94677955b6b2   false                      -                     -                -                  -

Placement                   ID
                            r006-9994e3ab-18ae-49a7-95cf-25c77e09fa76

Created                     2023-03-29T22:02:59+05:30
```
{: screen}

For more examples of the `ibmcloud is instance-template-create` command, see the [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#instance-template-create).

When you create an instance template, validation steps are performed that ensure you can use your template to provision a virtual server instance. Need more help? You can always run `ibmcloud is help instance-template-create` to display help for creating an instance template.
{: tip}

### Next steps
{: #next-steps-instance-template}

With your instance template created, you can provision a single virtual server instance, or you can provision multiple instances at the same time as part of an instance group. Optionally, you can set auto scaling policies for your instance group to dynamically add or remove virtual server instances from your group. For more information, see the following topics:

- [Bulk provisioning instances with instance groups](/docs/vpc?topic=vpc-bulk-provisioning)
- [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group)
- [Managing an instance template](/docs/vpc?topic=vpc-managing-instance-template)
