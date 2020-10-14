---

copyright:
  years: 2017, 2020
lastupdated: "2020-10-12"

keywords:  

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Setting up security groups by using the APIs
{: #setting-up-security-groups-using-the-apis}

The following example demonstrates how to create and manage security groups by using the {{site.data.keyword.vpc_short}} APIs.
{:shortdesc}

## Prerequisites
{: #security-group-prerequisites}

To use security groups, first you must have a running {{site.data.keyword.vpc_short}}.

For instructions about creating a VPC and subnet, see [Creating a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis).

## Step 1: Create a security group
{: #step-1-create-a-security-group}

Create a security group named `my-security-group` in your {{site.data.keyword.vpc_short}}.

```
curl -X POST "$api_endpoint/v1/security_groups?version=$api_version&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-security-group",
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Save the ID in a variable so you can use it later; for example, the variable `sg`:

```
sg=0738-2d364f0a-a870-42c3-a554-000000632953
```
{: pre}

## Step 2: Add a rule to allow SSH connections
{: #step-2-add-a-rule-to-allow-ssh-connections}

Create a rule on the security group to allow inbound connections on port 22.

```
curl -X POST "$api_endpoint/v1/security_groups/$sg/rules?version=$api_version&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
        "direction": "inbound",
        "protocol": "tcp",
        "port_min": 22,
        "port_max": 22
      }'
```
{: pre}

## Step 3: Delete the security group (optional)
{: #step-3-delete-the-securiy-group-optional}

To clean up the security group, it cannot be associated with any network interfaces, and it cannot be referenced by a rule in a different security group.

```
curl -X DELETE "$api_endpoint/v1/security_groups/$sg?version=$api_version&generation=2" \
  -H "Authorization: $iam_token"
```
{: pre}
