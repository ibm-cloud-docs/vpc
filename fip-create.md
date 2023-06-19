---

copyright:
  years: 2022
lastupdated: "2022-11-11"

keywords: floating ip, reserving, bare metal, vnic, public gateways

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating network interfaces with floating IP addresses
{: #fip-working}

You can reserve a floating IP address, then add it to a network interface to allow traffic from the internet to access your VPC public gateway, virtual server instance, or Bare Metal server.

## Adding a floating IP address to a virtual server instance
{: #fip-add-vsi}

To add a floating IP to a network interface to allow internet traffic to access your VSI, perform the following procedure:

1. In the {{site.data.keyword.cloud_notm}} console, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
1. Click the name of a virtual server instance that includes the network interface that you want to edit. Or you can add a new network interface to the virtual server instance.
1. On the instance details page, find the **Network interfaces** section.
1. If you are adding a floating IP address to the virtual server instance for the first time, identify the primary network interface in the **Network interfaces** section of the Instance details page.

    By default, the first interface is named `eth0`. Initially associating the floating IP address with the primary network interface helps
establish the data path. Later, you can associate the floating IP to a different network interface if you desire.

1. Click the pencil icon to edit the primary network interface.
1. On the **Edit network interface** page, locate the **Floating IP address** field.
1. You can select an existing floating IP address to add to the network interface, or you can reserve a new floating IP and add it to the network interface. To do so:
   1. Select **Reserve a new floating IP**.
   1. Enter your geography, region, and zone information.
   1. Provide the details for the floating IP, including its name and resource group.
   1. (Optional) Add any tags you want associated with the IP.
   1. (Optional) Add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](https://cloud.ibm.com/docs/account?topic=account-access-tags-tutorial).
   1. Select **Reserve**.
1. After making your selections, click **Save**.

## Adding a floating IP address to a Bare Metal server
{: #fip-add-bare-metal}

To add your floating IP to a network interface to allow internet traffic to access your Bare Metal server, perform the following procedure:

To associate multiple floating IPs to a network interface, make sure that both **Allow IP spoofing** and **Enable infrastructure NAT** are disabled on the network interface.
{: important}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Bare Metal server**.
1. Under the **Network interfaces** section of the **Bare metal server details** page, click the pencil icon of the interface you want to associate the floating IP with.
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

## Creating a public gateway for VPC with a floating IP address
{: #fip-create-public-gateway}

A public gateway allows all attached resources to communicate with the public internet. Your public gateway automatically assigns a floating IP to a subnet to allow internet traffic to access it.

To create a public gateway with a floating IP, refer to [Creating public gateways](/docs/vpc?topic=vpc-create-public-gateways&interface=ui).
