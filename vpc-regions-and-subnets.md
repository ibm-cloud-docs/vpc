---

copyright:
  years: 2019, 2026
lastupdated: "2026-02-25"

keywords: address prefixes, regions, subnets, zones, IP, ranges, CIDR

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Bring your own subnet
{: #configuring-address-prefixes}

You can bring your own IPv4 address ranges (public and private) from your on-premises deployment to your {{site.data.keyword.vpc_full}} (VPC) by creating address prefixes. You can then create subnets within these IP ranges.
{: shortdesc}

Each zone of your VPC is assigned a default address prefix, which specifies the address range in which subnets can be created. If the default address scheme does not suit your requirements, you can customize the address prefixes.

When a new VPC is created, the default address prefixes are assigned to each zone in the region, as shown in the following table.

Region name      |Zone           | Address prefix
-----------------|---------------|---------------
Dallas        |`us-south-1`   | `10.240.0.0/18`
Dallas        |`us-south-2`   | `10.240.64.0/18`
Dallas        |`us-south-3`   | `10.240.128.0/18`
Washington DC |`us-east-1`    | `10.241.0.0/18`
Washington DC |`us-east-2`    | `10.241.64.0/18`
Washington DC |`us-east-3`    | `10.241.128.0/18`
London        |`eu-gb-1`      | `10.242.0.0/18`
London        |`eu-gb-2`      | `10.242.64.0/18`
London        |`eu-gb-3`      | `10.242.128.0/18`
Frankfurt     |`eu-de-1`      | `10.243.0.0/18`
Frankfurt     |`eu-de-2`      | `10.243.64.0/18`
Frankfurt     |`eu-de-3`      | `10.243.128.0/18`
Madrid        |`eu-es-1`      | `10.251.0.0/18`
Madrid        |`eu-es-2`      | `10.251.64.0/18`
Madrid        |`eu-es-3`      | `10.251.128.0/18`
Tokyo         |`jp-tok-1`     | `10.244.0.0/18`
Tokyo         |`jp-tok-2`     | `10.244.64.0/18`
Tokyo         |`jp-tok-3`     | `10.244.128.0/18`
Sydney        |`au-syd-1`     | `10.245.0.0/18`
Sydney        |`au-syd-2`     | `10.245.64.0/18`
Sydney        |`au-syd-3`     | `10.245.128.0/18`
Osaka         |`jp-osa-1`     | `10.248.0.0/18`
Osaka         |`jp-osa-2`     | `10.248.64.0/18`
Osaka         |`jp-osa-3`     | `10.248.128.0/18`
Chennai - Airtel |`in-che-1`     | `10.254.0.0/18`
Chennai - Airtel |`in-che-2`     | `10.254.64.0/18`
Chennai - Airtel |`in-che-3`     | `10.254.128.0/18`
Montreal      |`ca-mon-1`     | `10.253.0.0/18`
Montreal      |`ca-mon-2`     | `10.253.64.0/18`
Montreal      |`ca-mon-3`     | `10.253.128.0/18`
Toronto       |`ca-tor-1`     | `10.249.0.0/18`
Toronto       |`ca-tor-2`     | `10.249.64.0/18`
Toronto       |`ca-tor-3`     | `10.249.128.0/18`
Sao Paulo     |`br-sao-1`     | `10.250.0.0/18`
Sao Paulo     |`br-sao-2`     | `10.250.64.0/18`
Sao Paulo     |`br-sao-3`     | `10.250.128.0/18`
{: caption="Address prefixes assigned to a zone in a region" caption-side="bottom"}

For x86-64 dedicated host profiles, the Madrid region only supports dedicated host profiles with instance storage. For more information, see [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui).
{: important}

If you don't want these default address prefixes, you can choose to not assign them when you create your VPC. For example, add the `"address_prefix_management": "manual"` parameter when you create the VPC by using the API.

To bring your own subnets:

1. Create a VPC.
2. For each zone in which you plan to create subnets, create one or more address prefixes.
3. When you create subnets in each zone, specify IP ranges that are within one of the address prefixes that you created for that zone.

If you use an IP range outside of those ranges [RFC 1918](https://datatracker.ietf.org/doc/html/rfc1918){: external} (`10.0.0.0/8`, `172.16.0.0/12`, or `192.168.0.0/16`) defined for a subnet, the instances that you attach to that subnet might be unable to reach parts of the public internet. If you plan to configure VPCs that use both non-RFC-1918 addresses and also have public connectivity (floating IPs or public gateways), make sure to use a custom route that contains the `Delegate-VPC` action.
{: tip}

## Address prefixes and the {{site.data.keyword.cloud_notm}} console
{: #address-prefixes-and-the-ibm-cloud-console-ui}

When you create a VPC by using the {{site.data.keyword.cloud_notm}} console, the system selects your address prefixes automatically and requires you to create subnets within the default prefixes. If this address scheme does not suit your requirements, you can clear the **Default address prefixes** option to not assign default address prefixes to each zone in your VPC. After you create your VPC, go to its details page and set your own address prefixes. Then, you can create subnets within the address prefixes you specified.

## CLI example
{: #cli-example-byoip}

The following example shows you how to use the CLI to bring your own IP addresses for subnets in the 'us-south-1' and 'us-south-2' zones of your VPC.

1. Create an address prefix named `my-first-prefix` in the `us-south-1` zone:

   ```sh
   ibmcloud is vpc-address-prefix-create my-first-prefix $VPC us-south-1 172.16.0.0/23
   ```
   {: pre}

1. Create an address prefix named `another-prefix` in the `us-south-2` zone:

   ```sh
   ibmcloud is vpc-address-prefix-create another-prefix $VPC us-south-2 172.16.2.0/23
   ```
   {: pre}

1. Create a subnet named `my-subnet` within your new address prefix in the `us-south-1` zone:

   ```sh
   ibmcloud is subnet-create my-subnet $vpc us-south-1 --ipv4-cidr-block "172.16.0.0/25"
   ```
   {: pre}

1. Create a subnet named `another-subnet` within your new address prefix in the `us-south-2` zone:

   ```sh
   ibmcloud is subnet-create another-subnet $vpc us-south-2 --ipv4-cidr-block "172.16.2.0/25"
   ```
   {: pre}
