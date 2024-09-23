---

copyright:
  years: 2022, 2024
lastupdated: "2024-09-23"

keywords: backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Enabling event notifications for Backup for VPC
{: #event-notifications-events}

As an administrator of IBM Cloud Backup for VPC, you might want to send notifications of events in IBM Cloud Backup for VPC to other users, or human destinations, by using email, SMS, or other supported delivery channels. Additionally, you might want to send these notifications of events to other applications to build logic by using event-driven programming with webhooks, for example. It is made possible by the integration between IBM Cloud Backup for VPC and {{site.data.keyword.en_full}}.
{: shortdesc}

To send information to {{site.data.keyword.en_short}}, you must connect your IBM Cloud Backup for VPC instance to {{site.data.keyword.en_short}}. For more information about working with {{site.data.keyword.en_short}}, see [Getting started with {{site.data.keyword.en_short}}](/docs/event-notifications?topic=event-notifications-getting-started).

## How events are collected and sent by IBM Cloud Backup for VPC
{: #event-notifications-how}

Backup jobs fulfill 2 types of tasks. One task is responsible for taking the backup snapshots as scheduled, the other task manages the existing backups and applies the retention rule on the backups. If a backup job fails, IBM Cloud Backup for VPC communicates with a connected {{site.data.keyword.en_short}} instance to forward a notification to a [supported destination](/docs/event-notifications?topic=event-notifications-supported-destinations).

## Events for IBM Cloud Backup for VPC
{: #event-notifications-list}

The following table lists the IBM Cloud Backup for VPC events.

| Event type                                 | Description |
|--------------------------------------------|-------------|
| Volume backup job creation failure \n - `service-to-service-policy-missing` \n - `snapshot-quota-reached` \n - `snapshots-bad-state` \n - `snapshots-source-volume-busy` \n - `snapshot-volume-too-large` \n - `snapshot-volume-unavailable` \n - `snapshots-encryption-key-invalid` | These events are created when a backup job can't create a backup snapshot due to missing authorization, too many existing snapshots, or when the source volume is busy. For more information, see [snapshot limit](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-3), [snapshot lifecycle states](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-status), and [service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth). |
| Volume backup job retention failure \n - `service-to-service-policy-missing` \n - `snapshot-bad-state` \n - `snapshot-in-pending-state` | These events are created when a backup job can't delete a backup snapshot due to missing authorization, or because the snapshot was in `pending` or `bad` state. For more information, see [snapshot lifecycle states](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-status), and [service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth).  |
| Instance backup job creation failure \n - `service-to-service-policy-missing` \n - `snapshot_consistency_group-quota-reached` \n - `snapshot_consistency_group-bad-state` \n - `snapshot_consistency_group-source-volume-busy` \n - `snapshot_consistency_group-volume-too-large` \n - `snapshot_consistency_group-volume-unavailable`  \n - `snapshot_consistency_group-encryption-key-invalid` | These events are created when a backup job can't create the snapshots of a consistency group due to various reasons. For example, missing authorization, unavailability of the instance that the volumes are attached to, inaccessible encryption key, or service limits for size of the volume or number of snapshots are reached. For more information, see [snapshot lifecycle states](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-status), and [service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth). |
| Instance backup job retention failure \n - `service-to-service-policy-missing` \n - `snapshot_consistency_group-bad-state` \n - `snapshot-consistency-group-in-pending-state` |  These events are created when a backup job can't delete the backup snapshots that are part of a consistency group due to missing authorization, or the snapshots are in `pending` state. For more information, see [snapshot lifecycle states](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-status), and [service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth). |
{: caption="Table 1. Actions that generate event notifications" caption-side="bottom"}

When you see these alerts, you can use the [API](/docs/vpc?topic=vpc-backup-view-policy-jobs&interface=api#backup-view-jobs-details-api) or [CLI](/docs/vpc?topic=vpc-backup-view-policy-jobs&interface=cli#backup-view-jobs-details-cli) to confirm the reason for the failed status.

For more information about how to resolve the issue, see [Troubleshooting Backup for VPC](/docs/vpc?topic=vpc-baas-troubleshoot).

## Enabling notifications
{: #event-notifications-enable}

Events that are generated by an instance of the IBM Cloud Backup for VPC service can be forwarded to an {{site.data.keyword.en_short}} service instance that is available in the same account. You can configure only one Backup service source to one {{site.data.keyword.en_short}} service instance.

If an IAM authorization between IBM Cloud Backup for VPC and {{site.data.keyword.en_short}} doesn't exist in your account yet, create one with the Event Source Manager role. For more information, see [Establishing service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth&interface=ui#backup-s2s-auth-procedure-en-ui).

Before you can enable notifications for IBM Cloud Backup for VPC, be sure that you have an [{{site.data.keyword.en_short}} service instance](/catalog/services/event-notifications){: external} that is in the same account as your IBM Cloud Backup for VPC instance. 

Then, you can use the **Actions > Event notifications** section in the IBM Cloud Backup policy details page to connect the services.{: ui} 
Then, you can connect to {{site.data.keyword.en_short}} programmatically by calling the Event Notification API.{: api}

### Connecting to {{site.data.keyword.en_short}} in the console
{: #event-notifications-enable-ui}
{: ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Backup policies**.
1. Click a policy name. 
1. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Event notifications**.
1. In the Connect to {{site.data.keyword.en_short}} side panel, review the source details for the connection, and provide a description.
1. Select the resource group and the {{site.data.keyword.en_short}} service instance that you want to connect.

   If an IAM authorization between IBM Cloud Backup for VPC and {{site.data.keyword.en_short}} doesn't exist in your account, follow the steps in [Enabling service-to-service authorization for Event Notifications](/docs/vpc?topic=vpc-backup-s2s-auth&interface=ui#backup-s2s-auth-procedure-en-ui) to set it up. AIM Admin role is required to set up authorizations.

1. To confirm the connection, click **Save**.

### Connecting to {{site.data.keyword.en_short}} from the CLI
{: #event-notifications-enable-cli}
{: cli}

To connect your IBM Cloud Backup for VPC source to from the CLI, use the `ibmcloud event-notifications sources-create` command.

```sh
ibmcloud event-notifications sources-create --instance-id INSTANCE-ID --name NAME --source BACKUP POLICY CRN --description DESCRIPTION [--enabled ENABLED]
```
{: pre}

For more information, see the [Event Notification CLI reference](/docs/event-notifications?topic=event-notifications-event-notifications-cli#en-cli-source-create-command){: external}.


### Connecting to {{site.data.keyword.en_short}} with the API
{: #event-notifications-enable-api}
{: api}

The following example shows a query that you can use to register your IBM Cloud Backup for VPC source details with {{site.data.keyword.en_short}}. When you call the API, replace the ID variables and IAM token with the values that are specific to your IBM Cloud Backup for VPC instance.

You can find the `event_notifications_instance_crn` value in the console by going to the Resource list page and clicking the {{site.data.keyword.en_short}} instance row.
{: tip}

```sh
curl -X POST "{base_url}/v1/instances/{instance_id}/sources"
    -H "Authorization: Bearer {iam_token}"   
    -H "Content-Type: application/json"   
    -d '{
      "name":"Event Notification Source - Backup policy",
      "source" : "crn:v1:bluemix:public:is:us-south:a/{{account_id}}::backup-policy:",
      "description":"Backup policy service registration. This source provides notifications for backup job failures.",
      "enabled":true,
      "event_notifications_instance_crn": "crn:v1:bluemix:public:event-notifications:<region>:a/<account-id>:<service-instance>::"
      }'
```
{: codeblock}

A successful response returns the CRN value of your connected {{site.data.keyword.en_short}} service instance. For more information about the required and optional request parameters, see the [Event Notification API docs](/apidocs/event-notifications#create-sources).

### Connecting to {{site.data.keyword.en_short}} with Terraform
{: #event-notifications-enable-terraform}
{: terraform}

To connect the IBM Cloud Backup for VPC service to {{site.data.keyword.en_short}}, use the `ibm_en_source` resource. For more information about the arguments and attributes, see the Terraform reference for the [ibm_en_source](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/en_source){: external} resource.

```terraform
resource "ibm_en_source" "en_source" {
  instance_guid = ibm_resource_instance.en_terraform_test_resource.guid
  name          = "EN Source for Backup jobs"
  source        = "crn:v1:bluemix:public:is:us-south:a/{{account_id}}::backup-policy:"
  description   = "API source for Event Notifications destinations"
  enabled       = true
}
```
{: codeblock}

For more information, see [Working with Terraform in Event Notifications](/docs/event-notifications?topic=event-notifications-en-tera-workwith).

## Delivering notifications to select destinations
{: #event-notifications-destinations}

After you enable notifications for IBM Cloud Backup for VPC, create topics and subscriptions in {{site.data.keyword.en_short}} so that alerts can be forwarded and delivered to your selected destinations.

For a complete list of supported destinations, see the [{{site.data.keyword.en_short}} documentation](/docs/event-notifications?topic=event-notifications-supported-destinations).
{: tip}

### Email notifications
{: #event-notifications-email}

You can use the [{{site.data.keyword.cloud_notm}} email service](/docs/event-notifications?topic=event-notifications-en-destinations-email) as a delivery channel for IBM Cloud Backup for VPC event notifications. [Create an {{site.data.keyword.en_short}} subscription](/docs/event-notifications?topic=event-notifications-en-create-en-subscription) between an existing topic and the {{site.data.keyword.cloud_notm}} email service to forward your alerts to various recipients by email.

An email from {{site.data.keyword.cloud_notm}} that contains information about an IBM Cloud Backup for VPC event resembles the following example:

```plaintext
Email Title: Volume Backup Job Creation Failure

Email Body:

Volume Backup Job Creation Failure. Resource: crn:v1:staging:public:is:us-south-1:a/be09dae34250437f96fc8b27ae7d233b::volume:r134-f6583aac-8ba9-4d04-ab7a-acfe7b420016. BackupJobID: r134-899ab8fb-3b71-4fc8-bca5-1544cb425e4f. Reason: Snapshot quota per volume reached.

Notification details:

 {
        "backup_policy": "r134-306fd8f6-b14c-410d-8f29-f05282b6daa9",
        "backup_policy_job": "r134-899ab8fb-3b71-4fc8-bca5-1544cb425e4f",
        "backup_policy_plan": "r134-444bb07f-3399-42d5-b895-febcaca8d1bc",
        "match_resource_type" : "volume",
        "job_type" : "creation",
        "reasons": [
          {
            "more_info": "https://cloud.ibm.com/docs/vpc?topic=vpc-troubleshooting-backup-for-vpc",
            "status": "source_volume_too_large"
          }
        ],
        "resource": "crn:v1:staging:public:is:us-south-1:a/be09dae34250437f96fc8b27ae7d233b::volume:r134-f6583aac-8ba9-4d04-ab7a-acfe7b420016"
 }
```
{: screen}

To receive detailed information about an event notification in your email, select the **Add notification payload** option when you create an {{site.data.keyword.en_short}} subscription. Your email displays the [notification payload details](#event-notifications-payload) that are associated with the event.
{: tip}

### Webhooks
{: #event-notifications-webhook}

You can configure a webhook destination so that an incoming notification can be consumed programmatically by an app or service. For more information about setting up webhooks, check out the [{{site.data.keyword.en_short}} documentation](/docs/event-notifications?topic=event-notifications-en-destinations-webhook).

## Notification payload details
{: #event-notifications-payload}

Successful events that are generated by IBM Cloud Backup for VPC contain various fields that help you to identify the source and details of an event.

Event notifications from IBM Cloud Backup for VPC contain only metadata properties, such as names or identifiers of resources. Sensitive data, for example API keys or passwords, are not included in generated events.
{: note}

The properties that are sent to {{site.data.keyword.en_short}} vary depending on the event type. For example, if an `Backup job creation failed` event takes place, IBM Cloud Backup for VPC sends a notification payload to {{site.data.keyword.en_short}} that is similar to the following example.

```json
{
  "ibmendefaultlong": "{\"status\":\"Backup job creation / retention failed\",
                        \"backup_policy\": r134-6da51cfe-6f7b-4638-a6ba-00e9c327b178, 
                        \"backup_policy_plan\":\"r134-076191ba-49c2-4763-94fd-c70de73ee2e6\",
                        \"status_reason\": \"source_volume_busy: The source volume has busy set (after multiple retries)\",
                        \"match_resource_type\": \"instance\",
                        \"job_type\": \"creation\",
                        \"more_info\": \"https://cloud.ibm.com/docs/vpc?topic=vpc-baas-troubleshoot\"}",
  "ibmendefaultshort": "Backup Job creation / retention failed",
  "id": "b2198eb8-04b1-48ec-a78c-ee87694dd845",
  "time": "2024-04-22T20:59:34Z",
  "ibmenseverity": "medium",
  "type": "com.ibm.cloud.is.backup-policy.backup-volume-creation",
  "source": "com.ibm.cloud.is.backup-policy",
  "specversion": "1.0",
  "ibmensourceid": "41cdb7d4-db3e-496b-b7df-d4ead362c412:api"
}
```
{: screen}
