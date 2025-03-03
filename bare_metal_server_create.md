---

copyright:
  years: 2021, 2025
lastupdated: "2025-03-03"

keywords: creating bare metal servers

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating Bare Metal Servers on VPC 
{: #creating-bare-metal-servers}

Use the following information to create a bare metal server on your {{site.data.keyword.vpc_full}} (VPC) with the configuration of your choice.
{: shortdesc}

If you want to use an operating system image that is hosted on your own server or on a public server, review [Network booting your own operating system with Bare Metal Servers on VPC](/docs/vpc?topic=vpc-network-boot-bare-metal-servers) before creating your bare metal server.

## Creating a bare metal server by using the UI
{: #creating-bare-metal-ui}
{: ui}

Use the following steps to create a bare metal server by using the {{site.data.keyword.cloud}} console. You can also check out the following [video](https://mediacenter.ibm.com/media/t/1_j2pdfua6){: external} to learn more about creating a bare metal server.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**.

1. Click **Create** and enter the information that is in Table 1.

1. For Advanced options, you can choose to complete extra server configurations. For more information, see Table 2.

1. Review the configuration **Summary** and click **Create bare metal server**.

| Field | Value |
|---|---|
| Location | Locations are composed of regions (specific geographic areas) and zones (fault tolerant data centers within a region). Select the location where you want your bare metal server. |
| Name | A name is required for your bare metal server. |
| Resource group | Select a resource group for the server. |
| Tags | You can assign labels to your server so that you can easily filter resources in your resource list. |
| Access management tags | Access management tags help you apply flexible access policies on specific resources. |
| Image | Click **Change image** to select an image. On the Select an image page, you can select from all available stock images and custom images. After you select your image, click **Save**. For more information, see [x86-64 bare metal server images](/docs/vpc?topic=vpc-bare-metal-image). For information about using custom images with your bare metal server, see [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images&interface=ui) |
| Profile | Click **Change profile** to select from all available vCPU and RAM combinations. The profile families are Balanced, Compute, and Memory. For more information, see [x86-64 bare metal server profiles](/docs/vpc?topic=vpc-bare-metal-servers-profile). |
| SSH key | Select an existing public SSH key or click **Create an SSH key** to create a new one. For more information about creating an SSH key, see [Creating your SSH key by using the UI](/docs/vpc?topic=vpc-ssh-keys&interface=ui#generate-ssh-keys-ui). You must specify at least one SSH key.  \n - For x86 architecture, the SSH key is used to automatically generate a password that is required for accessing VMware&reg; ESXi Direct Console User Interface (DCUI) and the ESXi web client.  \n  \n **Note:**  SSH keys can either be RSA or Ed25519. You can generate new RSA key pairs using the UI. Pre-existing RSA and Ed25519 SSH keys can be uploaded. Ed25519 can be used only if the operating system supports this key type. Ed25519 can't be used with Windows or VMware images. |
| Virtual private cloud | Specify the VPC where you want to create your server. You can use the default VPC, another existing VPC, or you can create a new VPC. |
| Network bandwidth | If you provision a Sapphire Rapids server, select the network bandwidth to allocate to your server. You can also [adjust network bandwidth after provisioning](/docs/vpc?topic=vpc-managing-bare-metal-servers&interface=ui#viewing-bare-metal-server-ui) a Sapphire Rapids server. |
| Network interfaces | By default the bare metal server is created with a single primary network interface. You can click the pencil icon to edit the details of the network interface. For example, the subnet or security group that's associated with the interface. To include extra secondary network interfaces, click **New interface**.  \n - For x86 architecture, you can create and assign up to eight PCI network interfaces and up to 20 PCI + VLAN network interfaces for each server. For more information about advanced networking configurations, see [Managing network interfaces for a bare metal server](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers).  \n  \n With the virtual network interface feature, you can select the type of network interface that you want to use. You can select the new option **Network attachment with a virtual network interface** or the older option **Instance network interface**. Whichever type of network interface option that you select when you provision, the bare metal server persists through the lifecycle of the bare metal server. You can click **Attach** to create a network attachment with an existing virtual network interface. For more information, see [About virtual network interfaces](/docs/vpc?topic=vpc-vni-about).|
{: caption="Bare metal server provisioning selections" caption-side="bottom"}

Adjustable network bandwidth for Sapphire Rapids bare metal servers is only available in US South (Dallas).
{: preview}

| Advanced option | Value |
|---|---|
| User data | Paste your user data to the **User data (optional)** field or click **Import user data** to upload from your user data. For example, you can enable SSH by adding the following script to the **User data (optional)** field. For more information about user data, see [User data](/docs/vpc?topic=vpc-user-data).|
| Trusted Platform Module (TPM) | Click the toggle to enable Trusted Platform Module capabilities. Then, select the mode that you want to use. For more information, see [Secure boot with Trusted Platform Module (TPM)](/docs/vpc?topic=vpc-secure-boot-tpm&interface=ui). |
| Secure boot | Click the toggle to enable secure boot. For more information, see [Secure boot with Trusted Platform Module (TPM)](/docs/vpc?topic=vpc-secure-boot-tpm&interface=ui). |
| Add to reservation (beta) | If you have an active reservation, click the toggle to add the server to that reservation. For more information about reservations, see [About Reservations for VPC](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc). |
{: caption="Bare metal server advanced options" caption-side="bottom"}



For x86 architecture-based bare metal servers, the DHCP response for all interfaces (PCI or VLAN) includes a gateway. So, if you create multiple interfaces on different subnets, consider a static IP configuration or use separate network namespaces to handle the different gateways.
{: note}

## Creating a bare metal server by using the API
{: #bare-metal-creating-using-api}
{: api}

You can create a bare metal server by using the API. Use the following steps to create a bare metal server by using the API.

### Before you begin
{: #bare-metal-server-api-prereq}

1. Make sure that you set up your API environment. For more information, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

   To learn more about the API, click **Get sample API call** on the create pages in {{site.data.keyword.cloud}} console. You can view the correct sequence of API requests and better understand actions and their dependencies.
   {: tip}

1. Make sure that you create a VPC and a subnet before you create a bare metal server.

### Gathering information for the bare metal server
{: #bare-metal-server-api-gather-info}

Before you use the API to create bare metal server, see the following table for the required information.

| Server detail | Listing options |
|---------|---------|
| Image | [List all images](/apidocs/vpc/latest#list-images) |
| Keys | [List all keys](/apidocs/vpc/latest#list-keys)  \n  \n If you don't have any available SSH keys, use [Create a key](/apidocs/vpc/latest#create-key) to create one. For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys).  \n  \n **Note:**  SSH keys can either be RSA or Ed25519. You can generate new RSA key pairs using the UI. Pre-existing RSA and Ed25519 SSH keys can be uploaded. Ed25519 can be used only if the operating system supports this key type. Ed25519 can't be used with Windows or VMware images. |
| Subnet | [List all subnets](/apidocs/vpc/latest#list-subnets) |
| Security groups (optional) | [List all security groups](/apidocs/vpc/latest#list-security-groups) |
| Profile | [List all bare metal server profiles](/apidocs/vpc/latest#list-bare-metal-server-profiles) |
| Zone | [List all regions](/apidocs/vpc/latest#list-regions)  \n [List all zones in a region](/apidocs/vpc/latest#list-region-zones) |
{: caption="Information that you need to create a bare metal server by using the API" caption-side="bottom"}

### Creating a bare metal server
{: #api-request-create-bare-metal-server}

After you have all the information, use the [Create bare metal server](/apidocs/vpc/latest#create-bare-metal-server) API request to create a bare metal server.

* For x86 architecture, you can create a bare metal server with the following example configuration:

   * ESXi image ID: "r006-31c8ca90-2623-48d7-8cf7-737be6fc4c3e"
   * SSH Key ID: "a6b1a881-2ce8-41a3-80fc-36316a73f803"
   * Name of the bare metal server: "my-bare-metal-server"
   * A primary network interface with the following configurations:
      * Allows VLAN with the ID of "4" to be attached
      * Name: "my-primary-network-interface"
      * Subnet ID: "7ec86020-1c6e-4889-b3f0-a15f2e50f87e"
   * A secondary VLAN interface with ID "4". This VLAN interface is floatable.
   * Profile name: "bx2d-metal-192x768"
   * Zone: "us-south-1"

   The API request is similar to:
   ```sh
   curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers?version=2021-03-09&generation=2" \
   -H "Authorization: $iam_token" \
   -d '{
       "initialization": {
           "image": {
           "id": "r006-31c8ca90-2623-48d7-8cf7-737be6fc4c3e"
           },
           "keys": [
           {"id": "a6b1a881-2ce8-41a3-80fc-36316a73f803"}
           ]
       },
       "primary_network_interface": {
           "interface_type": "pci",
           "name": "my-primary-network-interface",
           "subnet": {
           "id": "7ec86020-1c6e-4889-b3f0-a15f2e50f87e"
           },
           "allowed_vlans": [4]
       },
       "network_interfaces":[
          {
           "interface_type": "vlan",
           "name": "my-vlan-interface",
           "allow_interface_to_float": true,
           "subnet": {
           "id": "2302-6d5fe694-12f7-4161-b979-21bb4e872696"
            },
           "vlan": 4
          }
       ],
        "name": "my-bare-metal-server",
        "profile": {
            "name": "bx2d-metal-192x768"
        },
        "zone": {
            "name": "us-south-1"
        }
      }' | jq
    ```
    {: pre}

    The example request uses the JSON processing utility _jq_ to format the response. You can modify the command to use another parsing tool or remove `" | jq"` to receive an unformatted response.
    {: note}

    You see a response that is similar to the following example:

    ```json
    "bandwidth": 100000,
    "boot_target": {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/bare_metal_servers/2302-5e095b83-ceb4-49b5-9699-0aa5a2c996a4/disks/2302-3744f199-6ccc-4698-8772-bb3937348c96",
      "id": "2302-3744f199-6ccc-4698-8772-bb3937348c96",
      "name": "zipfile-delude-slang-knoll",
      "resource_type": "bare_metal_server_disk"
    },
    "cpu": {
      "architecture": "amd64",
      "core_count": 96,
      "socket_count": 4,
      "threads_per_core": 2
    },
    "created_at": "2021-03-12T09:29:17.000Z",
    "crn": "crn:[...]",
    "disks": [
      {
       "created_at": "2021-03-12T09:29:17.000Z",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/bare_metal_servers/2302-5e095b83-ceb4-49b5-9699-0aa5a2c996a4/disks/2302-3744f199-6ccc-4698-8772-bb3937348c96",
        "id": "2302-3744f199-6ccc-4698-8772-bb3937348c96",
        "interface_type": "sata",
        "name": "zipfile-delude-slang-knoll",
        "resource_type": "bare_metal_server_disk",
        "size": 960
      },
      {
        "created_at": "2021-03-12T09:29:17.000Z",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/bare_metal_servers/2302-5e095b83-ceb4-49b5-9699-0aa5a2c996a4/disks/2302-86003aba-47db-4d07-bd76-62e00cca83e5",
        "id": "2302-86003aba-47db-4d07-bd76-62e00cca83e5",
        "interface_type": "nvme",
        "name": "elderly-mountain-trout-opponent",
        "resource_type": "bare_metal_server_disk",
        "size": 3200
      },
      {
        "created_at": "2021-03-12T09:29:17.000Z",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/bare_metal_servers/2302-5e095b83-ceb4-49b5-9699-0aa5a2c996a4/disks/2302-8372237f-77cb-47e4-9c61-b9d19ddfdbcd",
        "id": "2302-8372237f-77cb-47e4-9c61-b9d19ddfdbcd",
        "interface_type": "nvme",
        "name": "could-kilt-twisty-unloaded",
        "resource_type": "bare_metal_server_disk",
        "size": 3200
      },
      {
        "created_at": "2021-03-12T09:29:17.000Z",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/bare_metal_servers/2302-5e095b83-ceb4-49b5-9699-0aa5a2c996a4/disks/2302-e544d72d-ca08-4924-b748-a8f67b66286d",
        "id": "2302-e544d72d-ca08-4924-b748-a8f67b66286d",
        "interface_type": "nvme",
        "name": "wildcat-impromptu-dribble-hesitate",
        "resource_type": "bare_metal_server_disk",
        "size": 3200
      },
      {
        "created_at": "2021-03-12T09:29:17.000Z",
        "href": "us-south.iaas.cloud.ibm.com/v1/bare_metal_servers/2302-5e095b83-ceb4-49b5-9699-0aa5a2c996a4/disks/2302-de34647b-e7fb-405b-85af-d28c6dfe142c",
        "id": "2302-de34647b-e7fb-405b-85af-d28c6dfe142c",
        "interface_type": "nvme",
        "name": "imperfect-stimulate-culpable-thumb",
        "resource_type": "bare_metal_server_disk",
        "size": 3200
      }
    ],
    "enable_secure_boot": false,
    "href": "https://us-south.iaas.cloud.ibm.com/v1/bare_metal_servers/2302-5e095b83-ceb4-49b5-9699-0aa5a2c996a4",
    "id": "2302-5e095b83-ceb4-49b5-9699-0aa5a2c996a4",
    "memory": 768,
    "name": "my-bare-metal-server",
    "network_interfaces": [
      {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/bare_metal_servers/2302-5e095b83-ceb4-49b5-9699-0aa5a2c996a4/network_interfaces/2302-c094c123-5c82-49a2-8bb7-847bd3a7b62d",
        "id": "2302-c094c123-5c82-49a2-8bb7-847bd3a7b62d",
        "name": "my-primary-network-interface",
        "primary_ipv4_address": "10.240.128.5",
        "resource_type": "network_interface",
        "subnet": {
          "crn": "crn:[...]",
          "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/2302-6d5fe694-12f7-4161-b979-21bb4e872696",
          "id": "7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
          "name": "my-subnet"
        }
      },
      {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/bare_metal_servers/2302-5e095b83-ceb4-49b5-9699-0aa5a2c996a4/network_interfaces/2302-edd97be7-588e-4f0b-a875-7e0bec68f767",
        "id": "2302-edd97be7-588e-4f0b-a875-7e0bec68f767",
        "name": "my-vlan-interface",
        "primary_ipv4_address": "",
        "resource_type": "network_interface",
        "subnet": {
          "crn": "crn:[...]",
          "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/2302-6d5fe694-12f7-4161-b979-21bb4e872696",
          "id": "7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
          "name": "my-subnet"
        }
      }
    ],
    "primary_network_interface": {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/bare_metal_servers/2302-5e095b83-ceb4-49b5-9699-0aa5a2c996a4/network_interfaces/2302-c094c123-5c82-49a2-8bb7-847bd3a7b62d",
      "id": "2302-c094c123-5c82-49a2-8bb7-847bd3a7b62d",
      "name": "my-primary-network-interface",
      "primary_ipv4_address": "10.240.128.5",
      "resource_type": "network_interface",
      "subnet": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/2302-6d5fe694-12f7-4161-b979-21bb4e872696",
        "id": "7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
        "name": "my-subnet"
      }
    },
    "profile": {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/bare_metal_server/profiles/bx2d-metal-192x768",
      "name": "bx2d-metal-192x768"
    },
    "resource_group": {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/resource_groups/823edda102129f3232a0dc0655fcff94",
      "id": "823edda102129f3232a0dc0655fcff94",
      "name": "Default"
    },
    "resource_type": "bare_metal_server",
    "status": "pending",
    "status_reasons": [],
    "trusted_platform_module": {
      "enabled": false,
      "mode": ""
    },
    "vpc": {
      "crn": "crn:[...]",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/r006-96cb322a-6a59-4ac4-8783-1f059f87e4a9",
      "id": "r006-96cb322a-6a59-4ac4-8783-1f059f87e4a9",
      "name": "my-vpc"
    },
    "zone": {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
      "name": "us-south-1"
    }
    }
    ```
    {: pre}



The status displays "Pending" until the server is created.
{: tip}

For more information about the API request, see [Create a bare metal server](/apidocs/vpc/latest#create-bare-metal-server).

### Viewing your server
{: #viewing-bms-api}

When the server status changes to "Running", use the following request to view it.

```sh
curl -X GET "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

## Creating a bare metal server by using the CLI
{: #creating-bare-metal-server-using-cli}
{: cli}

You can use the CLI to create a bare metal server. Use the following steps to create a bare metal server by using the {{site.data.keyword.cloud}} CLI.

### Before you begin
{: #bare-metal-server-cli-prereq}

1. Make sure that you set up your [CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
1. Make sure that you create a VPC and a subnet before you create a bare metal server.

For more information, see [Using the CLI to create VPC resources](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli#creating-a-vpc-using-cli).

### Gathering information to create a bare metal server
{: #bare-metal-server-cli-gather-info}

Before you can use the CLI to create bare metal server, you need to gather the information that is needed.

| Server details | Listing options |
|---------|---------|
| Image | [List all images](/docs/vpc?topic=vpc-vpc-reference#images-list) |
| Keys | [List all keys](/docs/vpc?topic=vpc-vpc-reference#keys)  \n  \n If you don't have any available SSH keys, use [Create a key](/docs/vpc?topic=vpc-vpc-reference#key-create) to create one.  \n  \n **Note:**  SSH keys can either be RSA or Ed25519. You can generate new RSA key pairs using the UI. Pre-existing RSA and Ed25519 SSH keys can be uploaded. Ed25519 can be used only if the operating system supports this key type. Ed25519 can't be used with Windows or VMware images.  \n For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| Subnet | [List all subnets](/docs/vpc?topic=vpc-vpc-reference#subnets-list) |
| Security groups (optional) | [List all security groups](/docs/vpc?topic=vpc-vpc-reference#security-groups-list) |
| Profile | [List all bare metal server profiles](/docs/vpc?topic=vpc-vpc-reference#bare-metal-server-profiles-list) |
| Zone | [List all regions](/docs/vpc?topic=vpc-vpc-reference#regions)  \n List all zones in a region](/docs/vpc?topic=vpc-vpc-reference#zones) |
{: caption="Information that you need to create a bare metal server by using the CLI" caption-side="bottom"}

### Creating a bare metal server
{: #cli-command-create-bare-metal-server}

After you have all the information ready, you can use the CLI to create a bare metal server.

For example, you can create a bare metal server with the following configuration:

* For x86 architecture:
   * ESXi image ID: "r006-31c8ca90-2623-48d7-8cf7-737be6fc4c3e"
   * SSH Key ID: "a6b1a881-2ce8-41a3-80fc-36316a73f803"
   * Name of the bare metal server: "my-bare-metal-server"
   * A primary network interface with the following configurations:

      * VLAN with the ID of "4" to attach to the server
      * Name: "my-primary-network-interface"
      * Subnet ID: "7ec86020-1c6e-4889-b3f0-a15f2e50f87e"

   * A secondary VLAN interface with ID "4". This VLAN interface is floatable.
   * Profile name: "bx2d-metal-192x768"
   * Zone: "us-south-1"

   ```sh
   ibmcloud is bare-metal-server-create --name my-bare-metal-server --zone us-south-1 --profile mx2-metal-96x768 --image r006-31c8ca90-2623-48d7-8cf7-737be6fc4c3e --keys a6b1a881-2ce8-41a3-80fc-36316a73f803 --pnic-subnet 7ec86020-1c6e-4889-b3f0-a15f2e50f87e –pnic-name my-primary-network-interface --pnic-allowed-vlans 4 --network-interfaces '[{"name": "my-vlan-interface", "interface_type": "vlan", "vlan": 4, "allow_interface_to_float": true, "subnet": {"id":"7ec86020-1c6e-4889-b3f0-a15f2e50f87e"}}]' --output JSON
   ```
   {: pre}



### Viewing your server
{: #viewing-bms-cli}

When the server status changes to **Running**, use the following command to view it.

```sh
ibmcloud is bare-metal-server $bare_metal_server_id --output JSON
```
{: pre}



## Next steps
{: #next-steps-after-creating-bare-metal-server}

When the bare metal server status changes to **Running**, you can connect to it.

* For x86 architecture, you can connect to VMware ESXi Direct Console User Interface (DCUI) and ESXi's web client. For more information, see [Connecting to ESXi bare metal servers](/docs/vpc?topic=vpc-connect-to-ESXi-bare-metal-servers).
