---

copyright:
  years: 2022, 2023
lastupdated: "2025-04-23"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Accessing metadata from an instance
{: #imd-access-instance-metadata}

Most often, you want to access metadata from a running instance and use it to bootstrap an instance. This topic describes the general procedure for enabling the metadata service, creating an instance identity access token, and accessing the metadata.
{: shortdesc}

## Before you begin
{: #imd-prereq-byb-identities-procedure}

1. Install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

1. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli#create-a-vpc-cli).

1. Configure a floating IP so that you can SSH into the virtual servers over the floating IP address.

## General procedure to access instance metadata
{: #imd-gen-procedure}

Table 1 describes the steps that are involved in accessing instance metadata. The information provides the context for each step and links to specific information for completing the step. In an actual environment, steps 3 - 5 would more likely be started by your instance's initialization software at startup or by using cloud-init.

| Step | Context | Service Called | User Action |
|------|---------|----------------|-------------|
| 1    | IBM Cloud | VPC UI, CLI, API | Create a virtual server instance and enable the metadata service on it. (The service is disabled by default.) You can also enable the service on an existing instance, in the [UI](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-enable-on-instance-ui) or with the API. With the API, make a request to create a virtual server instance that is configured with the [metadata service](/docs/vpc?topic=vpc-imd-configure-service) enabled. Customer user data is specified on instance creation. |
| 2    | IBM Cloud | - | Sign on to the instance by using the normal startup operations. |
| 3    | VPC instance | Metadata service | Provide a `curl` command to call the metadata token service to [acquire an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-json-token). |
| 4    | VPC instance | Metadata service | Provide a `curl` command to [call the metadata service](/docs/vpc?topic=vpc-imd-get-metadata#imd-retrieve-instance-data). The token from the previous step is passed and the metadata is returned.|
| 5    | VPC instance | - | Parse the JSON returned in the previous step to acquire the user data. |
{: caption="General procedure for accessing instance metadata" caption-side="bottom"}

## End-to-end procedure for accessing metadata from an instance
{: #imd-access-md-ex}

1. Log in to IBM Cloud CLI.

2. Go to an existing instance. Use the following command to locate a running instance in which to enable the metadata service. The instance must have a VPC that is associated with it.

   ```sh
   ibmcloud is instance {instance_id}
   ```
   {: pre}

3. List security group rules so you can SSH into the virtual servers over the floating IP address:

   ```sh
   ibmcloud is security-group-rules {security_group_id}
   ```
   {: pre}

4. SSH to get a connection into the virtual server to issue the APIs for the metadata service.

5. Log in to the virtual server. You can be running a stock image or custom image. The metadata service is supported on all stock and custom images, and CPU profiles. Now, the floating IP is assigned. The security groups are in place, and you're now working within the virtual server.

6. Make an API call to the metadata token service to retrieve an instance identity access token. Specify how long the token is valid, for example 3600 seconds (1 hour). In this example, the command is run through the `jq` parser to format the JSON response. You can choose the parser that you prefer.

   ```json
   export instance_identity_token=`curl -X PUT "http://api.metadata.cloud.ibm.com/instance_identity/v1/token?version=2024-11-12"\
     -H "Metadata-Flavor: ibm"\
     -H "Accept: application/json"\
     -d '{
           "expires_in": 3600
         }' | jq -r '(.access_token)'`
   ```
   {: codeblock}

   The response is the instance identity access token payload.

7. You can now make an API call to the metadata service. The first call is for the initialization information:

   ```sh
   curl -X GET "http://api.metadata.cloud.ibm.com/metadata/v1/instance/initialization?version=2024-11-12"\
      -H "Accept: application/json"\
      -H "Authorization: Bearer $instance_identity_token"\
      | jq -r
   ```
   {: codeblock}

   Information in the response shows the SSH key and user data that was specified when the virtual server was provisioned. If you configured passwords, that information is also returned.

8. Access metadata about the instance, such as volume attachments, dedicated hosts, memory, vCPUs, and so on.

   ```sh
   curl -X GET "http://api.metadata.cloud.ibm.com/metadata/v1/instance?version=2024-11-12"\
      -H "Accept: application/json"\
      -H "Authorization: Bearer $instance_identity_token"\
      | jq -r
   ```
   {: codeblock}

9. Continue by making calls for other metadata for the instance, SSH keys, and placement groups by issuing REST API calls within the instance. For more information, see [Use the instance metadata service](/docs/vpc?topic=vpc-imd-get-metadata).

## Next steps
{: #imd-access-md-next-steps}

Use the trusted profile for the instance and generate an IAM token from the instance identity access token to other IAM-enabled services. See [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).
