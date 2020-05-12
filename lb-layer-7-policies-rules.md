---

copyright:
  years: 2018, 2020
lastupdated: "2020-04-02"

keywords: load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network, layer-7

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


# Layer-7 load balancing
{: #layer-7-load-balancing}

Both public and private load balancers support Layer-7 load balancing. Data traffic is distributed based on configured policies and rules. A policy defines the action to take, which means how the traffic is distributed, when the incoming request matches the rules that are associated with the policy.

## Policies
{: #layer-7-policy}

You can define policies for HTTP and HTTPS listeners. For each policy, you define one or more rules. The policy is applied only when all its rules are matched.

You can attach more than one policy to a listener. In general, a policy with the lowest priority is evaluated first. Each policy must have a different priority.

If the incoming request does not match the rules for any of the policies, the request is redirected to the default pool of the listener.

The following actions are supported for a Layer-7 policy:

* **Reject:** The request is denied with a 403 response.
* **Redirect:** The request is redirected to a configured URL and response code.
* **Forward:** The request is sent to a specific back-end pool.

Policies set to `reject` are evaluated first, regardless of their priority. Next, policies set to `redirect` are evaluated. Finally, policies set to `forward` are evaluated.

Within each action category, the policies are evaluated in ascending order of priority (lowest priority to highest priority). Table 2 defines these policy properties.

## Policy properties
{: #layer-7-policy-properties}

Property  | Description
------------- | -------------
Name | The name of the policy. The name must be unique within the listener.
Action | The action to take when all policy rules match. The acceptable values are `reject`, `redirect`, and `forward`. A policy for the `reject` action is always evaluated first, regardless of its priority. Policies with `redirect` actions are evaluated next, followed by policies with `forward` actions.
Priority | Policies are evaluated based on ascending order of priority.
URL | The URL to which the request is redirected, if the action is set to `redirect`.
HTTP status code | Status code of the response returned by the load balancer when the action is set to `redirect`. The acceptable values are: 301, 302, 303, 307 or 308.
Target | The back-end pool of virtual server instances to which the request is forwarded, if the action is set to `forward`.
{: caption="Table 1. Description of policy properties" caption-side="top"}

## Rules
{: #layer-7-rules}

A Layer-7 rule defines how a request should be matched. Three types of rules are supported, described in Table 2.

Type      |  Description
----------| -----------------------
hostname | The request matches the specified hostname, such as `api.my_company.com`.
header    | The request matches an HTTP header field, such as `Cookie`.
path     | The request matches a path in the URL, such as `/index.html`.
{: caption="Table 2. Layer-7 rules" caption-side="top"}

To match a request, a `condition` statement must be defined in a rule. Three conditions are supported, described in Table 3.

Condition |  Type of evaluation
----------------|---------------------
contains        |  Verify whether the extracted field contains the specified string.
equals        |  Verify whether the extracted field is identical to the specified string.
matches_regex           |  Match the extracted field with the specified regular expression.
{: caption="Table 3. Condition statements defined in a rule" caption-side="top"}

## Rule properties
{: #layer-7-rule-properties}

Table 4 describes Layer-7 policy rule properties.

Property  | Description
------------- | -------------
Type | Specifies the type of rule. The acceptable values are `hostname`, `header`, or `path`.
Condition | Specifies the condition with which a rule is evaluated. Condition can be: `contains`, `equals`, or `matches_regex`.
Field | Specifies the HTTP header field name. This field is applicable only to the `header` rule type. For example, to match a cookie in the HTTP header, the field can be set to `Cookie`.
Value |  The value to be matched.
{: caption="Table 4. Descriptions of rule properties" caption-side="top"}

The following characters are not allowed for `field` or `value` if the rule type is `header`: `"(),/:;<=>?@[\]{}'`.
{: tip}

## Examples: Creating policies and rules
{: #layer-7-examples-create-policy-and-rules}

The following Layer-7 examples show how policies and rules are created and associated with a listener.

### Example 1: Create an HTTPS listener with redirect policies

```
bash
curl -H "Authorization: $iam_token" -X POST
"$api_endpoint/v1/load_balancers/$lbId/listeners" \
    -d '{
            "certificate_instance": {
                "crn": "crn:v1:staging:public:cloudcerts:us-south:a/1111111111111111111111111111:22222222-3333-4444-5555-666666666666:certificate:77777777777777777777777777777777"
            },
            "connection_limit": 2000,
            "port": 443,
            "protocol": "https",
            "policies": [
                {
                    "name": "hostname_header",
                    "action": "redirect",
                    "priority": 1,
                    "target": {
                        "url": "https://www.examples.com/",
                        "http_status_code": 307
                    },
                    "rules": [
                        {
                            "condition": "contains",
                            "type": "header",
                            "field": "aheader",
                            "value": "avalue"
                        },
                        {
                            "condition": "equals",
                            "type": "hostname",
                            "value": "abc.com"
                        }
                    ]
                },
                {
                    "name": "header_cookie",
                    "action": "redirect",
                    "priority": 5,
                    "target": {
                        "url": "https://www.mycookies.com/",
                        "http_status_code": 302
                    },
                    "rules": [
                        {
                            "condition": "contains",
                            "type": "header",
                            "field": "aheader",
                            "value": "avalue"
                        },
                        {
                            "condition": "equals",
                            "type": "header",
                            "field": "cookie",
                            "value": "flavor=oatmeal"
                        }
                    ]
                },
                {
                    "name": "path_hostname",
                    "action": "redirect",
                    "priority": 10,
                    "target": {
                        "url": "https://www.myexamples.com/",
                        "http_status_code": 301
                    },
                    "rules": [
                        {
                            "condition": "contains",
                            "type": "hostname",
                            "value": "abc"
                        },
                        {
                            "condition": "equals",
                            "value": "/test",
                            "type": "path"
                          }
                    ]
                }
            ]
        }'
```
{: codeblock}

### Example 2: Create policies to forward requests to pools and associate it with an existing listener
```
bash
curl -H "Authorization: $iam_token" -X POST
"$api_endpoint/v1/load_balancers/$lbId/listeners/$listenerId/policies" \
    -d '{
            "policies": [
                {
                    "action": "forward",
                    "priority": 1,
                    "target": {
                        "id": "7df616da-4dd6-43d3-881d-801ae29e29fe"
                    },
                    "rules": [
                        {
                            "condition": "equals",
                            "type": "header",
                            "field": "cookie",
                            "value": "flavor=oatmeal"
                        }
                    ]
                },
                {
                    "action": "forward",
                    "priority": 5,
                    "target": {
                        "id": "0738-8061c411-0d50-4c79-b475-102666796434"
                    },
                    "rules": [
                        {
                            "condition": "contains",
                            "type": "header",
                            "field": "aheader",
                            "value": "avalue"
                        }
                    ]
                },
                {
                    "action": "forward",
                    "priority": 10,
                    "target": {
                        "id": "0738-62914e09-3928-4d89-b7f7-1bb7a6d7fe85"
                    },
                    "rules": [
                        {
                            "condition": "matches_regex",
                            "type": "hostname",
                            "value": "abc[a-z]*.com"
                        }
                    ]
                },
                {
                    "action": "forward",
                    "priority": 6,
                    "target": {
                        "id": "0738-62914e09-3928-4d89-b7f7-1bb7a6d7fe85"
                    },
                    "rules": [
                        {
                            "condition": "equals",
                            "type": "path",
                            "value": "/test/testtest"
                        }
                    ]
                }
            ]
        }'
```
{: codeblock}
