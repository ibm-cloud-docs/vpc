---

copyright:
  years: 2021, 2022
lastupdated: "2022-01-28"

keywords: bare metal servers, create, create bare metal, create bare metal server, new bare metal, new bare metal server

subcollection: vpc

---

{:codeblock: .codeblock}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:preview: .preview}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Creating Bare Metal Servers on VPC
{: #creating-bare-metal-servers}

Use the following information to create a bare metal server on the {{site.data.keyword.vpc_full}} (VPC) with the configuration of your choice.
{: shortdesc}

## Creating a bare metal server by using the UI
{: #creating-using-ui}
{: ui}

Follow these steps to create a bare metal server by using the IBM Cloud console. You can also check out the following [video](https://mediacenter.ibm.com/media/t/1_j2pdfua6){: external} to learn more about creating a bare metal server. 

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Bare metal servers**

2. Click **Create** and enter the information that is in Table 1. 

3. Review the configuration **Summary** and click **Create**.

| Field | Value |
|---|---|
| Name | A name is required for your bare metal server. |
| Resource group | Select a resource group for the server. |
| Tags | You can assign labels to this server so that you can easily filter resources in your resource list. |
| Location | Locations are composed of regions (specific geographic areas) and zones (fault tolerant data centers within a region). Select the location where you want your bare metal server. |
| Operating system | Select the operating system and version from an image. `ibm-esxi-7-amd64-1` installs a licensed ESXi 7.x hypervisor. `ibm-esxi-7-byol-amd64-1` installs bring-your-own-license ESXi 7.x and control the licensing for your bare metal server. |
| Profile | Select from popular profiles or all available vCPU and RAM combinations. The profile families are Balanced, Compute, and Memory. For more information, see [Bare Metal Servers for VPC profiles](/docs/vpc?topic=vpc-bare-metal-servers-profile).
| SSH key | Select an existing public SSH key or click **New SSH key** to add a new one. You must specify at least one SSH key. This key is used to automatically generate a password that is required for accessing VMware ESXi Direct Console User Interface (DCUI) and ESXi’s web client. |
| User data | Paste your user data to the **User data (optional)** field or click **Import user data** to upload from your user data. For example, you can enable SSH by adding the following script to the **User data (optional)** field. For more information about user data, see [User data](/docs/vpc?topic=vpc-user-data).|
| Virtual private cloud | Specify the IBM Cloud VPC where you want to create your server. You can use the default VPC, another existing VPC, or you can create a new VPC. To create a new VPC, click **New VPC**. | 
| Network interfaces | By default the bare metal server is created with a single primary network interface. You can click the pencil icon to edit the details of the network interface. For example, the subnet or security group that's associated with the interface. To include extra secondary network interfaces, click **New interface**. You can create and assign up to eight PCI network interfaces and up to 20 PCI + VLAN network interfaces for each server. For more information about advanced networking configurations, see [Managing network interfaces for a bare metal server](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers). |
{: caption="Table 1. Bare metal server provisioning selections" caption-side="bottom"}

## Creating a bare metal server by using the API
{: #creating-using-api}
{: api}

Follow these steps to create a bare metal server by using the API.

### Before you begin
{: #api-prereq}

1. Make sure that you set up your API environment. For more information, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

   A good way to learn more about the API is to click **Get sample API call** on the create pages in IBM Cloud console. You can view the correct sequence of API requests and better understand actions and their dependencies.
   {: tip}

2. Make sure that you have created a VPC and a subnet before you create a bare metal server.

### Gathering information for the bare metal server
{: #api-info}

Before you can use API to create bare metal server, you need to gather all the information that is needed. Refer to the table below for detailed instructions.

| Server Details | Listing Options |
|---------|---------|
| Image | [List all images](/apidocs/vpc#list-images) |
| Keys | [List all keys](/apidocs/vpc#list-keys)<br><br>**Note:** If you don't have any available SSH keys, use [Create a key](/apidocs/vpc#create-key) to create one. For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| Subnet | [List all subnets](/apidocs/vpc#list-subnets) |
| Security groups (optional) | [List all security groups](/apidocs/vpc#list-security-groups) |
| Profile | [List all bare metal server profiles](/apidocs/vpc#list-bare-metal-server-profiles) |
| Zone | [List all regions](/apidocs/vpc#list-regions)<br><br>[List all zones in a region](/apidocs/vpc#list-region-zones) |
{: caption="Table 1. Information needed for creating a bare metal server (API)" caption-side="bottom"}

### Creating a bare metal server
{: #api-request}

After you have all the information, use the [Create bare metal server](/apidocs/vpc#create-bare-metal-server) API request to create a bare metal server.

As an example, you create a bare metal server with the following configuration:

* ESXi image ID: "r134-31c8ca90-2623-48d7-8cf7-737be6fc4c3e"
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

```
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token" \
-d '{
    "initialization": {
        "image": {
        "id": "r134-31c8ca90-2623-48d7-8cf7-737be6fc4c3e"
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

The example request uses the JSON processing utility jq to format the response. You can modify the command to use another parsing tool or remove " | jq" to receive an unformatted response.
{: note}

You see a response that is similar to:

```
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
  "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/r134-96cb322a-6a59-4ac4-8783-1f059f87e4a9",
  "id": "r134-96cb322a-6a59-4ac4-8783-1f059f87e4a9",
  "name": "my-vpc"
},
"zone": {
  "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
  "name": "us-south-1"
}
}
```
{: pre}

The status displays "Pending" until the bare metal server is created.
{: tip}

For more information about the API request, see [Create a bare metal server](/apidocs/vpc#create-bare-metal-server).

### Viewing your server
{: #viewing-bare-metal-servers-api}

When the status changes to "Running", use the following request to view it.

```
curl -X GET "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

## Creating a bare metal server by using the CLI
{: #creating-using-cli}
{: cli}

Use the following steps to create a bare metal server by using the IBM Cloud CLI.

### Before you begin
{: #cli-prereq}

1. Make sure that you set up your [CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
2. Make sure that you have created a VPC and a subnet before creating a bare metal server.

For more information, see [Using the CLI to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-cli).

### Gathering information to create an bare metal server
{: #cli-info}

Before you can use the CLI to create bare metal server, you need to gather the information that is needed.

| Server details | Listing options |
|---------|---------|
| Image | [List all images](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#images) |
| Keys | [List all keys](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#keys)<br><br>**Note:** If you don't have any available SSH keys, use [Create a key](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#key-create) to create one. For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| Subnet | [List all subnets](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#subnets) |
| Security groups (optional) | [List all security groups](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference##security-groups) |
| Profile | [List all bare metal server profiles](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#bare-metal-server-profiles) |
| Zone | [List all regions](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#regions)<br><br>[List all zones in a region](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#zones) |
{: caption="Table 2. Information to create a bare metal server by using the CLI" caption-side="bottom"}

### Creating a bare metal server
{: #cli-command}

After you have all the information ready, you can use the CLI to create a bare metal server.

For example, you can create a bare metal server with the following configuration:

* ESXi image ID: "r134-31c8ca90-2623-48d7-8cf7-737be6fc4c3e"
* SSH Key ID: "a6b1a881-2ce8-41a3-80fc-36316a73f803"
* Name of the bare metal server: "my-bare-metal-server"
* A primary network interface with the following configurations:

   * Allows the VLAN with the ID of "4" to attach to the server
   * Name: "my-primary-network-interface"
   * Subnet ID: "7ec86020-1c6e-4889-b3f0-a15f2e50f87e"

* A secondary VLAN interface with ID "4". This VLAN interface is floatable.
* Profile name: "bx2d-metal-192x768"
* Zone: "us-south-1"

```
ibmcloud is bare-metal-server-create --name my-bare-metal-server --zone us-south-1 --profile mx2-metal-96x768 --image r134-31c8ca90-2623-48d7-8cf7-737be6fc4c3e --keys a6b1a881-2ce8-41a3-80fc-36316a73f803 --pnic-subnet 7ec86020-1c6e-4889-b3f0-a15f2e50f87e –pnic-name my-primary-network-interface --pnic-allowed-vlans 4 --network-interfaces '[{"name": "my-vlan-interface", "interface_type": "vlan", "vlan": 4, "allow_interface_to_float": true, "subnet": {"id":"7ec86020-1c6e-4889-b3f0-a15f2e50f87e"}}]' --output JSON
```
{: pre}

### Viewing your server
{: #viewing-bare-metal-servers-cli}

When the status changes to **Running**, use the following command to view it.

```
ibmcloud is bare-metal-server $bare_metal_server_id --output JSON
```
{: pre}

## Next steps
{: #next-step}

When the bare metal server status changes to **Running**, you can try connecting to the VMware ESXi Direct Console User Interface (DCUI) and ESXi's web client. For more information, see [Connecting to ESXi bare metal servers](/docs/vpc?topic=vpc-connect-to-ESXi-bare-metal-servers).
