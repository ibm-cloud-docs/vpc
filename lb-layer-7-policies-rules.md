---

copyright:
  years: 2018, 2024
lastupdated: "2024-07-08"

keywords: application load balancer, alb, polices, rules

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Layer 7 load balancing
{: #layer-7-load-balancing}

Both public and private application load balancers support layer 7 load balancing. Data traffic is distributed based on configured policies and rules. A policy defines the action to take, which means how the traffic is distributed, when the incoming request matches the rules that are associated with the policy.

## Policies
{: #layer-7-policy}

You can define policies for HTTP and HTTPS listeners. For each policy, you must define one or more rules. The policy is applied only when all its rules are matched.

You can attach more than one policy to a listener. In general, a policy with the lowest priority is evaluated first. Each policy must have a different priority.

If the incoming request does not match the rules for any policy, the system request redirects to the configured HTTPS redirect listener, if present. Otherwise, the system redirects the request to the default pool of the listener. The HTTPS redirect has a higher precedence over the default pool on an HTTP listener.

The following actions are supported for a layer 7 policy:

* **Reject** - The request is denied with a 403 response.
* **Redirect** - The request is redirected to a configured URL and response code.
* **Forward** - The request is sent to a specific back-end pool.
* **HTTPS Redirect** - The HTTP request redirects to an HTTPS listener.

## Policy properties
{: #layer-7-policy-properties}

Property  | Description
------------- | -------------
Name | The name of the policy. The name must be unique within the listener.
Action | The action to take when all policy rules match. The acceptable values are `reject`, `redirect`, `forward`, and `https_redirect`.
Priority | Policies are evaluated based on ascending order of priority.
URL | The URL to which the request is redirected, if the action is set to `redirect`. You must provide either a full URL or the parameters of a URI. When using a URL, all incoming traffic redirects to this URL. When using URI parameters, values from incoming traffic requests can be retained by using the incoming values of the parameters. The default values of the URI parameters are equal to their original incoming values. To retain the incoming values, provide them as `{protocol}`, `{port}`, `{host}`, `{path}`, and `{query}`. For example, if the host of the incoming request is `ibm.com`, then the default value will be `{host}` equal to the incoming `ibm.com` value.
HTTP status code | Status code of the response returned by the application load balancer when the action is set to `redirect` or `https_redirect`. The acceptable values are: `301`, `302`, `303`, `307`, or `308`.
Target | The back-end pool of virtual server instances to which the request is forwarded, if the action is set to `forward`.
Listener | The HTTPS listener to which the request is redirected, if the action is set to `https_redirect`.
URI | The relative URI to which the request is redirected, if the action is `https_redirect`. This property is optional.
{: caption="Table 1. Description of policy properties" caption-side="bottom"}

## Rules
{: #layer-7-rules}

A layer 7 rule defines how a request is to be matched. Both URI-based routing and parameter-based routing are supported. Five types of rules are supported, described in Table 2.

Type      |  Description
----------| -----------------------
`hostname` | The request matches the specified `hostname`, such as `api.my_company.com`.
`header`    | The request matches an HTTP `header` field and value, such as `Cookie: xxxx`.
`path`     | The request matches the `path` in the URL after the `hostname`, such as `/index.html`.
`query`    | The request matches the `query` in the URL, for example `x=y`. The `query` string must be percent-encoded, and it is case-sensitive.
`body`     | If the request `body` of the `POST` request is form encoding. The request matches the body, for example `key=value`. It is case-sensitive.
{: caption="Table 2. Layer 7 rules" caption-side="bottom"}

To match a request, a `condition` statement must be defined in a rule. Three conditions are supported, described in Table 3.

Condition |  Type of evaluation
----------------|---------------------
`contains`        |  Verify whether the value extracted based on the `type` contains the string that is specified in the `value`.
`equals`        |  Verify whether the value extracted based on the `type` is identical to the string specified in the `value`.
`matches_regex`           |  Match the value extracted based on the `type` with the regular expression that is specified in the `value`.
{: caption="Table 3. Condition statements defined in a rule" caption-side="bottom"}

## Rule properties
{: #layer-7-rule-properties}

Table 4 describes layer 7 policy rule properties.

Property  | Description
------------- | -------------
`type` | Specifies the type of rule. The acceptable values are `hostname`, `header`, `path`, `query`, or `body`.
`condition` | Specifies the condition with which a rule is evaluated. Condition can be: `contains`, `equals`, or `matches_regex`.
`field` | Specifies the field name. This field is applicable only to the `header` `query` and `body` rule type and does not support regular expression and wildcard characters. For example, to match a cookie in the HTTP header, the field can be set to `cookie`. When the rule type is `query` and `body`, this field is optional.
`value` | The string to be matched. Does not support wildcard characters. Supports regular expression if the `condition` is set to `matches_regex`.
{: caption="Table 4. Descriptions of rule properties" caption-side="bottom"}

**Notes**:

* If the rule type is `header`, the following characters are not allowed for `field` and `value`: `"(),/:;<=>?@[\]{}'`.
* If the rule type is `body`, the following characters are not allowed for `field` and `value`: `"'=,()&` and space.
* If the rule type is `query`, `field` and `value` must be a percent-encoded string.

## Examples: Creating policies and rules
{: #layer-7-examples-create-policy-and-rules}

The following layer 7 examples show how policies and rules are created and associated with a listener.

### Example 1: Create an HTTPS listener with redirect policies
{: #create-https-listener-redirect}

```bash
curl -H "Authorization: $iam_token" -X POST
"$vpc_api_endpoint/v1/load_balancers/$lbId/listeners" \
    -d '{
            "certificate_instance": {
                "crn": "crn:v1:bluemix:public:cloudcerts:us-south:a/1111111111111111111111111111:22222222-3333-4444-5555-666666666666:certificate:77777777777777777777777777777777"
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
                },
                {
                    "name": "uri_redirect",
                    "action": "redirect",
                    "priority": 10,
                    "target": {
                        "url": "https://{host}:8080/{path}?{query}",
                        "http_status_code": 301
                    },
                    "rules": [
                        {
                            "condition": "contains",
                            "type": "hostname",
                            "value": "pqr"
                        }
                    ]
                }
            ]
        }'
```
{: codeblock}

### Example 2: Create policies to forward requests to pools and associate it with an existing listener
{: #create-policies-forward-requests}

```bash
curl -H "Authorization: $iam_token" -X POST
"$vpc_api_endpoint/v1/load_balancers/$lbId/listeners/$listenerId/policies" \
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

### Example 3: Create an HTTP listener with https redirect policies
{: #create-http-listener-https-redirect}

```bash
curl -H "Authorization: $iam_token" -X POST
"$vpc_api_endpoint/v1/load_balancers/$lbId/listeners" \
    -d '{
            "connection_limit": 2000,
            "port": 80,
            "protocol": "http",
            "policies": [
                {
                    "name": "hostname_header",
                    "action": "https_redirect",
                    "priority": 1,
                    "target": {
                        "listener": {
                            "id": "0134-d578be10-31e3-46b3-8513-79babb852319"
                        },
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
                    "action": "https_redirect",
                    "priority": 2,
                    "target": {
                        "listener": {
                            "id": "0456-a578be10-31e3-46b3-8513-79babb852398"
                        },
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
                    "action": "https_redirect",
                    "priority": 5,
                    "target": {
                        "listener": {
                            "id": "0386-d898be10-21e3-66b3-9513-69babb852390"
                        },
                        "http_status_code": 301,
                        "uri": "/test/sample"
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
