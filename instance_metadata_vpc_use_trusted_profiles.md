---

copyright:
  years: 2021
lastupdated: "2021-08-23"

keywords: metadata, virtual private cloud, instance, virtual server

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:beta: .beta}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Using a trusted profile to call IAM-enabled services (Beta)
{: #imd-trusted-profile-metadata} 

You can create a trusted profile for compute resource identities in IAM and then assign a virtual server instance access rights for IAM-enabled services. These services can be called from an instance without having to manage and distribute IAM secrets to the instance. Use this option when you want to call IAM-enabled services as part of instance initialization.
{:shortdesc}

This service is available only to accounts with special approval to preview this beta feature.
{: beta}

## About trusted profiles for compute resource identities
{: #imd-compute-res-identity}

Trusted profiles for compute resource identities let you assign an IBM Cloud IAM identity to an individual compute resource. Instead of having to create a service ID, generate an API key, get the application to store and validate that key, you can assign IAM identity directly to the instance and acquire access tokens.

The compute resource identity service creates IAM object called a **trusted profile**, against which you assign access rights to enable the instance to call IAM-enabled services, such as Cloud Object Storage and Key Protect. You create a trusted profile within the virtual server instance. Trusted profiles afford the benefit of defining authorization for all applications that are running on an instance.

## Before you begin
{: #imd-byb-identities-procedure}

1. Install the IBM Cloud CLI and the VPC CLI plug-in (see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup)).

2. After you install the VPC CLI plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.
   
3. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli).

4. Configure a floating IP so that you can ping the virtual machines over the floating IP address and SSH into them.

5. Create a trusted profile. For information, see [Creating trusted profiles](/docs/account?topic=account-create-trusted-profile).

## Using trusted profiles for compute resource identities
{: #imd-procedure-trusted-profile}

The general procedure and context for using trusted profiles are described Table 1.

You can streamline the process by setting up dynamic rules in the first step to link the trusted profile automatically to new instances. 

The IAM token is obtained by exchanging the instance identity access token acquired through the metadata service with an IAM token. 

Table 1 describes the steps involved.

| Step | Context | Service Called | User Action |
|------|---------|----------------|-------------|
| 1    | IBM Cloud | IAM | Create an [IAM trusted profile for compute resources](/docs/account?topic=account-trustedprofile-compute-tutorial). |
| 2    | IBM Cloud | IAM | Assign access rights to the IAM trusted profile. |
| 3    | IBM Cloud | VPC API | Make a call to create a new instance, configured with the [metadata service](/docs/vpc?topic=vpc-imd-configure-service) enabled. Specify any user data. Alternately, [enable an existing instance to use the metadata service](/docs/vpc?topic=vpc-imd-configure-service#imd-metadata-service-enable). |
| 4    | IBM Cloud | IAM | Make a call to [link the instance to the trusted profile](/apidocs/iam-identity-token-api#create-link). |
| 5    | VPC instance | VPC | Make a call to the metadata token service and get an [access token](/docs/vpc?topic=vpc-imd-configure-service#imd-get-token). |
| 6    | VPC instance | IAM |  Make a call to [exchange the metadata service access token for an IAM token](/docs/vpc?topic=vpc-imd-configure-service#imd-token-exchange). The IAM token allows access to all IAM-enabled services. |
| 7    | VPC instance | IAM-enabled service | Pass the IAM token to call an IAM-enabled service API. The required access rights to the service exist in the trusted profile. |
{: caption="Table 1. Procedure for using a trusted profile for accessing instance metadata" caption-side="top"}

For more information, see the IAM documentation on [setting up trusted profiles](/docs/account?topic=account-create-trusted-profile).

## End-to-end procedure for using a trusted profile to access metadata
{: #imd-trusted-profile-md-ex}

1. Make sure you have created a [trusted profiles](/docs/account?topic=account-create-trusted-profile) for the instance.

2. Use the IAM API to [Link the instance to the trusted profile](/apidocs/iam-identity-token-api#create-link). To see a list of links, make a [`GET /profiles/{profile-id}/links` call](/apidocs/iam-identity-token-api#list-link).

3. Follow the steps 1 - 7 in the [End-to-end procedure for accessing metadata from an instance](/docs/vpc?topic=vpc-imd-access-instance-metadata#imd-access-md-ex). The procedure obtains an access token and creates an environment variable for it. When finished, continue with these steps:

4. Exchange the instance identity access token acquired using the metadata token service with an IAM token. 

5. Use the IAM token to make calls to IAM-enabled services.

## Next steps
{: #imd-trusted-profile-next-steps}

Make calls to the instance metadata service for instances, SSH keys, and placement group data. See [Using the instance metadata service](/docs/vpc?topic=vpc-imd-get-metadata).
