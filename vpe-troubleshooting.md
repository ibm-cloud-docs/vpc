---

copyright:
  years: 2018, 2020
lastupdated: "2020-10-30"

keywords: vpe, virtual private endpoints, troubleshoot, tips, error, problem, debug, endpoint gateway

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:troubleshoot: .troubleshoot}

# Troubleshooting virtual private endpoints
{: #vpc-troubleshooting-vpe}

Describes common problems that you might encounter, troubleshooting steps, and helpful tips when using {{site.data.keyword.cloud}} Virtual Private Endpoints (VPE) for VPC.
{:shortdesc}

## Cannot create a reserved IP
{: #vpe-cannot-create-reserved-IP}

When creating endpoint gateways, you can specify as many reserved IPs as there are availability zones (AZs) in the region, typically 3 (1 reserved IP per AZ). Also, you can have only one reserved IP per subnet because subnets don't span AZs.  

So, why can't you create a reserved IP?

1. If the endpoint gateway exists, and a reserved IP is being added to it, there might be a reserved IP already associated with the endpoint gateway in the same AZ as the reserved IP you are trying to add.
1. If a new endpoint gateway is being created with multiple reserved IPs, there might be 2 reserved IPs being specified in the same AZ.

## VSI can access a service using the reserved IP, but cannot access the service's URL
{: #vpe-vsi-cannot-access-service-url}

The DNS service was not able to change the default DNS server for the VSI trying to access the service. To correct this issue, follow steps similar to the following Ubuntu commands:

1. In the VSI, check the last lease in `/var/lib/dhcp/dhclient.leases`. The entry for DNS servers should read option `domain-name-servers 161.26.0.7,161.26.0.8`, not `161.26.0.10,161.26.0.11`.
1. If this entry is not updated, run `/sbin/dhclient` to force the DHCP client update.

## Reserved IPs associated with an endpoint gateway can't be found
{: #vpe-reserved-ips-cannot-be-found}

The reserved IP associated with the endpoint gateway might have been automatically deleted with the subnet. A reserved IP is part of a subnet and if the subnet gets deleted, all reserved IPs belonging to the subnet get deleted.  

## Associated reserved IP shows address `0.0.0.0`
{: #vpe-associated-reserved-ip-zeros}

Reserved IPs belong to a subnet. These IPs can be created explicitly by using subnet APIs (`POST /subnets/{subnet_id}/reserved_ips`), or implicitly as part of an endpoint gateway call by specifying the subnet.

The allocation of the reserved IP address is completed in the background after the call is made. The value of the IP address shows `0.0.0.0` until the allocation of the IP address is complete, which can take up to 30 seconds.

If the IP address is not allocated within this timeframe, try the following:

1. Detach the reserved IP from the endpoint gateway and retry the process.
1. If reattaching does not work, try to create a reserved IP on its own as a subnet operation. If this works, you can try to create the endpoint gateway by using the explicitly created reserved IP.

## Cannot create multiple endpoint gateways for one service instance
{: #vpe-cannot-create-multiple-egs}

An endpoint gateway can only be associated with service instances that are created in your VPC. In addition, an endpoint gateway has a 1:1 association with a service instance. For example, if you purchase an instance of KeyProtect with a unique CRN, only one endpoint gateway can be associated with it. You can associate multiple reserved IPs with the endpoint gateway, one per AZ in the region. Your VSI in any AZ in the region has access to the endpoint gateway by using any of the reserved IPs, provided no network ACL rules prevent access.

## Endpoint gateway is healthy, but cannot reach the target IBM Cloud service
{: #vpe-epg-cannot-reach-cloud-service}

Check whether the cloud service instance is available to others by trying to access the public endpoint cloud service instance directly. If you cannot access the service’s public endpoint, open an IBM Support case.

To check the "Health State" and "Lifecycle State" of your VPE, you can use the UI, or enter the following command:

   ```
   ibmcloud is endpoint-gateway <endpoint_gateway_id>
   ```

   To list all endpoint gateways, run this command: `ibmcloud is egs`
   {: tip}     

## Cannot communicate with IBM Cloud service
{: #vpe-cannot-access-service}

If you are unable to communicate with the IBM Cloud service from your VSI, check to make sure that the endpoint gateway is set up correctly. To do so, follow these steps:

1. Check whether the service instance is valid (not deleted).
1. Check all reserved IPs associated with the endpoint gateway and make sure they have valid IP addresses. See [Associated reserved IP shows address `0.0.0.0`](#vpe-associated-reserved-ip-zeros) for details.
1. Check whether the service can be reached by using the reserved IP address instead of the URL, assuming one was provided by the service. See [VSI can access a service using the reserved IP, but cannot access the service's URL](#vpe-vsi-cannot-access-service-url) for details.
1. Check whether connectivity to cloud service endpoints is functional - `host google.com 161.26.0.10`. If this fails, open an IBM Support case.
1. Check whether network ACLs are defined on the VSI (or reserved IP) subnet that prevents communication between the two.
