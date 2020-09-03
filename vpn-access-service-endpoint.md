---



copyright:
  years: 2019, 2020
lastupdated: "2020-08-12"

keywords: VPN, network, encryption, authentication, algorithm, IKE, IPsec, policies, gateway, access endpoint

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:important: .important}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Accessing service endpoints through VPN
{: #build-se-connectivity-using-vpn} 

A VPN for VPC gateway allows your VPC to get access to cloud service endpoints from an on-premises network and from your VPC?.

To set up access to a service endpoint:

1. Get the IP of the service endpoint. {{site.data.keyword.cloud_notm}} supports two types of service endpoints: Infrastructure as a Service (IaaS) endpoints and {{site.data.keyword.cloud_notm}} service endpoints. The IaaS endpoints are hosted in the IP address ranges `161.26.0.0/16`; {{site.data.keyword.cloud_notm}} service endpoints are hosted in the IP address ranges `166.8.0.0/14`. For more information about endpoints, see [IaaS endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc#infrastructure-as-a-service-iaas-endpoints) and [Using service endpoints](/docs/account?topic=account-vrf-service-endpoint#use-service-endpoint).
2. Create a VPN gateway and a VPN connection. For the VPN connection, the local subnets should include the range `161.26.0.0/16` for IaaS endpoints and `166.8.0.0/14` for service endpoints.
