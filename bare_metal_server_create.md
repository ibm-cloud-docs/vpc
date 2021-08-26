---

copyright:
  years: 2021
lastupdated: "2021-08-26"

keywords: bare metal servers, create, tutorial

subcollection: vpc

---

{:beta: .beta}
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

# Creating bare metal servers on VPC
{: #creating-bare-metal-servers}

This tutorial walks through the procedure to create a bare metal server on the VPC with the configuration of your choice.

This is a Beta feature that requires special approval. Contact your IBM Sales representative if you are interested in getting access.
{: beta}

Start [creating a bare metal server](/vpc-ext/provision/bm) on VPC now.
{: tip}

## Prerequisites
{: #prereq}

You must be assigned the following IAM (Identity and Access Management) roles:

* **Editor** of the target resource group: This role allows you to create bare metal servers in this resource group.
* **Bare Metal Console Administrator**: This role allows you to access ESXi Direct Console User Interface (DCUI), which is the administrator console of ESXi.
* **Bare Metal Advanced Network Operator**: This role allows you to modify the Allow IP spoofing and Enable Infrastructure NAT (network address translation) configuration on a bare metal server's network interface.

  Source IP spoofing and infrastructure NAT configuration cannot be modified in Beta. By default, source IP spoofing is not allowed on the network interface and infrastructure NAT is enabled.
  {: important}

**Bare Metal Console Administrator** and **Bare Metal Advanced Network Operator** are two new roles that are added with this feature. This roles are not applied automatically.
{: note}

   To check whether you are assigned the required roles, go to the [IAM Users ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/iam/users){: new_window} page in the IBM Cloud console and select your account under **User**, then select **Access policies**. You should see an access policy that assigns you the **Editor** (or above) role, **Bare Metal Console Administrator** role, and **Bare Metal Advanced Network Operator** role to the **Resource Attributes** of the target bare metal server. Otherwise, you would need to contact an administrator of your account to assign you the roles by taking the steps below:

 **Step 1.** Go to the [IAM Users ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/iam/users){: new_window} page in the IBM Cloud console and select the target user.

 **Step 2.** On the **Access policies** tab, click **Assign access**.

 **Step 3.** In the **Assign users additional access** section, select **IAM services** and complete the following tasks:
    * From the **What type of access do you want to assign?** list, select **VPC Infrastructure Services**.
    * Under **How do you want to scope the access?**, select **All resources** or **Resources based on selected attributes**. The administrators can further scope the access by adding attributes.
    * In the **Platform access** area, select from **Editor** or **Administrator**.
    * In the **Service access** area, select **Bare Metal Console Administrator** and **Bare Metal Advanced Network Operator**.
    * Scroll to the end of the page and click **Add**.
    * Review the **Access summary** side pane, and click **Assign**.

For more information about the IAM roles, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started).

## Creating a bare metal server by using the UI
{: #creating-using-ui}

To create a bare metal server using the IBM Cloud console:

**Step 1.** In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Bare metal servers**

**Step 2.** Click **Create**

**Step 3.** On **New bare metal server for VPC**, specify the following information:

   * Give the bare metal server a name
   * Select the resource group that it will belong to.
   * Select the data center where the bare metal will be deployed in under **Location**.
 <!--* Add tags to the bare metal server as needed.-->
 
**Step 4.** Select the bare metal server's operating system and version from an image. `ibm-esxi-7-amd64-1` installs a licensed ESXi 7.x hypervisor. `ibm-esxi-7-byol-amd64-1` installs bring-your-own-license ESXi 7.x and allows you to handle the licensing for your bare metal server.

**Step 5.** Select a profile. For more information, see [Profile](/docs/vpc?topic=vpc-bare-metal-servers-profile).

**Step 6.** Select an existing public SSH key or click **New SSH key** to add a new one.
   
   You must specify at least one SSH key. This key will be used to automatically generate a password required for accessing VMware ESXi Direct Console User Interface (DCUI) and ESXi’s web client.
   {: note}

**Step 7.** Paste your user data to the **User data (optional)** field or click **Import user data** to upload from your local machine. For example, you can enable SSH by adding the script below to the **User data (optional)** field:

   ```
   vim-cmd hostsvc/enable_ssh
   vim-cmd hostsvc/start_ssh
   echo "<public ssh key>" >> /etc/ssh/keys-root/authorized_keys
   ```
For more information about user data, see [User data](/docs/vpc?topic=vpc-user-data).

**Step 8.** Select a VPC that the bare metal server will be part of, or create a new one.

**Step 9.** Configure network interfaces

   This tutorial creates a primary network interface and a secondary VLAN interface on the bare metal server. For more information, see [Managing network interfaces of bare metal servers](/docs/vpc?topic=vpc-managing-bare-metal-servers).
   {: note}
   
   **9.1** Edit the primary PCI interface.
   
   * Click **Edit** of the default PCI interface.
   * Edit the name of the PCI interface.
   * Select a subnet. You must select a subnet that is in the VPC specified in **Step 8**.
   * Click **Save**.

<!--   * Specify the primary IPv4 address. **Note:** If specified, it must be an available address on the network interface's subnet. If unspecified, an available address on the subnet will be automatically selected. * Configure **Allow IP spoofing**. If this is off, source IP spoofing is prevented on this interface; If on, source IP spoofing is allowed on this interface. * Configure **Enable infrastructure NAT**. Turning this on allows the VPC infrastructure to perform any needed NAT operations. If off, the packet would be passed unmodified to/from the network interface, allowing the workload to perform any needed NAT operations.-->

   **9.2** Add secondary network interfaces.
   
   You can add one or more secondary network interfaces. To add a secondary VLAN interface:
   
   * Click **New interface**.
   * Edit the name of the PCI interface.
   * Select a subnet. You must select a subnet that is in the VPC specified in **Step 8**.
   * Select **VLAN** in **Interface type**.
   * Specify a **VLAN ID**.
   * Configure **Allow interface to float**. This specify whether the interface can float to other servers within the same resource group. If this is on, the interface will float automatically if the network detects a GARP or RARP on another bare metal server in the resource group.
   * Click **Create**.
<!--* Configure **Allow IP spoofing**.   * Configure **Enable infrastructure NAT**. -->

The VLAN interface is associated with the primary PCI interface if no extra PCI interface is created. If you plan to create extra PCI interfaces, you must specify which PCI interface that the VLAN interface gets associated with.
{: note}

**Step 10.** Review the configuration and estimated cost in **Summary**.

**Step 11.** Click **Create**.

### Viewing your server
{: #viewing-bare-metal-servers-ui}

To view the bare metal server, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Bare metal servers**, click the name of the bare metal server.

## Creating a bare metal server by using the REST API
{: #creating-using-api}

Take the following steps to create a bare metal server by using REST API.

### Before you begin
{: #api-prereq}

1. Ensure that you have set up your API environment following [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment) if this is the first time you using the REST API on IBM Cloud.

   In brief, you need to complete the following steps:
   
   * Store your API key as a variable 
   * Get an IBM Identity and Access Management (IAM) token
   * Store the API endpoint as a variable

   A good way to learn more about the API is to click **Get sample API call** on the create pages in IBM Cloud console. You can view the correct sequence of API requests and better understand actions and their dependencies.
  {: tip}

2. Make sure that you have created a VPC and a subnet before creating a bare metal server.

   (1) Use the following API request to create a VPC:
   
   ```
   curl -X POST "$vpc_api_endpoint/v1/vpcs?version=2021-03-09&generation=2" \
   -H "Authorization: $iam_token" \
   -d '{
         "name": "my-vpc"
   }'
   ```
   {: pre}

   To use the example API request to create a VPC, make sure that you have already stored your own IAM token and API endpoint in the `$iam_token` and `$vpc_api_endpoint` variables.
   {: note}
   
   (2) Use the following API request to create a subnet:
   
   ```
   curl -X POST "$vpc_api_endpoint/v1/subnets?version=2021-03-09&generation=2" \
   -H "Authorization: $iam_token" \
   -d '{
     "name": "my-subnet-1",
     "total_ipv4_address_count": 256,
     "ip_version": "ipv4",
     "zone": { "name": "us-south-1" },
     "vpc": { "id": "a0819609-0997-4f92-9409-86c95ddf59d3" }
   }'
   ```
   {: pre}
  
To use the example API request to create a VPC, make sure that you have already stored your own IAM token and API endpoint in the `$iam_token` and `$vpc_api_endpoint` variables. At the same time, replace the example VPC ID with the ID of your VPC.
  {: note}

For more information about the API requests, see [Virtual Private Cloud API](/apidocs/vpc).

### Gathering information for the bare metal server
{: #api-info}

Before you can use API to create bare metal server, you need to gather all the information that is needed. Refer to the table below for detailed instructions.

| Server Details | Listing Options |
|---------|---------|
| Image | [List all images](/apidocs/vpc#list-images) |
| Keys | [List all keys](/apidocs/vpc#list-keys)<br><br>**Note:** If you don't have any available SSH keys, use [Create a key](/apidocs/vpc#create-key) to create one. For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| Subnet | [List all subnets](/apidocs/vpc#list-subnets) |
| Profile | [List all bare metal server profiles](/apidocs/vpc#list-bare-metal-server-profiles) |
| Zone | [List all regions](/apidocs/vpc#list-regions)<br><br>[List all zones in a region](/apidocs/vpc#list-region-zones) |
{: caption="Table 1. Information needed for creating a bare metal server (API)" caption-side="top"}

<!--| Security groups (optional) | [List all security groups](/apidocs/vpc#list-security-groups) |-->

### Creating a bare metal server
{: #api-request}

After you have all the information ready, use the [Create bare metal server](/apidocs/vpc#create-bare-metal-server) API request to create a bare metal server.

For example, to create a bare metal server with the following configuration:

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

The API request would be similar to:

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

You would see a response that is similar to:

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

When the status has turned to "Running", use the following request to view it.

```
curl -X GET "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

## Creating a bare metal server by using the CLI
{: #creating-using-cli}

Take the following steps to create a bare metal server by using the IBM Cloud CLI.

### Before you begin
{: #cli-prereq}

1. Make sure that you set up your [CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
2. Make sure that you have created a VPC and a subnet before creating a bare metal server.

For more information, see [Using the CLI to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-cli).

For the beta release, you must enable the bare metal servers CLI by running: `export IBMCLOUD_IS_FEATURE_BARE_METAL_SERVER=true`
{: important}

### Gathering information to create an instance by using the CLI
{: #cli-info}

Before you can use the CLI to create bare metal server, you need to gather the information that is needed.

| Server Details | Listing Options |
|---------|---------|
| Image | [List all images](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#images) |
| Keys | [List all keys](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#keys)<br><br>**Note:** If you don't have any available SSH keys, use [Create a key](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#key-create) to create one. For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| Subnet | [List all subnets](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#subnets) |
| Profile | [List all bare metal server profiles](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#bare-metal-server-profiles) |
| Zone | [List all regions](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#regions)<br><br>[List all zones in a region](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#zones) |
{: caption="Table 2. Information needed for creating a bare metal server (CLI)" caption-side="top"}

<!--| Security groups (optional) | [List all security groups](/apidocs/vpc#list-security-groups) |-->

### Creating a bare metal server
{: #cli-command}

After you have all the information ready, you can use the CLI to create a bare metal server.

For example, to create a bare metal server with the following configuration:

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

```
ibmcloud is bare-metal-server-create --name my-bare-metal-server --zone us-south-1 --profile mx2-metal-96x768 --image r134-31c8ca90-2623-48d7-8cf7-737be6fc4c3e --keys a6b1a881-2ce8-41a3-80fc-36316a73f803 --pnic-subnet 7ec86020-1c6e-4889-b3f0-a15f2e50f87e –pnic-name my-primary-network-interface --pnic-allowed-vlans 4 --network-interfaces '[{"name": "my-vlan-interface", "interface_type": "vlan", "vlan": 4, "allow_interface_to_float": true, "subnet": {"id":"7ec86020-1c6e-4889-b3f0-a15f2e50f87e"}}]' --output JSON
```
{: pre}

### Viewing your server
{: #viewing-bare-metal-servers-cli}

When the status has turned to **Running**, use the following command to view it.

```
ibmcloud is bare-metal-server $bare_metal_server_id --output JSON
```
{: pre}

## Next steps
{: #next-step}

When the bare metal server moves to **Running**, you can try connecting to the VMware ESXi Direct Console User Interface (DCUI) and ESXi's web client.

For more information, see [Connecting to ESXi bare metal servers](/docs/vpc?topic=vpc-connect-to-ESXi-bare-metal-servers).
