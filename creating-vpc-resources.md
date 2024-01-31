---

copyright:
  years: 2018, 2023
lastupdated: "2023-10-24"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating VPC resources with CLI and API
{: #creating-vpc-resources-with-cli-and-api}

In addition to creating a VPC, you can also create VPC resources such as an instance, a subnet, and a Block Storage volume.
{: shortdesc}

For more information about creating VPC resources by using {{site.data.keyword.cloud_notm}} console, see [Using the {{site.data.keyword.cloud_notm}} console to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{: tip}

## Using the CLI to create VPC resources
{: #creating-a-vpc-using-cli}
{: cli}

You can create and configure an {{site.data.keyword.vpc_full}} by using the {{site.data.keyword.cloud}} CLI.
{: shortdesc}

To create and configure your virtual private cloud (VPC) and other attached resources, perform the steps in the sections that follow, in this order:

1. Create a VPC and subnet to define the network.
1. If you want to allow all resources in the subnet to communicate with the public internet, attach a public gateway.
1. Create a virtual server instance. By default, a boot volume is attached to the instance. For most virtual server instances, the default boot volume size is 100 GB. The default boot volume size for a z/OS virtual server instance is 250 GB.
1. If you want more storage, create a Block Storage volume and attach it to your instance.
1. To define the allowed inbound and outbound traffic for the instance, configure its security group.
1. If you want your instance to be reachable from the internet, reserve and associate a floating IP address.

### Before you begin
{: #before-cli-tutorial}

Make sure that you set up your CLI [environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
<!--Good place to mention the IBM Cloud SHell? -->

### Log in to IBM Cloud
{: #log-in-to-ibm-cloud}

```sh
ibmcloud login --sso -a cloud.ibm.com
```
{: pre}

This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After the authentication steps, you'll be prompted to choose your account. If you have access to multiple users, select the user you want to log in as.

When you are prompted to select a region, select us-south.

Respond to any remaining prompts to finish logging in.

### Selecting the generation of VPC
{: #select-the-generation }

Use the following command to configure the CLI plug-in to target generation 2 virtual server instances for VPC.

```sh
ibmcloud is target --gen 2
```
{: pre}

### Creating a VPC
{: #create-a-vpc-cli}

Use the following command to create a VPC named _my-vpc_.

```sh
ibmcloud is vpc-create my-vpc
```
{: pre}

From the returned output, save the ID in a variable so you can use it later, for example:

```sh
vpc="0738-59de4046-3434-4d87-bb29-0c99c428c96e"
```
{: pre}

The previous example does not create a VPC with classic access. If the VPC needs access to your classic resources, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure). You can enable a VPC for classic access only while you're creating it. In addition, you can have only one classic access VPC in your account at any time.
{: important}

### Creating a private catalog
{: #cli-create-private-catalog-optional}

This step is optional. If you plan share images from a private catalog, the private catalog must be created first. If you select a catalog image that belongs to a different account, you have further considerations and limitations to review. See [Using cross-account image references in a private catalog in the UI](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#private-catalog-image-reference-vpc-ui). To create a private catalog, see the tutorial [Onboarding software to your catalog by using the CLI](/docs/account?topic=account-create-private-catalog&interface=cli#create-cicd-product).

### Creating a subnet
{: #create-a-subnet-cli}

Before you create a subnet, select the zone and address prefix in which to create it. To list the address prefixes for each zone in your VPC, run the following command:

```sh
ibmcloud is vpc-address-prefixes $vpc
```
{: pre}

Now you need to select the default address prefix for the `us-south-3` zone. From the command output, note the CIDR block of the address prefix. When you create a subnet, you must specify an IP range that's within one of the address prefixes of the selected zone.

A subnet cannot be resized after it is created.
{: important}

```sh
ibmcloud is subnet-create my-subnet $vpc us-south-3 --ipv4-cidr-block "10.0.1.0/24"
```
{: pre}

From the returned output, save the ID in a variable so you can use it later, for example:

```sh
subnet="0738-658756a4-1106-4914-969a-3b43b338524a"
```
{: pre}

The status of the subnet is `pending` when you first create it. Before you can create resources in the subnet, the subnet needs to move to the `available` status, which takes a few seconds. To check the status of the subnet, run this command:

```sh
ibmcloud is subnet $subnet
```
{: pre}

### Attaching a public gateway
{: #attach-public-gateway-cli}

Attach a public gateway to the subnet if you want to allow all attached resources to communicate with the public internet.

To create a public gateway, run the following command:

```sh
ibmcloud is public-gateway-create my-gateway $vpc us-south-3
```
{: pre}

From the returned output, save the ID in a variable so you can use it later, for example:

```sh
gateway="0738-446c0c63-f0b1-4043-b30d-644f55fde391"
```
{: pre}

To attach the public gateway to your subnet, run the following command:

```sh
ibmcloud is subnet-update $subnet --pgw $gateway
```
{: pre}

Only one public gateway per zone is allowed in a VPC, but that public gateway can be attached to multiple subnets in the zone. To find the public gateway for a zone, run the `ibmcloud is public-gateways` command and look for the particular VPC and Zone values.
{: tip}

### Adding an SSH key
{: #add-ssh-key-cli}

Add your public SSH key to your {{site.data.keyword.cloud_notm}} account. This key is specified when you create the instance, and you need it later to log in to the instance. You can use one key to provision multiple instances.

To see the available keys in your IBM Cloud account, run this command:

```sh
ibmcloud is keys
```
{: pre}

To add a key, run the following command. Substitute the path to your SSH key file. For an RSA SSH key, you can use the path to your `id_rsa.pub` file. For an Ed25519 SSH key, you can use the path to your `id_ed25519.pub` file.

```sh
ibmcloud is key-create my-key @$HOME/.ssh/id_rsa.pub
```
{: pre}

From the returned output, save the ID in a variable so you can use it later, for example:

```sh
key="0738-859b4e97-7540-4337-9c64-384792b85653"
```
{: pre}

### Selecting a profile for the instance
{: #cli-select-a-profile-for-the-instance}

To list all available instance profiles, run the following command:

```sh
ibmcloud is instance-profiles
```
{: pre}

Save the profile that you plan to use as a variable, which is used later to provision an instance.

### Selecting an image for the instance
{: #cli-select-a-image-for-the-instance}

You can create an instance by using a stock image, a custom image from your account, or an image that was shared with your account from a private catalog. Run one of the following commands based on the image that you plan to use.

* Select a stock image or a custom image from your account for your instance.

    1. To list all available images, run the `ibmcloud is images` command. If the list is too long, consider filtering it like the following example does.

       ```sh
       $ ibmcloud is images | grep -i "debian.*available.*amd64.*public"
       r006-17cc6c08-f628-46f4-8bf4-a8edab749722   ibm-debian-10-13-minimal-amd64-4                    available    amd64   debian-10-amd64                      10.x Buster/Stable - Minimal Install                     1               public       provider     none           Default          -   
       r006-b721bf3b-393b-4386-a618-d1ea8c004e1c   ibm-debian-11-7-minimal-amd64-2                     available    amd64   debian-11-amd64                      11.x Bullseye/Stable - Minimal Install                   1               public       provider     none           Default          -   
       r006-e66ebfcd-4aac-44e2-b504-1f6f4c003518   ibm-debian-12-0-minimal-amd64-1                     available    amd64   debian-12-amd64                      12.x Bookworm/Stable - Minimal Install                   1               public       provider     none           Default          -   
       ```
       {: screen}

       Deprecated images do not include the most current support.
       {: tip}

    2. Select an image like `ibm-debian-12-0-minimal-amd64-1`, and save the image ID as a variable to be used later to provision an instance.

       ```text
       image=$(ibmcloud is images | grep -i "ibm-debian-12-0-minimal-amd64-1" | cut -d" " -f1)
       ```
       {: pre}

* Select an image shared from a private catalog for the instance

     If you select a catalog image that belongs to a different account, you have further considerations and limitations to review. See [Using cross-account image references in a private catalog in the CLI](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#private-catalog-image-reference-vpc-cli)
     {: note}

    - To list all available private catalog image offerings, run the following command.

       ```sh
       ibmcloud is catalog-image-offerings
       ```
       {: pre}

       This command returns the identifier of each image offering and the identifier of the private catalog where the image resides. Save the `offering_id` and `catalog_id` in variables, which is used later to provision an instance.

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

       When you provision an instance, you can either provision the instance from the private catalog-managed image in the latest version in a catalog product offering by using the `offering_crn` value or from the specific version in the catalog product offering by using the `offering_version_crn` value.

       Save the `offering_crn` and `offering_version_crn`in variables, which is used later to provision an instance.

       ```sh
       offering_crn="crn:v1:bluemix:public:globalcatalog-collection:global:a/a1234567:offering:136559f6-4588-4af2-8585-f3c625eee09d"
       offering_version_crn="crn:v1:bluemix:public:globalcatalog-collection:global:a/a1234567:version:136559f6-4588-4af2-8585-f3c625eee09d/8ae92879-e253-4a7c-b09f-8d30af12e518"
       ```
       {: pre}

### Creating the instance
{: #create-an-instance}

Create the instance in the newly created subnet. Pass in your public SSH key so that you can log in after the instance is provisioned.

Run one of the following CLI commands based on the image that you plan to use.

* Create an instance by using a stock image or custom image from your account for your instance.

    ```sh
    ibmcloud is instance-create my-instance $vpc us-south-3 bx2-2x8 $subnet --image-id $image --key-ids $key
    ```
    {: pre}

* Create an instance by using a private catalog-managed image from the latest image version.

    ```sh
    ibmcloud is instance-create my-instance $vpc us-south-3 bx2-2x8 $subnet --catalog-offering $offering_crn --keys $key
    ```
    {: pre}

* Create an instance by using a private catalog-managed image from a specific version of the image.

    ```sh
    ibmcloud is instance-create my-instance $vpc us-south-3 bx2-2x8 $subnet --catalog-offering-version $offering_version_crn --keys $key
    ```
    {: pre}

Information about the network interface that is created for the new instance is not returned after the instance is created.
{: note}

From the returned output, save the ID of the instance in a variable so you can use it later, for example:

```sh
instance="0738-21179496-964e-4c00-8210-cf23d75750b3"
```
{: pre}

The status of the instance is `pending` when you first create it. Before you can proceed, the instance needs to move to the `running` status, which takes a few minutes. To check the status of the instance, run this command:

```sh
ibmcloud is instance $instance
```
{: pre}

From the returned output, save the ID of the primary network interface (`Primary Interface`) in a variable so you can use it later, for example:

```sh
nic="0738-4d9b3a58-f796-4e6a-b5ac-84f4216e9b68-glhvl"
```
{: pre}

### Creating a Block Storage data volume
{: #create-block-storage-data-volume-cli}

You can create a Block Storage volume and attach it to your virtual server instance if you want more storage. When you create a Block Storage volume, you select a profile to optimize the performance of your compute workloads. For more information about volume capacity and IOPS ranges based on the volume profile you select, see [Profiles](/docs/vpc?topic=vpc-block-storage-profiles#block-storage-profiles).

To see a list of volume profiles, run:

```sh
ibmcloud is volume-profiles
```
{: pre}

Run this command to create a Block Storage data volume. Specify a name for your volume, volume profile, and the zone where you are creating the volume. To attach a Block Storage data volume to an instance, the instance and the Block Storage data volume must be created in the same zone.

```sh
ibmcloud is volume-create my-volume custom us-south-3 --iops 1000 --size 500
```
{: pre}

From the returned output, save the ID of the volume in a variable so you can use it later:

```sh
vol=0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
```
{: pre}

The status of the volume is `pending` when it is first created. Before you can proceed, the volume needs to move to `available` status, which takes a few minutes.

To check the status of the volume, run this command:

```sh
ibmcloud is volume $vol
```
{: pre}

### Attaching a Block Storage data volume to an instance
{: #attach-block-storage-data-volume-cli}

Use the following command to attach the volume to the virtual server instance, by using the variables that you created:

```sh
ibmcloud is instance-volume-attachment-add my-volume-attachment $instance $vol --auto-delete true
```
{: pre}

### Adding rules to the default security group
{: #add-rules-to-default-security-group}

You can configure the security group to define the inbound and outbound traffic that is allowed for the instance. For example, you can add a rule to allow SSH traffic.

Find the security group for the VPC:

```sh
ibmcloud is vpc-sg $vpc
```
{: pre}

From the returned output, save the ID in a variable so you can use it later:

```sh
sg=0738-2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Now create a rule to allow SSH traffic:

```sh
ibmcloud is sg-rulec $sg inbound tcp --port-min=22 --port-max=22
```
{: pre}

Optionally, you can also add a rule to allow ping traffic:

```sh
ibmcloud is sg-rulec $sg inbound icmp --icmp-type 8 --icmp-code 0
```
{: pre}

For Windows images, make sure the security group that is associated with the instance allows inbound and outbound Remote Desktop Protocol traffic (TCP port 3389).
{: tip}

### Creating a floating IP address for the instance
{: #create-floating-ip-address-cli}

Create a floating IP address if you want your instance to be reachable from the internet.

```sh
ibmcloud is floating-ip-reserve my-fip --nic $nic
```
{: pre}

From the returned output, save the `Address` in a variable so you can use it later:

```sh
address=169.48.88.0
```
{: pre}

### Logging in to your instance
{: #log-in-to-your-instance-using-your-private-ssh-key}

For example, on Linux you can use a command of this form:

```sh
ssh -i $HOME/.ssh/id_rsa root@$address
```
{: pre}

The user login 'root' is disabled by default in Fedora Core OS. The user login 'core' can be used to log in to Fedora Core OS instances.
{: note}

When you're prompted to continue connecting, type `yes`.

SSH access into the instance might be prevented by security groups. Make sure the instance's security group allows SSH access.
{: tip}

To connect to a Windows image, log in using its decrypted password. For instructions, see [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows).

### Monitoring your instance
{: #monitoring-your-instance-cli-tutorial}

You can monitor the CPU, volume, memory, and network usage of your instance over time in the {{site.data.keyword.cloud_notm}} console. Because the monitoring data is stored in {{site.data.keyword.monitoringlong_notm}}, you must be authenticated to an instance of the Monitoring service in your account. For more information, see [Getting started with monitoring](/docs/monitoring?topic=monitoring-getting-started).

For {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}} instances, the memory metrics cannot be collected because {{site.data.keyword.hpvs}} for VPC instances are created by using Secure Execution images and the memory of a secure execution instance is not accessible.
{: note}

### Creating a VPN gateway
{: #create-a-vpn-gateway}

Create a VPN gateway on the subnet if you want to securely connect your VPC to another private network.

To create a VPN gateway, run the following command:

```sh
ibmcloud is vpn-gateway-create my-vpn-gateway $subnet
```
{: pre}

From the returned output, save the ID of the VPN gateway in a variable so you can use it later, for example:

```sh
vpn_gateway="0757-7e91085b-dc11-4707-aa4d-66e735e9a2bc"
```
{: pre}

The status of the VPN gateway is `pending` when you first create it. Before you can proceed, the VPN gateway needs to move to the `available` status, which takes a few minutes. To check the status of the VPN gateway, run this command:

```sh
ibmcloud is vpn-gateway $vpn_gateway
```
{: pre}

To create a VPN connection on the VPN gateway to peer address `169.61.161.150` and pre-shared key `mykey`, run the following command:

```sh
ibmcloud is vpn-gateway-connection-create my-vpn-conn $vpn_gateway 169.61.161.150 mykey
```
{: pre}

In this example, `169.61.161.150` is the peer gateway address, which specifies the IP address of the VPN gateway for the network outside your VPC. The `mykey` value is the authentication key of the VPN gateway for the network outside your VPC. It's a string of hexadecimal digits, or a passphrase of printable ASCII characters. To be compatible with most peer gateway types, this string must follow these rules:
      * Can be a combination of digits, lower or upper case characters, or the following special characters: `- + & ! @ # $ % ^ * ( ) . , :`
      * The length of the string must be 6 - 128 characters.
      * Cannot start with `0x` or `0s`.    

The status of the VPN connection is `down` when it is first created and becomes `up` after the connection is established. To check the status of VPN connections on a VPN gateway, run this command:

```sh
ibmcloud is vpn-gateway-connections $vpn_gateway
```
{: pre}

### Congratulations!
{: #congratulations-cli-tutorials}

You successfully created and configured your VPC from the {{site.data.keyword.cloud_notm}} CLI. To try out more CLI commands, see the [CLI reference](/docs/vpc?topic=vpc-vpc-reference#vpc-reference).

## Using the REST APIs to create VPC resources
{: #creating-a-vpc-using-the-rest-apis}
{: api}

You can create and configure {{site.data.keyword.vpc_full}} resources by using the REST APIs.
{: shortdesc}

To create and configure your virtual private cloud (VPC) and other attached resources, perform the steps in the sections that follow, in this order:

1. Create a VPC and subnet to define the network.
1. If you want to allow all resources in the subnet to communicate with the public internet, attach a public gateway.
1. Create a virtual server instance. By default, a 100 GB boot volume is attached to the instance.
1. If you want more storage, create a Block Storage volume and attach it to your instance.
1. To define the allowed inbound and outbound traffic for the instance, configure its security group.
1. If you want your instance to be reachable from the internet, reserve and associate a floating IP address.

### Before you begin
{: #before-api-tutorial}

Define variables for the IAM token, API endpoint, and API version. For instructions, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

A good way to learn more about the API is to click **Get sample API call** on the provisioning pages in {{site.data.keyword.cloud_notm}} console. You can view the correct sequence of API requests and better understand actions and their dependencies.
{: tip}

### Creating a VPC
{: #create-vpc-api-tutorial}

Create an {{site.data.keyword.vpc_short}} called `my-vpc`.

```bash
curl -X POST "$vpc_api_endpoint/v1/vpcs?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
      	"name": "my-vpc"
      }'
```
{: pre}

You must send the `generation` parameter with every API request to specify which generation to use. For generation 2 virtual server instances, specify `generation=2`. For more information, see **Generation** in the [Virtual Private Cloud API](/apidocs/vpc/latest#api-generation-parameter)
{: important}

For the rest of the calls, you need to know the ID of the newly created VPC. Save the ID in a variable, for example:

```bash
vpc="0738-59de4046-3434-4d87-bb29-0c99c428c96e"
```
{: pre}

To verify that the variable was saved, run `echo $vpc` and make sure that the response is not empty.

The previous example does not create a VPC with classic access. If the VPC needs access to your classic resources, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure). You can enable a VPC for classic access only while you're creating it. In addition, you can have only one classic access VPC in your account at any time.
{: important}

### Creating a private catalog
{: #api-create-private-catalog}

This step is optional. If you plan share images from a private catalog, the private catalog must be created first. If you select a catalog image that belongs to a different account, you have further considerations and limitations to review. See [Using cross-account image references in a private catalog in the UI](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#private-catalog-image-reference-vpc-ui). To create a private catalog, see the tutorial [Onboarding software to your catalog by using the CLI](/docs/account?topic=account-create-private-catalog&interface=cli#create-cicd-product).

### Creating a subnet
{: #create-subnet-api-tutorial}

Before you create a subnet, select the zone and address prefix in which to create it. To list the address prefixes for each zone in your VPC, run the following command:

```bash
curl -X GET "$vpc_api_endpoint/v1/vpcs/$vpc/address_prefixes?version=$api_version&generation=2" \
  -H "Authorization:$iam_token"
```
{: pre}

Now, select the default address prefix for the us-south-3 zone. From the response, note the CIDR block of the address prefix. When you create a subnet, you must specify an IP range that's within one of the address prefixes of the selected zone.

A subnet cannot be resized after it is created.
{: important}

```bash
curl -X POST "$vpc_api_endpoint/v1/subnets?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-subnet",
        "ipv4_cidr_block": "10.0.1.0/24",
        "zone": { "name": "us-south-3" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Save the ID of the subnet in a variable.

```bash
subnet="0738-35fb0489-7105-41b9-99de-033fae723006"
```
{: pre}

To provision resources in your subnet, the subnet must be in the `Ready` status. Query the subnet resource and make sure that the status is `Ready` before you continue. If the status is `failed`, contact [support](/docs/get-support?topic=get-support-using-avatar) with the details. You can attempt to continue by trying to provision another subnet.

```bash
curl -X GET "$vpc_api_endpoint/v1/subnets/$subnet?version=$api_version&generation=2" \
  -H "Authorization: $iam_token"
```
{: pre}

### Attaching a public gateway
{: #attach-public-gateway-api-tutorial}

Attach a public gateway to the subnet if you want to allow all attached resources to communicate with the public internet.

Create a public gateway for the zone:

```bash
curl -X POST "$vpc_api_endpoint/v1/public_gateways?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-gateway",
        "zone": { "name": "us-south-3" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Save the ID of the public gateway in a variable.

```bash
gateway="0738-ad0cded3-53a3-4d4a-9809-8c59b50d2b80"
```
{: pre}

Attach the public gateway to your subnet.

```bash
curl -X PUT "$vpc_api_endpoint/v1/subnets/$subnet/public_gateway?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
        "id": "'$gateway'"
      }'
```
{: pre}

Only one public gateway per zone is allowed in a VPC, but that public gateway can be attached to multiple subnets in the zone. To find the public gateway for a zone, run the 'ibmcloud is public-gateways` command and look for the particular VPC and Zone values.
{: tip}

You can then retrieve and view the public gateway that is attached to the subnet by running the following command.

```bash
curl -X GET "$vpc_api_endpoint/v1/subnets/$subnet/public_gateway?version=$api_version&generation=2" \
  -H "Authorization: $iam_token"
```
{: pre}

### Adding an SSH key
{: #add-ssh-key-api-tutorial}

Add your public SSH key to your {{site.data.keyword.cloud_notm}} account. This key is specified when you create the instance, and you need it later to log in to the instance. You can use one key to provision multiple instances.

```bash
curl -X POST "$vpc_api_endpoint/v1/keys?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-key",
        "public_key": "ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQB/nAmOjTmezNUDKYvEeIRf2YnwM9/uUG1d0BYsc8/tRtx+RGi7N2lUbp728MXGwdnL9od4cItzky/zVdLZE2cycOa18xBK9cOWmcKS0A8FYBxEQWJ/q9YVUgZbFKfYGaGQxsER+A0w/fX8ALuk78ktP31K69LcQgxIsl7rNzxsoOQKJ/CIxOGMMxczYTiEoLvQhapFQMs3FL96didKr/QbrfB1WT6s3838SEaXfgZvLef1YB2xmfhbT9OXFE3FXvh2UPBfN+ffE7iiayQf/2XR+8j4N4bW30DiPtOQLGUrH1y5X/rpNZNlWW2+jGIxqZtgWg7lTy3mXy5x836Sj/6L"
      }'
```
{: pre}

Save the ID of the SSH key in a variable, for example:

```bash
key="0738-35fb0489-7105-41b9-8764-033fae723006"
```
{: pre}

### Selecting a profile for your instance
{: #select-profile}

Call the API to list all profiles. The following command lists the profiles available.

```bash
curl -X GET "$vpc_api_endpoint/v1/instance/profiles?version=$api_version&generation=2" \
  -H "Authorization:$iam_token"
```
{: pre}

Save the name of the profile in a variable, which is used later to provision an instance. For example,

```bash
profile_name="b2-2x8"
```
{: pre}

### Selecting an image for your instance
{: #api-select-image}

 You can create an instance by using a stock image, a custom image from your account, or an image that was shared with your account from a private catalog. Run one of the following API calls based on the image you plan to use.

* Select a stock image or a custom image from your account for your instance.

    The following command lists the images available.

    ```bash
    curl -X GET "$vpc_api_endpoint/v1/images?version=$api_version&generation=2" \
      -H "Authorization:$iam_token"
    ```
    {: pre}

    Save the ID of the image in a variable, which is to be used later to provision an instance. For example,

    ```bash
    image_id="0738-660198a6-52c6-21cd-7b57-e37917cef586"
    ```
    {: pre}

* Select an image shared from a private catalog for your instance.

    If you select a catalog image that belongs to a different account, you have further considerations and limitations to review. See [Using cross-account image references in a private catalog in the API](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#private-catalog-image-reference-vpc-api)
     {: note}

    You can either provision an instance from the private catalog image at the latest version in a catalog product offering or from a specific version in the catalog product offering.

    To select the private catalog image from the latest version of a catalog product offering, see [Catalog Management API - Get offering](/apidocs/resource-catalog/private-catalog?code=java#get-offering). Find the offering CRN, and save it into a variable for later use:

    ```bash
     offering_crn="crn:v1:bluemix:public:globalcatalog-collection:global::1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc:offering:00111601-0ec5-41ac-b142-96d1e64e6442-global"
     ```
     {: pre}

     To select the private catalog image from a specific version of a catalog product offering, see [Catalog Management API - Get offering](/apidocs/resource-catalog/private-catalog?code=java#get-offering). Then, go to **Get offering > Kinds > Versions > CRN** to retrieve the version's CRN and save it into a variable for later use:

     ```bash
     version_crn="crn:v1:bluemix:public:globalcatalog-collection:global::1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc:version:00111601-0ec5-41ac-b142-96d1e64e6442-global/ec66bec2-6a33-42d6-9323-26dd4dc8875d-global"
     ```
     {: pre}

### Creating the instance
{: #create-instance-api-tutorial}

Create the instance in the newly created subnet. Pass in your public SSH key so that you can log in after the instance is provisioned. Run one of the following API calls based on the type of image you plan to use.

* Select a stock image or a custom image from your account for your instance.

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

* Create an instance by using a private catalog image from the latest version of a catalog product offering.

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

Save the ID of the instance in a variable, for example:

```bash
instance="0738-35fb0489-7105-41b9-99de-033fae723006"
```
{: pre}

The status of the instance is `stopped` when you create it. Before you can proceed, the instance needs to move to the `running` status, which takes a few minutes. Query the status of the instance and make sure it is `running`.

```bash
curl -X GET "$vpc_api_endpoint/v1/instances/$instance?version=$api_version&generation=2" \
  -H "Authorization: $iam_token"
```
{: pre}

Save the ID of the primary network interface of the instance returned in the `GET/instance API` call, for example:

```bash
network_interface="0738-7710e766-dd6e-41ef-9d36-06f7adbef33d"
```
{: pre}

You can't get the ID of the primary network interface until you query the specific instance.
{: important}

### (Optional) Creating and attaching a Block Storage data volume
{: #create-and-attach-storage-api-tutorial}

You can create a Block Storage data volume and attach it to your virtual server instance as auxiliary storage. Create a data volume with a request similar to this example. This procedure shows the volume profiles, crreates a volume, saves the volume ID in a variable, checks the status of the volume, and then creates the volume attachment.

Show a list of volume profiles:

```bash
curl -X GET "$vpc_api_endpoint/v1/volumes/profiles?version=$api_version&generation=2" \
  -H "Authorization: $iam_token" \
```
{: pre}

Profiles can be general-purpose (3 IOPS/GB), 5iops-tier, 10-iops-tier, and custom. For more information about volume capacity and IOPS ranges based on the volume profile you select, see [Profiles](/docs/vpc?topic=vpc-block-storage-profiles#block-storage-profiles).

Create the data volume:

```bash
curl -X POST "$vpc_api_endpoint/v1/volumes?version=$api_version&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-volume",
        "iops": 1000,
        "capacity": 100,
        "zone": {
            "name": "us-south-3"
            },
        "profile": {
            "name": "custom"
            }
      }'
```
{: pre}

Save the ID of the volume in a variable:

```bash
volume_id="0738-640774d7-2adc-4609-add9-6dfd96167a8f"
```
{: pre}

The status of the volume is `pending` when it is first created. Before you can proceed, the volume needs to move to `available` status, which takes a few minutes.

Check the status of the volume:

```bash
curl -X GET "$vpc_api_endpoint/v1/volumes/$volume_id?version=$api_version&generation=2" \
  -H "Authorization: $iam_token"
```
{: pre}

Create a volume attachment to attach the new data volume to the virtual server instance. Use the instance ID variable that you created earlier in the request. Use the volume ID variable to specify the volume.

Create the volume attachment:

```bash
curl -X POST "$vpc_api_endpoint/v1/instances/$instance/volume_attachments?version=$version&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-volume-attachment",
        "volume": {
            "id": "'$volume_id'"
            }
      }'
```
{: pre}

### Adding rules to the default security group
{: #add-sg-rule-api-tutorial}

You can configure the security group to define the inbound and outbound traffic that is allowed for the instance. For example, you can add a rule to allow SSH traffic.

Find the security group for the VPC:

```bash
curl -X GET "$vpc_api_endpoint/v1/vpcs/$vpc/default_security_group?version=$api_version&generation=2" \
  -H "Authorization:$iam_token"
```
{: pre}

Save the ID of the security group in a variable, for example:

```bash
sg=0738-2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Now create a rule to allow inbound SSH traffic so that you can connect to the instance:

```bash
curl -X POST "$vpc_api_endpoint/v1/security_groups/$sg/rules?version=$api_version&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
        "direction": "inbound",
        "protocol": "tcp",
        "port_min": 22,
        "port_max": 22
      }'
```
{: pre}

For Windows images, make sure the security group that is associated with the instance allows inbound and outbound Remote Desktop Protocol traffic (TCP port 3389).
{: tip}

### Creating a floating IP address
{: #create-floating-ip-api-tutorial}

Create a floating IP address if you want your instance to be reachable from the internet. Use the instance's primary network interface as the target for the floating IP address.

```bash
curl -X POST "$vpc_api_endpoint/v1/floating_ips?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-floatingip",
        "target": {
            "id":"'$network_interface'"
        }
      }
'
```
{: pre}

Save the ID of the floating IP address in a variable, for example.

```bash
floating_ip="0738-35fb0489-7105-41b9-99de-033fae723006"
```
{: pre}

### Logging in to your instance
{: #log-in-to-instance-api-tutorial}

To connect to the instance, use the floating IP address that you created. To get the floating IP address, run the following command:

```bash
curl -X GET "$vpc_api_endpoint/v1/floating_ips/$floating_ip?version=$api_version&generation=2" \
  -H "Authorization:$iam_token"
```
{: pre}

On Linux, use the `address` of the floating IP to connect to the instance with SSH:

```bash
ssh -i <private_key_file> root@<floating ip address>
```
{: pre}

The user login 'root' is disabled by default in Fedora Core OS. The user login 'core' can be used to log in to Fedora Core OS instances.
{: note}

To connect to a Windows image, log in using its decrypted password. For instructions, see [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows).
{: tip}

### Monitoring your instance
{: #monitoring-your-instance-api-tutorial}

You can monitor the CPU, volume, memory, and network usage of your instance over time in the {{site.data.keyword.cloud_notm}} console. Because the monitoring data is stored in {{site.data.keyword.mon_full_notm}}, you must be authenticated to an {{site.data.keyword.mon_full_notm}} instance in your account. For more information, see [IBM Cloud monitoring services](/docs/cloud-infrastructure?topic=cloud-infrastructure-monitoring).

### (Optional): Deleting the resources
{: #delete-resources-api-tutorial}

You can delete resources anytime. However, a resource can't be deleted if it is required by other resources.
For example, a VPC can't be deleted if it contains instances, subnets, or public gateways. For instructions on deleting a VPC and all its resources, see [Deleting a VPC with the REST APIs](/docs/vpc?topic=vpc-deleting-vpc-resources&interface=api).

### Congratulations!
{: #congratulations-api-tutorial}

You successfully created and configured your VPC by using the REST APIs. To try out more API commands, see the [Virtual Private Cloud API](/apidocs/vpc).
