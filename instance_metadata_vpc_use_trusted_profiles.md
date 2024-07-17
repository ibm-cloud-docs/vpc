---

copyright:
  years: 2022, 2024
lastupdated: "2024-07-17"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Using a trusted profile to call IAM-enabled services
{: #imd-trusted-profile-metadata}

You can create a trusted profile for compute resource identities in {{site.data.keyword.iamlong}}, and then assign access rights for IAM-enabled services to a virtual server instance. These services can be called from an instance without having to manage and distribute IAM secrets to the instance. Use this option when you want to call IAM-enabled services as part of instance initialization.
{: shortdesc}

## About trusted profiles for compute resource identities
{: #imd-compute-res-identity}

Trusted profiles for compute resource identities let you assign an {{site.data.keyword.iamshort}} identity to an individual compute resource, such as a virtual server instance. Instead of having to create a service ID, generate an API key, get the application to store and validate that key, you can assign IAM identity directly to the instance and acquire an instance identity access token.

The compute resource identity service creates the trusted profile, against which you assign access rights to enable the instance to call IAM-enabled services, such as {{site.data.keyword.cos_full_notm}} and {{site.data.keyword.keymanagementservicelong}}. You link a trusted profile when you provision an instance. Trusted profiles define authorization for all applications running on the instance.

The trusted profile that you specify when you provision the instance becomes the default trusted profile. It's automatically linked to the instance when you use the UI. From the API, you can link it in the same call to create the instance.

The instance inherits the access rights that are defined in the default trusted profile. VPC uses that trusted profile for any instance identity request to generate an IAM token from an instance identity access token, where the token generation request does not specify a trusted profile.

## Before you begin
{: #imd-byb-identities-procedure}

1. Install the IBM Cloud CLI and the VPC CLI plug-in (see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup)).

2. After the installation of the VPC CLI plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.

3. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli#create-a-vpc-cli).

4. Configure a floating IP so that you can ping the virtual servers over the floating IP address and SSH into them.

5. Create a trusted profile. For more information, see [Establishing trust with compute resources](/docs/secure-enterprise?topic=secure-enterprise-create-trusted-profile&interface=ui#create-profile-compute).

## IAM authorizations for linking trusted profiles
{: #imd-iam-auth}

To link a trusted profile to an instance, you must have sufficient authorization. With the correct IAM permissions, you can see all trusted profiles that are linked to an instance in the UI.

Verify that your access permissions are assigned as Administrator or Editor:

In the UI,
1. Go to [Manage access and users](/iam/users){: external}.
2. Click **Users**, select your name, and click **Access policies**.

From the API, make a `GET /iam-identity.profile.get` call.

If you need to update your profile, make these calls:

* `POST /iam-identity.profile.update`
* `POST /iam-identity.profile.linkToResource`

For more information, see the [IAM API reference](/apidocs/iam-identity-token-api#create-link-permissions).

## Using trusted profiles for compute resource identities
{: #imd-procedure-trusted-profile}

The general procedure and context for using trusted profiles are described in Table 1.

You can streamline the process by setting up dynamic rules in the first step to link the trusted profile automatically to new instances.

The IAM token is obtained by exchanging the instance identity access token that was acquired through the metadata service with an IAM token.

| Step | Context | Service Called | User Action |
|------|---------|----------------|-------------|
| 1    | IBM Cloud | IAM | [Create an IAM trusted profile](/docs/account?topic=account-trustedprofile-compute-tutorial). |
| 2    | IBM Cloud | IAM | Assign access rights to the IAM trusted profile. |
| 3    | IBM Cloud | VPC API or UI| With the API, make a request to create a new instance, configured with the [metadata service](/docs/vpc?topic=vpc-imd-configure-service) enabled. Specify any user data and the default trusted profile. Specify to link the trusted profile in the same call. \n In the UI, [create an instance](/docs/vpc?topic=vpc-creating-virtual-servers) and select a trusted profile and link it to the instance. You can also [enable an existing instance to use the metadata service](/docs/vpc?topic=vpc-imd-configure-service#imd-metadata-service-enable). |
| 4    | VPC instance | VPC API | Make an API request to the metadata token service and get an [instance identity access token](/docs/vpc?topic=vpc-imd-configure-service#imd-get-token). |
| 5    | VPC instance | Metadata service API |  Make an API request to [generate an IAM token from the instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange). The IAM token allows access to all IAM-enabled services. |
| 6    | VPC instance | IAM-enabled service | Pass the IAM token to call an IAM-enabled service API. The required access rights to the service exist in the trusted profile. |
{: caption="Table 1. Procedure for using a trusted profile" caption-side="bottom"}

For more information, see the IAM documentation on [setting up trusted profiles](/docs/account?topic=account-create-trusted-profile).

## End-to-end procedure for using a trusted profile to call IAM-enabled services
{: #imd-trusted-profile-md-ex}

Default trusted profiles cannot be changed on existing instances. A default trusted profile can be specified only while you provision an instance.
{: important}

1. Create a [trusted profile](/docs/account?topic=account-create-trusted-profile) for the new instance.

2. Create the instance and specify a default trusted profile while you provision the instance. With the VPC API, set the `auto_link` property to `true` to automatically link the trusted profile to the instance. Specify the trusted profile ID or the CRN of the trusted profile. For example:

    ```curl
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

    You must have Administrator or Editor IAM permissions to complete this step. See [IAM permissions for linking trusted profiles](#imd-iam-auth) in this topic for more information.
    {: requirement}

3. Obtain an instance identity access token and create an environment variable for it:

   1. Log in to IBM Cloud CLI.
   2. Go to an existing instance. Use `ibmcloud is instances` to locate a running instance in which to enable the metadata service. The instance must have a VPC associated with it.
   3. List security group rules so you can ping the virtual servers over the floating IP address:

       ```sh
       ibmcloud is security-group-rules {id}
       ```
       {: pre}

   4. SSH to get a connection into the virtual server to issue the APIs for the metadata service.
   5. Log in to the virtual server. You can be running a stock image or custom image. The metadata service is supported on all stock and custom images, and CPU profiles At this point, the floating IP is assigned, the security groups are in place, and you're now working within the virtual server.
   6. Make an API request to the metadata token service to retrieve an instance identity access token. Specify how long the token is valid, for example 3600 seconds (1 hour). In this example, the command is run through the `jq` parser to format the JSON response. You can choose the parser that you prefer.

      ```json
      instance_identity_token=`curl -X PUT "http://169.254.169.254/instance_identity/v1/token?version=2021-12-12"\
        -H "Metadata-Flavor: ibm"\
        -H "Accept: application/json"\
        -d '{
             "expires_in": 3600
             }' | jq -r '(.instance_identity_token)'`
      ```
      {: codeblock}

      The response is the access token payload.

   7. You can now make a request to the metadata service. The first call is to get the initialization information:

       ```curl
       curl -X GET "http://169.254.169.254/metadata/v1/instance/initialization?version=2021-12-12"\
          -H "Accept: application/json"\
          -H "Authorization: Bearer $instance_identity_token"
          | jq -r
       ```
       {: pre}

       Information in the response shows the SSH key and user data that were specified when the virtual server was provisioned. If you configured passwords, that information is also returned.

4. [Generate an IAM token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange) from the instance identity access token that was acquired from the metadata service.

5. Use the IAM token to make calls to IAM-enabled services.

## Next steps
{: #imd-trusted-profile-next-steps}

Make calls to the instance metadata service for instances, SSH keys, and placement group data. See [Using the instance metadata service](/docs/vpc?topic=vpc-imd-get-metadata).
