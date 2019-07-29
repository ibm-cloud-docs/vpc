---

copyright:
  years: 2019
lastupdated: "2019-07-29"

keywords: address prefix, region, subnet, zone, reserved, IP, ranges, deleting, creating, CIDR

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:download: .download}


# Bring your own subnet
{: #configuring-address-prefixes}

You can bring public IPv4 address ranges from your on-premises deployment to your {{site.data.keyword.vpc_full}} (VPC) by creating *address prefixes*. You can then create subnets within these IP ranges.

Each zone of your VPC is assigned a default address prefix, which specifies the address range in which subnets can be created. If the default address scheme does not suit your requirements, you can customize the address prefixes.
{:shortdesc}

When a new VPC is created, the default address prefixes are assigned to each zone in the region, as shown in the following table.

Zone         | Address prefix
-------------|---------------
us-south-1   | 10.240.0.0/18
us-south-2   | 10.240.64.0/18
us-south-3   | 10.240.128.0/18

To bring your own subnets:
1. Create a VPC.
2. For each zone in which you plan to create subnets, create one or more address prefixes.
3. When you create subnets in each zone, specify IP ranges that are within one of the address prefixes you created for that zone.

If you use an IP range outside of those ranges defined by [RFC 1918](https://tools.ietf.org/html/rfc1918) (`10.0.0.0/8`, `172.16.0.0/12`, or `192.168.0.0/16`) for a subnet, the instances attached to that subnet might be unable to reach parts of the public internet.
{:tip}

<!-- ## IBM Cloud console example
{: #ibm-cloud-console-example}
The following example shows how you can use the IBM Cloud console to bring your own IP addresses for subnets in the 'us-south-1' and 'us-south-2' zones of your VPC.
1. Create a VPC

Create an address prefix named `my-first-prefix` in the 'us-south-1' zone:
  
   ```
   ibmcloud is vpc-address-prefix-create my-first-prefix $VPC us-south-1 172.16.0.0/23 
   ```
   {: pre}

1. Create an address prefix named `another-prefix` in the 'us-south-2' zone:
  
   ```
   ibmcloud is vpc-address-prefix-create another-prefix $VPC us-south-2 172.16.2.0/23 
   ```
   {: pre}

1. Create a subnet named `my-subnet` within your new address prefix in the 'us-south-1' zone:
  
   ```
   ibmcloud is subnet-create my-subnet $vpc us-south-1 --ipv4-cidr-block "172.16.0.0/25"
   ```
   {: pre}

1. Create a subnet named `another-subnet` within your new address prefix in the 'us-south-2' zone:
  
   ```
   ibmcloud is subnet-create another-subnet $vpc us-south-1 --ipv4-cidr-block "172.16.2.0/25"
   ```
   {: pre} -->

### Address prefixes and the IBM Cloud console UI
{: #address-prefixes-and-the-ibm-cloud-console-ui}

When you create a VPC using the IBM Cloud Console UI, the system selects your address prefix automatically and requires you to create a subnet within that default prefix. If this address scheme does not suit your requirements, you can customize the address prefixes after you create the VPC. You can then create subnets in your customized address prefixes, and delete the subnet you created with the default prefix.

This workaround is needed to use BYOIP through the IBM Cloud Console UI.
{:note}



## CLI example
{: #cli-example-byoip}
The following example shows how you can use the CLI to bring your own IP addresses for subnets in the 'us-south-1' and 'us-south-2' zones of your VPC.
1. Create an address prefix named `my-first-prefix` in the 'us-south-1' zone:
  
   ```
   ibmcloud is vpc-address-prefix-create my-first-prefix $VPC us-south-1 172.16.0.0/23 
   ```
   {: pre}

1. Create an address prefix named `another-prefix` in the 'us-south-2' zone:
  
   ```
   ibmcloud is vpc-address-prefix-create another-prefix $VPC us-south-2 172.16.2.0/23 
   ```
   {: pre}

1. Create a subnet named `my-subnet` within your new address prefix in the 'us-south-1' zone:
  
   ```
   ibmcloud is subnet-create my-subnet $vpc us-south-1 --ipv4-cidr-block "172.16.0.0/25"
   ```
   {: pre}

1. Create a subnet named `another-subnet` within your new address prefix in the 'us-south-2' zone:
  
   ```
   ibmcloud is subnet-create another-subnet $vpc us-south-1 --ipv4-cidr-block "172.16.2.0/25"
   ```
   {: pre}



