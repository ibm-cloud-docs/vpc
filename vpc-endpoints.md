---

copyright:
  years: 2019, 2020

lastupdated: "2020-08-12"

keywords: CSE, endpoint, service, DNS, resolver, mirror, object, storage, bandwidth, charges

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Endpoints available
{: #service-endpoints-for-vpc}

Before you can run workloads in an {{site.data.keyword.vpc_short}}, you must first set up your environment to access the VPC API. That is, if you choose to manage your VPC resources programmatically. The following information lists the regional API endpoints you can use to access your VPC resources. 

For more information about setting up your VPC API environment or referencing methods to access your VPC resources, see [Setting up your CLI or API environment](/docs/vpc?topic=vpc-set-up-environment) or the [Virtual Private Cloud API reference](https://cloud.ibm.com/apidocs/vpc). 

Use one of the following public endpoints to connect to the VPC infrastructure API. The endpoints are based on the region of the service. 

| Region            | Endpoint                              |
|-------------------|---------------------------------------|
| US South          | `https://us-south.iaas.cloud.ibm.com` |
| US East           | `https://us-east.iaas.cloud.ibm.com`  |
| United Kingdom    | `https://eu-gb.iaas.cloud.ibm.com`    |
| EU - Germany      | `https://eu-de.iaas.cloud.ibm.com`    |
| Tokyo             | `https://jp-tok.iaas.cloud.ibm.com`   |
{: caption="Table 1. VPC API Regional Endpoints" caption-side="top"}
 
After you've created and are accessing the resources in your VPC, you're ready to run workloads. From inside the VPC infrastructure, you can access two types of {{site.data.keyword.cloud_notm}} endpoints: platform as a service (PaaS) endpoints, also known as service endpoints, and infrastructure as a service (IaaS) endpoints. 

Although the addresses for these endpoints look as if they communicate through the public internet, traffic to and from these endpoints does not leave {{site.data.keyword.cloud_notm}}. Therefore, this traffic avoids the bandwidth charges associated with traffic that exits the cloud and goes onto the public internet.

## Service endpoints
{: #cloud-service-endpoints}

Use service endpoints to securely connect to {{site.data.keyword.cloud_notm}} services over the {{site.data.keyword.cloud_notm}} private network. These endpoints are available through DNS (Domain Name System) names in the `cloud.ibm.com` domain and resolve to `166.9.x.x` addresses. 

Service endpoints must be enabled in your account before they can be accessed. For instructions, see [Enabling service endpoints](/docs/account?topic=account-vrf-service-endpoint#service-endpoint).

For more information about service endpoints, see [Services that support service endpoints](/docs/account?topic=account-vrf-service-endpoint#use-service-endpoint). After you provision a service as a private endpoint, ping the endpoint from your virtual server instance to verify that the endpoint is reachable.

You can also use VPN for VPC to access service endpoints. For more information, see [Access service endpoints through VPN](/docs/vpc?topic=vpc-using-vpn#build-se-connectivity-using-vpn).

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
{: caption="Table 1. Ports required for network traffic" caption-side="top"}


### DNS resolver endpoints
{: #dns-domain-name-system-resolver-endpoints}

{{site.data.keyword.dns_full_notm}} provides private DNS to VPC users. Private DNS zones are resolvable only on IBM Cloud, and only from explicitly permitted networks in an account. For more information about DNS Services, see [Getting started with {{site.data.keyword.dns_full_notm}}](/docs/dns-svcs?topic=dns-svcs-getting-started).

DNS resolvers use IP address, rather than names. For shared cloud service endpoints, use the DNS server addresses `161.26.0.10` and `161.26.0.11`.

### Ubuntu and Debian APT Mirrors
{: #ubuntu-apt-mirrors}

APT mirrors for updating Ubuntu and Debian images are available from `mirrors.adn.networklayer.com`, which resolves to `161.26.0.6`.

For instances that are provisioned with stock images for CentOS, Red Hat Enterprise Linux, or Windows, update connections are  configured as part of the provisioning process.  
{: tip}

###  NTP servers
{: #network-time-protocol-ntp-servers}

NTP is widely used to synchronize a computer to Internet time servers or other sources. The IBM NTP server is available for VPC instances to use for time synchronization.

An NTP server is available from `time.adn.networklayer.com`, which resolves to `161.26.0.6`.

### {{site.data.keyword.cloud_notm}} Object Storage
{: #object-storage}

{{site.data.keyword.cos_full_notm}} stores encrypted and dispersed data across multiple geographic locations. For more information about {{site.data.keyword.cos_full_notm}}, see [Getting started with {{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage).

To reach Cloud Object Storage from a VPC see [Connecting to {{site.data.keyword.cloud_notm}} Object Storage from VPC](/docs/vpc?topic=vpc-connecting-vpc-cos).





