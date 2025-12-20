---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-20"

keywords: floating ip, reserving, bare metal, vnic, public gateways

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating network interfaces with floating IP addresses
{: #fip-working}

You can reserve a floating IP address, then add it to a network interface to allow traffic from the internet to access your VPC public gateway, virtual server instance, or Bare Metal server.

## Adding floating IP addresses to network interfaces with the console
{: #fip-add-ni-ui}
{: ui}

You can add floating IP addresses to network interfaces with the console.

### Adding a floating IP address to a virtual server instance with the console
{: #fip-add-vsi-ui}

To add a floating IP to a network interface to allow internet traffic to access your VSI, perform the following procedure:

1. In the {{site.data.keyword.cloud_notm}} console, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
1. Click the name of a virtual server instance that includes the network interface that you want to edit. Or you can add a new network interface to the virtual server instance.
1. On the instance details page, find the **Network interfaces** section.
1. If you are adding a floating IP address to the virtual server instance for the first time, identify the primary network interface in the **Network interfaces** section of the Instance details page.

    By default, the first interface is named `eth0`. Initially associating the floating IP address with the primary network interface helps
establish the data path. Later, you can associate the floating IP to a different network interface if you desire.

1. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to edit the primary network interface.
1. On the **Edit network interface** page, locate the **Floating IP address** field.
1. You can select an existing floating IP address to add to the network interface, or you can reserve a new floating IP and add it to the network interface. To do so:
   1. Select **Reserve a new floating IP**.
   1. Enter your geography, region, and zone information.
   1. Provide the details for the floating IP, including its name and resource group.
   1. (Optional) Add any tags you want associated with the IP.
   1. (Optional) Add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
   1. Select **Reserve**.
1. After making your selections, click **Save**.

### Adding a floating IP address to a Bare Metal server with the console
{: #fip-add-bare-metal-ui}

To add your floating IP to a network interface to allow internet traffic to access your Bare Metal server, perform the following procedure:

To associate multiple floating IPs to a network interface, make sure that both **Allow IP spoofing** and **Enable infrastructure NAT** are disabled on the network interface. Note that **Enable infrastructure NAT** is not supported on LinuxONE Bare Metal servers.
{: important}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare Metal server**.
1. Under the **Network interfaces** section of the **Bare metal server details** page, click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") of the interface you want to associate the floating IP with.
1. On the **Edit network interface** page, locate the **Floating IP address** field.
1. You can select an existing floating IP address to add to the network interface, or you can reserve a new floating IP and add it to the network interface. To do so:
   1. Select **Reserve a new floating IP**.
   1. Enter your geography, region, and zone information.
   1. Provide the details for the floating IP, including its name and resource group.
   1. Add any tags you want associated with the IP.
   1. Select **Reserve**.

   Associating a floating IP to the secondary network interface only works when you configure a default gateway to the secondary network interface within the operating system.
    {: note}

1. After making your selections, click **Save**.

### Adding a floating IP address to a virtual network interface with the console
{: #fip-create-vni-ui}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual network interfaces**.
1. Click the name of the virtual network interface in the table to view its Details page.
1. In the Floating IPs section, click **Attach**.

   * If a floating IP is already attached, the virtual network interface will not be accepted as a file share mount target.
   * If infrastructure NAT is enabled, at most one floating IP can be attached.
1. In the Attach floating IP side panel, choose one of the following options:

   * Click **Reserve new floating IP** to create floating IP, complete the information, and then click **Reserve**.
   * Select an existing floating IP address from the menu, then click **Attach**.

## Adding floating IP addresses to network interfaces with the CLI
{: #fip-add-ni-cli}
{: cli}

You can add floating IP addresses to network interfaces with the CLI.

### Adding a floating IP address to a virtual server instance with the CLI
{: #fip-add-vsi-cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

First get the instance, in order to retrieve the NIC name:

   ```sh
   ibmcloud is instance my-instance
   ```
   {: pre}


Next, create a floating IP that targets that instance and the NIC:

   ```sh
   ibmcloud is floating-ip-reserve my-ip --nic eth0 --in my-instance
   ```
   {: pre}


### Adding a floating IP address to a Bare Metal server with the CLI
{: #fip-add-bare-metal-cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

First get the instance, in order to retrieve the NIC name:

   ```sh
   ibmcloud is bare-metal-server my-instance
   ```
   {: pre}

Next, create a floating IP that targets that instance and the NIC:

   ```sh
   ibmcloud is floating-ip-reserve my-ip --nic eth0 --in my-bare-metal-server
   ```
   {: pre}

### Adding a floating IP address to a virtual network interface with the CLI
{: #fip-create-vni-cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

You can directly associate a floating IP to a VNI with the following command:

   ```sh
   ibmcloud is floating-ip-reserve my-ip --nic 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3
   ```
   {: pre}

## Adding floating IP addresses to network interfaces with the API
{: #fip-add-ni-api}
{: api}

You can add floating IP addresses to network interfaces with the API.

### Adding a floating IP address to a virtual server instance with the API
{: #fip-add-vsi-api}

To add a floating IP address to a virtual server instance with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. If you do no know the VNIC ID of your VSI, acquire it with the following command:

   ```sh
   curl -H "Authorization: Bearer $TOKEN" -X POST "https://us-south.iaas.cloud.ibm.com/v1/instances/$INSTANCE/network_interfaces?generation=2&version=2019-10-01"
   ```
   {: pre}

1. Then create the floating IP with the following command:

   ```sh
   curl -H "Authorization: Bearer $TOKEN" -X POST "https://us-south.iaas.cloud.ibm.com/v1/floating_ips?generation=2&version=2019-10-01" -d '{"name":"my-floating-ip", "target":{"id":"69e55145-cc7d-4d8e-9e1f-cc3fb60b1793"}}'
   ```
   {: pre}

### Adding a floating IP address to a Bare Metal server with the API
{: #fip-add-bare-metal-api}

To add a floating IP address to a virtual server instance with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. If you do no know the VNIC ID of your VSI, acquire it with the following command:

   ```sh
   curl -H "Authorization: Bearer $TOKEN" -X POST "https://us-south.iaas.cloud.ibm.com/v1//bare_metal_servers/$SERVER_ID/network_interfaces?generation=2&version=2019-10-01"
   ```
   {: pre}

1. Then create the floating IP with the following command:

   ```sh
   curl -H "Authorization: Bearer $TOKEN" -X POST "https://us-south.iaas.cloud.ibm.com/v1/floating_ips?generation=2&version=2019-10-01"  -d '{"name":"my-floating-ip", "target":{"id":"69e55145-cc7d-4d8e-9e1f-cc3fb60b1793"}}'
   ```
   {: pre}

### Adding a floating IP address to a virtual network interface with the API
{: #fip-create-vni-api}

To add a floating IP address to a virtual server instance with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Run the following command to create your floating UP:

   ```sh
   curl -H "Authorization: Bearer $TOKEN" -X POST "https://us-south.iaas.cloud.ibm.com/v1/floating_ips?generation=2&version=2019-10-01" d '{"name":"my-floating-ip", "target":{"id":"69e55145-cc7d-4d8e-9e1f-cc3fb60b1793"}}'
   ```
   {: pre}

## Creating a public gateway for VPC with a floating IP address
{: #fip-create-public-gateway}

A public gateway allows all attached resources to communicate with the public internet. Your public gateway automatically assigns a floating IP to a subnet to allow internet traffic to access it.

To create a public gateway with a floating IP, refer to [Creating public gateways](/docs/vpc?topic=vpc-create-public-gateways&interface=ui).
