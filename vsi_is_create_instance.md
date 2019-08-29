---

copyright:
  years: 2018, 2019 
lastupdated: "2019-07-29"

keywords: vpc, vsi, virtual server instance, creating, UI, console

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Creating virtual server instances using the UI
{: #creating-virtual-servers}

You can create one or more instances in your VPC by using the {{site.data.keyword.cloud_notm}} console.
{:shortdesc}

Before you can create an instance, you need to [create an IBM Cloud VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{:important}

To create an instance:
1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
2. Click **New instance** and enter the following information:

    <table>
    <CAPTION>Table 1. Instance provisioning selections</CAPTION>
    <THEAD>
    <TR>
    <th>Field</th>
    <th>Value</th>
    </TR>
    </THEAD>
    <TBODY>
    <tr>
    <td>Name </td>
    <td>A name is required for your virtual server instance.</td>
    </tr>
    <tr>
    <td>Virtual private cloud</td>
    <td>Specify the IBM Cloud VPC where you want to create your instance.</td>
    </tr>
    <tr>
    <td>Resource group</td>
    <td>Select a resource group for the instance.</td>
    </tr>
    <tr>
    <td>Location</td>
    <td>Locations are composed of regions (specific geographic areas) and zones (fault tolerant data centers within a region). Select the location where you want your virtual server instance to be created.</td>
    </tr>
    <tr>
    <td>Profile</td>
    <td><p>
    Select from popular profiles or all available vCPU and RAM combinations. The following families are supported:
    <ul>
    <li>Balanced</li>
    <li>Compute</li>
    <li>Memory</li>
    </ul>
    </p>
    <p>For more information, see [Profiles](/docs/vpc?topic=vpc-profiles).</p>
    </td>
    </tr>
    <td>SSH Key</td>
    <td>
    <p>You must select an existing SSH key or upload a new SSH key to use before you can create the instance. SSH keys are used to securely connect to the instance after it's running.</p>
    <p>Note: Alpha-numeric combinations are limited to 100 characters.</p>
    <p>For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys).</p></td>
    </tr>
    <tr>
    <td>User data</td>
    <td>
    <p>You can add user data that automatically performs common configuration tasks or runs scripts. <p>For more information, see [User data](/docs/vpc?topic=vpc-user-data).</p>
    </td>
    </tr>
   <tr>
    <td>Boot volume</td>
    <td><p>The default boot volume size for all profiles is 100 GB.</p>
    </td>
    </tr>
        <tr>
    <td>Image</td>
    <td><p>All images use cloud-init, which allows you to enter user metadata associated with the instance for post provisioning scripts.</p>
    </td>
    </tr>
    <tr>
    <td>Attached block storage volume</td>
    <td><p>You can add one or more secondary data volumes to be included when you provision the instance. To add volumes, click **New block storage volume**.</p>
    </td>
    </tr>
    <tr>
    <td>Network interfaces</td>
    <td>Assign networking options to connect into the IBM Cloud VPC. You can create and assign up to 5 network interface cards to each instance.</td>
    </tr>
    </TBODY>
    </table>

    
3. Click **Create virtual server instance** when you are ready to provision.

Do you prefer to create an instance using the CLI? For more information, see [Creating an instance using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers-cli).
{: tip}

## Next steps
{: #next-steps-creating-virtual-servers-ui}

A series of emails are sent to your administrator: acknowledgment of the virtual server instance order, order approval and processing, and a message stating the instance is created.

After the instance is created, associate a floating IP address to the instance. Then you can connect to your instance. For more information, see [Connecting to your Linux instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux) or [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows).
