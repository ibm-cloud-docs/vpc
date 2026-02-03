---

copyright:
  years: 2022, 2026
lastupdated: "2026-02-03"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Integrating an application load balancer with IBM Cloud DNS Services
{: #lb-dns}

With {{site.data.keyword.cloud}} Application Load Balancer for VPC, you can bind a DNS zone from [IBM Cloud DNS Services](/docs/dns-svcs?topic=dns-svcs-getting-started) to move all DNS resolutions into private networks.
{: shortdesc}

{{site.data.keyword.cloud_notm}} DNS Services provides private DNS to VPC users. Private DNS zones are resolvable only on {{site.data.keyword.cloud_notm}}, and only from networks that are explicitly permitted in an account or networks with cross-account access.

Do not remove or modify the DNS records created by the load balancer. Doing so can cause your private DNS and load balancer configurations to be out of sync, resulting in data path issues.
{: attention}

For more information about linking a private DNS record name to a load balancer, see [Team-based privacy using IAM, VPC, Transit Gateway, and DNS](/docs/vpc?topic=vpc-vpc-tg-dns-iam&interface=ui).
{: tip}

## Before you begin
{: #prerequisite-integration-with-dns-service}

Before binding DNS zones to load balancers, you must first create DNS zones and grant load balancer access.

1. Create DNS zones to bind to a load balancer. For more information, see [Managing DNS zones](/docs/dns-svcs?topic=dns-svcs-managing-dns-zones).

1. To give a load balancer access to your DNS zone, enable service-to-service authorization, which grants your load balancer access to the DNS zone. For more information, see [Granting access between services](/docs/account?topic=account-serviceauth&interface=ui#create-auth). Make sure to choose **VPC Infrastructure Services** as the source service, **Load Balancer for VPC** as the resource type, **DNS Services** as the target service, and assign the **Manager** service access role.


You can use DNS zones in the same account or across different accounts. When binding DNS zones across different accounts, enable service-to-service authorization in the target account where the DNS zone is hosted, mentioning the source account where the load balancer exists. 
{: note}

For example, if you have a load balancer in Account A and a DNS zone in Account B, you must create service-to-service authorization in account B mentioning the source as Account A. Then, as indicated in step 2, choose **VPC Infrastructure Services** as the source service. Select **Load Balancer for VPC** as the resource type, **DNS Services** as the target service, and assign the **Manager** service access role.

## Working with DNS zones in the console
{: #dns-zones-ui}
{: ui}

You can bind and unbind DNS zones to a new application load balancer during provisioning, or to an existing load balancer.

### Binding DNS zones when creating a load balancer in the console
{: #create-lb-with-dns-zone-ui}

You can bind a DNS zone to a load balancer when you create a load balancer. If you do not specify a DNS zone during load balancer creation, the default zone is used. By default, the load balancer hostname is a subdomain of `lb.appdomain.cloud` and is publicly visible.

To bind a DNS zone when you [create a load balancer](/docs/vpc?topic=vpc-load-balancers), follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. Click **Create**.
1. Configure the VPC, type, subnet, listeners, and pools as needed.
1. Under the DNS type section, choose **Private**.
1. Click **Bind** to enter DNS instance, and DNS zone information.
1. Click **Create** to provision the load balancer.

### Binding a DNS zone when creating a load balancer in the console (Cross-Account)
{: #create-lb-with-dns-zone-ui-cross-account}

To bind a DNS zone when you [create a load balancer](/docs/vpc?topic=vpc-load-balancers) cross-account, follow these steps:

1. Make sure that service-to-service authorization is provided.
2. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. Click **Create**.
1. Configure the VPC, type, subnet, listeners, and pools as needed.
1. Under the DNS type section, choose **Private**.
1. Select **Specify instance option** and provide the DNS CRN and Zone ID.
1. Click **Bind** to bind the DNS zone to your load balancer.
1. Click **Create** to provision the load balancer.
   
### Binding a DNS zone to an existing load balancer in the console
{: #binding-dns-zone-to-lb-ui}

To bind a DNS zone to an existing load balancer, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. From the list of load balancers, select the load balancer to view its details page. Navigate to the Attached resources section and scroll to the end of the page.
1. Click **Bind** in the Private DNS section.
1. Select the DNS instance, and DNS zone information. 
1. Click **Bind** to bind the DNS zone to your load balancer.

When you migrate to a private DNS zone for an existing load balancer, the default A records in `lb.appdomain.cloud` are removed. To prepare your client devices, create a CNAME record with the default hostname under the wanted private DNS zone before the migration. After you configure all client devices to use the new private DNS zone, you can then delete the CNAME record and begin the migration. This CNAME record, created before the migration, allows the client devices to cache the private DNS hostname until the A records are created.
{: tip}

### Binding a DNS zone to an existing load balancer in the console (cross-account)
{: #binding-dns-zone-to-lb-ui-cross-account}

To bind a DNS zone to an existing load balancer cross-account, follow these steps:

1. After ensuring that service-to-service authorization is provided, from your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. From the list of load balancers, select the load balancer to view its details page. Navigate to the "Attached resources" section and scroll to the end of the page.
1. Select "Specify instance" option and provide DNS CRN and Zone ID. 
1. Click **Bind** to bind the DNS zone to your load balancer.

### Unbinding a DNS zone from an existing load balancer in the console
{: #unbinding-dns-zone-to-lb-ui}

To unbind a DNS zone from a load balancer, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. From the list of load balancers, select the load balancer to view its details page.
1. Click **Unbind** in the Private DNS section
1. Verify the action by clicking **Unbind** again.

## Working with DNS zones from the CLI
{: #dns-zones-cli}
{: cli}

You can bind and unbind DNS zones to an application load balancer during provisioning, or to an existing load balancer.

You can use DNS zones in the same account or across different accounts. When binding DNS zones across different accounts, enable service-to-service authorization in the target account where the DNS zone is hosted, mentioning the source account where the load balancer exists. 
{: note}

### Creating a load balancer that is bound to a private DNS zone from the CLI
{: #create-lb-with-dns-zone-cli}

You need the CRN of the DNS Services instance that you want to bind to your load balancer. To find it, click **Navigation menu** ![Menu icon](../../icons/icon_hamburger.svg) > **Resource List** from the {{site.data.keyword.cloud_notm}} console. Click the table row of the DNS whose CRN you want to find. The CRN is shown in the side window that appears.
{: attention}

To create an application load balancer with a private DNS zone, follow these steps:

1. Set up your [CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Use the terminal to log in to your account. After you enter the password, the system prompts which account and region that you want to use:

   ```sh
   ibmcloud login --sso
   ```
   {: pre}

1. Create your load balancer:

```sh
ibmcloud is lbc pdns-alb-example private --subnet 0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024 --family application --dns-instance-crn crn:v1:bluemix:public:dns-svcs:global:a/be636a7a6e4d4b6296bedf669ce8f757:c7295f57-4d25-4832-9d7e-a032de0c278c:: --dns-zone-id 0671b738-57f0-468e-8086-10bc480ac00e
```
{: pre}

Sample output:

```sh
ID                           r014-e424acb0-24af-49a0-ab50-c5969304eb94
Name                         pdns-alb-example
CRN                          crn:v1:bluemix:public:is:us-east:a/be636a7a6e4d4b6296bedf669ce8f757::load-balancer:r014-e424acb0-24af-49a0-ab50-c5969304eb94
Family                       Application
Datapath logging active      false
Host name                    e424acb0-us-east.example.com
Subnets                      ID                                          Name
                             0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024   lb-subnet

Public IPs
Private IPs
Provision status             create_pending
Operating status             offline
Is public                    false
Listeners
Pools                        ID   Name

Resource group               ID                                 Name
                             42c4f51adc3147b4b4049ad9826c30a1   Default

Created                      2023-02-10T13:18:11-06:00
DNS                          Instance CRN                                                                                                      Zone
                             crn:v1:bluemix:public:dns-svcs:global:a/be636a7a6e4d4b6296bedf669ce8f757:c7295f57-4d25-4832-9d7e-a032de0c278c::   0671b738-57f0-468e-8086-10bc480ac00e

Instance Group Supported     -
SourceIP Session Supported   -
Security groups              ID                                          Name
                             r014-b739c959-fb3c-44cc-9ec5-41c45b4d637e   astonish-backing-shadiness-frostlike-cosmos-hunk

UDP Supported                false
```
{: screen}

### Binding an existing load balancer to a private DNS zone from the CLI
{: #binding-dns-zone-to-lb-cli}

You need the CRN of the DNS Services instance that you want to bind to your load balancer. To find it, click **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg) > **Resource List** from the {{site.data.keyword.cloud_notm}} console. Click the table row of the DNS whose CRN you want to find. The CRN is shown in the side window that appears.
{: attention}

To use the CLI to update an application load balancer with a private DNS zone, follow these steps:

1. Set up your CLI environment.
1. Log in to your account by using the CLI. After you enter the password, the system prompts which account and region that you want to use:

   ```sh
   ibmcloud login --sso
   ```

1. Update your load balancer with the private DNS zone information:

   ```sh
   ibmcloud is load-balancer-update pdns-alb-update-example --dns-instance-crn crn:v1:bluemix:public:dns-svcs:global:a/be636a7a6e4d4b6296bedf669ce8f757:c7295f57-4d25-4832-9d7e-a032de0c278c:: --dns-zone-id 0671b738-57f0-468e-8086-10bc480ac00e
   ```
   {: pre}

Sample output:

```sh
ID                           r014-e68a40fb-2b94-4a92-b8bd-dc9bb92c3ef3
Name                         pdns-alb-update-example
CRN                          crn:v1:bluemix:public:is:us-east:a/be636a7a6e4d4b6296bedf669ce8f757::load-balancer:r014-e68a40fb-2b94-4a92-b8bd-dc9bb92c3ef3
Family                       Application
Datapath logging active      false
Host name                    e68a40fb-us-east.example.com
Subnets                      ID                                          Name
                             0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024   lb-subnet

Public IPs
Reserved IPs                 ID                                          Address        Subnet
                             0767-16871e0f-c43a-4b6e-8491-a3e075362046   10.241.64.11   0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024
                             0767-a7db5d37-42cf-473e-ab27-07b679d64ceb   10.241.64.12   0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024

Provision status             update_pending
Operating status             online
Is public                    false
Listeners
Pools                        ID   Name

Resource group               ID                                 Name
                             42c4f51adc3147b4b4049ad9826c30a1   Default

Created                      2023-02-10T13:36:44-06:00
DNS                          Instance CRN                                                                                                      Zone
                             crn:v1:bluemix:public:dns-svcs:global:a/be636a7a6e4d4b6296bedf669ce8f757:c7295f57-4d25-4832-9d7e-a032de0c278c::   0671b738-57f0-468e-8086-10bc480ac00e

Instance Group Supported     -
SourceIP Session Supported   -
Security groups              ID                                          Name
                             r014-b739c959-fb3c-44cc-9ec5-41c45b4d637e   astonish-backing-shadiness-frostlike-cosmos-hunk

UDP Supported                false
```
{: screen}

### Unbinding an existing load balancer from a private DNS zone from the CLI
{: #unbinding-dns-zone-to-lb-cli}

To update an application load balancer with the default public DNS zone, follow these steps:

1. Set up your CLI environment.
1. Log in to your account. After you enter the password, the system prompts which account and region that you want to use:

   ```sh
   ibmcloud login --sso
   ```
   {: pre}

1. Update your load balancer with `reset-dns` flag.

   ```sh
   ibmcloud is load-balancer-update pdns-alb-update-example --reset-dns
   ```
   {: pre}

Sample output:

```sh
ID                           r014-e68a40fb-2b94-4a92-b8bd-dc9bb92c3ef3
Name                         pdns-alb-update-example
CRN                          crn:v1:bluemix:public:is:us-east:a/be636a7a6e4d4b6296bedf669ce8f757::load-balancer:r014-e68a40fb-2b94-4a92-b8bd-dc9bb92c3ef3
Family                       Application
Datapath logging active      false
Host name                    e68a40fb-us-east.lb.appdomain.cloud
Subnets                      ID                                          Name
                             0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024   lb-subnet

Public IPs
Reserved IPs                 ID                                          Address        Subnet
                             0767-16871e0f-c43a-4b6e-8491-a3e075362046   10.241.64.11   0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024
                             0767-a7db5d37-42cf-473e-ab27-07b679d64ceb   10.241.64.12   0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024

Provision status             update_pending
Operating status             online
Is public                    false
Listeners
Pools                        ID   Name

Resource group               ID                                 Name
                             42c4f51adc3147b4b4049ad9826c30a1   Default

Created                      2023-02-10T13:36:44-06:00
Instance Group Supported     -
SourceIP Session Supported   -
Security groups              ID                                          Name
                             r014-b739c959-fb3c-44cc-9ec5-41c45b4d637e   astonish-backing-shadiness-frostlike-cosmos-hunk

UDP Supported                false
```
{: screen}

## Working with DNS zones with the API
{: #dns-zones-api}
{: api}

You can bind and unbind DNS zones to an application load balancer during provisioning, or to an existing load balancer.

You can use DNS zones in the same account or across different accounts. When binding DNS zones across different accounts, enable service-to-service authorization in the target account where the DNS zone is hosted, mentioning the source account where the load balancer exists. 
{: note}

### Creating a load balancer that is bound to a private DNS zone with the API
{: #create-lb-with-dns-zone-api}

You need the CRN of the DNS Services instance that you want to bind to your load balancer. To find it, click **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg) > **Resource List** from the {{site.data.keyword.cloud_notm}} console. Click the table row of the DNS whose CRN you want to find. The CRN is shown in the side window that appears.
{: attention}

To specify a private DNS zone during creation:

Specify the `dns` information in the `POST /load_balancers` call. For more information about this creation payload, see [Creating an application load balancer with the API](/docs/vpc?topic=vpc-load-balancers&interface=api#lb-api-creating-application-load-balancer).
{: note}

```sh
{
  "name": "pdns-alb-example",
  "is_public": false,
  "dns": {
    "instance": {
       "crn": "crn:v1:bluemix:public:dns-svcs:global:a/be636a7a6e4d4b6296bedf669ce8f757:c7295f57-4d25-4832-9d7e-a032de0c278c::"
    },
    "zone": {
        "id": "0671b738-57f0-468e-8086-10bc480ac00e"
    }
  },
  "subnets": [
    {
      "id": "0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024"
    }
  ]
}
```
{: pre}

Sample output:

```sh
{
  "id": "r014-eac21cde-a4ce-4980-b037-fc3507112955",
  "name": "pdns-alb-example",
  "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-eac21cde-a4ce-4980-b037-fc3507112955",
  "crn": "crn:v1:bluemix:public:is:us-east:a/be636a7a6e4d4b6296bedf669ce8f757::load-balancer:r014-eac21cde-a4ce-4980-b037-fc3507112955",
  "is_public": false,
  "created_at": "2023-02-13T18:58:12Z",
  "hostname": "eac21cde-us-east.example.com",
  "listeners": [],
  "operating_status": "offline",
  "pools": [],
  "private_ips": [],
  "provisioning_status": "create_pending",
  "public_ips": [],
  "subnets": [
    {
      "id": "0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
      "href": "https://us-east.iaas.cloud.ibm.com/v1/subnets/0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
      "crn": "crn:v1:bluemix:public:is:us-east-2:a/be636a7a6e4d4b6296bedf669ce8f757::subnet:0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
      "name": "lb-subnet"
    }
  ],
  "resource_group": {
    "id": "42c4f51adc3147b4b4049ad9826c30a1",
    "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/42c4f51adc3147b4b4049ad9826c30a1",
    "name": "Default"
  },
  "resource_type": "load_balancer",
  "logging": {
    "datapath": {
      "active": false
    }
  },
  "profile": {
    "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancer/profiles/dynamic",
    "name": "dynamic",
    "family": "Application"
  },
  "security_groups": [
    {
      "id": "r014-b739c959-fb3c-44cc-9ec5-41c45b4d637e",
      "href": "https://us-east.iaas.cloud.ibm.com/v1/security_groups/r014-b739c959-fb3c-44cc-9ec5-41c45b4d637e",
      "crn": "crn:v1:bluemix:public:is:us-east:a/be636a7a6e4d4b6296bedf669ce8f757::security-group:r014-b739c959-fb3c-44cc-9ec5-41c45b4d637e",
      "name": "astonish-backing-shadiness-frostlike-cosmos-hunk"
    }
  ],
  "security_group_supported": true,
  "route_mode": false,
  "udp_supported": false,
  "dns": {
    "instance": {
      "crn": "crn:v1:bluemix:public:dns-svcs:global:a/be636a7a6e4d4b6296bedf669ce8f757:c7295f57-4d25-4832-9d7e-a032de0c278c::"
    },
    "zone": {
      "id": "0671b738-57f0-468e-8086-10bc480ac00e"
    }
  }
}
```
{: screen}

### Binding an existing load balancer to a private DNS zone with the API
{: #binding-dns-zone-to-lb-api}

You need the CRN of the DNS Services instance that you want to bind to your load balancer. To find it, click **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg) > **Resource List** from the {{site.data.keyword.cloud_notm}} console. Click the table row of the DNS whose CRN you want to find. Locate the CRN in the side window that appears.
{: attention}

To use the API to bind an existing load balancer to a private DNS zone:

Specify the `dns` information in the `PATCH /load_balancers` call. For more information about this `PATCH` payload, see [Updating an application load balancer with the API](/docs/vpc?topic=vpc-alb-updating&interface=api#alb-updating-frontend-listener-port-api).
{: note}

```sh
{
  "dns": {
    "instance": {
      "crn": "crn:v1:bluemix:public:dns-svcs:global:a/be636a7a6e4d4b6296bedf669ce8f757:c7295f57-4d25-4832-9d7e-a032de0c278c::"
    },
    "zone": {
      "id": "0671b738-57f0-468e-8086-10bc480ac00e"
    }
  }
}
```
{: pre}

Sample output:

```sh
{
  "id": "r014-e68a40fb-2b94-4a92-b8bd-dc9bb92c3ef3",
  "name": "pdns-alb-update-example",
  "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-e68a40fb-2b94-4a92-b8bd-dc9bb92c3ef3",
  "crn": "crn:v1:bluemix:public:is:us-east:a/be636a7a6e4d4b6296bedf669ce8f757::load-balancer:r014-e68a40fb-2b94-4a92-b8bd-dc9bb92c3ef3",
  "is_public": false,
  "created_at": "2023-02-10T19:36:44Z",
  "hostname": "e68a40fb-us-east.example.com",
  "listeners": [],
  "operating_status": "online",
  "pools": [],
  "private_ips": [
    {
      "address": "10.241.64.11",
      "href": "https://us-east.iaas.cloud.ibm.com/v1/subnets/0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024/reserved_ips/0767-16871e0f-c43a-4b6e-8491-a3e075362046",
      "id": "0767-16871e0f-c43a-4b6e-8491-a3e075362046",
      "name": "yapping-extract-reawake-bullhorn",
      "resource_type": "subnet_reserved_ip"
    },
    {
      "address": "10.241.64.12",
      "href": "https://us-east.iaas.cloud.ibm.com/v1/subnets/0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024/reserved_ips/0767-a7db5d37-42cf-473e-ab27-07b679d64ceb",
      "id": "0767-a7db5d37-42cf-473e-ab27-07b679d64ceb",
      "name": "dioxide-stylized-headway-despise",
      "resource_type": "subnet_reserved_ip"
    }
  ],
  "provisioning_status": "update_pending",
  "public_ips": [],
  "subnets": [
    {
      "id": "0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
      "href": "https://us-east.iaas.cloud.ibm.com/v1/subnets/0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
      "crn": "crn:v1:bluemix:public:is:us-east-2:a/be636a7a6e4d4b6296bedf669ce8f757::subnet:0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
      "name": "lb-subnet"
    }
  ],
  "resource_group": {
    "id": "42c4f51adc3147b4b4049ad9826c30a1",
    "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/42c4f51adc3147b4b4049ad9826c30a1",
    "name": "Default"
  },
  "resource_type": "load_balancer",
  "logging": {
    "datapath": {
      "active": false
    }
  },
  "profile": {
    "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancer/profiles/dynamic",
    "name": "dynamic",
    "family": "Application"
  },
  "security_groups": [
    {
      "id": "r014-b739c959-fb3c-44cc-9ec5-41c45b4d637e",
      "href": "https://us-east.iaas.cloud.ibm.com/v1/security_groups/r014-b739c959-fb3c-44cc-9ec5-41c45b4d637e",
      "crn": "crn:v1:bluemix:public:is:us-east:a/be636a7a6e4d4b6296bedf669ce8f757::security-group:r014-b739c959-fb3c-44cc-9ec5-41c45b4d637e",
      "name": "astonish-backing-shadiness-frostlike-cosmos-hunk"
    }
  ],
  "security_group_supported": true,
  "route_mode": false,
  "udp_supported": false,
  "dns": {
    "instance": {
      "crn": "crn:v1:bluemix:public:dns-svcs:global:a/be636a7a6e4d4b6296bedf669ce8f757:c7295f57-4d25-4832-9d7e-a032de0c278c::"
    },
    "zone": {
      "id": "0671b738-57f0-468e-8086-10bc480ac00e"
    }
  }
}
```
{: screen}

### Unbinding an existing load balancer from a private DNS zone with the API
{: #unbinding-dns-zone-to-lb-api}

To use the API to unbind an existing load balancer to a private DNS zone:

Specify null `dns` information in the `PATCH /load_balancers call`. For more information about this `PATCH` payload, see [Updating an application load balancer with the API](/docs/vpc?topic=vpc-alb-updating&interface=api#alb-updating-frontend-listener-port-api).
{: note}

```sh
{
  "dns": null
}
```
{: pre}

Sample output:

```sh
{
  "id": "r014-e68a40fb-2b94-4a92-b8bd-dc9bb92c3ef3",
  "name": "pdns-alb-update-example",
  "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-e68a40fb-2b94-4a92-b8bd-dc9bb92c3ef3",
  "crn": "crn:v1:bluemix:public:is:us-east:a/be636a7a6e4d4b6296bedf669ce8f757::load-balancer:r014-e68a40fb-2b94-4a92-b8bd-dc9bb92c3ef3",
  "is_public": false,
  "created_at": "2023-02-10T19:36:44Z",
  "hostname": "e68a40fb-us-east.lb.appdomain.cloud",
  "listeners": [],
  "operating_status": "online",
  "pools": [],
  "private_ips": [
    {
      "address": "10.241.64.11",
      "href": "https://us-east.iaas.cloud.ibm.com/v1/subnets/0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024/reserved_ips/0767-16871e0f-c43a-4b6e-8491-a3e075362046",
      "id": "0767-16871e0f-c43a-4b6e-8491-a3e075362046",
      "name": "yapping-extract-reawake-bullhorn",
      "resource_type": "subnet_reserved_ip"
    },
    {
      "address": "10.241.64.12",
      "href": "https://us-east.iaas.cloud.ibm.com/v1/subnets/0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024/reserved_ips/0767-a7db5d37-42cf-473e-ab27-07b679d64ceb",
      "id": "0767-a7db5d37-42cf-473e-ab27-07b679d64ceb",
      "name": "dioxide-stylized-headway-despise",
      "resource_type": "subnet_reserved_ip"
    }
  ],
  "provisioning_status": "update_pending",
  "public_ips": [],
  "subnets": [
    {
      "id": "0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
      "href": "https://us-east.iaas.cloud.ibm.com/v1/subnets/0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
      "crn": "crn:v1:bluemix:public:is:us-east-2:a/be636a7a6e4d4b6296bedf669ce8f757::subnet:0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
      "name": "lb-subnet"
    }
  ],
  "resource_group": {
    "id": "42c4f51adc3147b4b4049ad9826c30a1",
    "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/42c4f51adc3147b4b4049ad9826c30a1",
    "name": "Default"
  },
  "resource_type": "load_balancer",
  "logging": {
    "datapath": {
      "active": false
    }
  },
  "profile": {
    "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancer/profiles/dynamic",
    "name": "dynamic",
    "family": "Application"
  },
  "security_groups": [
    {
      "id": "r014-b739c959-fb3c-44cc-9ec5-41c45b4d637e",
      "href": "https://us-east.iaas.cloud.ibm.com/v1/security_groups/r014-b739c959-fb3c-44cc-9ec5-41c45b4d637e",
      "crn": "crn:v1:bluemix:public:is:us-east:a/be636a7a6e4d4b6296bedf669ce8f757::security-group:r014-b739c959-fb3c-44cc-9ec5-41c45b4d637e",
      "name": "astonish-backing-shadiness-frostlike-cosmos-hunk"
    }
  ],
  "security_group_supported": true,
  "route_mode": false,
  "udp_supported": false,
  "dns": null
}
```
{: screen}
