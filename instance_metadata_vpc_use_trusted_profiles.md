---

copyright:
  years: 2022, 2025
lastupdated: "2025-07-31"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Using a trusted profile to call IAM-enabled services
{: #imd-trusted-profile-metadata}



You can create a trusted profile for compute resource identities in {{site.data.keyword.iamlong}}, and then assign access rights for IAM-enabled services to a virtual server instance. These services can be called from an instance without having to manage and distribute IAM secrets to the instance. Use this option when you want to call IAM-enabled services as part of instance initialization.
{: shortdesc}

For more information about creating a trusted profile, see [Establishing trust with compute resources in the console](/docs/account?topic=account-create-trusted-profile&interface=ui&q=trusted+profiles&tags=account#create-profile-compute){: ui}[Establishing trust with compute resources by using the CLI](/docs/account?topic=account-create-trusted-profile&interface=cli&q=trusted+profiles&tags=account#create-profile-compute-cli){: cli}[Establishing trust with compute resources with the API](/docs/account?topic=account-create-trusted-profile&interface=api&q=trusted+profiles&tags=account#create-profile-compute-api){: api}.

## About trusted profiles for compute resource identities
{: #imd-compute-res-identity}

Trusted profiles for compute resource identities allow for fine-grained authorization for all applications that run in a virtual server instance. They eliminate the need for creating service IDs or managing API key lifecycles for applications. Instead, compute resources become identities as part of a trusted profile, and trust is established through conditions based on resource attributes.

You can assign an IAM identity directly to the instance so that the applications that are running on the virtual server instance can securely access other IBM Cloud services without storing API keys.

You can assign access rights to the trusted profile to enable the instance to call IAM-enabled services, such as {{site.data.keyword.cos_full_notm}} and {{site.data.keyword.keymanagementservicelong}}.

You can link a trusted profile when you provision an instance. You can also link and unlink trusted profiles to and from the instance after it is provisioned by using IAM interfaces. Trusted profiles define authorization for all applications that are running on the instance.

The trusted profile that you specify when you provision the instance becomes the default trusted profile. Trusted profiles that are linked after the instance is provisioned might define more authorizations, but they cannot replace or become the default trusted profile for the instance.

The trusted profile can be linked to a compute instance either explicitly when the instance is created or implicitly through [trusted profile dynamic rules](/docs/account?topic=account-iam-condition-properties).

{{site.data.keyword.iamlong}} is the source of truth for trusted profile linkage. The trusted profile information cannot be changed after the instance creation. The information may become stale as any trusted profile-related changes made in IAM are not reflected.
{: important}

The instance inherits the access rights that are defined in all the trusted profiles that are linked to the instance. For any instance identity request that doesn't specify a trusted profile, VPC generates an IAM token from an instance identity access token that uses the default trusted profile. 

If you don't want to use the instance's default trusted profile for these requests, you must specify the trusted profile that you want to be used in the request.

## Before you begin
{: #imd-byb-identities-procedure}

### IAM authorizations for linking trusted profiles
{: #imd-iam-auth}

To link a trusted profile to an instance, you must have sufficient authorization. With the correct IAM permissions, you can see all trusted profiles that are linked to an instance in the console.

You must be assigned the administrator, or editor role within the account, or on the IAM Identity Service to manage trusted profiles.
{: requirement}

Verify that your access permissions are assigned as Administrator or Editor in the console:
1. Go to **Manage** > **Access (IAM)** > **[Users](/iam/users){: external}**.
1. Find your name in the list, and click it to display your user details.
1. Click **Access** and scroll to **Access policies**.

### Gather the required information
{: #imd-byb-environment}

Create or retrieve a trusted profile. You need either its ID or CRN.
- In the console, go to **Manage > Access (IAM)**, and select [**Trusted profiles**](/iam/trusted-profiles). For more information about creating a trusted profile and linking it to existing VPC virtual server instance, see [Establishing trust with compute resources in the console](/docs/account?topic=account-create-trusted-profile&interface=ui&q=trusted+profiles&tags=account#create-profile-compute).{: ui}
- From the CLI, you can run the `ibmcloud iam trusted-profile-create` command to create a trusted profile or run the `ibmcloud iam trusted-profiles` command to list existing trusted profiles. For more information,  [Establishing trust with compute resources by using the CLI](/docs/account?topic=account-create-trusted-profile&interface=cli&q=trusted+profiles&tags=account#create-profile-compute-cli).{: cli}
- You can make a [`GET /v1/profiles`](/apidocs/iam-identity-token-api#list-profiles) or [`POST /v1/profiles`](/apidocs/iam-identity-token-api#create-profile) request to the IAM Identity Services API. For more information, see [Establishing trust with compute resources with the API](/docs/account?topic=account-create-trusted-profile&interface=api&q=trusted+profiles&tags=account#create-profile-compute-api).{: api}

## End-to-end procedure for using a trusted profile to call IAM-enabled services
{: #imd-trusted-profile-md-ex}

1. [Create an instance](/docs/vpc?topic=vpc-creating-virtual-servers) with a linked trusted profile, and [enable the metadata service in the console](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-enable-service-ui){: ui}[enable the metadata service from the cli](/docs/vpc?topic=vpc-imd-configure-service&interface=cli#imd-metadata-service-enable-cli){: cli}[enable the metadata service with the API](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-metadata-service-enable){: api}.
   - In the console, you find the Metadata service in the **Advanced options**. Click the toggle to enable it. Next, click **Select a trusted profile**. In the side panel, select a trusted profile from the list. Click **Select trusted profile**. **Auto-link** is enabled, if you want to manage linking the trusted profile through the IAM interface, you can click the toggle to disable it. For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui).{: ui}
   - From the CLI, use the `ibmcloud is instance-create` command and specify `--default-trusted-profile YOUR_DEFAULT_TRUSTED_PROFILE`, `--default-trusted-profile-auto-link true`, and `--metadata-service true` options For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli).{: cli}
   - With the VPC API, specify the default trusted profile and set the `auto_link` property to `true` to automatically link the trusted profile to the instance. You can specify the ID or the CRN of the trusted profile. For more information, see the API VPC reference: [Create an instance](/apidocs/vpc/latest#create-instance). See the following example.{: api}
      ```sh
      curl -X POST "$vpc_api_endpoint/v1/instances?version=2022-03-01&generation=2" -H "Authorization: Bearer $iam_token"
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
      {: codeblock}{: api}

      

1. If the instance has a floating IP address already, use that address to establish a secure connection to the server. If it does not have a floating IP address, assign one to it. For more information, see the [Next steps](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#next-steps-after-creating-virtual-servers-ui) in the Creating virtual server instances topic.

1. From the virtual server instance, make an API request to the metadata token service to retrieve an instance identity access token. In this example, the command is run through the `jq` parser to format the JSON response. You can choose another parser if you prefer.
      ```sh
      curl -X PUT "$vpc_metadata_api_endpoint/instance_identity/v1/token?version=2022-03-01" -H "Metadata-Flavor: ibm" -d '{"expires_in": 3600}'| jq -r '(.instance_identity_token)'
      ```
      {: pre}

      The response is the access token payload. For more information, see the Metadata API reference: [Create an instance identity access token](/apidocs/vpc-metadata#create-access-token).

1. Use the instance identity token to [generate an IAM token](/docs/vpc?topic=vpc-imd-identity-operations#imd-token-exchange).

       ```sh
       curl -X POST "$vpc_metadata_api_endpoint/instance_identity/v1/iam_token?version=2025-06-10" -H "Authorization: Bearer $instance_identity_token" -d '{"trusted_profile": {"id": "Profile-8dd84246-7df4-4667-94e4-8cede51d5ac5"}}'
       ```
       {: pre}

       During the exchange a trusted profile against which the token is being requested must be provided to IAM service. If a trusted profile is not specified, the API uses the default trusted profile, which was linked during instance provisioning. If multiple trusted profiles are linked to an instance, you can choose not to use the default trusted profile, and specify a different trusted profile by ID or CRN.

1. Use the IAM token to make calls to IAM-enabled services.

## Next steps
{: #imd-trusted-profile-next-steps}

Make calls to the metadata service for instances, SSH keys, and placement group data. See [Retrieving metadata from an instance](/docs/vpc?topic=vpc-imd-access-instance-metadata).
