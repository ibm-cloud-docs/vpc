---

copyright:
  years: 2025
lastupdated: "2025-12-31"

keywords: load balancer, network, faqs, internal error

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# I received an internal error. What should I do?
{: #internal-error}
{: troubleshoot}
{: support}

When working with Load Balancer for VPC, you might receive an internal error that prevents an operation from completing successfully. This error is typically caused by a temporary issue within the service and is not related to your configuration.
{: shortdesc}

You receive the following error message when attempting to create, update, or delete a load balancer or related resource:
{: tsSymptoms}

`Operation Failed. An internal error has occurred.`
   
This error occurs due to an unexpected internal condition within the Load Balancer for VPC service. In general:
{: tsCauses}

* The error is not caused by invalid user input or misconfiguration.
* The issue is usually temporary.
* The operation fails before the resource action is completed.
 
Wait a few minutes, then retry the request. In many cases, the error resolves without further action.
{: tsResolve} 

If you are attempting to create an HTTPS listener with a public certificate that is stored in IBM Cloud Secrets Manager, see [this topic](/docs/vpc?topic=vpc-internal-error&interface=ui) to troubleshooting your configuration.
{: note}

If the error persists after multiple attempts, contact [IBM Support](/unifiedsupport/cases/add){: external} and create a support case.
