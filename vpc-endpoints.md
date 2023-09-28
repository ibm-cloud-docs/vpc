---

copyright:
  years: 2019, 2023

lastupdated: "2023-09-21"

keywords: CSE, endpoint, service, DNS, resolver, mirror, object, storage, bandwidth, charges

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Endpoints available
{: #service-endpoints-for-vpc}

Before you can run workloads in an {{site.data.keyword.vpc_short}}, you must first set up your environment to access the VPC API. That is, if you choose to manage your VPC resources programmatically. The following information lists the regional API endpoints that you can use to access your VPC resources.

For more information about setting up your VPC API environment or referencing methods to access your VPC resources, see [Setting up your CLI or API environment](/docs/vpc?topic=vpc-set-up-environment) or the [Virtual Private Cloud API reference](/apidocs/vpc).

Use one of the following public endpoints to connect to the VPC infrastructure API. The endpoints are based on the region of the service.

|   Location     | Region | Public Endpoint | Private Endpoint |
| ------- | ------ | ------ | ------ |
| US South (Dallas) | `us-south` | `https://us-south.iaas.cloud.ibm.com` | `https://us-south.private.iaas.cloud.ibm.com` |
| US East (Washington DC) | `us-east` | `https://us-east.iaas.cloud.ibm.com`  | `https://us-east.private.iaas.cloud.ibm.com` |
| Brazil (São Paulo) | `br-sao` | `https://br-sao.iaas.cloud.ibm.com`   | `https://br-sao.private.iaas.cloud.ibm.com` |
| Canada (Toronto) | `ca-tor` | `https://ca-tor.iaas.cloud.ibm.com`   | `https://ca-tor.private.iaas.cloud.ibm.com` |
{: class="simple-tab-table"}
{: tab-title="Americas"}
{: caption="Table 1. VPC API Regional Endpoints for North and South America" caption-side="bottom"}
{: summary="This table displays the VPC API Regional Endpoints."}
{: tab-group="vpc-api-endpoints"}
{: #vpc-americas-endpoints}

|   Location     | Region | Public Endpoint | Private Endpoint |
| ------- | ------ | ------ | ------ |
| United Kingdom (London) | `eu-gb` |  `https://eu-gb.iaas.cloud.ibm.com`    | `https://eu-gb.private.iaas.cloud.ibm.com` |
| EU Germany (Frankfurt) | `eu-de` | `https://eu-de.iaas.cloud.ibm.com`    | `https://eu-de.private.iaas.cloud.ibm.com` |
| Spain (Madrid) | `eu-es` | `https://eu-es.iaas.cloud.ibm.com` | `https://eu-es.private.iaas.cloud.ibm.com` |
{: class="simple-tab-table"}
{: tab-title="Europe"}
{: caption="Table 1. VPC API Regional Endpoints for Europe" caption-side="bottom"}
{: summary="This table displays the VPC API Regional Endpoints."}
{: tab-group="vpc-api-endpoints"}
{: #vpc-europe-endpoints}

The Madrid region only supports dedicated host profiles with instance storage. For more information, see [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui).
{: important}

|   Location     | Region | Public Endpoint | Private Endpoint |
| ------- | ------ | ------ | ------ |
| Japan (Tokyo) | `jp-tok` | `https://jp-tok.iaas.cloud.ibm.com`   | `https://jp-tok.private.iaas.cloud.ibm.com` |
| Japan (Osaka) | `jp-osa`| `https://jp-osa.iaas.cloud.ibm.com`   | `https://jp-osa.private.iaas.cloud.ibm.com` |
| Australia (Sydney) | `au-syd` | `https://au-syd.iaas.cloud.ibm.com`   | `https://au-syd.private.iaas.cloud.ibm.com` |
{: class="simple-tab-table"}
{: tab-title="Asia Pacific"}
{: caption="Table 1. VPC API Regional Endpoints for Asia Pacific" caption-side="bottom"}
{: summary="This table displays the VPC API Regional Endpoints."}
{: tab-group="vpc-api-endpoints"}
{: #vpc-asia-pacific-endpoints}

LinuxONE (s390x processor architecture) profiles are supported on virtual server instances in the Japan (Tokyo), Brazil (São Paulo), Spain (Madrid), Canada (Toronto), and United Kingdom (London) regions.    

After resources are created and accessible in your VPC, you're ready to run workloads. From inside the VPC infrastructure, you can access two types of {{site.data.keyword.cloud_notm}} endpoints: platform as a service (PaaS) endpoints, also known as service endpoints, and infrastructure as a service (IaaS) endpoints.

Although the addresses for these endpoints look as if they communicate through the public internet, traffic to and from these endpoints does not leave {{site.data.keyword.cloud_notm}}. Therefore, this traffic avoids the bandwidth charges associated with traffic that exits the cloud and goes onto the public internet.

## Service endpoints
{: #cloud-service-endpoints}

Use service endpoints to securely connect to {{site.data.keyword.cloud_notm}} services over the {{site.data.keyword.cloud_notm}} private network. These endpoints are available through DNS (Domain Name System) names in the `cloud.ibm.com` domain and resolve to `166.8.0.0/14` addresses.

Traffic to and from service endpoints are subject to ACL and security group rules. In other words, these mechanisms can be used in cases where you want to limit what virtual server instances use a particular service endpoint.

VPCs are automatically able to reach service endpoints. For more information about service endpoints, see [Services that support service endpoints](/docs/account?topic=account-vrf-service-endpoint#use-service-endpoint). After you provision a service as a private endpoint, ping the endpoint from your virtual server instance to verify that the endpoint is reachable.

You can also use {{site.data.keyword.vpn_vpc_short}} to access service endpoints. For more information, see [Access service endpoints through VPN](/docs/vpc?topic=vpc-using-vpn).

## IaaS endpoints
{: #infrastructure-as-a-service-iaas-endpoints}

Infrastructure services are available by using certain DNS names from the `adn.networklayer.com` domain, and they resolve to `161.26.0.0/16` addresses. Services that you can reach include:

* DNS resolvers
* Ubuntu and Debian APT (Advanced Packaging Tool) Mirrors
* Network Time Protocol (NTP)
* {{site.data.keyword.cloud_notm}} Object Storage

The following ports must be open to allow ADN network traffic to flow for the following services.

| Protocol | Port        | Service |
| -------- | ----------- | ----------- |
| UDP      | 53          | DNS         |
| TCP      | 80          | HTTP      |
| TCP      | 443         | HTTPS       |
{: caption="Table 2. Ports required for network traffic" caption-side="bottom"}

## Virtual private endpoints
{: #virtual-private-endpoints}

{{site.data.keyword.cloud_notm}} services available through {{site.data.keyword.cloud_notm}} Virtual Private Endpoints (VPE) for VPC are listed at [VPE supported services](/docs/vpc?topic=vpc-vpe-supported-services). VPE supports both service and IaaS endpoints. For more information about private connectivity and VPE, see [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).

### DNS resolver endpoints
{: #dns-domain-name-system-resolver-endpoints}

{{site.data.keyword.dns_full_notm}} provide private DNS to VPC users. Private DNS zones are resolvable only on {{site.data.keyword.cloud_notm}}, and only from explicitly permitted networks in an account. For more information about DNS Services, see [Getting started with {{site.data.keyword.dns_full_notm}}](/docs/dns-svcs?topic=dns-svcs-getting-started).

DNS resolvers use IP address, rather than names. For shared cloud service endpoints, use the DNS server addresses `161.26.0.10` and `161.26.0.11`.

### Ubuntu and Debian APT Mirrors
{: #ubuntu-apt-mirrors}

APT mirrors for updating Ubuntu and Debian images are available from `mirrors.adn.networklayer.com`, which resolves to `161.26.0.6`.

For instances that are provisioned with stock images for CentOS, Red Hat Enterprise Linux, or Windows, update connections are configured as part of the provisioning process.  
{: tip}

###  NTP servers
{: #network-time-protocol-ntp-servers}

NTP is widely used to synchronize a computer to internet time servers or other sources. The IBM NTP server is available for VPC instances to use for time synchronization.

An NTP server is available from `time.adn.networklayer.com`, which resolves to `161.26.0.6`.

### {{site.data.keyword.cloud_notm}} Object Storage
{: #object-storage}

{{site.data.keyword.cos_full_notm}} stores encrypted and dispersed data across multiple geographic locations. For more information about {{site.data.keyword.cos_full_notm}}, see [Getting started with {{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage).

For more information about reaching {{site.data.keyword.cos_short}} from a VPC see [Connecting to {{site.data.keyword.cos_full_notm}} from VPC](/docs/vpc?topic=vpc-connecting-vpc-cos).
