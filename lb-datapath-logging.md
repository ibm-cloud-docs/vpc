---

copyright:
  years: 2020, 2023
lastupdated: "2023-02-06"

keywords: application load balancer, datapath logging

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Datapath log forwarding
{: #datapath-logging}

Data and health check logs are valuable for debugging and maintenance purposes. With the datapath logging feature enabled, {{site.data.keyword.vpc_full}} {{site.data.keyword.alb_full}} (ALB) forwards these logs to your account's [{{site.data.keyword.la_full_notm}}](https://cloud.ibm.com/observe/logging){: external} dashboard.
{: shortdesc}

To enable or disable the datapath logging feature, you can:

* Create a load balancer and enable or disable the toggle button.

* Use the CLI to set the `--logging-datapath-active` property to `true` for existing load balancers.

* Use the API to enable the datapath logging.

If you do not have a {{site.data.keyword.la_short}} instance, you must create one before you enable datapath logging.
{: note}

## Viewing logs in the IBM Log Analysis service
{: #viewing-logs-in-the-ibm-cloud-log-analysis-service}

Log in to [{{site.data.keyword.la_full_notm}}](https://cloud.ibm.com/observe/logging){: external} with your IBM Cloud account. You can view logs from the {{site.data.keyword.la_short}} instance. For more information, see [Getting started with {{site.data.keyword.la_full_notm}}](/docs/log-analysis?topic=log-analysis-getting-started).

To create a {{site.data.keyword.la_short}} instance, follow these steps:

1. Select **Create a logging instance**. The logging instance creation page shows.

2. Choose the region from the menu list that corresponds to the data center where you provisioned the load balancer. For example, for a load balancer in SYD01, choose the region of Sydney.

   You can find full information about the mapping between regions and data centers in [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).
   {: tip}

After you choose your region, click **Create** to create the logging instance, then configure it by clicking **Configure the platform service logs**.

## Log output examples
{: #log-output-examples}

The following output is an example of {{site.data.keyword.cloud_notm}} {{site.data.keyword.alb_full}} datapath logging:

```sh
Sep 28 11:25:04 is-load-balancer crn:v1:bluemix:public:is:us-south:a/a1234567::load-balancer:r006-6ba32c0e-830c-483c-871a-0240c10662cf
{"PRIORITY":"info", "MSG_timestamp":"2020-09-28T03:25:03.136101+00:00", "SentByHost":"150.238.66.162", "MESSAGE":" Connect from 222.72.143.92:38605 to 10.240.128.5:62776 (r006-6ba32c0e-830c-483c-871a-0240c10662cf/HTTP)", "logSourceCRN":"crn:v1:bluemix:public:is:us-south:a/a1234567::load-balancer:r006-6ba32c0e-830c-483c-871a-0240c10662cf", "saveServiceCopy":false}
```
{: screen}

The logs contain the log header and the JSON string.

The log header is built with datetime (`Sep 28 11:25:04`), log source (`is-load-balancer`) and log source CRN (`crn:v1:bluemix:public:is:us-south...`).

The datapath log is a JSON string, containing the following fields:

| Field Name | Type | Description |
| ---- | --- | ----- |
| PRIORITY | string | The log level associated with each message on the severity of the log. |
| MSG_timestamp | string | The timestamp that indicates when the log was generated. |
| SentByHost | string | The IP address of the host. |
| MESSAGE | string | Description about the log file. |
| logSourceCRN | string | Where the log file is saved in the {{site.data.keyword.la_short}} instance of the account indicated in the CRN. |
| saveServiceCopy | bool | Indicates whether to save a log in the {{site.data.keyword.la_short}} STS; the default value is `false`. |
{: caption="Table 1. Datapath log fields" caption-side="bottom}

The following is an example of the JSON schema of a datapath log:

```json
{
    "type": "object",
    "properties": {
        "PRIORITY": {
            "type": "string"
        },
        "MSG_timestamp": {
            "type": "string"
        },
        "SentByHost": {
            "type": "string"
        },
        "MESSAGE": {
            "type": "string"
        },
        "logSourceCRN": {
            "type": "string"
        },
        "saveServiceCopy": {
            "type": "boolean"
        }
    }
}
```
{: codeblock}

Note that:

* `PRIORITY` is the log level that is associated with each message on the severity of the log. Currently, the only choice is `info`.
* `MSG_timestamp` is the timestamp in Coordinated Universal Time.
* `SentByHost` is the VIP of the appliance. For public load balancers, this is the floating IP; for private load balancers, this is a private IP.
* `MESSAGE` is the content of the log message.
* `logSourceCRN` indicates which {{site.data.keyword.la_short}} instance to use to save the logs for the account.
* `saveServiceCopy` is `false` (by default) and cannot be changed.

The format of the logs can be impacted by internal upgrades. It is recommended to use these messages only for debugging purposes, not for build automation.
{: note}

## Related links
{: #datapath-logging-related-links}

* [{{site.data.keyword.la_full_notm}}](https://cloud.ibm.com/observe/logging){: external}
* [Getting started with {{site.data.keyword.la_full_notm}}](/docs/log-analysis?topic=log-analysis-getting-started)
* [Activity Tracker with {{site.data.keyword.la_short}} events](/docs/vpc?topic=vpc-at-events#events-load-balancers)
