---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-19"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Before you begin
{: #lb-troubleshooting-prereqs}
{: troubleshoot}

Before you start any load balancer troubleshooting procedure, do the following:

* Verify that the load balancer's VPC has security groups that allow the health check and the data path ports.
* Verify that the `health` field in each member is `ok`. If the `health` field is shown as `faulted` or `unknown`, make sure that the load balancer member's virtual server instance is responding to the load balancer health-check requests by taking the following actions:
   * Verify in the load balancer member virtual server that a process (for example, an Nginx server) is running and listening on the back-end pool health monitor port.
   * If the back-end pool health monitor port is not specified during the back-end pool creation, health-check requests are sent to the member port. In this case, verify in the load balancer member virtual server that a process is running and listening on the load balancer member port.

## Error codes
{: #error-codes}

When requests are made to the load balancer APIs, you might encounter errors. An error response contains an error code and a corresponding error message. For more information, see the [VPC API reference](/apidocs/vpc/latest#list-load-balancer-profiles).

## Quota errors
{: #quota-errors}

Check the [Quotas and service limits](/docs/vpc?topic=vpc-quotas) for the allowed quotas on load balancer resources.
