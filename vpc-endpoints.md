---

copyright:
  years: 2019

lastupdated: "2019-07-29"

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

# Service endpoints available for IBM Cloud VPC
{: #service-endpoints-for-vpc}

When you're ready to run workloads, you can reach Infrastructure as a Service (IaaS) endpoints.
{:shortdesc}

These endpoints use _routable addresses_; that is, addresses outside of those specified in RFC 1918. These addresses might look as if they are communicating through the public Internet, but traffic to and from these endpoints does not leave the cloud. Therefore, this traffic avoids the bandwidth charges associated with traffic that exits the cloud and goes onto the public internet.

Infrastructure services are available by using certain DNS names from the `adn.networklayer.com` domain, and they resolve to `161.26.0.0/16` addresses. Services you can reach include:

* DNS resolvers
* Ubuntu and Debian APT (Advanced Packaging Tool) Mirrors
* Cloud Object Storage
* NTP

## DNS (Domain Name System) resolver endpoints
{: #dns-domain-name-system-resolver-endpoints}

DNS resolvers use IP address, rather than names. For shared cloud service endpoints, use the DNS server addresses `161.26.0.10` and `161.26.0.11`.

## Ubuntu and Debian APT Mirrors
{: #ubuntu-apt-mirrors}

APT mirrors for updating Ubuntu and Debian images are available from `mirrors.adn.networklayer.com`, which resolves to `161.26.0.6`.

## IBM Cloud Object Storage (COS)
{: #ibm-cloud-object-storage}

For example, to reach a private endpoint for an object storage service on the IBM Cloud private network, replace the "domain" portion of the URL with `cloud-object-storage.appdomain.cloud`. To reach a COS bucket created in another region, use the endpoint for the bucket, not the endpoint of the region in which the virtual server instance has been created. For more information, see [Connecting to IBM Cloud Object Storage from VPC](/docs/vpc?topic=vpc-connecting-vpc-cos).

## Network Time Protocol (NTP) servers
{: #network-time-protocol-ntp-servers}

An NTP server is available from `time.adn.networklayer.com`, which resolves to `161.26.0.6`.
