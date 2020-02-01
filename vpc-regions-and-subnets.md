---

copyright:
  years: 2019, 2020
lastupdated: "2019-10-31"

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

When a new VPC is created, the default address prefixes are assigned to each zone in the region, as shown in the Table 1.

Zone         | Address prefix
-------------|---------------
us-south-1   | 10.240.0.0/18
us-south-2   | 10.240.64.0/18
us-south-3   | 10.240.128.0/18
{: caption="Table 1. Address prefixes assigned to a zone in a region" caption-side="top"}

If you don't want these default address prefixes, you can choose to not assign default address prefixes when you create your VPC. For example, add the `"address_prefix_management": "manual"` parameter when you create the VPC by using the API.

To bring your own subnets:
1. Create a VPC.
2. For each zone in which you plan to create subnets, create one or more address prefixes.
3. When you create subnets in each zone, specify IP ranges that are within one of the address prefixes you created for that zone.

If you use an IP range outside of those ranges that are defined by [RFC 1918](https://tools.ietf.org/html/rfc1918) (`10.0.0.0/8`, `172.16.0.0/12`, or `192.168.0.0/16`) for a subnet, the instances attached to that subnet might be unable to reach parts of the public internet.
{:tip}

### Address prefixes and the {{site.data.keyword.cloud_notm}} console
{: #address-prefixes-and-the-ibm-cloud-console-ui}

When you create a VPC by using the {{site.data.keyword.cloud_notm}} console, the system selects your address prefixes automatically and requires you to create subnets within the default prefixes. If this address scheme does not suit your requirements, you can clear the **Default address prefixes** option to not assign default address prefixes to each zone in your VPC. After you create your VPC, go to its details page and set your own address prefixes. Then, you can create subnets within the address prefixes you specified.

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



