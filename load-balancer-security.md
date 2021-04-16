---
copyright:
  years: 2021
lastupdated: "2021-04-08"

keywords: security and compliance for *Load Balancer for VPC*, security for *Load Balancer for VPC*, compliance for *Load Balancer for VPC*,

subcollection: vpc

---

{:external: target="_blank" .external}
{:note: .note}
{:term: .term}
{:shortdesc: .shortdesc}
{:table: .aria-labeledby="caption"}

# Managing security and compliance with load balancers for VPC
{: #manage-security-compliance}

{{site.data.keyword.cloud_notm}} {{site.data.keyword.alb_full}} (ALB) and {{site.data.keyword.nlb_full}} (NLB) are integrated with the {{site.data.keyword.compliance_short}} to help you manage security and compliance for your organization.
{: shortdesc}

With the {{site.data.keyword.compliance_short}}, you can:

* Monitor for controls and goals that pertain to ALB and NLB.
* Define rules for ALB and NLB that can help standardize resource configuration.

## Monitoring security and compliance posture with load balancers
{: #monitor-lb}

As a security or compliance focal, you can use {{site.data.keyword.alb_full}} and {{site.data.keyword.nlb_full}} [goals](#x2117978){: term} to help ensure that your organization is adhering to the external and internal standards for your industry. By using the {{site.data.keyword.compliance_short}} to validate the resource configurations in your account against a [profile](#x2034950){: term}, you can identify potential issues as they arise.

All of the goals for ALB and NLB are added to the {{site.data.keyword.cloud_notm}} Best Practices Controls 1.0 profile, but can also be mapped to other profiles.
{: note}

To start monitoring your resources, check out [Getting started with {{site.data.keyword.compliance_short}}](/docs/security-compliance?topic-security-compliance-getting-started)

### Available goals for load balancers
{: #available-goals-lb}

* Ensure the application and network load balancer instances belong to a specific profile family, and are configured with no public access.

## Governing load balancer resource configuration
{: #govern-lb}

As a security or compliance focal, you can use the {{site.data.keyword.compliance_short}} to define configuration rules for the ALB and NLB instances that you create.

[Config rules](#x3084914){: term} are used to enforce the configuration standards that you want to implement across your accounts. To learn more about the about the data that you can use to create a rule for application and network load balancers, review the following table.

| Resource kind | Property | Operator | Value | Description |
|---------------|----------|---------------|-------|-------------|
| *instance* | *profile_family* | *string_equals* | *['application', 'network']* | *A list of strings matching load balancer profile family name from load balancer profile family. Ex: ['application', 'network']* |
| *instance* | *load_balancer_type* | *string_equals* | *['private', 'public']* | *A list of strings indicating what type of the load balancer can be provisioned. Ex: ['public', 'private']* |
{: caption="Table 1. Rule properties for application and network load balancers*" caption-side="top"}

The following is an example of rule configuration with only private access enabled:

```
{
  target: {
    service_name: "is.load-balancer",
    resource_kind: "instance",
    additional_target_attributes: []
  },
  required_config: {
    description: "Public access disabled",
    and: [
      {
        property: "load_balancer_type",
        operator: "string_equals",
        value: "private"
      }
    ]
  }
}
```

The following is an example of a rule configuration which prevents users from creating public type application load balancers:

```
{
  target: {
    service_name: "is.load-balancer",
    resource_kind: "instance",
    additional_target_attributes: []
  },
  required_config: {
    description: "Public access disabled for application load balancer",
    or: [
      {
        property: "profile_family",
        operator: "string_equals",
        value: "network"
      },
      {
        and: [
          {
            property: "profile_family",
            operator: "string_equals",
            value: "application"
          },
          {
            property: "load_balancer_type",
            operator: "string_equals",
            value: "private"
          }
        ]
      }
    ]
  }
}
```

Ensure that *enforcement_actions* is set with *action: disallow* to apply the rule and meet the complaince criteria. For more information, see [How to configure Rules](/docs/security-compliance?topic=security-compliance-rules).
{: note}

## Checking results for load balancers
{: #results-lb}

As a security or compliance focal, you can use the {{site.data.keyword.compliance_short}} to monitor compliance results for the application and network load balancer instances under your account against the configuration rules.

The evaluation results are only available for a limited period of time and are updated once a day. It is recommended that reports are downloaded and organized to maintain a history of compliance for audit purposes. For more information on reporting results, see [View evaluation results](https://cloud.ibm.com/security-compliance/compliance-posture/rules).
{: note}

To learn more about config rules, check out [What is a config rule?](/docs/security-compliance?topic=security-compliance-what-is-rule).
