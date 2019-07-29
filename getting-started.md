---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-29"

keywords: Early Access, Virtual Private Cloud, permissions, infrastructure, VPC, SSH key, CLI, API, console, public gateway, floating IP, IP ranges, BYoIP 

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Getting started with Virtual Private Cloud (Early Access)
{: #getting-started}

With {{site.data.keyword.vpc_full}} (VPC) Early Access, you can use the UI, CLI, and API to quickly provision VPC infrastructure resources. This Early Access release offers basic VPC functionality that will be expanded in future releases. [Learn how to get access to VPC](/docs/vpc?topic=vpc-getting-access#getting-access).
{:shortdesc}

## Before you begin
{: #prereqs}

1. Set up your account to access VPC (early access). Make sure that your account is [upgraded to a paid account](/docs/account?topic=account-accountfaqs#changeacct){: new_window}. 
2. Make sure you have a public SSH key, which will be used to connect to the virtual server instance. For example, generate an SSH key on your Linux server by running the following command:

    ```
    ssh-keygen -t rsa
    ``` 
    {: pre}

   This command generates two files, `id_rsa` and `id_rsa.pub`. The generated public key is in the `id_rsa.pub` file under an ``.ssh`` directory in your home directory, for example, ``.../.ssh/id_rsa.pub``.

For a complete list of prerequisites for using the API and CLI, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

## Creating and configuring a VPC
{: #create-and-configure-vpc}

To create and configure your VPC and other attached resources:

1. Create a VPC.
2. Create subnets in one or more zones. You can create subnets in suggested prefix ranges or in your own IP ranges that you bring to IBM Cloud.
3. Attach a public gateway to allow all resources in a subnet to communicate with the public internet.
4. Create virtual server instances with the core and RAM configuration that's right for your workload.
5. Attach block storage volumes to add secondary data volumes to your instances.
5. Reserve and associate floating IP addresses to enable instances to be reachable from the internet.
5. Deploy your service or applications across the instances.

## Next steps
{: #getting-started-next-steps}

To learn how to create VPC resources, see these tutorials:

* [Creating a VPC using the {{site.data.keyword.cloud_notm}} console](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console)
* [Creating a VPC using the CLI](/docs/vpc?topic=vpc-creating-a-vpc-using-cli)
* [Creating a VPC using the REST APIs](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis)

For a complete list of features that are included in this release, see [Supported features](/docs/vpc?topic=vpc-supported-features).

For a general overview of VPC, see [About Virtual Private Cloud](/docs/vpc?topic=vpc-about-vpc#about-vpc).
