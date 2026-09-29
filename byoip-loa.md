---

copyright:
  years: 2026
lastupdated: "2026-09-29"

keywords: LoA, IPv4 prefixes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Template: Letter of Authorization for IBM to announce IPv4 prefixes
{: #ipv4-letter-of-authorization}

[BYOIP for IPv4 is a beta feature](/docs/vpc?topic=vpc-release-notes#vpc-sep2926) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

A Letter of Authorization (LoA) is required when you bring your own publicly routable IPv4 address range (BYOIP) to {{site.data.keyword.vpc_short}}. The LoA is a signed document on company letterhead that authorizes IBM to announce your company-owned IPv4 prefixes to its BGP peers on your behalf. It is submitted as part of the IBM Support case when you request provisioning of a customer-owned custom authorized CIDR.

IBM-assigned CIDRs do not require an LoA.
{: note}

Use the following template to prepare your LoA. Complete all fields, sign the document, and attach it to your support case.
{: shortdesc}

```text
COMPANY LETTERHEAD

Subject: Authorization for IBM to Announce IPv4 Prefixes

Date:  ____________________


To Whom It May Concern,

This letter authorizes IBM to announce the following <COMPANY NAME> owned and directly allocated IPv4 prefixes to all of
its directly connected BGP peers as IBM sees fit on behalf of <CUSTOMER NAME>:

Authorized IPv4 prefixes

Example:        w.x.y.z/nm

Prefix 1:  _________________________________

Prefix 2:  _________________________________


In connection with the above authorization, IBM also has full permission to maintain Internet Routing Registry (IRR) objects and related routing registry entries necessary to support routing policy publication and route filtering for these prefixes.

We respectfully request that all applicable routing and prefix filters be updated to permit acceptance and propagation of the above-listed prefixes when originated by IBM.

This authorization is effective as of the date of this letter and shall remain in effect until revoked or modified in writing by an authorized representative of <COMPANY NAME>.

Should you require verification of this authorization, please contact:

Name:  ______________________________________

Title:  _____________________________________

Company Name:  ______________________________

Email:  _____________________________________

Phone:  _____________________________________


Sincerely,

Signature:  _____________________________   Date:  _______________

Print Name:  ________________________________

Title:  _____________________________________

Company Name:  ______________________________

Address:  ___________________________________

Email:  _____________________________________

Phone:  _____________________________________
```
{: codeblock}
