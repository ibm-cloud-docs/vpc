---

copyright:
  years: 2018, 2020
lastupdated: "2020-08-10"

keywords: vpe, virtual private endpoint, troubleshoot, tips, error, problem, debug

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

# Troubleshooting Virtual Private Endpoints for VPC (Beta)
{: #vpc-troubleshooting-vpe}

As the VPE beta release gets underway, this document will be updated to reflect common difficulties you might encounter as well as helpful tips learned along the way.
{:shortdesc}

## I created an endpoint gateway. It states that the gateway is healthy, but I cannot reach the target IBM Cloud service. What should I do?

Check if the cloud service instance is up and running. To do so, try to access the instance using the public endpoint cloud service instance. If you cannot access the service’s public endpoint, check with the IBM Support team for the cloud service instance.

If IBM Support can access the cloud service public endpoint, follow these steps:  

1. Check the "Health State" (`ok`) and "Lifecycle State" (`stable`) of your VPE. To do so, you can use the UI or enter the following command:

   ```
   ibmcloud is endpoint-gateway <vpe_id>
   ```

   To list all endpoint gateways, run this command: `ibmcloud is egs`
   {: tip}

2. Check the health of the service instance and then attemt to access the service through its public endpoint.
