---

copyright:
  years: 2022, 2025
lastupdated: "2025-05-28"

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

Trusted profiles for compute resource identities allow for fine-grained authorization for all applications that run in a virtual server instance. They eliminate the need for creating service IDs or managing API key lifecycles for applications. Instead, compute resources become identities as part of a trusted profile, and trust is established through conditions based on resource attributes.

You can assign an IAM identity directly to the instance so that the applications that are running on the virtual server instance can securely access other IBM Cloud services without storing API keys.

You can assign access rights to the trusted profile to enable the instance to call IAM-enabled services, such as {{site.data.keyword.cos_full_notm}} and {{site.data.keyword.keymanagementservicelong}}.

You can link a trusted profile when you provision an instance. You can also link trusted profiles to an instance after it is provisioned. Trusted profiles define authorization for all applications that are running on the instance.

The trusted profile that you specify when you provision the instance becomes the default trusted profile. Trusted profiles linked after the instance is provisioned might define more authorizations, but they cannot replace or become the default trusted profile for the instance.

The instance inherits the access rights that are defined in all the trusted profiles that are linked to the instance. For any instance identity request that doesn't specify a trusted profile, VPC generates an IAM token from an instance identity access token that uses the default trusted profile. If you don't want to use the instance's default trusted profile used for these requests, you must specify the trusted profile that you want to be used for the instance identity request.

## Before you begin
{: #imd-byb-identities-procedure}

### Setting up your environment
{: #imd-byb-environment}

* Install the IBM Cloud CLI and the VPC CLI plug-in (see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup)).

* Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli#create-a-vpc-cli).

* Configure a floating IP so that you can ping the virtual servers over the floating IP address and SSH into them.

* Create a trusted profile. For more information, see [Managing access for apps in compute resources](/docs/account?topic=account-trustedprofile-compute-tutorial#trusted-profile-compute-create).

### IAM authorizations for linking trusted profiles
{: #imd-iam-auth}

To link a trusted profile to an instance, you must have sufficient authorization. With the correct IAM permissions, you can see all trusted profiles that are linked to an instance in the console.

You must be assigned the administrator, or editor role within the account, or on the IAM Identity Service to manage trusted profiles.
{: requirement}

Verify that your access permissions are assigned as Administrator or Editor in the console:
1. Go to **Manage** > **Access (IAM)** > **[Users](/iam/users){: external}**.
1. Find your name in the list, and click it to display your user details.
1. Click **Access** and scroll to **Access policies**.

## Using trusted profiles for compute resource identities
{: #imd-procedure-trusted-profile}

The general procedure and context for using trusted profiles are described in the following table. You can streamline the process by setting up dynamic rules in the first step to link the trusted profile automatically to new instances. The IAM token is obtained by exchanging the instance identity access token that was acquired through the metadata service with an IAM token.

| Step | Context | Service Called | User Action |
|------|---------|----------------|-------------|
| 1    | IBM Cloud | IAM | [Create an IAM trusted profile](/docs/account?topic=account-trustedprofile-compute-tutorial). |
| 2    | IBM Cloud | IAM | Assign access rights to the IAM trusted profile. |
| 3    | IBM Cloud | VPC API or UI| With the API, make a request to create a new instance with the [metadata service](/docs/vpc?topic=vpc-imd-configure-service) enabled. Specify any user data and the default trusted profile. Specify to link the trusted profile in the same call. \n In the console, [create an instance](/docs/vpc?topic=vpc-creating-virtual-servers) and select a trusted profile and link it to the instance. You can also [enable an existing instance to use the metadata service](/docs/vpc?topic=vpc-imd-configure-service#imd-metadata-service-enable). |
| 4    | VPC instance | VPC API | Make an API request to the metadata token service and get an [instance identity access token](/docs/vpc?topic=vpc-imd-configure-service#imd-get-token). |
| 5    | VPC instance | Metadata service API |  Make an API request to [generate an IAM token from the instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange). The IAM token allows access to all IAM-enabled services. |
| 6    | VPC instance | IAM-enabled service | Pass the IAM token to call an IAM-enabled service API. The required access rights to the service exist in the trusted profile. |
{: caption="Procedure for using a trusted profile" caption-side="bottom"}

For more information, see the IAM documentation on [setting up trusted profiles](/docs/account?topic=account-create-trusted-profile).

## End-to-end procedure for using a trusted profile to call IAM-enabled services
{: #imd-trusted-profile-md-ex}

Default trusted profiles cannot be changed on existing instances. A default trusted profile can be specified only while you provision an instance.
{: important}

1. Retrieve the information about the [trusted profile](/docs/account?topic=account-create-trusted-profile) that you want to use for the new instance.

2. Create the instance and specify a default trusted profile while you provision the instance. With the VPC API, set the `auto_link` property to `true` to automatically link the trusted profile to the instance. Specify the trusted profile ID or the CRN of the trusted profile. See the following example.
   ```sh
   curl -X POST "$vpc_api_endpoint/v1/instances?version=2025-04-22&generation=2" -H "Authorization: Bearer $iam_token"
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

   For more information, see the API VPC reference: [Create an instance](/apidocs/vpc/latest#create-instance).

3. Obtain an instance identity access token and create an environment variable for it:
   1. Log in to IBM Cloud CLI.
   1. Go to an existing instance. Use `ibmcloud is instances` to locate a running instance in which to enable the metadata service. The instance must be associated with a VPC. The instance can be running a stock or a custom image. The metadata service is supported on all stock and custom images, and CPU profiles.
   1. [Make an SSH connection to the virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#next-steps-after-creating-virtual-servers-ui) to log in.
   1. From the virtual server instance, make an API request to the metadata token service to retrieve an instance identity access token. Specify how long the token is valid, for example 3600 seconds (1 hour). In this example, the command is run through the `jq` parser to format the JSON response. You can choose another parser if you prefer.
      ```sh
      curl -X PUT "$vpc_metadata_api_endpoint/instance_identity/v1/token?version=2025-04-22" -H "Metadata-Flavor: ibm" -d '{"expires_in": 3600}'| jq -r '(.instance_identity_token)'
      ```
      {: pre}

      The response is the access token payload. For more information, see the Metadata API reference: [Create an instance identity access token](/apidocs/vpc-metadata#create-access-token).

   1. You can now make a request to the metadata service. The first call is to get the initialization information:
      ```sh
      curl -X GET "$vpc_metadata_api_endpoint/metadata/v1/instance/initialization?version=2025-04-22" -H "Authorization: Bearer $instance_identity_token"
      ```
      {: pre}

      Information in the response shows the SSH key and user data that were specified when the virtual server was provisioned. If you configured passwords, that information is also returned. For more information, see the Metadata API reference: [Retrieve initialization information](/apidocs/vpc-metadata#get-instance-initialization).

4. [Generate an IAM token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange) from the instance identity access token that was acquired from the metadata service.

5. Use the IAM token to make calls to IAM-enabled services.

## Next steps
{: #imd-trusted-profile-next-steps}

Make calls to the instance metadata service for instances, SSH keys, and placement group data. See [Using the instance metadata service](/docs/vpc?topic=vpc-imd-get-metadata).
