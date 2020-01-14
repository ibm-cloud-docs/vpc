---

copyright:
  years: 2020
lastupdated: "2020-01-14"

keywords: vpc, region, zone, deploy, datacenter, data, center, federated, CLI, API, account, failover, disaster, recovery, DR

subcollection: vpc-on-classic

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
{:DomainName: data-hd-keyref="DomainName"}

# Creating a VPC in a different region
{: #creating-a-vpc-in-a-different-region}

A region is a specific geographical location where you can deploy apps, services, and other {{site.data.keyword.cloud}} resources. Regions consist of one or more zones, which are physical data centers that house the compute, network, and storage resources, with related cooling and power, for host services and applications. Zones are isolated from each other, which ensures that there is no shared single point of failure within a region.

The IBM Cloud VPC service is regional. To allow for disaster recovery (DR), you must provide the ability for recovery using failover to an alternate region, by establishing a VPC in one of our other regions.
{: note}

Virtual Private Cloud is available in the following {{site.data.keyword.cloud}} regions.

|   Location     | Region | API Endpoint | 
| ------- | :------: | :------: |
| Dallas | us-south | `us-south.iaas.cloud.ibm.com`| 
| Washington DC | us-east | `us-east.iaas.cloud.ibm.com`| 

The Regional API (VPC) endpoint is automatically set by the IBM Cloud CLI when you log in to a specific region.
{: note}

## Log in to a specific region using the CLI
{: #log-in-to-a-specific-region-using-the-cli}

When you log in to IBM Cloud, you can specify a region or choose it later. For example, to log into the global API endpoint in the Dallas (`us-south`) region directly, run the following commands, which vary according to whether you have a federated account (SSO) or a non-federated account.

For a federated account,

```
ibmcloud login -a https://cloud.ibm.com -r us-south --sso
```
{: pre}

For a non-federated account,

```
ibmcloud login -a https://cloud.ibm.com -r us-south
```
{: pre}

To choose the region later, do not specify the `-r <region>` parameter and the CLI will prompt you to choose a region.

Example output:

```
API endpoint: cloud.ibm.com

Get One Time Code from https://identity-2.eu-central.iam.cloud.ibm.com/identity/passcode to proceed.
Open the URL in the default browser? [Y/n]> y
One Time Code >
Authenticating...
OK

Select an account:
1. MyAccount (00a11aa1a11aa11a1111a1111aaa11aa) <-> 1234567
2. TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321
Enter a number> 2
Targeted account TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321


Targeted resource group Default

Select a region (or press enter to skip):
1. au-syd
2. jp-tok
3. eu-de
4. eu-gb
5. us-south
6. us-east
Enter a number> 5
Targeted region us-south


API endpoint:      https://cloud.ibm.com   
Region:            us-south   
User:              first.last@email.com   
Account:           TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321  
Resource group:    Default   
CF API endpoint:      
Org:                  
Space:                

...
```
{: screen}

## Switch regions using the CLI
{: #switch-regions-using-the-cli}

To get the latest status of the VPC regions, run the following command:

```
ibmcloud is regions
```
{: pre}

To switch to a different region, run the `ibmcloud target -r <region>` command. For example, to switch to the Washington DC region, run the following command:

```
ibmcloud target -r us-east
```
{: pre}

To check your current location, run the following command:

```
ibmcloud target
```
{: pre}

## Switch regions using the API  
{: #switch-regions-using-the-api}

To interact with the Regional VPC API using REST, direct your request to the API endpoint associated with the region in which you want to create resources. The region's API endpoint is shown in the previous table. You also can find the endpoints associated with the regions by running the following command:

```
ibmcloud is regions
```
{: pre}


For example, to get the list of VPCs in the `us-south` region, run the following command:

```
curl "https://us-south.iaas.cloud.ibm.com/v1/vpcs?version=2019-05-31&generation=1" -H "Authorization: $iam_token"
```
{: pre}


## Get zones
{: #get-zones}

To get the list of zones available for each region, run the command `ibmcloud is zones <region>`. For example, to get the list of zones in region `us-south`, run the following command:

```
ibmcloud is zones us-south
```
{: pre}
