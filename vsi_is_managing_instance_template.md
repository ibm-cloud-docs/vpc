---

copyright:
  years: 2020, 2024
lastupdated: "2024-03-05"

keywords: manage instance template, auto scale, UI, console, edit instance template, duplicate instance template, delete instance template

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing an instance template 
{: #managing-instance-template}

After you create an instance template, you can view it to display its configuration details, duplicate it, delete it, or use it to create a virtual server instance.   
{: shortdesc}

To manage your instance template from the Instance templates for VPC page, complete the following steps.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Instance templates**.
2. On the **Instance templates for VPC** page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the instance template that you want to manage and select from the options, **Duplicate**, **Create virtual server**, or **Delete**. 
3. To view the details of an instance template, click the name of the instance template to access the **Instance template details** page. 

## Duplicating an instance template
{: #duplicating-instance-template}

Duplicating an instance template makes a copy of the current settings and presents you with the **New instance template** page, with fields prepopulated. You can make any necessary changes and then create a new instance template based on the instance template that you duplicated.

## Creating a virtual server
{: #creating-instance-from-template}

You can provision a virtual server instance directly from an instance template. This instance is not associated with any instance group for auto scaling. When you create a virtual server instance from an instance template, the instance is created with the configuration that is specified in the instance template.

## Deleting an instance template
{: #deleting-instance-template}

If you no longer need the instance template, you can delete the instance template to permanently remove it from your account. 

An instance template that is specified in an instance group cannot be deleted. 
{: note}

## Using image templates with a reservation
{: #using-instance-templates-with-reservation}

Keep the following information in mind when you use an image template with a reservation. For more information about reservations, see [About Reservations for VPC](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc).

* Instance templates inherit the new reservation identifier attribute to attach an instance to a reservation.

* An instance template that is created with a reservation identifier must specify a matching profile as the specified reservation resource. A mismatch results in a 409 error.

* You can't change an instance template after you create it. Therefore, any instances that are created with this template are attached only to the specified reservation.
