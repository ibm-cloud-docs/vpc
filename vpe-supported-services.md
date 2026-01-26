---

copyright:
  years: 2020, 2026
lastupdated: "2026-01-26"

keywords: virtual private endpoints, endpoint gateway, VPE services

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to a service
{: #vpe-supported-services}

You can create a Virtual Private Endpoint (VPE) gateway to connect to an IBM Cloud service, or an IBM service or third-party application that is hosted outside of IBM Cloud.

Here are your two choices:

* **IBM Cloud service**

   Select this option to connect to an IBM Cloud service. When configuring a VPE gateway, you are shown a customized list of common services and any services that were provisioned
with your account. See [VPE-enabled services](/docs/vpc?topic=vpc-vpe-supported-services#vpe-enabled-supported-services) for a complete list of IBM Cloud services that are enabled for VPE gateway connectivity.

* **Non-IBM Cloud service**

   Select this option if you received connection information from a provider of a service or application that includes a cloud resource name (CRN) to a Private Path service. A Private Path service is what your provider sets up to create a connection to their service or application over the IBM Cloud private network.

   After you create a VPE gateway, a connection request is sent for review to the provider of the service that you are trying to connect to. Keep in mind that your account ID is shared in this request. To track the progress of your request, go to the [Virtual private endpoint gateways for VPC](/infrastructure/network/endpointGateways){: external} dashboard to view the status of your request. The status of your VPE gateway remains `Pending` until your connection request is either accepted (`Stable`) or rejected (`Failed`).

   If you have any questions, contact your service provider.

## VPE-enabled services
{: #vpe-enabled-supported-services}

The following IBM Cloud services are enabled for Virtual Private Endpoints (VPE) gateway connectivity.
{: shortdesc}

* {{site.data.keyword.appconfig_short}} ([Endpoint URLs](/apidocs/app-configuration#endpoint-url))
* Catalog Management ([Endpoint URLs](/apidocs/resource-catalog/private-catalog#endpoint-url))
* Cloud Object Storage ([Instructions](/docs/cloud-object-storage?topic=cloud-object-storage-vpes))
* Code Engine ([Instructions](/docs/codeengine?topic=codeengine-vpe))
* {{site.data.keyword.registryshort_notm}} ([Instructions](/docs/Registry?topic=Registry-registry_vpe))
* Context-based restrictions ([Endpoint URLs](/apidocs/context-based-restrictions#endpoint-urls))
* Databases ([Instructions](/docs/cloud-databases?topic=cloud-databases-vpes))
* Direct Link ([Instructions](/docs/dl?topic=dl-vpe-connection))
* DNS Services ([Instructions](/docs/dns-svcs?topic=dns-svcs-vpe-for-dns-svcs#vpe-for-dns-svcs))
* Enterprise Application Service for Java ([Instructions](https://www.ibm.com/docs/en/ease?topic=security-connecting-applications-using-virtual-private-endpoints)){: external}
* Enterprise Billing Units ([Endpoint URLs](/apidocs/enterprise-apis/billing-unit#endpoint-url))
* Enterprise Management ([Endpoint URLs](/apidocs/enterprise-apis/enterprise#endpoint-url))
* Enterprise Usage Reports ([Endpoint URLs](/apidocs/enterprise-apis/resource-usage-reports#endpoint-url))
* {{site.data.keyword.en_short}} ([Endpoint URLs](/apidocs/event-notifications#event-notifications-endpoint-url))
* Event Streams for IBM Cloud ([Endpoint URLs](/apidocs/event-streams/adminrest))
* Global Catalog ([Endpoint URLs](/apidocs/resource-catalog/global-catalog#endpoint-url))
* Global Search ([Endpoint URLs](/apidocs/search#endpoint-url))
* Global Tagging ([Endpoint URLs](/apidocs/tagging#endpoint-url))
* {{site.data.keyword.hscrypto}} ([Instructions](/docs/hs-crypto?topic=hs-crypto-virtual-private-endpoints-for-vpc))
* IAM Access Groups ([Endpoint URLs](/apidocs/iam-access-groups#endpoint-urls))
* IAM Identity Services ([Endpoint URLs](/apidocs/iam-identity-token-api#endpoints))
* IAM Policy Management ([Endpoint URLs](/apidocs/iam-policy-management#endpoint-urls))
* {{site.data.keyword.logs_full_notm}} ([Instructions](/docs/cloud-logs?topic=cloud-logs-vpe-connection&interface=cli))
* Key Protect ([Instructions](/docs/key-protect?topic=key-protect-virtual-private-endpoints))
* MQ ([Instructions](https://www.ibm.com/docs/en/mq-as-a-service?topic=api-virtual-private-endpoints){: external})
* Network Time Protocol
* Resource Controller ([Endpoint URLs](/apidocs/resource-controller/resource-controller#endpoint-url))
* Resource Manager ([Endpoint URLs](/apidocs/resource-controller/resource-manager#endpoint-urls))
* Schematics ([Instructions](/docs/schematics?topic=schematics-private-endpoints#endpoint-setup))
* {{site.data.keyword.secrets-manager_short}} ([Instructions](/docs/secrets-manager?topic=secrets-manager-endpoints))
* Transit Gateway ([Instructions](/docs/transit-gateway?topic=transit-gateway-vpe-connection)) 
* Usage Metering ([Endpoint URLs](/apidocs/usage-metering#endpoint))
* Usage Reports ([Endpoint URLs](/apidocs/metering-reporting#endpoint))
* User Management ([Endpoint URLs](/apidocs/user-management#endpoint-url))
* VPC API ([Endpoint URLs](/apidocs/vpc#endpoint-url)) ([About VPE for VPC](/docs/vpc?topic=vpc-about-vpe))
* Watson Assistant ([Instructions](/docs/watson?topic=watson-virtual-private-endpoints))
* Watson Discovery ([Instructions](/docs/watson?topic=watson-virtual-private-endpoints))
* Watson Speech to Text ([Instructions](/docs/watson?topic=watson-virtual-private-endpoints))
* Watson Text to Speech ([Instructions](/docs/watson?topic=watson-virtual-private-endpoints))

This topic updates as additional services are supported.
{: note}
