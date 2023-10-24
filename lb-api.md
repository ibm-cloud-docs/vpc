---

copyright:
  years: 2018, 2020
lastupdated: "2020-06-16"

keywords: application load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network, layer 7

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Application load balancer API example
{: #lbaas-apis-available-alb}
{: api}

To make API calls, you must use a REST client. For example, you can use a `curl` command to retrieve all existing load balancers:

```sh
curl -X GET "$vpc_api_endpoint/v1/load_balancers?version=$api_version&generation=2" -H "Authorization: $iam_token"
```

For detailed information on APIs available for application load balancers in your VPC environment, see the load balancers section in the [VPC API reference](/apidocs/vpc#list-all-load-balancers).

In the following example, use the API to create an application load balancer in front of two VPC virtual server instances (`192.168.100.5` and `192.168.100.6`) running a web application that listens on port `80`. The load balancer has a front-end listener, which allows secure access to the web application by using HTTPS. You can use the API to get details of the load balancer after it is created, and to delete the load balancer.

The example steps that follow skip the prerequisite steps of using the [IBM Cloud console](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console), [CLI](/docs/vpc?topic=vpc-creating-a-vpc-using-cli), or [API](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis) to provision a VPC, subnets, and instances.

You can also create a load balancer using the UI or CLI. For instructions, see [Creating an IBM Cloud Application Load Balancer for VPC](/docs/vpc?topic=vpc-load-balancer) or the [CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).
{: tip}

## Step 1. Create a load balancer with a listener, pool, and attached server instances (pool members)
{: #step-1-create-a-load-balancer-alb}

```sh
curl -H "Authorization: $iam_token" -X POST
"$vpc_api_endpoint/v1/load_balancers?version=$api_version&generation=2" \
    -d '{
        "name": "example-balancer",
        "logging": {
            "datapath": {
                "active": true
            }
        },
        "is_public": true,
        "listeners": [
            {
                "certificate_instance": {
                    "crn": "crn:v1:bluemix:public:cloudcerts:us-south:a/123456:b8877ea4-b8eg-467e-912a-da1eb7f031cg:certificate:43219c4c97d013fb2a95b21dddde1234"
                },
                "port": 443,
                "protocol": "https",
                "default_pool": {
                    "name": "example-pool"
                }
            }
        ],
        "pools": [
            {
                "algorithm": "round_robin",
                "health_monitor": {
                    "delay": 5,
                    "max_retries": 2,
                    "timeout": 2,
                    "type": "http",
                    "url_path": "/"
                },
                "name": "example-pool",
                "protocol": "http",
                "session_persistence": {
                    "cookie_name": "string",
                    "type": "source_ip"
                },
                "members": [
                    {
                        "port": 80,
                        "target": {
                            "address": "192.168.100.5"
                        },
                        "weight": 50
                    },
                    {
                        "port": 80,
                        "target": {
                            "address": "192.168.100.6"
                        },
                        "weight": 50
                    }
                ]
            }
        ],
        "subnets": [
            {
                "id": "7ec87131-1c7e-4990-b4f0-a26f2e61f98e"
            }
        ]
        }'
```
{: codeblock}

Sample output:

```sh
{
    "created_at": "2018-07-12T23:17:07.5985381Z",
    "crn": "crn:v1:bluemix:public:is:us-south:a/123456::load-balancer:dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
    "hostname": "ac34687d.lb.appdomain.cloud",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
    "id": "0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
    "is_public": true,
    "logging": {
        "datapath": {
            "active": true
        }
    },
    "listeners": [
        {
            "id": "0738-70294e14-4e61-11e8-bcf4-0242ac110004",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727/listeners/70294e14-4e61-11e8-bcf4-0242ac110004"
        }
    ],
    "name": "example-balancer",
    "operating_status": "offline",
    "pools": [
        {
            "id": "0738-70294e14-4e61-11e8-bcf4-0242ac110004",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727/pools/70294e14-4e61-11e8-bcf4-0242ac110004",
            "name": "example-pool"
        }
    ],
    "provisioning_status": "create_pending",
    "resource_group": {
        "id": "56969d60-43e9-465c-883c-b9f7363e78e8"
    },
    "subnets": [
        {
            "id": "0738-7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
            "name": "example-subnet"
        }
    ]
}
```
{: screen}

Save the ID of the load balancer to use in the next steps. For example, save it in the variable `lbid`.

```sh
lbid=0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727
```
You can deploy load balancer appliances to multiple subnets. To achieve higher availability and redundancy, deploy the load balancer to subnets in different zones.
{: tip}

## Step 2. Get details about the load balancer
{: #step-2-get-details-about-the-alb}

```sh
curl -H "Authorization: $iam_token" -X GET "$vpc_api_endpoint/v1/load_balancers/$lbid?version=$api_version&generation=2"
```
{: pre}

Allow some time for provisioning. When the load balancer is ready, it is set to `online` and `active` status, as shown in the following sample output:

Sample output:

```sh
{
  "id": "0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
  "crn": "crn:v1:bluemix:public:is:us-south:a/123456::load-balancer:dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
  "name": "example-balancer",
  "created_at": "2018-07-13T22:22:24.489Z",
  "hostname": "dd754295-e9e0-4c9d-bf6c-58fbc59e5727.lb.appdomain.cloud",
  "is_public": true,
  "logging": {
      "datapath": {
          "active": true
      }
  },
  "listeners": [
    {
      "id": "0738-70294e14-4e61-11e8-bcf4-0242ac110004",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727/listeners/70294e14-4e61-11e8-bcf4-0242ac110004"
    }
  ],
  "operating_status": "online",
  "pools": [
    {
      "id": "0738-70294e14-4e61-11e8-bcf4-0242ac110004",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727/pools/70294e14-4e61-11e8-bcf4-0242ac110004",
      "name": "example-pool"
    }
  ],
  "private_ips": [
    {
      "address": "192.168.10.5"
    },
    {
      "address": "192.168.10.6"
    }
  ],
  "provisioning_status": "active",
  "public_ips": [
    {
        "address": "169.11.111.115"
    },
    {
        "address": "169.11.111.116"
    }
  ],
  "resource_group": {
    "id": "0738-56969d60-43e9-465c-883c-b9f7363e78e8"
  },
  "subnets": [
    {
      "id": "0738-7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
      "name": "example-subnet"
    }
  ]
}
```
{: screen}

## Step 3. Delete the application load balancer
{: #step-3-delete-the-alb}

```sh
curl -H "Authorization: $iam_token" -X DELETE "$vpc_api_endpoint/v1/load_balancers/$lbid?version=$api_version&generation=2"
```
{: pre}
