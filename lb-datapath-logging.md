---

copyright:
  years: 2020
lastupdated: "2020-09-15"

keywords: application load balancer, datapath logging

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Datapath log forwarding with LogDNA
{: #datapath-logging}

Data and health check logs are valuable for debugging and maintenance purposes. With the datapath logging feature enabled, {{site.data.keyword.vpc_full}} {{site.data.keyword.alb_full}} (ALB) forwards these logs to [IBM Log Analysis with LogDNA](https://cloud.ibm.com/observe/logging){:external} under your account.
{:shortdesc}

You can enable or disable this feature by:

* Creating a load balancer and setting this feature to on.

![Datapath Logging](images/lb-datapath-logging.png "Datapath Logging")

* Using the CLI to set `--logging-datapath-active` property to `true` for existing load balancers. 

* Using the API to enable the datapath logging.

If you do not have a LogDNA instance, you must create one before enable the datapath logging.
{: note}

## Viewing logs in the IBM Cloud logging analysis service
{: #viewing-logs-in-the-ibm-cloud-logging-analysis-service}

Log in to [IBM Log Analysis with LogDNA](https://cloud.ibm.com/observe/logging){:external} with your IBM Cloud account. Logs can be viewed from the LogDNA instance. Refer to [Getting started with IBM Log Analysis with LogDNA](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-getting-started#getting-started) for more information. 

To create a LogDNA instance, follow these steps:

1. Select **Create a logging instance**. The logging instance creation dialog shows.

2. Choose the region from the dropdown list that corresponds to the data center where you provisioned the load balancer.

  For a load balancer in SYD01, you would choose the region of Sydney.
  {: tip}

  The following table shows the mapping between regions and data center:

| Region | Data center |
| ------ | ----------- |
| Sydney | SYD01, SYD05, SYD04, MEL01 |
| Tokyo | CHE01, HKG02, SNG01, TOK02, TOK04, TOK05, SEO01, OSA02 |
| Frankfurt | AMS01, AMS03, FRA02, FRA04, FRA05, MIL01, PAR01 |
| London | LON01, LON02, LON04, LON05, LON06, OSL01 |
| Dallas | DAL00, DAL01, DAL02, DAL05, DAL06, DAL07, DAL08, DAL09, DAL10, DAL12, DAL13, HOU01, HOU02, MEX01, SJC01, SJC03, SJC04, SEA01, SAO01 |
| Washington DC | MON01, TOR01, WDC01, WDC04, WDC06, WDC07 |
{: caption="Table 1. Mapping between region and datacenter" caption-side="top"}

After you choose your region, click **Create** to create the logging instance, then configure it by clicking **Configure the platform service logs**.

## Log output examples
{: #log-output-examples}

The following output is an example of an {{site.data.keyword.cloud_notm}} {{site.data.keyword.alb_full}} datapath logging:

```
Sep 28 11:25:04 is-load-balancer crn:v1:bluemix:public:is:us-south:a/8c8a02225526799f56330a6701d939eb::load-balancer:r134-6ba32c0e-830c-483c-871a-0240c10662cf
{"PRIORITY":"info", "MSG_timestamp":"2020-09-28T03:25:03.136101+00:00", "SentByHost":"150.238.66.162", "MESSAGE":" Connect from 222.72.143.92:38605 to 10.240.128.5:62776 (r134-6ba32c0e-830c-483c-871a-0240c10662cf/HTTP)", "logSourceCRN":"crn:v1:bluemix:public:is:us-south:a/8c8a02225526799f56330a6701d939eb::load-balancer:r134-6ba32c0e-830c-483c-871a-0240c10662cf", "saveServiceCopy":false}
```
{: screen}

The logs contain the log header and the JSON string.

The log header is built with datetime (`Sep 28 11:25:04`), log source (`is-load-balancer`) and log source CRN (`crn:v1:bluemix:public:is:us-south...`).

The datapath log is a JSON string and contaisn the following fields:

| Field Name | Type | Description |
| ---- | --- | ----- |
| PRIORITY | string | The log level associated with each message on the severity of the log |
| MSG_timestamp | string | The timestamp on when the log was generated |
| SentByHost | string | The IP address of the host |
| MESSAGE | string | Description about the log |
| logSourceCRN | string | Log will be saved in the LogDNA instance of the account indicated in the CRN |
| saveServiceCopy | bool | Indicates if a copy will be saved in the LogDNA STS (the default is false) |

Below is the JSON Schema of the datapath log:

```
json
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

Note that:
* `PRIORITY` is the The log level associated with each message on the severity of the log. In the current feature, it will always be `info`.
* `MSG_timestamp` is Coordinated Universal Time.
* `SentByHost` is the VIP of the appliance. For public load balancers it will be a floating IP, for private load balancers it will be a private IP.
* `MESSAGE` is the content of the log message.
* `logSourceCRN` is used to indicate which LogDNA instance should be used to save the logs for the account.
* `saveServiceCopy` will be `false` by default and cannot be changed.

The format of the logs can be impacted by internal upgrades. You can only use these messages for debugging and not for any build automation that relies on them.
{: note}

## Related links
{: #permissions-related-links-alb}

* [Load balancer CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#alb-anchor)
* [Load balancer API reference](https://{DomainName}/apidocs/vpc#list-load-balancer-profiles)
* [Required permissions for VPC resources](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls)
* [IBM Log Analysis with LogDNA](https://cloud.ibm.com/observe/logging){:external} 
* [Getting started with IBM Log Analysis with LogDNA](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-getting-started#getting-started) 
* [Activity Tracker with LogDNA events](/docs/vpc?topic=vpc-at-events#events-load-balancers)
* [FAQs for application load balancers](/docs/vpc?topic=vpc-load-balancer-faqs)
