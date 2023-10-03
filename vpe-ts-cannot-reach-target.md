---

copyright:
  years: 2018, 2023
lastupdated: "2023-01-27"

keywords: VPE, virtual private endpoint, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't my endpoint gateway reach the target {{site.data.keyword.cloud_notm}} service?
{: #troubleshoot-cannot-reach-target}
{: troubleshoot}
{: support}

Even though the endpoint gateway is healthy, it cannot reach the target {{site.data.keyword.cloud}} service.
{: shortdesc}

Verify that the cloud service instance is available to others by trying to access the public endpoint cloud service instance directly. If you cannot access the service’s public endpoint, open an IBM Support case.
{: tsResolve}

To check the `Health` and `Lifecycle` states of your VPE, you can use the UI, or enter the following command:

```sh
ibmcloud is endpoint-gateway <endpoint_gateway_id>
```
{: pre}

To list all endpoint gateways, run this command:
```sh
ibmcloud is egs
```
{: pre}
