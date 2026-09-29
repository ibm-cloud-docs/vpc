---

copyright:
  years: 2026
lastupdated: "2026-09-29"

keywords: BYOIP, deprovision, custom authorized CIDR, bring your own IP

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deprovisioning a custom authorized CIDR
{: #deprovision-custom-authorized-cidr}

[IPv4 BYOIP is an allowlisted beta feature](/docs/vpc?topic=vpc-release-notes#vpc-sep2926) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

You can deprovision a custom authorized CIDR to fully release the IP address range from {{site.data.keyword.cloud_notm}}. When the CIDR is deprovisioned, IBM stops announcing the range and the IP addresses are released from your account.
{: shortdesc}

Deprovisioning a custom authorized CIDR is performed by IBM in response to an IBM Support case that you open. You cannot deprovision an authorized CIDR yourself directly through the CLI, API, or Terraform — those interfaces initiate the support case workflow and submit the request to IBM. This process takes a minimum of two weeks.
{: important}

Before you begin, you must release all allocated resources from the CIDR.

## Before you begin
{: #before-deprovision-byoip}

Before you deprovision a custom authorized CIDR, you must delete all public address ranges that are allocated from it.

If any allocated resources remain, the deprovision request is blocked. The console displays the message: *Delete associated public address ranges before deprovisioning CIDR.*

## Deprovisioning a custom authorized CIDR in the console
{: #deprovision-custom-authorized-cidr-console}
{: ui}

You can deprovision a single CIDR or multiple CIDRs at the same time.

### Deprovisioning a single CIDR
{: #deprovision-single-cidr-console}

To deprovision a single custom authorized CIDR in the console, follow these steps:

1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**.
1. Click the **Custom authorized CIDRs (BYOIP)** tab.
1. In the table, click the **Actions** menu ![Actions menu](../icons/action-menu-icon.svg "Actions") for the CIDR and select **Deprovision CIDR**.
1. In the **Deprovision IPv4 authorized CIDR** window, review the information and click **Create support case**.
1. In the support case form, include the following details in the **Description** field:
   - The CIDR or CIDRs you want to deprovision
   - The region where the CIDRs are configured
   - Confirmation that all resources using the CIDR have been deleted (state **Yes**)
1. Complete any other optional information, then review the case summary and click **Submit case**.

Alternatively, you can deprovision a custom authorized CIDR from the CIDR's details page.
{: tip}

### Deprovisioning multiple CIDRs
{: #deprovision-multiple-cidrs-console}

To deprovision multiple custom authorized CIDRs at the same time in the console, follow these steps:

1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**.
1. Click the **Custom authorized CIDRs (BYOIP)** tab.
1. Select the checkboxes for all CIDRs that you want to deprovision.
1. In the toolbar that appears, click **Deprovision CIDRs**.
1. The console checks each selected CIDR for allocated resources. If any CIDR still has allocated public address ranges, the deprovision request is blocked for that CIDR. Delete all allocated resources and try again.
1. For each CIDR that has no allocated resources, review the **Deprovision IPv4 authorized CIDRs** window and click **Create support case**.
1. In the support case form, include the following details in the **Description** field:
   - The CIDR or CIDRs you want to deprovision
   - The region where the CIDRs are configured
   - Confirmation that all resources using the CIDR have been deleted (state **Yes**)
1. Complete any other optional information, then review the case summary and click **Submit case**.

After IBM processes your request, you receive a notification and the CIDR is removed from the **Custom authorized CIDRs (BYOIP)** tab.

## Deprovisioning a custom authorized CIDR from the CLI
{: #deprovision-custom-authorized-cidr-cli}
{: cli}

Before you can use the CLI, you must install the {{site.data.keyword.cloud_notm}} CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

Deprovisioning a custom authorized CIDR is performed by IBM. Running this command submits a deprovision request to IBM; you cannot complete the operation yourself. IBM processes the request and removes the CIDR from your account. This process takes a minimum of two weeks.

Before you run this command, delete all public address ranges allocated from the CIDR.

```sh
ibmcloud is public-address-range-authorized-cidr-delete AUTHORIZED_CIDR [-f, --force] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`AUTHORIZED_CIDR`
:   The ID or name of the custom authorized CIDR to deprovision.

`-f, --force`
:   Force the operation without confirmation.

`--output`
:   The output format, only JSON is supported. One of: JSON.

`-q, --quiet`
:   Suppress verbose output.

## Deprovisioning a custom authorized CIDR with the API
{: #deprovision-custom-authorized-cidr-api}
{: api}

Before you begin, set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).

Deprovisioning a custom authorized CIDR is performed by IBM. Sending this request submits the deprovision request to IBM; you cannot complete the operation yourself. Before you send the deprovision request, delete all public address ranges allocated from the CIDR.

Send a `DELETE` request to the `/v1/public_address_range/authorized_cidrs/{id}` endpoint:

```sh
curl -sX DELETE "$vpc_api_endpoint/v1/public_address_range/authorized_cidrs/$cidr_id?version=$api_version&generation=2" \
  -H "Authorization: Bearer $iam_token"
```
{: pre}

IBM processes the request and removes the CIDR from your account. This process takes a minimum of two weeks.



## Next steps
{: #next-steps-deprovision-byoip}

* [Prepare a Letter of Authorization](/docs/vpc?topic=vpc-ipv4-letter-of-authorization).
