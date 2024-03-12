---

copyright:
  years: 2023
lastupdated: "2023-12-19"

keywords: private path

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About account policies
{: #pps-about-account-policies}

The beta release of IBM Cloud Private Path services is only available to allowlisted users. Contact your IBM Support representative if you are interested in getting early access to this beta offering.
{: beta}

As a service provider, you are responsible for managing your consumer account IDs. Currently, tracking or validating account IDs is not supported. For more information, see [Responsibilities for managing consumer account IDs](/docs/vpc?topic=vpc-pps-consumer-account-id-responsibilities&interface=ui).
{: attention}

You can create account policies for individual accounts as well as set a default policy that takes effect when a specific account policy isn't in place. For example, the default policy **Review all requests** sends all incoming connection requests to the service provider for review unless there is a specific account policy that takes precedence.
{: shortdesc}

## Default policy
{: #pps-default-policy}

When you create a Private Path service, the default policy is set to **Review all requests.**  This setting is recommended until the provider creates account-specific policies to help automate the connection request process for their service.

You can set the following account policies when creating a Private Path service:

- **Review all requests** - All incoming requests are sent to the service provider to be reviewed and triaged (permit or deny the connection request). The service provider is notified of all connection requests that do not have account policies associated with the account IDs.

- **Permit all requests** - This setting automatically approves connection requests for all account IDs that do not have account policies. This is the most expedient policy for both parties (the service provider and consumer). However, accepting "all" requests also presents a security risk, especially if the Private Path service's cloud resource name (CRN) is widely distributed.

- **Deny all requests** - All incoming requests to the service are rejected. If you want to allow specific IDs to connect to the service and reject all others, make sure to set up account policies set up for specific IDs before selecting this setting.

   Consumer requests that are denied cannot be reactivated. A new VPE gateway request must be created.
   {: note}

## Account-specific policies
{: #pps-account-policies}

You can create an account policy during the provisioning of a Private Path service, on a Private Path service's Details page, or when you review a connection request. Account-specific policies take precedence over the default policy. For example, if you permit a policy for account ID `lauren`, but the default policy is **Review all requests**, you will not get notified of requests from `lauren`.

Account policies differ from default policies in the following ways:

- **Review** - All requests from the specific account ID are automatically flagged for review.
- **Permit** - All requests from the specific account ID are automatically accepted.
- **Deny** - All requests from the specific account ID are automatically rejected.

   Consumer requests that are denied cannot be reactivated. A new VPE gateway request must be created.
   {: note}

You can edit account policies at any time. Editing an account policy does not change the status of connection requests that were previously permitted or denied by the account policy associated with the account ID.

## Related links
{: #pps-account-policies-related-links}

- [Creating an account policy](/docs/vpc?topic=vpc-pps-create-account-policy&interface=ui){: external}
- [Updating and deleting an account policy](/docs/vpc?topic=vpc-pps-update-account&interface=ui){: external}
