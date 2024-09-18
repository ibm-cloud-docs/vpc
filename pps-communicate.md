---

copyright:
  years: 2023, 2024
lastupdated: "2024-09-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Communicating connection information to consumers
{: #pps-ui-communicate}

The beta release of IBM Cloud Private Path services is only available to allowlisted users. Contact your IBM Support representative if you are interested in getting early access to this beta offering.
{: beta}

To complete the connection to your service, you must communicate the following information to your consumers:

* Your Private Path service’s cloud resource name (CRN)

   Open the Private Path details page of your [Private Path service](/vpc-ext/network/privatePathServices){: external} to locate its CRN. Click the ![Copy to clipboard icon](../../icons/copy.svg) to copy and paste the CRN in your communication to consumers (for example, `crn:v1:public:is:us-south:a/efe5afc483594adaa8325e2b4d1290df::private-path-service-gateway:r006-5fc6b76d-0ff3-46d1-8e39-c0ac0a38a27b`).

* Instructions on creating a Virtual Private Endpoint (VPE) gateway

   Point consumers to instructions on [Creating a VPE gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway){: external} to create a non-IBM Cloud service connection request.

* The approximate timeframe your consumer must wait to get connected to your service

   The time it takes for a consumer to get connected depends on you. For example, if you set the default policy to `permit`, all requests are automatically connected. Or, if you set an account policy to `permit` a certain account, again, the connection is approved and the consumer is connected. If, however, you set the default or an account-specific policy to `review`, then you must review the connection request and either `permit` or `deny` it. If you don't review an incoming request within the expiration period set when you created the Private Path service, their request expires and the consumer must create a new VPE gateway request. To learn more, see [Reviewing connection requests](/docs/vpc?topic=vpc-pps-ui-reviewing&interface=ui){: external}.

* Your contact information
* A URL to access the service, if necessary
* Any necessary port information that you might need to communicate
