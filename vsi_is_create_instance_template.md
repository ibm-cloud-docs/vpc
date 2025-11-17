---

copyright:
  years: 2020, 2025
lastupdated: "2025-11-17"

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

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation menu** icon![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Instance templates**.
2. Click **New instance template** and enter the information that is in Table 1.
3. Click **Create instance template** when the information is complete.

When you create an instance template, validation steps are performed to make sure that you can use your template to provision a virtual server instance.
{: tip}


{{site.data.content.create-instance-template-table}}

## Creating an instance template with the CLI
{: #solo-instance-template-cli}
{: cli}

The instance template defines the details of the virtual server instances that are created from the template. For example, specify the profile (vCPU and memory), image, attached volumes, and network interfaces for the instance template. You can create one or more instance templates in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

For more information about creating a virtual server instance with a custom image shared from a private catalog, see [Provision from a private catalog image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#instance-create-from-private-catalog-image-cli).

### Before you begin
{: #before-instance-cli}

Make sure that you set up your [{{site.data.keyword.cloud}} CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup)
and your [{{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli).


### Gathering information to create an instance template
{: #cli-options-instance-template-create}



Ready to create an instance template? Before you can run the `ibmcloud is instance-template-create` command, you must gather the details that you need to include for your instance template and command options. For example, you need the ID or name of the VPC, and the name of the zone where the instance template is to be created. You also need the name of the instance profile that you want to use, and the ID or name of the subnet where the template is to be created.

Gather the following required instance template details.

|    Instance template details   |       Listing command           | VPC CLI reference documentation |
| ----------------------------- | -------------------------------- |----------------------------------|
| VPC | `ibmcloud is vpcs` | [List all VPCs](/docs/vpc?topic=vpc-vpc-reference#vpcs-list) |
| Zone | `ibmcloud is zones` | [List all zones in the region](/docs/vpc?topic=vpc-vpc-reference#zones-list) |
| Profile | `ibmcloud is instances` | [List all virtual server instances](/docs/vpc?topic=vpc-vpc-reference#instances-list) |
| Subnet | `ibmcloud is subnets` | [List all subnets](/docs/vpc?topic=vpc-vpc-reference#subnets-list) |
| Image | `ibmcloud is image` | [List all images](/docs/vpc?topic=vpc-vpc-reference#images-list)|
| Keys | `ibmcloud is keys` | [List all keys](/docs/vpc?topic=vpc-vpc-reference#keys) |
| Placement groups | `ibmcloud is placement-groups` | [List all placement groups](/docs/vpc?topic=vpc-vpc-reference#placement-groups-list) |
{: caption="Required instance template details" caption-side="bottom"}

Use the following commands to determine the required information for creating an instance template.

1. List the regions that are associated with your account.

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

1. List the zones that are associated with the target region.

   ```sh
   ibmcloud is zones
   ```
   {: pre}

   In the following example, the command is run in the `us-south` region and the output shows the available zones that are in the region.

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

   If you don't have one available, you can create an {{site.data.keyword.vpc_short}} by using the `ibmcloud is vpc-create` command. For more information about creating an {{site.data.keyword.vpc_short}}, see [IBM Cloud VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference#vpcs-cli-ref).

1. List the available profiles for creating your instance template.

   ```sh
   ibmcloud is instance-profiles
   ```
   {: pre}

   The command output is similar to the following example.

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

   The command output is similar to the following example.

   ```sh
   ID                                          Name                       Status      Subnet CIDR       Addresses   ACL                                   Public Gateway   VPC                     Zone         Resource group
   0717-cd4f74f1-bfae-423a-886b-9163256ec4a9   subnet-south-1             available   10.240.0.0/24     124/256     acl-test-1                            -                cdl                     us-south-1   Default
   0727-9447a2c3-efb8-44d2-a180-115531e0b0d3   cdl                        available   10.240.64.0/24    238/256     acl-test-1                            -                cdl                     us-south-2   Default
   ```
   {: screen}

   For the best performance of an instance group, make sure that you use a subnet size of 32 or greater.

   If you don't have a subnet available, you can create one by using the `ibmcloud is subnet-create` command. For more information about creating a subnet, see the [IBM Cloud VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference#subnet-cli-ref).

1. List the available images for creating your instance template.

   ```sh
   ibmcloud is images
   ```
   {: pre}

   The command output is similar to the following example.

   ```sh
   ID                                          Name                                                Status       Arch    OS name                              OS version                                               File size(GB)   Visibility   Encryption   Resource group   Catalog Offering User Data Format   Remote Account ID
   r006-3fa3bea4-7f9c-4eeb-8248-ab1f6e03185b   my-test-image                                       available    amd64   centos-7-amd64                       7.x - Minimal Install                                    1               private      none         Default          -                cloud_init
   r006-c55167af-ea8c-4c27-871f-ccc9c868753e   ibm-centos-stream-8-amd64-1                         available    amd64   centos-stream-8-amd64                8                                          2               public       none         Default          -                cloud_init       811f8abfbd32425597dc7ba40da98fa6
    ```
   {: screen}

1. List the available SSH keys that you can associate with your instance.

   ```sh
   ibmcloud is keys
   ```
   {: pre}

   The command output is similar to the following example.

   ```text
   ID                                          Name     Type   Length   FingerPrint          Resource group
   r006-89ec781c-9630-4f76-b9c4-a7d204828d61   my-key   rsa    4096     gtnf+pdX2PYI9Ofq..   Default
   ```
   {: screen}

   If you do not have an SSH key available, you can create an SSH key by using the [ibmcloud is key-create](/docs/vpc?topic=vpc-vpc-reference#key-create) command.

   RSA and ED25519 are the two types of SSH keys that you can use. However, you can't use the ED25519 SSH key type with Windows or VMware images. You can use only RSA SSH keys for these images. For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys).
   {: note}

1. List all the available placement groups that you can associate with your instance.

   ```sh
   ibmcloud is placement-groups
   ```
   {: pre}

   The command output is similar to the following example.

   ```sh
   Listing placement groups for generation 2 compute in all resource groups and region us-east under account vpcdemo as user    yaohaif@cn.ibm.com...
   ID                                            Name                             State    Strategy       Resource Group
   c5f1f366-b92a-4080-991a-aa5c2e33d96b          placement-group-region-us-east   stable   power_spread       5018a8564e8120570150b0764d39ebcc
   placement-group-cccc-cccc-cccc-cccccccccccc   vsi-placementGroup1              stable   host_spread    5018a8564e8120570150b0764d39ebcc
   placement-group-bbbb-bbbb-bbbb-bbbbbbbbbbbb   vsi-placementGroup2              stable   power_spread     5018a8564e8120570150b0764d39ebcc
   placement-group-aaaa-aaaa-aaaa-aaaaaaaaaaaa   vsi-placementGroup3              stable   power_spread   1d18e482b282409e80eff354c919c6a2
   ```
   {: screen}

### Creating an instance template from the CLI
{: #creating-instance-template-example-cli}

After you know these values, use them to run the `instance-template-create` command. In addition to the information that you gathered, you must specify a name for the instance.

```sh
ibmcloud is instance-template-create INSTANCE_TEMPLATE_NAME VPC ZONE_NAME PROFILE_NAME SUBNET --image IMAGE
```
{: pre}

The following example command creates an instance template that is named _my-instance-template_ in the VPC that is specified by the ID _r006-6e8fb140-5668-45b8-b98a-d5cb0e0bf39b_. The template is created in the _us-south-3_ ZONE and uses the _bx2-2x8_ PROFILE. The SUBNET is defined by its ID _0726-298acd6c-e71e-4204-a04f-fe4a4dd89805_. The IMAGE is specified by its ID _r006-534ef2ac-6158-45b3-9657-57629fa85305_.

```sh
ibmcloud is instance-template-create my-instance-template r006-6e8fb140-5668-45b8-b98a-d5cb0e0bf39b us-south-3 bx2-2x8 0076-2249dabc-8c71-4a54-bxy7-953701ca3999 --image r006-534ef2ac-6158-45b3-9657-57629fa85305 --placement-group r006-953db18c-068c-4a11-9b07-645684b444b2
Creating instance template my-instance-template under account Test Account as user test-user@ibm.com...

ID                          0727-0fdac0fb-ad59-4d7b-9f03-604ddc1db002
Name                        my-instance-template
CRN                         crn:v1:bluemix:public:is:us-south-2:a/a1234567::instance-template:0727-0fdac0fb-ad59-4d7b-9f03-604ddc1db002
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


The following example creates an instance template that is named _my-gen3-instance-template_ in the VPC that is specified by the ID _r006-6e8fb140-5668-45b8-b98a-d5cb0e0bf39b_. The template is created in the us-south-3_ ZONE and uses the _cx3d-8x20_ PROFILE. The SUBNET is defined by its ID _00726-298acd6c-e71e-4204-a04f-fe4a4dd89805_. The IMAGE is specified by its ID _r006-534ef2ac-6158-45b3-9657-57629fa85305_. When the selected [compute profile](/docs/vpc?group=profile-details) supports pooled bandwidth allocation for data volumes, the option `--storage_qos_modes` can also be included.

```sh
ibmcloud is instance-template-create my-gen3-instance-template r006-6e8fb140-5668-45b8-b98a-d5cb0e0bf39b us-south-3 cx3d-8x20 00726-298acd6c-e71e-4204-a04f-fe4a4dd89805 --image r006-534ef2ac-6158-45b3-9657-57629fa85305 --storage_qos_modes pooled
```
{: pre}

For more examples of the `ibmcloud is instance-template-create` command, see the [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference#instance-template-create).

When you create an instance template, validation steps are performed to make sure that you can use your template to provision a virtual server instance. Need more help? You can always run `ibmcloud is help instance-template-create` to display help for creating an instance template.
{: tip}

## Creating an instance template with the API
{: #solo-instance-template-api}
{: api}

The instance template defines the details of the virtual server instances that are created from the template. For example, specify the profile (vCPU and memory), image, attached volumes, and network interfaces for the instance template. You can create one or more instance templates in your {{site.data.keyword.vpc_short}} by using the application programming interface (API).

For more information about creating a virtual server instance with a custom image shared from a private catalog, see [Provision from a private catalog image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api#create-instance-private-catalog-image-api).

### Before you begin
{: #before-instance-api}

Define variables for the IAM token, API endpoint, and API version. For instructions, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

Make sure that you have the required access. To call these methods, you must be assigned one or more IAM access roles that include the following actions, depending on any listed conditions. You can check your access by going to the **Users** page of [{{site.data.keyword.iamshort}} dashboard](https://cloud.ibm.com/iam/overview){: external}.

A good way to learn more about the API is to click **Get sample API call** on the provisioning pages in {{site.data.keyword.cloud_notm}} console. You can view the correct sequence of API requests and better understand actions and their dependencies.
{: tip}

### Gathering information to create an instance template
{: #api-options-instance-template-create}

Ready to create an instance template? Before you can make the `POST` request with the `/instance/templates` method, you need to know the details that you want to include for your instance template. For example, you need the ID or name of the VPC, and the name of the zone where the instance template is to be created. You also need the name of the instance profile that you want to use, and the ID or name of the subnet where the template is to be created.

Gather the following required instance template details by making the following API calls:

|    Instance details   |  Listing methods                | API spec documentation |
| --------------------- | ------------------------------- | ---------------------------------------------------------------------- |
| VPC                   | `GET /vpcs`                     | [List all VPCs](/apidocs/vpc/latest#list-vpcs) |
| Zone                  | `GET /regions/<region>/zones`   | [List all zones in a region](/apidocs/vpc/latest#list-region-zones) |
| Profile               | `GET /instance/profiles`        | [List all instance profiles](/apidocs/vpc/latest#list-instance-profiles) |
| Subnet                | `GET /subnets`                  | [List all subnets](/apidocs/vpc/latest#list-subnets) |
| Image                 | `GET /images`                   | [List all images](/apidocs/vpc/latest#list-images) |
| Key                   | `GET /keys`                     | [List all keys](/apidocs/vpc/latest#list-keys) |
| Placement groups      | `GET /placement_groups`         | [List all placement groups](/apidocs/vpc/latest#list-placement-groups) |
{: caption="Required API instance template details" caption-side="bottom"}

Use the following commands to determine the required information for creating a new instance template.

1. List the {{site.data.keyword.vpc_short}}s that are associated with your account.

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/vpcs?version=2023-07-13&generation=2" -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

   If you don't have one available, you can create an {{site.data.keyword.vpc_short}} by using the `POST /vpcs` method. For more information about creating an {{site.data.keyword.vpc_short}}, see [IBM Cloud API Docs - Create a VPC](/apidocs/vpc/latest#create-vpc).

1. List the regions associated with your account.

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/regions?version=2023-07-13&generation=2" -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

1. List the zones that are associated with the region.

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/regions/$region_name/zones?version=2023-07-13&generation=2" -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

1. List the available profiles for creating your instance template.

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/instance/profiles?version=2023-07-13&generation=2" -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

1. List the subnets that are associated with the {{site.data.keyword.vpc_short}}.

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/subnets?version=2023-07-13&generation=2" -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

   For the best performance of an instance group, make sure that you use a subnet size of 32 or greater.

   If you don't have a subnet available, you can create one by using the `POST /subnets` method. For more information about creating an {{site.data.keyword.vpc_short}}, see [IBM Cloud API Docs - Create a subnet](/apidocs/vpc/latest#create-subnet).

1. List the available images for creating your instance template.

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/images?version=2023-07-13&generation=2" -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

1. List the available SSH keys that you can associate with your instance.

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/keys?version=2023-07-13&generation=2" -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

   If you do not have an SSH key available, you can create an SSH key by using the [ibmcloud is key-create](/docs/vpc?topic=vpc-vpc-reference#key-create) command.

   If you don't have an SSH key available, you can create one by using the `POST /keys` method. For more information about creating an {{site.data.keyword.vpc_short}}, see [IBM Cloud API Docs - Create a key](/apidocs/vpc/latest#create-key).

   RSA and ED25519 are the two types of SSH keys that you can use. However, you can't use the ED25519 SSH key type with Windows or VMware images. You can use only RSA SSH keys for these images. For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys).
   {: note}

1. List all the available placement groups that you can associate with your instance.

    ```sh
    curl -X GET "$vpc_api_endpoint/v1/placement_groups?version=2023-07-13&generation=2" -H "Authorization: Bearer $iam_token"
    ```
    {: pre}

### Creating the instance template with the API
{: #creating-instance-template-example-api}

After you know these values, use them to make a `POST` request with the `/instance/templates` method. In addition to the information that you gathered, you must specify a name for the instance.

The following example creates an instance template that is named _my-instance-template_ in _us-south-1_ and uses the _bx2-2x8_ profile.

```sh
curl -X POST "$vpc_api_endpoint/v1/instance/templates?version=2023-07-13&generation=2"
  -H "Authorization: Bearer $iam_token"
  -d '{
    "zone": {"name": "us-south-1"},
    "resource_group": {"id": "db8e8d865a83e0aae03f25a492c5b39e"},
    "name": "my-instance-template",
    "vpc": {"id": "r006-6e8fb140-5668-45b8-b98a-d5cb0e0bf39b"},
    "user_data": "",
    "profile": {"name": "bx2-2x8"},
    "keys": [{"id": "r006-68f8333a-1169-42da-ba01-75268bac8362"}],
    "volume_attachments": [],
    "boot_volume_attachment": {
      "volume": {
        "name": "my-instance-template-boot-1720804230000",
        "capacity": 100,
        "profile": {"name": "general-purpose"}},
      "delete_volume_on_instance_delete": true
    },
    "metadata_service": {
      "enabled": true,
      "protocol": "https",
      "response_hop_limit": 1},
    "primary_network_attachment": {
      "name": "eth0",
      "virtual_network_interface": {
        "allow_ip_spoofing": false,
        "auto_delete": true,
        "enable_infrastructure_nat": true,
        "primary_ip": {"auto_delete": true,
        "subnet": {"id": "0726-298acd6c-e71e-4204-a04f-fe4a4dd89805"},
        "security_groups": [{"id": "r006-ccdd1f58-f0f2-4ea0-8774-a24bbe61b5d9"}],
        "protocol_state_filtering_mode": "auto"}
    },
    "network_attachments": [],
    "image": {"id": "r006-534ef2ac-6158-45b3-9657-57629fa85305"}}`
```
{: pre}

The following example creates an instance template that is named _my-gen3-instance-template_ in _us-south-3_ and uses the _cx3d-8x20_ profile. When the selected [compute profile](/docs/vpc?group=profile-details) supports pooled bandwidth allocation for data volumes, the property `--storage_qos_modes` can also be included.

```sh
curl -X POST "$vpc_api_endpoint/v1/instance/templates?version=2024-07-12&generation=2"
  -H "Authorization: Bearer $iam_token"
  -H "Content-Type: application/json" \
  -H "accept: application/json" \
  -d '{
  "zone": {"name": "us-south-3"},
  "resource_group": {"id": "db8e8d865a83e0aae03f25a492c5b39e"},
  "name": "my-gen3-instance-template",
  "vpc": {"id": "r006-6e8fb140-5668-45b8-b98a-d5cb0e0bf39b"},
  "user_data": "",
  "profile": {"name": "cx3d-8x20"},
  "keys": [{"id": "r006-68f8333a-1169-42da-ba01-75268bac8362"},
  "volume_attachments": [
    {"volume": {
        "name": "my-data-volume-1",
        "capacity": 3000,
        "profile": {"name": "5iops-tier"}},
      "delete_volume_on_instance_delete": false},
    {"volume": {
        "name": "my-data-volume-2",
        "capacity": 3000,
        "profile": {"name": "5iops-tier"}},
      "delete_volume_on_instance_delete": false},
    {"volume": {
        "name": "my-data-volume-3",
        "capacity": 3000,
        "profile": {"name": "5iops-tier"}},
      "delete_volume_on_instance_delete": false}
  ],
  "boot_volume_attachment": {
    "volume": {
      "name": "my-gen3-instance-template-boot-1720803726000",
      "capacity": 100,
      "profile": {"name": "general-purpose"}
    },
    "delete_volume_on_instance_delete": true
  },
  "metadata_service": {
    "enabled": false,
    "response_hop_limit": 1},
  "primary_network_attachment": {
    "name": "eth0",
    "virtual_network_interface": {
      "allow_ip_spoofing": false,
      "auto_delete": true,
      "enable_infrastructure_nat": true,
      "primary_ip": {"auto_delete": true},
      "subnet": {"id": "0726-298acd6c-e71e-4204-a04f-fe4a4dd89805"},
      "security_groups": [
        {"id": "r006-ccdd1f58-f0f2-4ea0-8774-a24bbe61b5d9"}],
      "protocol_state_filtering_mode": "auto"}
  },
  "network_attachments": [],
  "image": {"id": "r006-534ef2ac-6158-45b3-9657-57629fa85305"},
  "enable_secure_boot": false
  "storage_qos_modes": "pooled"
}'
```
{: pre}

For more examples of the `/instance/templates` method, see the [Create an instance template](/apidocs/vpc/latest#create-instance-template) API documentation.

## Creating an instance template with Terraform
{: #solo-instance-template-terraform}
{: terraform}

You can create one or more instance templates in your {{site.data.keyword.vpc_short}} by using Terraform.

For more information about creating a virtual server instance with a custom image shared from a private catalog, see [Creating an instance by using Terraform](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#terraform-create-private-catalog).

### Before you begin
{: #before-instance-terraform}

Make sure that you set up [Terraform for VPC](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest).

### Gathering information to create an instance template
{: #terraform-options-instance-template-create}

Ready to create an instance template? Before you include the [`ibm_is_instance_template`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance_template){: external} resource in your configuration, you need to know the details that you want to include in your instance template. For example, you need the ID or name of the VPC, and the name of the zone where the instance template is to be created. You also need the name of the instance profile that you want to use, and the ID or name of the subnet where the template is to be created.

Gather the following required Terraform instance template details.

|    Instance template details   |       Listing block           | VPC Terraform data source reference documentation |
| ----------------------------- | -------------------------------- |----------------------------------|
| Instance profile | `ibm_is_instance_profiles` | [ibm_is_instance_profiles](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_instance_profiles){: external} |
| Image | `ibm_is_images` | [ibm_is_images](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_images){: external}|
| VPC                   | `ibm_is_vpc` | [ibm_is_vpc](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_vpc){: external} |
| Subnet | `ibm_is_subnets` | [ibm_is_subnets](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_subnets){: external} |
| Zone                  | `ibm_is_zone` | [ibm_is_zone](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_zone){: external} |
| Keys | `ibm_is_ssh_keys` | [ibm_is_ssh_keys](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_ssh_keys){: external} |
| Placement groups | `ibm_is_placement_groups` | [ibm_is_placement_groups](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_placement_groups){: external} |
{: caption="Required Terraform instance template details" caption-side="bottom"}

Use the following guide to determine the required information for creating a new instance template.

Gather the following information by using `DataSource` block.

1. Gather instance profile details. Run the following block for the profile that you select. See [x86 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#profiles) for a list of available profiles. For more information, see the Terraform documentation on [ibm_is_instance_profiles](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_instance_profiles). Use an instance profile by referring to the instance profile data source. For more information, see the Terraform documentation on [ibm_is_instance_profile](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_instance_profile).

   ```terraform
   data "ibm_is_instance_profile" "example_profile" {
      name = "bx2-2x8"
   }
   ```
   {: codeblock}

1. List the available images for creating your instance. You can use a stock image, a custom image from your account, or an image that was shared with your account from a private catalog. For more information, see the Terraform documentation on [ibm_is_image](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_image). If you plan to use an image that was shared from a private catalog, see the Terraform documentation on [ibm_cm_version](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/cm_version) or [ibm_cm_offering_instance](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/cm_offering_instance).

   * Select a stock image or custom image from your account for your instance.

   ```terraform
   data "ibm_is_image" "example_image" {
      name = "ibm-centos-7-6-minimal-amd64-2"
   }
   ```
   {: codeblock}

   * Select an image that is shared from a private catalog for the instance. For more information, see the Terraform documentation on [ibm_is_images](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_images).

   ```terraform
   data "ibm_is_images" "example_images" {
      catalog_managed = true
   }
   ```
   {: codeblock}

     If you select a catalog image that belongs to a different account, you have more considerations and limitations to review. See [Using cross-account image references in a private catalog in Terraform](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=terraform#private-catalog-image-reference-vpc-terraform).
     {: note}


1. Create a VPC resource or use an existing VPC by referring to the VPC data source. For more information, see the Terraform documentation on data source [ibm_is_vpc](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_vpc) or resource [ibm_is_vpc](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_vpc).

   ```terraform
   data "ibm_is_vpc" "example_vpc" {
      name = "example-vpc"
   }
   ```
   {: codeblock}

1. Create a subnet resource or use an existing subnet by referring to the subnet data source. For more information, see the Terraform documentation on data source [ibm_is_subnet](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_subnet) or resource [ibm_is_subnet](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_subnet).

   ```terraform
   data "ibm_is_subnet" "example_subnet" {
      name            = "example-subnet"
      vpc             = ibm_is_vpc.example_vpc.id
      zone            = "us-south-1"
      ipv4_cidr_block = "10.240.0.0/24"
   }
   ```
   {: codeblock}

1. Use an existing ssh-key by referring to the ssh-key data source [ibm_is_ssh_key](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_ssh_key){: external}. If you don't have any available SSH keys, use the resource [ibm_is_ssh_key](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_ssh_key){: external} to create one.

   RSA and ED25519 are the two types of SSH keys that you can use. However, you can't use the ED25519 SSH key type with Windows or VMware images. You can use only RSA SSH keys for these images. For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys).
   {: note}

   ```terraform
   resource "ibm_is_ssh_key" "example_sshkey" {
      name       = "example-sshkey"
      public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCKVmnMOlHKcZK8tpt3MP1lqOLAcqcJzhsvJcjscgVERRN7/9484SOBJ3HSKxxNG5JN8owAjy5f9yYwcUg+JaUVuytn5Pv3aeYROHGGg+5G346xaq3DAwX6Y5ykr2fvjObgncQBnuU5KHWCECO/4h8uWuwh/kfniXPVjFToc+gnkqA+3RKpAecZhFXwfalQ9mMuYGFxn+fwn8cYEApsJbsEmb0iJwPiZ5hjFC8wREuiTlhPHDgkBLOiycd20op2nXzDbHfCHInquEe/gYxEitALONxm0swBOwJZwlTDOB7C6y2dzlrtxr1L59m7pCkWI4EtTRLvleehBoj3u7jB4usR"
   }
   ```
   {: codeblock}

### Creating the instance template with Terraform
{: #create-instance-template-using-terraform}
{: terraform}

After you know these values, use them to write the `ibm_is_instance_template` resource block. In addition to the information that you gathered, you must specify a name for the instance.

The following example creates an instance template that is named _my-instance-template_ in the _us-south-1_ region and uses the _bx2-8x32_ profile.

```terraform
resource "ibm_is_instance_template" "my-instance-template" {
  name    = "my-instance-template"
  image   = r006-3fa3bea4-7f9c-4eeb-8248-ab1f6e03185b
  profile = "bx2-8x32"

  primary_network_interface {
    subnet            = 0727-944126da-e46d-4104-8ba4-ab5a5832864b
    allow_ip_spoofing = true
  }

  placement_group = r006-9994e3ab-18ae-49a7-95cf-25c77e
  vpc             = r006-212def4b-93f0-4b9a-a331-94178b99d474
  zone            = "us-south-1"
  keys            = [363f6d70-0000-0001-0000-00000013b96c]

  boot_volume {
    name                             = "example-boot-volume"
    delete_volume_on_instance_delete = true
  }
}
```
{: codeblock}

The following example creates an instance template that is named _my-gen3-instance-template_ in _us-south-3_ and uses the _cx3d-8x20_ profile. When the selected [compute profile](/docs/vpc?group=profile-details) supports pooled bandwidth allocation for data volumes, the `storage_qos_modes` option can also be included.

```terraform
resource "ibm_is_instance_template" "example" {
  name                = "my-gen3-instance-template"
  image               = r006-534ef2ac-6158-45b3-9657-57629fa85305
  profile             = "cx3d-8x20"
  primary_network_interface {
    subnet            = 0d933c75-492a-4756-9832-1200585dfa79
    allow_ip_spoofing = true
  }
  vpc                 = r006-6e8fb140-5668-45b8-b98a-d5cb0e0bf39b
  zone                = "us-south-3"
  keys                = [363f6d70-0000-0001-0000-00000013b96c]
  storage_qos_modes   = [pooled]
  boot_volume {
    name                             = "example-boot-volume"
    delete_volume_on_instance_delete = true
  }
  volume_attachments {
    delete_volume_on_instance_delete = false
    name                             = "example-data-vol-01"
    volume_prototype {
      profile = "5iops-tier"
      capacity = 3000
    }
  }
  volume_attachments {
    delete_volume_on_instance_delete = false
    name                             = "example-data-vol-02"
    volume_prototype {
      profile = "5iops-tier"
      capacity = 3000
    }
  }
}
```
{: codeblock}

For more examples of the `ibm_is_instance_template`, see the [ibm_is_instance_template](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance_template){: external} Terraform reference.

### Next steps
{: #next-steps-instance-template}

After you create an instance template, you can provision a single virtual server instance, or you can provision multiple instances at the same time as part of an instance group. Optionally, you can set auto scaling policies for your instance group to dynamically add or remove virtual server instances from your group. For more information, see the following topics:

- [Bulk provisioning instances with instance groups](/docs/vpc?topic=vpc-bulk-provisioning)
- [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group)
- [Managing an instance template](/docs/vpc?topic=vpc-managing-instance-template)
