---

copyright:
  years: 2019

lastupdated: "2019-08-27"

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

When you're ready to run workloads in your VPC, you can access two types of IBM Cloud endpoints: platform as a service (PaaS) endpoints, also known as service endpoints, and infrastructure as a service (IaaS) endpoints. 
{:shortdesc}

Although the addresses for these endpoints look as if they communicate through the public internet, traffic to and from these endpoints does not leave IBM Cloud. Therefore, this traffic avoids the bandwidth charges associated with traffic that exits the cloud and goes onto the public internet.

## Service endpoints
{: #cloud-service-endpoints}

Service endpoints enable you to securely connect to IBM Cloud services over the IBM Cloud private network. These endpoints are available through DNS names in the `cloud.ibm.com` domain and resolve to `166.9.x.x` addresses. 

For example, to reach a private endpoint for an IBM Cloud Object Storage (COS) instance, replace the "domain" portion of the URL with `cloud-object-storage.appdomain.cloud`. For more information, see [Connecting to IBM Cloud Object Storage from VPC](/docs/vpc?topic=vpc-connecting-vpc-cos).

For more information about service endpoints, see [Secure access to services using service endpoints](/docs/resources?topic=resources-service-endpoints) and [Services that support service endpoints](/docs/resources?topic=resources-private-network-endpoints#services-support-service-endpoints). After you provision a service as a private endpoint, ping the endpoint from your virtual server instance to verify that the endpoint is reachable.

If you can't connect to service endpoints, make sure that service endpoints are enabled in your account. For instructions, see [Enabling service endpoints](/docs/account?topic=account-vrf-service-endpoint#service-endpoint).
{: tip}

## IaaS endpoints
{: #infrastructure-as-a-service-iaas-endpoints}

Infrastructure services are available by using certain DNS names from the `adn.networklayer.com` domain, and they resolve to `161.26.0.0/16` addresses. Services you can reach include:

* DNS resolvers
* Ubuntu and Debian APT (Advanced Packaging Tool) Mirrors
* NTP

### DNS (Domain Name System) resolver endpoints
{: #dns-domain-name-system-resolver-endpoints}

DNS resolvers use IP address, rather than names. For shared cloud service endpoints, use the DNS server addresses `161.26.0.10` and `161.26.0.11`.

### Ubuntu and Debian APT Mirrors
{: #ubuntu-apt-mirrors}

APT mirrors for updating Ubuntu and Debian images are available from `mirrors.adn.networklayer.com`, which resolves to `161.26.0.6`.

### Network Time Protocol (NTP) servers
{: #network-time-protocol-ntp-servers}

An NTP server is available from `time.adn.networklayer.com`, which resolves to `161.26.0.6`.


