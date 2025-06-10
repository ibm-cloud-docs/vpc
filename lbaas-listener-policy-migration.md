---

copyright:
  years: 2025
lastupdated: "2025-06-10"

keywords: application load balancer migration, api migration, versioned change

subcollection: vpc

---

# Updating to the `2025-04-08` version (SNI hostname support for application load balancers)
{: #2025-04-08-migration-alb-listener-policy}

As described in the [VPC API](/apidocs/vpc/latest) reference [versioning](/apidocs/vpc#api-versioning) policy, most changes to the VPC APIs are fully backward compatible and are made available to all clients, regardless of the API version the client requests. However, the `2025-04-08` release of the VPC API necessitated incompatible changes in support of [creating a listener policy](/apidocs/vpc/latest#create-load-balancer-listener-policy) for application load balancers (ALBs).

Before you adopt the release version `2025-04-08` or later, review the changes described in this migration guidance that might require you to update your client.

## Changed application load balancer policy properties
{: #changed-application-load-balancer-listener-policy}

The following application load balancer listener policy property has changed for API requests that use a `version` query parameter of `2024-04-08` or later.

When [creating an application load balancer policy](/apidocs/vpc/latest#create-load-balancer-listener) (`POST /load_balancers/{load_balancer_id}/listeners/{listener_id}/policies`):

| Old property           |        New property      |
|------------------------|--------------------------|
|   `action.forward`     | `action.forward_to_pool` |
{: caption="Old and new properties when creating an ALB listener policy." caption-side="bottom"}

When [retrieving an ALB listener policy](/apidocs/vpc/latest#get-load-balancer-listener-policy) (`GET /load_balancers/{load_balancer_id}/listeners/{listener_id}/policies/{id}`):

| Old property           |        New property      |
|------------------------|--------------------------|
|   `action.forward`     | `action.forward_to_pool` |
{: caption="Old and new properties when retrieving an ALB listener policy." caption-side="bottom"}


When [listing policies for a load balancer listener](/apidocs/vpc/latest#list-load-balancer-listener-policies) (`GET /load_balancers/{load_balancer_id}/listeners/{listener_id}/policies`):

| Old property           |        New property      |
|------------------------|--------------------------|
|   `action.forward`     | `action.forward_to_pool` |
{: caption="Old and new properties when listing an ALB listener policies." caption-side="bottom"}

## Action needed
{: #action-needed-application-load-balancer-listener-policies}

Before you specify the `version` query parameter of `2025-04-08` or later, follow these actions to avoid regressions in client functionality.

If your clients continue to specify version `2025-04-07` or earlier, no changes are required.
{: note}


### Client migration
{: #client-migration-application-load-balancer-listener-policies}

Before you migrate a client to API version `2025-04-08` or later, review your code that uses the following methods:

- `POST /load_balancers/{load_balancer_id}/listeners/{listener_id}/policies`
- `GET /load_balancers/{load_balancer_id}/listeners/{listener_id}/policies/{id}`
- `GET /load_balancers/{load_balancer_id}/listeners/{listener_id}/policies`

Review the changes that were announced in the [API change log](/docs/vpc?topic=vpc-api-change-log#version-2025-04-08), and verify that your code adopts these changes in a manner that is appropriate for your programming language.

## Example
{: #example-create-application-load-balancer-listener-policy}

### Creating an ALB listener policy

The following example uses API version `2025-04-07` or earlier to create an application load balancer listener policy with the `action` `forward`.

```sh
curl -X POST "$vpc_api_endpoint/v1/load_balancers/$load_balancer_id/listeners/$listener_id/policies?version=2025-04-07&generation=2"
-H "Authorization: $iam_token"
 -d '{
      "action": "forward",
      "name": "older-version-2025-04-07",
      "priority": 3,
      "rules": [
        {
          "condition": "contains",
          "type": "hostname",
          "value": "www.example.com"
        }
      ],
      "target": {
        "id": "$target_pool_id"
      }
  }'
```
{: pre}

The following example uses API version `2025-04-08` or later to create an application load balancer listener policy with the `action` `forward_to_pool`.
```sh
curl -X POST "$vpc_api_endpoint/v1/load_balancers/$load_balancer_id/listeners/$listener_id/policies?version=2025-04-08&generation=2"
-H "Authorization: $iam_token"
 -d '{
      "action": "forward_to_pool",
      "name": "newer-version-2025-04-08",
      "priority": 3,
      "rules": [
        {
          "condition": "contains",
          "type": "hostname",
          "value": "www.example.com"
        }
      ],
      "target": {
        "id": "$target_pool_id"
      }
  }'
```
{: pre}
