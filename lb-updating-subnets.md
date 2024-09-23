---

copyright:
  years: 2022, 2024
lastupdated: "2024-09-23"

keywords: application load balancer, subnet, APIs, vpc network, update, detach, attach, etag

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating subnets for existing application load balancers
{: #alb-updating-subnets}

You can update subnets for an {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) with the UI, CLI, or API. ETag support is added for the load balancer resource, and is required for any resource that allows arrays to be updated.

## Updating subnets for an application load balancer using the UI
{: #alb-updating-subnets-ui}
{: ui}

To update subnets for an ALB by in the {{site.data.keyword.cloud_notm}} console, perform the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click  **Infrastructure** ![VPC icon](../../icons/vpc.svg)  > **Load balancers**.
1. Select the load balancer whose subnets you want to update.
1. Select **Overview**, click the **Edit subnets** button, then select the subnets you want to attach or detach.
   * **Attach**: Add a new subnet to the load balancer.
   * **Detach**: Remove a subnet from the load balancer.

   Load balancers must have at least one subnet. As a result, the **Detach** will not be available if only one subnet exists.
   {: important}

1. Select **Save** to finalize your changes.

## Updating subnets for an application load balancer using the CLI
{: #alb-updating-subnets-cli}
{: cli}

To use the CLI to update subnets for your application load balancer, follow these steps:

1. Set up your [CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Log in to your account using the CLI. After you enter the password, the system prompts which account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. Update your ALB's subnets:

    ```sh
    ibmcloud is load-balancer-update LOAD_BALANCER_ID --subnets SUBNET_ID_1, SUBNET_ID_2
    ```
    {: pre}

    Sample output:

    ```sh
    E.g ibmcloud is load-balancer-update r006-a3e1f3db-0e98-47ca-9f24-6e1d06dba086 --subnets 0717-ec96c7cd-46f4-4969-9009-7613f8e9e936

    Attach subnet 0717-ec96c7cd-46f4-4969-9009-7613f8e9e936 to application load balancer r006-a3e1f3db-0e98-47ca-9f24-6e1d06dba086 under account IBM Cloud Network Services as user test@ibm.com...

    ID              0717-ec96c7cd-46f4-4969-9009-7613f8e9e936
    Name            my-subnet
    Subnet cidr     10.0.0.0/24
    Subnet crn      crn:v1:bluemix:public:is:us-south-1:a123456::subnet:0717-ec96c7cd-46f4-4969-9009-7613f8e9e936
    Zone name       us-south-2
    Created         2020-08-27T14:45:42.038-05:00
    ```
    {: screen}

## Updating subnets for an application load balancer using the API
{: #alb-updating-subnets-api}
{: api}

The following example illustrates how to use the API to update subnets for an application load balancer.

To update subnets for an application load balancer with the API, perform the following procedure:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Use the following example to attach a subnet for your application load balancer:

   Save the ID and Etag of the load balancer:

   ```bash
   export lbId="r006-a3e1f3db-0e98-47ca-9f24-6e1d06dba086"
   export Etag="W/cy8hmdd4op0kws5nrta40kmrmnrwodzy4qwwghyuqwq"
   ```
   {: pre}

   ```bash
   curl -H "Authorization: $iam_token" -H "If-Match: $Etag" -X PATCH
   "$vpc_api_endpoint/v1/load_balancers/$lbId?version=$api_version&generation=2" \
      -d '{
               "subnets": [
                      {
                        "id": "0717-ec96c7cd-46f4-4969-9009-7613f8e9e936"
                     }
               ]
         }'
   ```
   {: codeblock}

   Sample output:

   ```sh
   {
       "created_at": "2020-08-24T23:36:22.990359Z",
       "id": "0717-ec96c7cd-46f4-4969-9009-7613f8e9e936",
       "name": "my-subnet",
       "subnet_cidr": "10.0.0.0/24",
       "subnet_crn": "crn:v1:bluemix:public:is:eu-gb-3:a/a123456::load-balancer:r006-a3e1f3db-0e98-47ca-9f24-6e1d06dba086",
       "zone_name": "us-south-2"
   }
   ```
   {: screen}
