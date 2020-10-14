---

copyright:
  years: 2020
lastupdated: "2020-04-27"

keywords: load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Integrating with the {{site.data.keyword.cloud_notm}} Activity Tracker
{: #activity-tracker-integration}

The {{site.data.keyword.nlb_full}} (NLB) integrates with IBM Cloud Activity Tracker with LogDNA, which records events in a manner compliant with the CADF standard, as triggered by user-initiated activities that change the state of the service in the cloud.

For a list of actions that are recorded as auditing events on the load balancer service instances, see [Activity tracker with LogDNA events](/docs/vpc?topic=vpc-at-events#events-load-balancers).

All auditing events are recorded to the {{site.data.keyword.cloud_notm}} Activity Tracker with LogDNA in the `us-south` region, regardless of which region the load balancer service is actually provisioned.
{: important}

To view events, you must provision an IBM Cloud Activity Tracker with LogDNA instance in the `us-south` region under your account. Users in your account must have an IAM policy that grants the **Viewer** platform access role and **Reader** service access role on the IBM Cloud Activity Tracker with LogDNA instance.

For more information about granting access, see [Activity Tracker with LogDNA events](/docs/vpc?topic=vpc-at-events).

IBM Cloud account users can monitor account-level operations that are performed on the load balancer service.
{: tip}

To provision an IBM Cloud Activity Tracker with LogDNA instance:

1. [Log in to {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.
2. Click the ![Menu icon](../../icons/icon_hamburger.svg) and select **Observability > Activity Tracker**.
3. Click **Create Instance**.
4. Define a service name.
5. Select `us-south` as the region and choose the resource group.
6. Choose a plan other than `lite` if you have a paid account.
7. Click **Create**.

For more information, see the [getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-getting-started).

Here is the sample activity tracker message for a **Create Listener** operation:

```
bash
{
    "logSourceCRN": "crn:v1:staging:public:is:eu-gb:a/<ACCOUNT_ID>::load-balancer:4633518f-8aac-48a1-a694-d15ee6bd70e3",
    "meta": {
        "serviceProviderName": "is-load-balancer",
        "serviceProviderProjectId": "48a7a7b7-6642-4aa1-8af9-c1be4ef82050",
        "serviceProviderRegion": "ng",
        "userAccountIds": [
            <ACCOUNT_ID>
        ]
    },
    "payload": {
        "action": "is.load-balancer.load-balancer.listener.create",
        "eventTime": "2019-05-30T18:42:48.96+0000",
        "eventType": "activity",
        "id": "e4ee1906d01a35efe8bd8303ce0a734e",
        "initiator": {
            "credential": {
                "type": "token"
            },
            "host": {
                "address": <CLIENT_IP>,
                "agent": "python-requests/2.19.1"
            },
            "id": <USER-ID>,
            "name": <USER_ID>,
            "project_id": <ACCOUNT_ID>,
            "typeURI": "service/security/account/user"
        },
        "message": "is.load-balancer: create listener 4633518f-8aac-48a1-a694-d15ee6bd70e3 success",
        "observer": {
            "id": "activity-tracker.cloud.ibm.com",
            "name": "ActivityTracker",
            "typeURI": "security/edge/activity-tracker"
        },
        "outcome": "success",
        "reason": {
            "reasonCode": 201,
            "reasonType": "https://www.iana.org/assignments/http-status-codes/http-status-codes.xml"
        },
        "requestData": "{\"headers\":{\"RayID\":\"4df2d9911a3ac2bd\"},\"extraData\":{\"resourceName\":\"4633518f-8aac-48a1-a694-d15ee6bd70e3\"}}",
        "requestPath": "/v1/load_balancers/4633518f-8aac-48a1-a694-d15ee6bd70e3/listeners",
        "severity": "normal",
        "target": {
            "host": {
                "address": <API_END_POINT>
            },
            "id": "crn:v1:staging:public:is:eu-gb:a/<ACCOUNT_ID>::load-balancer:4633518f-8aac-48a1-a694-d15ee6bd70e3",
            "name": "4633518f-8aac-48a1-a694-d15ee6bd70e3",
            "typeURI": "/v1/load_balancers/4633518f-8aac-48a1-a694-d15ee6bd70e3/listeners"
        },
        "typeURI": "http://schemas.dmtf.org/cloud/audit/1.0/event"
    },
    "saveServiceCopy": true
}
```
{: codeblock}

## Configuring ACLs for use with load balancers
{: #acls-nlb}

If you use access control lists (ACLs) to block traffic on the subnets in which the load balancer is deployed, make sure that ACL rules are in place to allow incoming traffic for the configured listener ports and management ports (ports 56500 - 56520). You must also allow traffic between the load balancer and back-end instances.
