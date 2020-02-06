---

copyright:
  years: 2019, 2020

lastupdated: "2020-2-6"

keywords: vpc, CSE, endpoint, service, DNS, resolver, mirror, object, storage, bandwidth, charges

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

# Endpoints available for VPC
{: #service-endpoints-for-vpc}

When you're ready to run workloads in your {{site.data.keyword.vpc_short}}, you can access two types of {{site.data.keyword.cloud_notm}} endpoints: platform as a service (PaaS) endpoints, also known as service endpoints, and infrastructure as a service (IaaS) endpoints. 
{:shortdesc}

Although the addresses for these endpoints look as if they communicate through the public internet, traffic to and from these endpoints does not leave {{site.data.keyword.cloud_notm}}. Therefore, this traffic avoids the bandwidth charges associated with traffic that exits the cloud and goes onto the public internet.

## Service endpoints
{: #cloud-service-endpoints}

Use service endpoints to securely connect to {{site.data.keyword.cloud_notm}} services over the {{site.data.keyword.cloud_notm}} private network. These endpoints are available through DNS (Domain Name System) names in the `cloud.ibm.com` domain and resolve to `166.9.x.x` addresses. 

For more information about service endpoints, see [Secure access to services using service endpoints](/docs/resources?topic=resources-service-endpoints) and [Services that support service endpoints](/docs/resources?topic=resources-private-network-endpoints#services-support-service-endpoints). After you provision a service as a private endpoint, ping the endpoint from your virtual server instance to verify that the endpoint is reachable.

If you can't connect to service endpoints, make sure that service endpoints are enabled in your account. For instructions, see [Enabling service endpoints](/docs/account?topic=account-vrf-service-endpoint#service-endpoint).
{: tip}

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

DNS resolvers use IP address, rather than names. For shared cloud service endpoints, use the DNS server addresses `161.26.0.10` and `161.26.0.11`.

### Ubuntu and Debian APT Mirrors
{: #ubuntu-apt-mirrors}

APT mirrors for updating Ubuntu and Debian images are available from `mirrors.adn.networklayer.com`, which resolves to `161.26.0.6`.

###  NTP servers
{: #network-time-protocol-ntp-servers}

An NTP server is available from `time.adn.networklayer.com`, which resolves to `161.26.0.6`.

### {{site.data.keyword.cloud_notm}} Object Storage
{: #object-storage}

To reach Cloud Object Storage from a VPC see [Connecting to {{site.data.keyword.cloud_notm}} Object Storage from VPC](/docs/vpc?topic=vpc-connecting-vpc-cos).

