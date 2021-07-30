---

copyright:
  years: 2018, 2021
lastupdated: "2021-07-30"

keywords: instances, virtual servers, creating virtual servers, virtual server instances, virtual machines, Virtual Servers for VPC, compute, vsi, vpc, creating, CLI, command line interface, generation 2, gen 2

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:note: .note}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# Creating virtual server instances by using the CLI
{: #creating-virtual-servers-cli}

You can create instances by using the command-line interface (CLI).
{:shortdesc}

{{site.data.keyword.cloud_notm}} CLI is not supported on LinuxONE (s390x processor architecture). However, you can install the CLI on another supported platform and use it with LinuxONE (s390x processor architecture) virtual server instances.
{:note}

## Before you begin
{: #vefore-creating-virtual-servers-cli}

1. Ensure that you downloaded, installed, and initialized the following CLI plug-ins:
    * {{site.data.keyword.cloud_notm}} CLI
    * The infrastructure-service plug-in
    For more information, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

2. Make sure that you already [created a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).

## Gathering information to create an instance by using the CLI
{: #gather-info-to-create-virtual-servers-cli}

Ready to create an instance? Before you can run the `ibmcloud is instances` command, you need to know the details about the instance, such as what profile or image you want to use.

Gather the following information:

|    Instance details   |  Listing options                |
| --------------------- | --------------------------------|
| Image                 | `ibmcloud is images`            |
| Profile               | `ibmcloud is instance-profiles` |
| Key                   | `ibmcloud is keys`              |
| VPC                   | `ibmcloud is vpcs`              |
| Subnet                | `ibmcloud is subnets`           |
| Zone                  | `ibmcloud is zones`             |   
| Placement groups      | `ibmcloud is placement-groups`  |
{: caption="Table 1. Required instance details" caption-side="top"}   

Use the following commands to determine the required information for creating a new instance.

1. List the regions associated with your account.
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

2. List the zones associated with the region.
   ```
   ibmcloud is zones us-south
   ```
   {:pre}

   For this example, you'd see a response similar to the following output:
   ```
   Name         Region     Status   
   us-south-1   us-south   available
   ```
   {:screen}

3. List the {{site.data.keyword.vpc_short}}s that are associated with your account.
   ```
   ibmcloud is vpcs
   ```
   {:pre}

   For this example, you'd see a response similar to the following output:
   ```
   ID                                          Name                                  Default   Created       Status      Tags   
   0738-xxx1xx23-4xx5-6789-12x3-456xx7xx123x   my-vpc                                yes       1 month ago   available   -   
   0738-xxxx1234-5678-9x12-x34x-567x8912x3xx   my-other-vpc                          no        4 days ago    available   -   
   ```
   {:screen}

   If you do not have one available, you can create an {{site.data.keyword.vpc_short}} by using the `ibmcloud is vpc-create` command. For more information about creating an {{site.data.keyword.vpc_short}}, see [IBM Cloud VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#vpcs).

4. List the subnets that are associated with the {{site.data.keyword.vpc_short}}.
   ```
   ibmcloud is subnets
   ```
   {:pre}

   For this example, you'd see a response similar to the following output:
   ```
   ID                                          Name                                     IPv*   Subnet CIDR         Addresses   Gen   Gateway   Created   Status      VPC                    Zone         Resource Group   Tags   
   0738-1234x12x-345x-1x23-45x6-x7x891011x1x   my-subnet                                ipv4   172.16.1.0/24       0/0         -     -         1 week ago    available   my-vpc(xxx1xx23-.)     us-south-1   -                -   
   0738-12xx345x-6789-1xxx-x2x3-x4x56xx78x9x   my-subnet-2                              ipv4   172.20.28.0/24      0/0         -     -         1 day ago     available   my-vpc(xxx1xx23-.)     us-south-1   -                -   
   0738-1x2x3xx4-xx56-7891-234x-xx5678x9x123   my-other-subnet                          ipv4   192.168.88.0/24     0/0         -     -         1 day ago     available   my-other-vpc(xxx1234-.)us-south-1   -                -   
   ```
   {:screen}

   If you do not have one available, you can create a subnet by using the `ibmcloud is subnet-create` command. For more information about creating a subnet, see [IBM Cloud VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#subnets).

5. List the available profiles for creating your instance.
   ```
   ibmcloud is instance-profiles
   ```
   {:pre}

   For this example, you'd see a response similar to the following output:
   ```
   Name             Architecture   Family              vCPUs   Memory(GiB)   Network(Gbps)   GPUs   Storage(GB)   
   bx2-2x8          amd64          balanced            2       8             4               -      -
   bx2d-2x8         amd64          balanced            2       8             4               -      1x75
   bx2-4x16         amd64          balanced            4       16            8               -      -
   cx2-16x32        amd64          compute             16      32            32              -      -
   mx2-4x32         amd64          memory              4       32            8               -      -   
   mx2d-4x32        amd64          memory              4       32            8               -      1x150
   gx2-16x128x1v00  amd64          gpu-v100            16      128           32              1      -
   gx2-16x128x2v00  amd64          gpu-v100            16      128           32              2      -     
   ```
   {:screen}

6. List the available images for creating your instance.
   ```
   ibmcloud is images   
   ```
   {:pre}

   For this example, you'd see a response similar to the following output:
   ```
   ID                                          Name                 OS                                                       Arch    Created         Status   Visibility   Tags   
   0738-cc8debe0-1b30-6e37-2e13-744bfb2a0c11   centos-7.x-amd64     CentOS (7.x - Minimal Install)                           amd64      9 hours ago     READY    public       -   
   0738-7eb4e35b-4257-56f8-d7da-326d85452591   ubuntu-16.04-amd64   Ubuntu Linux (16.04 LTS Xenial Xerus Minimal Install)    amd64      9 hours ago     READY    public       -   
   ```
   {:screen}

7. List the available SSH keys that you can associate with your instance.
   ```
   ibmcloud is keys
   ```
   {:pre}

   For this example, you'd see a response similar to the following output:
   ```
   ID                                          Name           Type   Length   FingerPrint          Created        Resource Group   Tags
   0738-1234xxxx-x12x-xxxx-34xx-xx1234xxxxxx   my-key         RSA    2048     PHcP/zyw/PNGIe/u..   5 days ago     -                -   
   0738-12xx3456-x78x-9123-4x56-78xx9xxx1x2x   my-other-key   RSA    2048     +rvkRMBhdFmz1dlT..   2 days ago     -                -    
   ```
   {:screen}

   If you do not have one available, you can create an SSH key by using the `ibmcloud is key-create` command. For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys).

8. List all the available placement groups that you can associate with your instance.
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

## Creating an instance by using the CLI
{: #create-instance-cli}

After you know these values, use them to run the `instance-create` command. In addition to the information that you gathered, you must specify a name for the instance.

1. Create an instance.
   ```
   ibmcloud is instance-create \
       <INSTANCE_NAME> \
       <VPC_ID> \
       <ZONE_NAME> \
       <PROFILE_ID> \
       <SUBNET_ID> \
       --image-id <IMAGE_ID> \
       --key-ids <KEY_IDS>
       --volume-attach <VOLUME_ATTACH_JSON or JSON file>
       --placement-group <PLACEMENT_GROUP_NAME>
   ```
   {:pre}

   For example, if you create an instance that is called _my-instance_ in _us-south-1_ and use the _b-2x4_ profile, your `instance-create` command would look similar to the following sample:

   ```
   ibmcloud is instance-create \
       my-instance \
       0738-xxx1xx23-4xx5-6789-12x3-456xx7xx123x \
       us-south-1 \
       b-2x4 \
       0738-1234x12x-345x-1x23-45x6-x7x891011x1x \
       --image-id 1xx2x34x-5678-12x3-x4xx-567x81234567 \
       --key-ids 1234xxxx-x12x-xxxx-34xx-xx1234xxxxxx
       --volume-attach @/Users/myname/myvolume-attachment_create.json
       --placement-group r134-a812ff17-cac5-4e20-8d2b-95b587be6637
   ```
   {:pre}

   Where:
   - `INSTANCE_NAME` is _my-instance_
   - `VPC_ID` is _VPC_ID_
   - `ZONE_NAME` is _us-south-1_
   - `PROFILE_ID` is _b-2x4_
   - `SUBNET_ID` is _SUBNET_ID_
   - `IMAGE_ID` is _IMAGE_ID_
   - `KEY_IDS` is _KEY_ID1, KEY_ID2, ..._
   - `VOLUME_ATTACH_JSON` is the volume attachment specification in JSON format, provided in the command or as a file. For an example volume attachment JSON file, see [Create a volume attachment JSON](/docs/vpc?topic=vpc-attaching-block-storage&interface=cli#volume_attachment_json).
   - `PLACEMENT_GROUP_ID` is _r134-a812ff17-cac5-4e20-8d2b-95b587be6637

   For this example, you'd see the following responses. **Note:** The following response varies depending on what optional values you use.
   ```
   ID                           0738-2x12xxx5-xx11-1234-x4x5-1xxx12345678
   Name                         my-instance
   Status                       pending
   Profile                      b-2x4
   Architecture                 amd64
   vCPUs                        2
   Memory                       4
   Network Performance (Gbps)   4
   Image                        ID                                          Name
                                0738-7eb4e35b-4257-56f8-d7da-326d85452591   ubuntu-16.04-amd64

   VPC                          ID                                          Name
                                xxx1xx23-4xx5-6789-12x3-456xx7xx123x        my-vpc

   Zone                         us-south-1
   Resource group               Default
   Created                      2020-08-24T20:25:50+08:00
   Boot volume                  ID   Name           Attachment ID                               Attachment name
                                -    PROVISIONING   0736-76436447-3262-4b3d-8b42-aa3fbb5927f6   volume-attachment
   Data volumes                 ID                                          Name        Attachment ID                               Attachment name
                                r134-dd9cefce-8f3e-4a63-b92a-066c8505e25c   my-volume   0738-xx55xx44-3xx1-1234-42x1-234xx5x678xx   my-volume-attachment
   Placement            ID                                          Name       Resource type      
                     r134-a812ff17-cac5-4e20-8d2b-95b587be6637   mypg2-re   placement_group  
   ```
   {:screen}

   Information about the network interfaces that are created for the new instance is not returned after the instance is created. You can view it using the command provided in **Step 2**.
   {: note}

   The status displays pending until the instance is created.
   {: tip}

2. When the status changes to *running*, verify that you can see your new instance and view the network interfaces that were created for your new instance.

   ```
   ibmcloud is instance 2x12xxx5-xx11-1234-x4x5-1xxx12345678
   ```
   {:pre}

   For this example, you'd see the following responses.
   ```
   ID                           0738-2x12xxx5-xx11-1234-x4x5-1xxx12345678
   Name                         my-instance
   Status                       running
   Profile                      b-2x4
   Architecture                 amd64
   vCPUs                        2
   Memory                       4
   Network Performance (Gbps)   4
   Image                        ID                                          Name
                                0738-7eb4e35b-4257-56f8-d7da-326d85452591   ubuntu-16.04-amd64

   VPC                          ID                                          Name
                                xxx1xx23-4xx5-6789-12x3-456xx7xx123x        my-vpc

   Zone                         us-south-1
   Resource group               Default
   Created                      2020-08-24T20:25:50+08:00
   Network Interfaces           Interface   Name      ID                                          Subnet      Subnet ID                                       Private IP     Floating IP   Security Groups
                                Primary     primary   0738-xx12x345-6xxx-7x89-123x-4x5xxx678x9x   my-subnet   0736-a50f4dbb-d374-492f-a8bf-877b4cbd8921   10.240.128.5   -             ajar-arrive-urging-daylong    
   Boot volume                  ID   Name           Attachment ID                               Attachment name
                                -    PROVISIONING   0736-76436447-3262-4b3d-8b42-aa3fbb5927f6   volume-attachment
   Data volumes                 ID                                          Name        Attachment ID                    Attachment name
                                r134-dd9cefce-8f3e-4a63-b92a-066c8505e25c   my-volume   0738-xx55xx44-3xx1-1234-42x1-234xx5x678xx my-volume-attachment
   ```
   {:screen}

3. Request a floating IP address to associate to your instance.

   ```
   ibmcloud is floating-ip-reserve \
       my-floatingip \
       --nic-id xx12x345-6xxx-7x89-123x-4x5xxx678x9x
   ```
   {:pre}

   For this example, you'd see the following responses.
   ```
     Address          xxx.xxx.xxx.xxx   
     Name             my-floatingip   
     Target           great-scott-stride-lilac-captivate-filtrate(xx12x345-.)   
     Target Type      intf   
     Target IP        172.16.1.4   
     Created          1 second ago   
     Status           available   
     Zone             us-south-1   
     Resource Group   -   
     Tags             -  
    ```
    {:screen}

    Remember the floating IP `Address` for later.  

Need more help? You can always run `ibmcloud is instance-create --help` to display help for creating an instance.
{: tip}

Do you prefer to create an instance by using the {{site.data.keyword.cloud_notm}} console? For more information, see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).
{: tip}

## Next steps
{: #next-step-after-creating-virtual-servers-cli}
A series of emails is sent to your administrator: Acknowledgment of the virtual server instance order, order approval and processing, and a message that the instance is created.

If you Choose a GPU profile, see [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus).

After the instance is created, associate a floating IP address to the instance. Then, you can connect to your instance. For more information, see [Connecting to your Linux instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux) or [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows).
