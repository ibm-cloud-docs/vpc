---

copyright:
  years: 2021
lastupdated: "2021-08-04"

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


# Accessing metadata from an instance (Beta)
{: #imd-access-instance-metadata}

Most often, you'll want to access metadata from a running instance and use that metadata to bootstrap an instance. This topic describes the general procedure for enabling the metadata service, creating an instance identity access token, and accessing the metadata. 

{:shortdesc}

This service is available only to accounts with special approval to preview this beta feature.
{:beta}

## Before you begin
{: #imd-byb-gen-procedure}

## Before you begin
{: #imd-byb-identities-procedure}

* Install the IBM Cloud CLI and the VPC CLI plug-in (see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup)).

2. After you install the VPC CLI plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.
   
3. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli).

4. Configure a floating IP so that you can ping the virtual machines over the floating IP address and SSH into them.

## General procedure to access instance metadata
{: #imd-gen-procedure}

Table 1 describes the steps involved to access instance metadata. The information provides the context for each step and links to specific information for completing the step. In an actual environment, steps 3 through 5 would more likely be invoked by your instance's initialization software at start up or using cloud-init.

| Step | Context | Service Called | User Action |
|------|---------|----------------|-------------|
| 1    | IBM Cloud | VPC API | Make a call to create a virtual server instance configured with the [metadata service](/docs/vpc?topic=vpc-imd-configure-service) enabled. Customer user data is specified on instance creation. Alternately, enable an existing instance to use the metadata service. |
| 2    | IBM Cloud | -- | Sign on to the instance using the normal startup operations. |
| 3    | VPC instance | Metadata service | Provide a `curl` command to call the metadata token service to acquire an access token. |
| 4    | VPC instance | Metadata service | Provide a `curl` command to [call the metadata service](/docs/vpc?topic=vpc-imd-get-metadata#imd-retrieve-instance-data). The token from the previous step is passed and the metadata is returned.| 
| 5    | VPC instance | -- | Parse the JSON returned in the previous step to acquire the user data. |
{: caption="Table 1. General procedure for accessing instance metadata" caption-side="top"}

## End-to-end procedure for accessing metadata from an instance
{: #imd-access-md-ex}

1. Log into IBM Cloud CLI.

2. Go to an existing instance. Use `ibmcloud is instances` to locate a running instance in which to enable the metadata service. The instance must have a VPC associated with it.

3. List security group rules so you can ping the virtual machines over the floating IP address:

   ```
   ibmcloud is security-group-rules {id}
   ```
   {:pre}

4.	SSH to get a connection into the virtual machine to issue the APIs for the metadata service.

5.	Log into the virtual machine. You can be running a stock image or custom image. The metadata service is supported on all stock and custom images, and CPU profiles. At this point, the floating IP has been assigned, the security groups are there, and you're now working within the virtual machine.

6.	Make a call to the metadata token service to retrieve an access token.  Specify how long the token is valid, for example 3600 seconds (1hour). In this example, the command is run through the `jq` parser to format the JSON response. You can choose the parser that you prefer.

   ```
   curl -X GET “http://169.254.169.254/instance_identity/v1/token/?version=06-29-21” \
       -H "Metadata-Flavor: IBM" \
       -H "Accept: application/json" \
       -H "Content-Type: application/json" \
       -d '{ 
         "expires_in": 3600 
       }' | jq -r '(.access_token)'`
   ```
   {:pre}
   
   The response is the access token payload. 

7. Copy the token and put it into an environment variable.

   ```
   access_token = <64-bit token string>
   ```
   {:pre}

8. You can now make a call to the metadata service. The first call is to initialization information, where you can access user data:

   ```json
   $ user_data=`curl -X GET "http://169.254.169.254/metadata/v1/instance/initialization?version=2021-06-29" \
     -H "Accept: application/json" \
     -H "Authorization: Bearer $access_token"
     | jq -r '(.user_data)'`
   ```
   {:codeblock}

   Information in the response shows the SSH key and user data specified when the virtual machine was provisioned. If you configured passwords, that information is also returned.

9.	Access metadata about the instance, such as volume attachments, dedicated hosts, memory, vCPUs, and so on.

   ```json
   $ `curl -X GET "http://169.254.169.254/metadata/v1/instance?version=2021-06-29" \
     -H "Accept: application/json" \
     -H "Authorization: Bearer $access_token"
     | jq -r`
   ```
   {:pre}

10. Continue by making calls for other metadata for the instance, SSH keys, and placement groups by issuing REST APIs all within the instance. For more information, see [Use the instance metadata service](/docs/vpc?topic=vpc-imd-get-metadata).

## Next steps
{: #imd-access-md-next-steps}

Want to steamline authorization when accessing instance metadata? Create and use trusted profiles to access metadata. Use the trusted profile to exchange the metadata token for an IAM token and access other IAM-enabled services. See [Creating an using a trusted profile to access instance metadata](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).
