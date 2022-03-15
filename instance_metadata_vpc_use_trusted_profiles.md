---

copyright:
  years: 2022
lastupdated: "2022-03-15"

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
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Using a trusted profile to call IAM-enabled services
{: #imd-trusted-profile-metadata} 

You can create a trusted profile for compute resource identities in IAM and then assign a virtual server instance access rights for IAM-enabled services. These services can be called from an instance without having to manage and distribute IAM secrets to the instance. Use this option when you want to call IAM-enabled services as part of instance initialization.
{: shortdesc}

## About trusted profiles for compute resource identities
{: #imd-compute-res-identity}

Trusted profiles for compute resource identities let you assign an IBM Cloud IAM identity to an individual compute resource, such as a virtual server instance. Instead of having to create a service ID, generate an API key, get the application to store and validate that key, you can assign IAM identity directly to the instance and acquire an instance identity access token.

The compute resource identity service creates the trusted profile, against which you assign access rights to enable the instance to call IAM-enabled services, such as Cloud Object Storage and Key Protect. You link a trusted profile when you provision an instance. Trusted profiles define authorization for all applications running on the instance.

The trusted profile you specify when you provision the instance becomes the default trusted profile. It's automatically linked to the instance when you use the UI. From the API, you can link it in the same call to create the instance. 

The instance inherits the access rights defined in the default trusted profile. VPC uses that trusted profile for any instance identity request to generate an IAM token from an instance identity access token, where the token generation request does not specify a trusted profile.

## Before you begin
{: #imd-byb-identities-procedure}

1. Install the IBM Cloud CLI and the VPC CLI plug-in (see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup)).

2. After you install the VPC CLI plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.
   
3. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli).

4. Configure a floating IP so that you can ping the virtual servers over the floating IP address and SSH into them.

5. Create a trusted profile. For information, see [Establishing trust with compute resources](/docs/account?topic=account-create-trusted-profile&interface=ui#create-profile-compute-ui).

## IAM authorizations for linking trusted profiles
{: #imd-iam-auth}

To link a trusted profile to an instance, you must have you must have sufficient authorization. Having correct IAM permissions also lets you to see all trusted profiles linked to an instance, for example, when using the dropdown menu in the UI.

Verify that your access permissions are assigned as Administrator or Editor:

1. In the UI, go to [Manage access and users](https://{DomainName}/iam/users){: external}.

2. Click **Users**, select your name, and click **Access policies**. 

From  the API, make a `GET /iam-identity.profile.get`call.

If you need to update your profile, make these calls:

* `POST /iam-identity.profile.update` 
* `POST /iam-identity.profile.linkToResource`

For more information see this information in the [IAM API reference](https://cloud.ibm.com/apidocs/iam-identity-token-api#create-link-permissions).

## Using trusted profiles for compute resource identities
{: #imd-procedure-trusted-profile}

The general procedure and context for using trusted profiles are described Table 1.

You can streamline the process by setting up dynamic rules in the first step to link the trusted profile automatically to new instances. 

The IAM token is obtained by exchanging the instance identity access token acquired through the metadata service with an IAM token.

Table 1 describes the steps involved.

| Step | Context | Service Called | User Action |
|------|---------|----------------|-------------|
| 1    | IBM Cloud | IAM | [Create an IAM trusted profile](/docs/account?topic=account-trustedprofile-compute-tutorial). |
| 2    | IBM Cloud | IAM | Assign access rights to the IAM trusted profile. |
| 3    | IBM Cloud | VPC API or UI| From the API, make a call to create a new instance, configured with the [metadata service](/docs/vpc?topic=vpc-imd-configure-service) enabled. Specify any user data and the default trusted profile. Specify to link the trusted profile in the same call. From the UI, [create an instance](/docs/vpc?topic=vpc-creating-virtual-servers) and select a trusted profile and link it to the instance. You can also [enable an existing instance to use the metadata service](/docs/vpc?topic=vpc-imd-configure-service#imd-metadata-service-enable). |
| 4    | VPC instance | VPC API | Make a call to the metadata token service and get an [instance identity access token](/docs/vpc?topic=vpc-imd-configure-service#imd-get-token). |
| 5    | VPC instance | Metadata service API |  Make a call to [generate an IAM token from the instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange). The IAM token allows access to all IAM-enabled services. |
| 6    | VPC instance | IAM-enabled service | Pass the IAM token to call an IAM-enabled service API. The required access rights to the service exist in the trusted profile. |
{: caption="Table 1. Procedure for using a trusted profile" caption-side="bottom"}

For more information, see the IAM documentation on [setting up trusted profiles](/docs/account?topic=account-create-trusted-profile).

## End-to-end procedure for using a trusted profile to call IAM-enabled services
{: #imd-trusted-profile-md-ex}

1. Create a [trusted profile](/docs/account?topic=account-create-trusted-profile) for the instance.

2. Create a new instance and specify a drfault trusted profile. From the VPC API, set the `auto_link` property to `true` to automatically link the trusted profile to the instance. Specify the trusted profile ID or the CRN of the trusted profile.  For example:

    ```
    curl -X POST "$vpc_api_endpoint/v1/instances?version=2022-01-11&generation=2" -H "Authorization: $iam_token" 
    -d '{
        "default_trusted_profile": {
           "auto_link": true,
           "target": {
              "crn": "crn:v1:bluemix:public:iam-identity::a/123456::profile:Profile-9fd84264-7de4-4627-94c4-8ecde51d5ac5
              }
           },
          .
          .
          .
        }'
    ```
    {: codeblock}

    You must have Adminstrator or Editor IAM permissions to complete this step. See [IAM permssions for linking trusted profiles](#imd-iam-auth) in this topic for more information.
    {: note}

3. Obtain an instance identity access token and create an environment variable for it:

    1. Log into IBM Cloud CLI.

    2. Go to an existing instance. Use `ibmcloud is instances` to locate a running instance in which to enable the metadata service. The instance must have a VPC associated with it.

    3. List security group rules so you can ping the virtual servers over the floating IP address:

       ```
       ibmcloud is security-group-rules {id}
       ```
       {: pre}

    4. SSH to get a connection into the virtual server to issue the APIs for the metadata service.

    5. Log into the virtual server. You can be running a stock image or custom image. The metadata service is supported on all stock and custom images, and CPU profiles. At this point, the floating IP has been assigned, the security groups are there, and you're now working within the virtual server.

    6. Make a call to the metadata token service to retrieve an access token.  Specify how long the token is valid, for example 3600 seconds (1hour). In this example, the command is run through the `jq` parser to format the JSON response. You can choose the parser that you prefer.

        ```
        instance_identity_token=`curl -X PUT "http://169.254.169.254/instance_identity/v1/token?version=2021-12-12"\
          -H "Metadata-Flavor: ibm"\
          -H "Accept: application/json"\
          -d '{ 
                "expires_in": 3600 
              }' | jq -r '(.instance_identity_token)'`
        ```
        {: pre}

       The response is the access token payload. 

    7. You can now make a call to the metadata service. The first call is to initialization information:

       ```
       curl -X GET "http://169.254.169.254/metadata/v1/instance/initialization?version=2021-12-12"\
          -H "Accept: application/json"\
          -H "Authorization: Bearer $instance_identity_token"
          | jq -r
       ```
       {: pre}

       Information in the response shows the SSH key and user data specified when the virtual server was provisioned. If you configured passwords, that information is also returned.

4. [Generate an IAM token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange) from the instance identity access token acquired from the metadata service.

5. Use the IAM token to make calls to IAM-enabled services.

## Next steps
{: #imd-trusted-profile-next-steps}

Make calls to the instance metadata service for instances, SSH keys, and placement group data. See [Using the instance metadata service](/docs/vpc?topic=vpc-imd-get-metadata).
