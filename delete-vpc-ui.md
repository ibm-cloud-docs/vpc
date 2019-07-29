---

copyright:
  years: 2019
lastupdated: "2019-07-29"

keywords: vpc, delete, resources, ui, console

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Deleting a VPC using the IBM Cloud console
{: #deleting-using-console}

Before you can delete a VPC in the IBM Cloud console, you must delete all of the VPC's public gateways, subnets, and attached resources.
{:shortdesc}

To delete a VPC using the console:

1. Find all subnets in the VPC.  Click **VPCs** in the navigation pane and select your VPC. 
2. On the VPC details page, go to the **Subnets in this VPC** list and select a subnet to view its details.
3. Delete any resources that are attached to the subnet. In the navigation pane, click **Attached resources**. Select each attached instance to go its details page and click the delete icon. 

  Although the resource's status immediately changes to **Deleting**, it can take up to 30 minutes for the delete operation to complete. The subnet can't be deleted until all attached resources are no longer displayed in the console.
  {: tip}

4. After all attached resources are deleted, go back to the subnet's details page and click the delete icon.
5. After you delete all subnets, go back to the details page for the VPC and click the delete icon. The VPC and any public gateways are deleted.
