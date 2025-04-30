---

copyright:
  years: 2022, 2025
lastupdated: "2025-04-30"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data, faqs

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for Enterprise Backup for VPC
{: #backup-service-enterprise-faq}

The following questions pertain to the VPC Backup service for Enterprise accounts. If you have other questions you'd like to see answered here, provide feedback by using the **Open doc issue** or **Edit topic** links after the FAQs.
{: shortdesc}

## What service-to-service authorizations are required?
{: faq}
{: #faq-baas-ee-1}

To create a backup policy and plans and for the backup jobs to run correctly, multiple service-to-service authorizations are required. The IBM Cloud Backup for VPC service needs to be authorized to work with {{site.data.keyword.block_storage_is_short}}, Block Storage Snapshots for VPC, and Virtual Server for VPC services. For more information, see [Establishing service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth).

## How can I tell whether authorizations are configured correctly?
{: faq}
{: #faq-baas-ee-2}

When you log in any of the child accounts in the console, you can view the IAM authorizations by clicking **Manage > Access (IAM) > Authorizations**. 

If any of the required authorizations are missing, the backup job fails. When the backup job fails for this reason, an error message is generated that looks like the following example.

```text
Backup Policy Service for VPC: create backup-policy-job PlanID:r123-d4567 Enterprise sub-account missing S2S setup. AccountID a1234567 -failure
```

For more information, see [Activity tracking events for IBM Cloud VPC](/docs/vpc?topic=vpc-at_events).

## How can I identify the number of resources a backup policy is applied to?
{: faq}
{: #faq-baas-ee-3}

Currently, the number of resources that a backup policy is applied to can't be seen from the enterprise account. When you view the Backup policies for VPC page of the enterprise account in the [console](/login){: external}, you can click the name of the backup policy that was created for the account. Then, click the applied resources tab to view the list of volumes that the policy applies to. The list includes volumes that were created by users for the account. If the policy is an enterprise-wide policy, the list shows volumes of the enterprise account, and not the volumes of its child accounts. For more information, see [Viewing the list of volumes that are associated to a backup policy in the console](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-vol-backup-policies).

One way to identify the volumes is to go to the child accounts and list their volumes and filter for the tag that the enterprise policy specified for target resources.

By using the API, you can make a `GET /volumes` request to list summary information about all volumes of an account and filter the response by the `user_tags` that associate the volumes to the backup policy.
See the following example that lists all volumes with the `dev:test` tag.

```sh
curl -X GET "$vpc_api_endpoint/v1/volumes?version=2023-08-04&generation=2&user_tags=dev:test" \
-H "Authorization: $iam_token"
```
{: pre}

For more information, see [Viewing all Block Storage for VPC volumes with the API](/docs/vpc?topic=vpc-viewing-block-storage&interface=api#viewall-vol-api).

From the CLI, you can run the `ibmcloud is backup-policies` command with the `--tag` option to list all the volumes that have the user tag that associates the volumes to the backup policy. See the following example.

```sh
ibmcloud is backup-policies --tag dev:test
```
{: pre}

For more information, see [Listing all backup policies that are filtered by user tags from the CLI](/docs/vpc?topic=vpc-backup-view-policies&interface=cli#backup-view-all-filter-by-tags-cli). 

## Where can I find all the backups created?
{: faq}
{: #faq-baas-ee-4}

The backup snapshots are created at the child account level and volumes can be restored at the same child account level. Subaccounts have access to their own backups and not the backups that belong to other child accounts.

The enterprise administrator can make a `GET /backup_policies/{backup_policy_id}/jobs` request to the VPC API to see a consolidated view of all the backup jobs that belong to the enterprise account backup policy. For more information, see [Viewing backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs).

## How to identify the enterprise CRN?
{: faq}
{: #faq-baas-ee-5}

When you want to create a backup policy for your enterprise account and all child accounts from the CLI or with the API, you need to fetch your enterprise account `crn`.  

To obtain the enterprise CRN programmatically, you need to make a `GET /accounts/{accountID}` request to the [Enterprise API](/apidocs/enterprise-apis/enterprise#get-account){: external}. See the following example.

```sh
curl -X GET "https://enterprise.cloud.ibm.com/v1/accounts/$ACCOUNT_ID" -H "Authorization: Bearer <IAM_Token>" -H 'Content-Type: application/json'
```
{: pre}

In the response, look for the "parent" CRN. The "parent" CRN contains the enterprise ID and the account ID.

To obtain the enterprise CRN from the CLI, run the following command. The output lists the enterprise account name, ID, and CRN.

```sh
ibmcloud enterprise show
```
{: pre}

### Obtain enterprise CRN in the console
{: #faq-baas-ee-5-ui}

In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the enterprise dashboard. From there, you can view the enterprise details, accounts, users, and billing information. For more information, see [What is an enterprise](/docs/enterprise-management?topic=enterprise-management-what-is-enterprise).

### Obtain enterprise CRN from the CLI
{: #faq-baas-ee-5-cli}

Run the following command to see the enterprise account name, ID, and CRN.

   ```sh
   ibmcloud enterprise show
   ```
   {: pre}

For more information, see the CLI reference for [ibmcloud enterprise show](/docs/enterprise-management?topic=enterprise-management-ibmcloud_enterprise#ibmcloud_enterprise_show).

### Obtain enterprise CRN with the API
{: #faq-baas-ee-5-api}

Make an API request to the Enterprise Management API like the following example.

```sh
curl -X GET "https://enterprise.cloud.ibm.com/v1/enterprises" -H "Authorization: Bearer <IAM_Token>" -H 'Content-Type: application/json'
```
{: pre}

For more information, see the API Spec for [list enterprises](/apidocs/enterprise-apis/enterprise#list-enterprises).

## What does the health state mean?
{: faq}
{: #faq-baas-ee-6}

When you make a `GET /backup_policies/{id}` request, the API returns a `health_state` value as part of the information about the backup policy.

| Health state | Meaning |
|--------------|---------|
|`ok`| No abnormal behavior was detected. |
|`degraded`| Experiencing compromised performance, capacity, or connectivity. |
|`faulted`| Unreachable, inoperative, or otherwise entirely incapacitated. |
|`inapplicable` | The health state does not apply because of the current lifecycle state. A resource with a lifecycle state of `failed` or `deleting` also has a health state of `inapplicable`. A `pending` resource can also have this state.|
{: caption="Backup policy health states." caption-side="bottom"}

For more information, see the API Spec for [Retrieve a backup policy](/apidocs/vpc/latest#get-backup-policy){: external}.

## Can I change the scope of my backup policy to apply to all the accounts within the enterprise?
{: faq}
{: #faq-baas-ee-7}

No, the scope cannot be changed for an existing backup policy. However, you can delete the old policy and create another with the enterprise-wide scope.
