---

copyright:
  years: 2020, 2021 
lastupdated: "2021-06-24"

keywords: create instance template, vsis, virtual server instance

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:preview: .preview}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Creating an instance template
{: #create-instance-template}

You can create an instance template to define instance details for provisioning one or more virtual servers. When your 
instance template is created, you can use it to provision single virtual server instances, or you can provision multiple instances 
at the same time as part of an instance group. 

## Creating an instance template with the UI
{: #instance-template-ui}
{: ui}

The instance template defines the details of the virtual server instances that are created from the template. For example, specify the profile (vCPU and memory), image, attached volumes, and network interfaces for the image template. 

To create an instance template, complete the following steps.
1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Auto scale > Instance templates**. 
2. On the Instance templates for VPC page click **Create** and enter the required information. For more information about specific fields, see [Creating an instance template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#creating-instance-template). 
3. Click **Create instance template** when the information is complete.

When you create an instance template, validation steps are performed that ensure you can use your template to provision a 
virtual server instance. 
{: tip}

## Creating an instance template with the CLI
{: #solo-instance-template-cli}
{: cli}

You can create one or more instance templates in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

### Before you begin
{: #before-instance-cli}

Make sure that you completed set up for your [{{site.data.keyword.cloud}} CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup) 
and your [{{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli).

### Gathering information to create an instance template
{: #cli-options-instance-template-create}

Ready to create an instance template? Before you can run the `ibmcloud is instance-template-create` command, you need to know 
the details you want to include for your instance template and command options, such as what profile and image you want to use. Follow these steps to prepare for running the command.

Gather the following required instance template details.

|    Instance template details   |       Listing command           |
| ----------------------------- | -------------------------------- |
| VPC | `ibmcloud is vpcs` |
| Zone | `ibmcloud is zones` |
| Profile | `ibmcloud is instance-profiles` |
| Subnet | `ibmcloud is subnets` |
| Image | `ibmcloud is images` |
| Placement groups | `ibmcloud is placement-groups`   |
{: caption="Table 1. Required instance template details" caption-side="top"}

Use the following commands to determine the required information for creating a new instance template.

1. List the {{site.data.keyword.vpc_short}}s that are associated with your account.
   ```
   ibmcloud is vpcs
   ```
   {:pre}

   For this example, you'd see a response similar to the following output:
   ```
   ID                                          Name                                  Default          Status      Tags
   0738-xxx1xx23-4xx5-6789-12x3-456xx7xx123x   my-vpc                                yes              available   -
   0738-xxxx1234-5678-9x12-x34x-567x8912x3xx   my-other-vpc                          no               available   -
   ```
   {:screen}

   If you don't have one available, you can create an {{site.data.keyword.vpc_short}} by using the `ibmcloud is vpc-create` 
   command. For more information about creating an {{site.data.keyword.vpc_short}}, see 
   [IBM Cloud VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#vpcs).

2. List the regions associated with your account.
   ```
   ibmcloud is regions
   ```
   {:pre}

   For this example, you'd see a response similar to the following output:
   ```
   Name       Endpoint               Status   
   us-south   /v1/regions/us-south   available
   ```
   {:screen}

3. List the zones associated with the region.
   ```
   ibmcloud is zones us-south
   ```
   {:pre}

   For this example, you'd see a response similar to the following output:
   ```
   Name         Region     Status   
   us-south-1   us-south   available   
   us-south-3   us-south   available   
   ```
   {:screen}

4. List the available profiles for creating your instance template.
   ```
   ibmcloud is instance-profiles
   ```
   {:pre}

   For this example, you'd see a response similar to the following output:
   ```
   Name           Architecture   Family     vCPUs   Memory(G)   Network Performance (Gbps)   GPUs   
   bx2-2x8        amd64          balanced   2       8           4                            -   
   bx2-4x16       amd64          balanced   4       16          8                            -   
   bx2-8x32       amd64          balanced   8       32          16                           -   
   bx2-16x64      amd64          balanced   16      64          32                           -   
   bx2-32x128     amd64          balanced   32      128         64                           -   
   bx2-48x192     amd64          balanced   48      192         80                           -   
   cx2-2x4        amd64          compute    2       4           4                            -   
   cx2-4x8        amd64          compute    4       8           8                            -   
   cx2-8x16       amd64          compute    8       16          16                           -   
   cx2-16x32      amd64          compute    16      32          32                           -   
   cx2-32x64      amd64          compute    32      64          64                           -   
   mx2-2x16       amd64          memory     2       16          4                            -   
   mx2-4x32       amd64          memory     4       32          8                            -   
   mx2-8x64       amd64          memory     8       64          16                           -  
   ```
   {:screen}

5. List the subnets that are associated with the {{site.data.keyword.vpc_short}}.
   ```
   ibmcloud is subnets
   ```
   {:pre}

   For this example, you'd see a response similar to the following output:
   ```
   ID                                          Name                     Status
   0076-2249dabc-8c71-4a54-bxy7-953701ca3999   subnet1                  available
   0767-173bn4aa-060b-47e7-am45-b3395a593897   subnet2                  available
   ```
   {:screen}
   
   For the best performance of an instance group, ensure that you use a subnet size of 32 or greater.

   If you don't have a subnet available, you can create one by using the `ibmcloud is subnet-create` command. For more 
   information about creating a subnet, see 
   [IBM Cloud VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#subnets).

6. List the available images for creating your instance template.
   ```
   ibmcloud is images   
   ```
   {:pre}

   For this example, you'd see a response similar to the following output:
   ```
   ID                                          Name                                               Status       Arch
   r007-60d279a0-b328-40eb-a379-595ca53bff89   ibm-redhat-7-6-amd64-sap-hana-1                    available    amd64
   r008-54e9238a-feaa-4f90-9742-7424cb2b9ff1   ibm-windows-server-2016-full-standard-amd64-3      available    amd64
    ```
   {:screen}
   
7. List all the available placement groups that you can associate with your instance.
    ```
    ibmcloud is placement-groups
    ```
    {:pre}

    For this example, you'd see a response similar to the following output.
    
    ```
    Listing placement groups for generation 2 compute in all resource groups and region us-east under account vpcdemo as user    yaohaif@cn.ibm.com...
    ID                                            Name                             State    Strategy       Resource Group   
    c5f1f366-b92a-4080-991a-aa5c2e33d96b          placement-group-region-us-east   stable   power_spread       5018a8564e8120570150b0764d39ebcc   
    placement-group-cccc-cccc-cccc-cccccccccccc   vsi-placementGroup1              stable   host_spread    5018a8564e8120570150b0764d39ebcc   
    placement-group-bbbb-bbbb-bbbb-bbbbbbbbbbbb   vsi-placementGroup2              stable   power_spread     5018a8564e8120570150b0764d39ebcc   
    placement-group-aaaa-aaaa-aaaa-aaaaaaaaaaaa   vsi-placementGroup3              stable   power_spread   1d18e482b282409e80eff354c919c6a2
    ```
 {: pre}

For example, if you create an instance template that is called _my-instance-template_ in _us-south-3_ and use the 
_bx2-2x8_ profile, your `instance-template-create` command would look similar to the following sample.

```
ibmcloud is instance-template-create my-instance-template 0738-xxx1xx23-4xx5-6789-12x3-456xx7xx123x us-south-3 bx2-2x8 0076-2249dabc-8c71-4a54-bxy7-953701ca3999 --image-id r008-54e9238a-feaa-4f90-9742-7424cb2b9ff1 --placement-group r134-953db18c-068c-4a11-9b07-645684b444b2
```
{: pre}

Where:
   - `INSTANCE_TEMPLATE_NAME` is _my-instance-template_
   - `VPC` is _0738-xxx1xx23-4xx5-6789-12x3-456xx7xx123x_
   - `ZONE_NAME` is _us-south-3_
   - `PROFILE_NAME` is _bx2-2x8_
   - `SUBNET_ID` is _0076-2249dabc-8c71-4a54-bxy7-953701ca3999_
   - `IMAGE_ID` is _r008-54e9238a-feaa-4f90-9742-7424cb2b9ff1_
   - `PLACEMENT_GROUP_ID` is _r134-953db18c-068c-4a11-9b07-645684b444b2
   
For this example, you'd see a response similar to the following output 

The following response varies depending on what values you use.
{: note}

```
ID                             0738-c3809e5b-8d48-4629-b258-33d5b14fa84f   
Name                           my-instance-template   
CRN                            crn:v1:staging:public:is:us-south-3:a/2d1bace7b46e4815a81e52c6ffeba5cf::instance-template:0738-c3809e5b-8d48-4629-b258-33d5b14fa84f   
Resource group                 Default   
VPC ID                         0738-xxx1xx23-4xx5-6789-12x3-456xx7xx123x   
Image ID                       r008-54e9238a-feaa-4f90-9742-7424cb2b9ff1   
Profile                        bx2-2x8   
Primary Network Interface ID   Name      Subnet ID                                   Security Groups      
                               primary   0076-2249dabc-8c71-4a54-bxy7-953701ca3999   r134-9fd0b586-6876-4e8a-a0a1-586aeff5167c
Placement         ID                                          Name    Resource type      
                  r134-953db18c-068c-4a11-9b07-645684b444b2   mypg1   placement_group 
```
{:screen}


For more examples of the `ibmcloud is instance-template-create` command, see the [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#instance-template-create).

When you create an instance template, validation steps are performed that ensure you can use your template to provision a 
virtual server instance. Need more help? You can always run `ibmcloud is help instance-template-create` to display help for 
creating an instance template.
{: tip}

### Next steps 
{: #next-steps-instance-template}

With your instance template created, you can provision a single virtual server instance, or you can provision multiple instances at the same time as part of an instance group. Optionally, you can set auto scaling policies for your instance group to dynamically add or remove virtual server instances from your group. For more information, see the following topics:

* [Bulk provisioning instances with instance groups](/docs/vpc?topic=vpc-bulk-provisioning)
* [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group)
* [Managing an instance template](/docs/vpc?topic=vpc-managing-instance-template)
