---

copyright:
  years: 2025, 2025
lastupdated: "2025-11-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Using a trusted profile to call IAM-enabled services for bare metal servers
{: #metadata-trusted-profile-bare-metal}

You can create a trusted profile for compute resource identities in {{site.data.keyword.iamlong}}, and then assign access rights for IAM-enabled services to a bare metal server. These services can be called from a bare metal server without having to manage and distribute IAM secrets to the bare metal server. Use this option when you want to call IAM-enabled services as part of bare metal server initialization.
{: shortdesc}

## About trusted profiles for compute resource identities
{: #metadata-compute-res-identity-bare-metal}

You use trusted profiles for compute resource identities to assign an {{site.data.keyword.iamshort}} identity to an individual compute resource, such as a bare metal server. Instead of creating a service ID, generating an API key, getting the application to store, and validating that key, you can assign IAM identity directly to the bare metal server and acquire a bare metal server identity access token.

The compute resource identity service creates the trusted profile, against which you assign access rights to enable the bare metal server to call IAM-enabled services, such as {{site.data.keyword.cos_full_notm}} and {{site.data.keyword.keymanagementservicelong}}. You link a trusted profile when you provision a bare metal server. Trusted profiles define authorization for all applications that are running on the bare metal server.

The trusted profile that you specify when you provision the bare metal server becomes the default trusted profile. It automatically links to the bare metal server when you use the UI. From the API, you can link it in the same call to create the bare metal server.

The bare metal server inherits the access rights that are defined in the default trusted profile. VPC uses that trusted profile for any bare metal server identity request to generate an IAM token from a bare metal server identity access token, where the token generation request does not specify a trusted profile.

## Before you begin
{: #metadata-byb-identities-procedure-bare-metal}

1. Install the IBM Cloud CLI and the VPC CLI plug-in (see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup)).

1. Make sure that you [created a bare metal server by using the CLI](/docs/vpc?topic=vpc-creating-bare-metal-servers&interface=cli).

1. Configure a floating IP so that you can ping the virtual servers over the floating IP address and SSH into them.

1. Create a trusted profile. For more information, see [Managing access for apps in compute resources](/docs/account?topic=account-trustedprofile-compute-tutorial#trusted-profile-compute-create).

## IAM authorizations for linking trusted profiles
{: #metadata-auth-trusted-profiles-bare-metal}

To link a trusted profile to a bare metal server, you must have sufficient authorization. With the correct IAM permissions, you can see all trusted profiles that are linked to a bare metal server in the console.

Verify that your access permissions are assigned as Administrator or Editor for the IAM Identity Service:

In the console,

1. Go to **Manage** > **Access** > **Manage identities** > [Users](/iam/users){: external}.
1. Click **Users**, select your name to display your details.
1. Click **Access**, and scroll to **Access policies**.

To create a link to a trusted profile, you need the following IAM access roles: `iam-identity.profile.update` and `iam-identity.profile.linkToResource`.

For more information, see the [IAM API reference](/apidocs/iam-identity-token-api#create-link).

## Using trusted profiles for compute resource identities
{: #metadata-procedure-trusted-profile-bare-metal}

The general procedure and context for using trusted profiles are described in Table 1.

You can streamline the process by setting up dynamic rules in the first step to link the trusted profile automatically to new bare metal servers.

The IAM token is obtained by exchanging the bare metal server identity access token that was acquired through the metadata service with an IAM token.

| Step | Context | Service Called | User Action |
|------|---------|----------------|-------------|
| 1    | IBM Cloud | IAM | [Create an IAM trusted profile](/docs/account?topic=account-trustedprofile-compute-tutorial). |
| 2    | IBM Cloud | IAM | Assign access rights to the IAM trusted profile. |
| 3    | IBM Cloud | VPC API or UI| With the API, make a request to create a new bare metal server, which is configured with the [metadata service](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal) enabled. Specify any user data and the default trusted profile. Specify to link the trusted profile in the same call. \n In the console, [create a bare metal server](/docs/vpc?topic=vpc-creating-bare-metal-servers&interface=ui) and select a trusted profile and link it to the bare metal server. You can also [Enable the metadata service for an existing bare metal server](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal#metadata-enable-on-bare-metal-server-ui-bare-metal). |
| 4    | Bare metal server | VPC API | Make an API request to the metadata token service and get a bare metal server identity access token](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal#metadata-json-token-bare-metal). |
| 5    | Bare metal server | Metadata service API |  Make an API request to [generate an IAM token from the bare metal server identity access token](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal#metadata-token-exchange-bare-metal). The IAM token allows access to all IAM-enabled services. |
| 6    | Bare metal server | IAM-enabled service | Pass the IAM token to call an IAM-enabled service API. The required access rights to the service exist in the trusted profile. |
{: caption="Procedure for using a trusted profile" caption-side="bottom"}

For more information, see the IAM documentation on [setting up trusted profiles](/docs/account?topic=account-create-trusted-profile).

## End-to-end procedure for using a trusted profile to call IAM-enabled services
{: #metadata-trusted-profile-md-ex-bare-metal}

Default trusted profiles cannot be changed on existing bare metal servers. A default trusted profile can be specified only while you provision a bare metal server.
{: important}

1. Create a [trusted profile](/docs/account?topic=account-create-trusted-profile) for the new bare metal server.

2. Create the bare metal server and specify a default trusted profile while you provision the bare metal server. With the VPC API, set the `auto_link` property to `true` to automatically link the trusted profile to the bare metal server. Specify the trusted profile ID or the CRN of the trusted profile. For example,

    ```sh
    curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers?version=2025-06-30&generation=2" -H "Authorization: $iam_token"
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

    You must have Administrator or Editor IAM permissions to complete this step. For more information, [IAM permissions for linking trusted profiles](#metadata-auth-trusted-profiles-bare-metal).
    {: requirement}

3. Obtain a bare metal server identity access token and create an environment variable for it:

   1. Log in to IBM Cloud CLI.
   2. Go to an existing bare metal server. Use `ibmcloud is bare-metal-server` to locate a running bare metal server in which to enable the metadata service. The bare metal server needs an associated VPC.
   3. List security group rules so you can ping the virtual servers over the floating IP address:

       ```sh
       ibmcloud is security-group-rules {id}
       ```
       {: pre}

   4. SSH to get a connection into the bare metal server to issue the APIs for the metadata service.
   5. Log in to the bare metal server. You can be running a stock image or custom image. The metadata service is supported on all stock and custom images, and CPU profiles. Then, the floating IP is assigned and the security groups are in place. Now you're working within the bare metal server.
   6. Make an API request to the metadata token service to retrieve a bare metal server identity access token. Specify how long the token is valid, for example 3600 seconds (1 hour). In this example, the command is run through the `jq` parser to format the JSON response. You can choose the parser that you prefer.

      ```json
      identity_token=`curl -X PUT "http://api.metadata.cloud.ibm.com /identity/v1/token?version=2025-06-30"\
        -H "Metadata-Flavor: ibm"\
        -H "Accept: application/json"\
        -d '{
             "expires_in": 3600
             }' | jq -r '(.identity_token)'`
      ```
      {: codeblock}

      The response is the access token payload.

   7. You can now make a request to the metadata service. The first call is to get the initialization information:

       ```sh
       curl -X GET "http://api.metadata.cloud.ibm.com/metadata/v1/bare_metal_servers/{id}/initialization?version=2025-06-30"\
          -H "Accept: application/json"\
          -H "Authorization: Bearer $identity_token"
          | jq -r
       ```
       {: pre}

       Information in the response shows the SSH key and user data that were specified when the virtual server was provisioned. If you configured passwords, that information is also returned.

4. [Generate an IAM token](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=api#metadata-token-exchange-bare-metal) from the bare metal server identity access token that was acquired from the metadata service.

5. Use the IAM token to make calls to IAM-enabled services.

## Next steps
{: #metadata-trusted-profile-next-steps-bare-metal}

Make calls to the bare metal server metadata service for bare metal servers, SSH keys, and placement group data. See [Using the bare metal server metadata service](/docs/vpc?topic=vpc-get-metadata-bare-metal).
