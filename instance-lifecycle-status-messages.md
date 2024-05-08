---

copyright:
  years: 2024
lastupdated: "2024-04-11"

keywords: instance lifecycle status reasons, lifecycle status, status reasons

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Virtual server instance lifecycle status messages
{: #instance-lifecycle-status-messages}

If you receive an instance lifecycle status message, you can use the following information to help resolve any problems.
{: shortdesc}

## Status: `pending_registration`
{: #pending-registration-instance-lifecycle}

**Status message**: _The instance's registration to the Resource Controller is being processed._

Wait for registration to complete.

If you still need help, contact [support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc). Make sure that you provide the CRN.

## Status: `failed_registration`
{: #failed-registration-instance-lifecycle}

**Status message**: _The instance's registration to the Resource Controller failed._

Delete the instance and provision a new one.

If you still need help, contact [support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc). Make sure that you provide the CRN.
