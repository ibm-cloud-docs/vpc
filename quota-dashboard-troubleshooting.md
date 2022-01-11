---

copyright:
  years:  2021, 2022
lastupdated: "2022-01-11"

keywords: IBM Cloud monitoring, platform metrics, metrics, vpc metrics, vpc monitoring metrics, quota

subcollection: cloud-infrastructure

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}

# Quota metrics dashboard troubleshooting
{: quota-troubleshooting}

If the quota dashboard does not display metrics as expected, the following methods can be used for troubleshooting. 

## Adjust the time range of the dashboard’s view
{: time-range}

When an event that generates a metric is not visible on the dashboard, first check the time interval set for the dashboard. If the interval is too large, or the event is out of the interval's scope, events are not visible.

To adjust the time interval for the dashboard

1. Select the time bar on the dashboard that displays a timeframe, such as *9:20 AM - 10:20 AM*.
2. Adjust the value in the *From* textbox to change the date and time the interval begins.
3. Adjust the value in the *To* text boxes to change the date and time the interval ends.
4. Save the values. The dashboard refreshes.

## Generating a metric
{: generate-metric}


### Create or delete a resource
{: create-delete-instance}

Creating or deleting one of the following resources sends metric events for the associated quota resource name.

- vpc
   - For more information on creating a VPC, see [Getting started with Virtual Private Cloud (VPC)](/docs/vpc?topic=vpc-getting-started&interface=ui#create-and-configure-vpc).
- security-group
   - For more information, see [About security groups](/docs/vpc?topic=vpc-using-security-groups&interface=ui).
- subnet
   - For more information on creating a subnet, see [Using the IBM Cloud console to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console), [Using the REST APIs to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis), or [Using the CLI to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-subnet-cli).
   - For more information on deleting a subnet, see [Deleting VPC resources by using the UI](/docs/vpc?topic=vpc-deleting-using-console), [Deleting VPC resources by using the CLI](/docs/vpc?topic=vpc-deleting-using-cli), or [Deleting VPC resources by using the API](/docs/vpc?topic=vpc-deleting-using-api).
- floating-ip
   - For more information on creating a floating IP, see [Using the IBM Cloud console to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console), [Using the REST APIs to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis), or [Using the CLI to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-subnet-cli).
   - For more information on deleting a floating IP, see [Deleting VPC resources by using the UI](/docs/vpc?topic=vpc-deleting-using-console), [Deleting VPC resources by using the CLI](/docs/vpc?topic=vpc-deleting-using-cli), or [Deleting VPC resources by using the API](/docs/vpc?topic=vpc-deleting-using-api).
- network-acl
   - For more information, see [About network ACLs](/docs/vpc?topic=vpc-using-acls).
- load-balancer
   - For more information, see [Creating a Network Load Balancer for VPC](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer&interface=ui) and [Deleting an IBM Cloud Network Load Balancer for VPC](/docs/vpc?topic=vpc-nlb-deleting&interface=ui).
<!--- volume
   - For more information, see [Creating block storage volumes](/docs/vpc?topic=vpc-creating-block-storage) and [Delete a block storage data volume](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#delete).--->

### Add or remove a rule
{: change-rule}

Adding or removing a rule for one of the following resources sends metric events for the associated quota resource name.

- network-acl-rule
   - [Working with ACLs and ACL rules](/docs/vpc?topic=vpc-using-acls#working-with-acls-and-acl-rules).
- security-group-rule
   - [About security groups](/docs/vpc?topic=vpc-using-security-groups&interface=ui).

### Create, delete, or resize an instance
{: update-instance}

Creating, deleting, or resizing a virtual server instance sends metric events for the following quota resource names.

- instance-vcpu
- instance-memory

For more information, see:
   - [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers) or [Creating virtual server instances by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers-cli).
   - [Managing virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui).
   - [Resizing a virtual server instance](/docs/vpc?topic=vpc-resizing-an-instance&interface=ui).

### Create or delete a dedicated host
{: create-delete-dedicated-host}

Creating or deleting a dedicated host generates a metric for the 'instance-vcpu' resource.

- instance-vcpu
   - For more information, see [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances&interface=ui) and [Managing dedicated hosts and groups](/docs/vpc?topic=vpc-manage-dedicated-hosts-groups&interface=ui).
