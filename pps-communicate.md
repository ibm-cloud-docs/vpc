---

copyright:
  years: 2023, 2025
lastupdated: "2025-08-06"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Communicating connection information to consumers
{: #pps-ui-communicate}

To complete the connection to your service, you must communicate the following information to your consumers:

* Your Private Path service’s cloud resource name (CRN)

   Open the Private Path details page of your [Private Path service](/infrastructure/network/privatePathServices) to locate its CRN. Click the Copy to clipboard icon to copy and paste the CRN in your communication to consumers.

* Instructions on creating a Virtual Private Endpoint (VPE) gateway

   Point consumers to instructions on [Creating a VPE gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway) to create a non-IBM Cloud service connection request.

* The approximate time frame that your consumer must wait to get connected to your service

   The time that it takes for a consumer to get connected depends on you. For example, if you set the default policy to `permit`, all requests are automatically connected. Or, if you set an account policy to `permit` a certain account, again, the connection is approved and the consumer is connected. However, if you set the default or an account-specific policy to `review`, then you must review the connection request and either `permit` or `deny` it. If you don't review an incoming request within the expiration period when you created the Private Path service, the request expires and the consumer must create a new VPE gateway request. For more information, see [Reviewing connection requests](/docs/vpc?topic=vpc-pps-ui-reviewing&interface=ui).

* Your contact information
* A URL to access the service, if necessary
* Any necessary port information that you might need to communicate
* Helpful videos, like Creating a Virtual Private Endpoint gateway to access a Private Path service:

![Creating a Virtual Private Endpoint Gateway to acces a Private Path service](https://www.kaltura.com/p/1773841/sp/177384100/embedIframeJs/uiconf_id/27941801/partner_id/1773841?iframeembed=true&entry_id=1_65e0wq3e){: video output="iframe" data-script="none" id="mediacenterplayer" frameborder="0" width="560" height="315" allowfullscreen webkitallowfullscreen mozAllowFullScreen}
