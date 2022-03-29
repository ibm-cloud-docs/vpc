---

copyright:
  years: 2022
lastupdated: "2022-03-29"

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
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Configure the instance metadata service
{: #imd-configure-service}

Configure the metadata service by obtaining an instance identity access token from the metadata service. Optionally, generate an IAM access token from this token to access IAM-enabled services in the account.
{: shortdesc}

## Accessing the metadata service using the instance identity access token service
{: #imd-get-token}

To access the instance metadata service, you must first obtain an instance identity access token (a JSON web token). You can later generate an IAM token from the instance identity token, and then use it to access IAM-enabled services.

Windows users have additional requirements to set up the metadata service. For information, see [Setting up windows servers for using the metadata service](/docs/vpc?topic=vpc-imd-windows-configuration).
{: note}

### Instance identity access token concepts
{: #imd-token-concepts}

An instance identity access token provides your security credential for accessing the metadata service. It's a signed token with a set of claims based on information about the instance and information passed in the token request.

To access the instance identity, you make a REST API `PUT "http://169.254.169.254/instance_identity/v1/token` call that invokes a well-known, non-routable IP address. You acquire the token from within the instance. Communication between the instance and metadata service never leaves the host.

In the data section of the request, you specify an expiration time for the token. The default is five minutes, but you can specify that it expire sooner or later (5 seconds to one hour).

The response (a JSON payload) contains the instance identity access token. Use this token to access the metadata service. 

You can also generate an IAM token from this token and use the RIAS API to call IAM-enabled services. For more information, see [Generate an IAM token from an instance identity access token](#imd-token-exchange).

### Acquire an instance identity access token
{: #imd-json-token}

Make `PUT "http://169.254.169.254/instance_identity/v1/token` call to get an instance identity access token from the token service. The following example uses `jq` to parse the JSON API response and then extract the access token value. You can use your preferred JSON parser.

In the example, the return value of the cURL command is the instance identity access token, which is extracted by `jq` and placed in the `instance_identity_token` evironment variable. You use specify this variable in a `GET` call to the metadata service to reach the metadata endpoint. For more information, see [Retrieve metadata from your running instances](/docs/vpc?topic=vpc-imd-get-metadata&interface=api#imd-retrieve-instance-data).

The example uses `jq` as a parser, a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/). `jq` may not come preinstalled on all VPC images available when creating an instance. You might need to install `jq` prior to use or use any parser of your choice.
{: note}

```
instance_identity_token=`curl -X PUT "http://169.254.169.254/instance_identity/v1/token?version=2022-03-08"\
  -H "Metadata-Flavor: ibm"\
  -d '{
        "expires_in": 3600
      }' | jq -r '(.access_token)'`
```
{: pre}

The JSON response shows the access token character string, date and time it was created, date and time it expires, and expiration time you set.  Note that the token expires in 5 minutes. For example:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IlZTSS1DUl91cy1lYXN0X2I5...",
  "created_at": "2022-03-04T11:08:39.363Z",
  "expires_at": "2022-03-04T11:13:39.363Z",
  "expires_in": 3600
}
```
{: codeblock}


## Generate an IAM token from an instance identity access token
{: #imd-token-exchange}
{: api}

To access IBM Cloud IAM-enabled services in the account, you can generate an IAM token from the instance identity access token using trusted profile information. After you generate the IAM token, you can use it to access IAM-enabled services, such as Cloud Object Storage, Cloud Database Service, as well as the VPC APIs. You can re-use the token multiple times.

Make a `POST /instance_identity/v1/iam_token` call and specify the ID of the trusted profile. This request uses the instance identity access token and a trusted profile linked to a virtual server instance to generate an IAM access token. The trusted profile can be linked either when you create the instance or provided in the request body.

The IAM API used to pass the instance identity access token and generate an IAM token is being deprecated. Beta users should migrate to the metadata service API to generate an IAM token using `POST /instance_identity/v1/iam_token`.
{: note}

Example request:

```
iam_token=`curl -X POST "http://169.254.169.254/instance_identity/v1/iam_token?version=2022-03-08"\
   -H "Authorization: Bearer $instance_identity_token"\
   -d '{
       "trusted_profile": {
          "id": "Profile-8dd84246-7df4-4667-94e4-8cede51d5ac5"
       }
      }' | jq -r '(.access_token)'`
```
{: pre}

The JSON response shows the IAM token. 

```
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0aGVfYmVzdCI6I8...",
  "created_at": "2022-03-07T14:10:15Z",
  "expires_at": "2022-03-07T15:10:15Z",
  "expires_in": 3600
}
```
{: codeblock}

For more information about trusted profiles, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).

## Create a trusted profile for the instance
{: #imd-trusted-profile-config}

Trusted profiles for compute resource identities is a feature that lets you assign an IBM Cloud IAM identity to an IBM Cloud resource, such as a virtual server instance. You can call any IAM-enabled service from an instance without having to manage and distribute IAM secrets to the instance. You can create a trusted profile when you generate an IAM token from an instance identity access token and link it to the instance. For more information, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).

## Enable or disable the instance metadata service
{: #imd-metadata-service-enable}

To retrieve metadata from an instance, you must first enable the service. You can do this for new instances and existing instances.

For this release, for instances created with metadata services disabled at time of creation and later updated to enable the metadata service, you must stop and restart the instance to fully enable the metadata service.
{: note}

### Enable or disable instance metadata using the UI
{: #imd-enable-service-ui}
{: ui}

From the IBM Cloud console, you can enable or disable the instance metadata service.

#### Enable the metadata service for an existing instance using the UI
{: #imd-enable-on-instance-ui}

Use the UI to enable the metadata service on an existing instance. 

1. Navigate to the list of instances.  In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

2. Click on the name of an instance to go to the details page.

3. On the details page, scroll to **Metadata**.

4. Click the toggle on (appears green). 

#### Enable the metadata service when creating a new instance using the UI
{: #imd-enable-new-instance-ui}

This procedure shows how to enable the metadata service when creating a new virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

2. Click **Create**.  

3. [Provision the instance](/docs/vpc?topic=vpc-creating-virtual-servers).

4. Under **Metadata**, the toggle is off by default (appears gray). Click the toggle on (appears green). 

 For more information about creating virtual server instances, see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).

### Disable the metadata service using the UI
{: #imd-disable-new-instance}

This procedure shows how to disable the metadata service for an instance on which it is enabled. By default, the metadatata service is disabled when you create a new instance.

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

2. Click on an instance from the list to go to its details page.  

3. Under **Metadata**, the click the toggle button off (appears gray).

#### Enable or disable the metadata service on instance templates using the UI
{: #imd-enable-instance-template-ui}

When you create an [instance template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=ui#creating-instance-template), the enable metadata service toggle is disabled by default. Click the toggle to enable the service.

When you view the details of an existing instance template, the page will indicate whether metadata was enabled or not for the template. Note that you can't change the instance template metadata setting after creating the template.

### Enable or disable instance metadata from the CLI
{: #imd-metadata-service-enable-cli}
{: cli}

Use the CLI to enable the metadata service when you create a new instance or on an existing instance.

Before you begin:

1. Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

2. After you install the IBM Cloud CLI, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.

3. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli).

#### Enable or disable the metadata service when creating a new instance from the CLI
{: #imd-enable-on-instance-cli}

Run the `ibmcloud is instance-create` command and set the `metadata-service` property to `true`. The metadata service is disabled by default. In the response, you will see `Metadata service enabled` set to `true`.

```
ibmcloud is instance-create test-instance-1 7002c1cd-9b0b-43ee-8112-5124dedbe84b us-south-1  bx2-2x8  0711-08206578-d749-49ea-86c9-1014622d1c6f --image-id 9f0050d0-636b-4fe6-82ea-931664fd9d91 --metadata-service true

Creating instance test under account VPC1 as user myuser@mycompany.com...
                              
ID                         264060c2-e5e9-44d4-994f-eea4a6688172
Name                       test-instance-1   
CRN                        crn:v1:public:is:us-south-1:a/edd5afc483594adaa8325e2b4d1290df::
                           instance:264060c2-e5e9-44d4-994f-eea4a6688172
Status                     pending
Startable                  true
Profile                    bx2-2x8
Architecture               amd64
vCPUs                      2
Memory(GiB)                8
Bandwidth(Mbps)            4000
Metadata service enabled   true
Image                      ID                                          Name
                           9f0050d0-636b-4fe6-82ea-931664fd9d91        ibm-ubuntu-20-04-minimal-amd64-1

VPC                        ID                                          Name
                           7002c1cd-9b0b-43ee-8112-5124dedbe84b        test-vpc1
                              
Zone                       us-south-1   
Resource group             ID                                  Name
                           21cabbd983d9c4beb82690daab08717e8   Default
                              
Created                    2021-12-27T22:12:11+05:30
Boot volume                ID   Name           Attachment ID                               Attachment name
                           -    PROVISIONING   954c1c47-906d-423f-a8c8-dd3adfafd278        my-vol-attachment1
```
{: codeblock}

#### Enable or disable the metadata service for an existing instance from the CLI
{: #imd-enable-on-instance-cli}

Run the `ibmcloud is instance-update` command and specify the instance ID. To enable the metadata service, set the `metadata-service` parameter to `true`; to disable, set it to `false`. An example command for enabling the service looks like this:

```
ibmcloud is instance-update e219a883-41f2-4680-810e-ee63ade35f98 --metadata-service true
```
{: codeblock}

#### Enable or disable the metadata service when creating new instance templates from the CLI
{: #imd-enable-instance-template-cli}

When you create an [instance template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=cli#creating-instance-template-cli) from the CLI, you can indicate whether metadata will be collected for instances created based on this template. 

Use the `ibmcloud is instance-create-from-template` command and specify `metadata-service true` to enable or `metadata-service false` to disable. Once you set the template value, you can't change it. 

For example, to create an instance template with the metadata service enabled, run this command:

```
ibmcloud is instance-template-create my-template-name {template_id} us-south-1 mx2-2x16 {subnet_id} --image-id {image_id} --metadata-service true
```
{: pre}

When you create an instance from this template, specify `metadata-service true` again to enable the service on the new instance:

```
ibmcloud is instance-create-from-template --template-id {template_id} --name my-instance --metadata-service true
```
{: pre}

If you override the instance template by running the `ibmcloud is instance-template-create-override-source-template` command, you can enable or disable the metadata service by specifying the `metadata-service` parameter with `true` or `false`.

For more information about these commands, see the [VPC CLI Reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference). For more information about creating an instance template from the CLI, see [Creating an instance template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=cli#creating-instance-template-cli).

### Enable or disable instance metadata from the API
{: #imd-metadata-service-enable-api}
{: api}

#### Enable or disable the metadata service when creating a new instance from the API
{: #imd-disable-on-instance-api}

The metadata service is disabled by default when you create a new instance by making a `POST /instances` call. 

You can enable the service by specifying the `metadata_service` parameter and setting `enabled` to `true`.

This example shows enabling the metadata service at instance creation:

```
curl -X POST "$vpc_api_endpoint/v1/instances?version=2022-01-12&generation=2"\
-H "Authorization: Bearer $iam_token"\
-d '{
      "image": {
         "id": "9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366"
      },
      "keys": [
         {
           "id": "363f6d70-0000-0001-0000-00000013b96c"
         }
      ],
      "name": "my-instance",
      "metadata_service": {
         "enabled": true
      },
      .
      .
      .
   }'
```
{: code_block}

The response will show the metadata parameter set to `true` when you enable the service. You can also verify the metadata service setting by making a `GET /instance/{id}` call.

#### Enable or disable the metadata service for an existing instance from the API
{: #imd-enable-on-instance-api}

To enable or disable the service from an existing instance, make a `PATCH /instance/{instance_id}` call and specify the `metadata_service` parameter. By default, the `enabled` property is set to `false`. To enable the service, set it to`true`.

This example call shows enabling the metadata service for an instance:

```
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2022-01-12&generation=2"\
    -H "Authorization: Bearer $iam_token"\ 
    -d '{
          "metadata_service": {
            "enabled": true
          }
      }'
```
{: code_block}

The response will show the metadata parameter set to `true` when you enable the service. You can also verify the metadata service setting by making a `GET /instance/{id}` call.

#### Enable or disable the metadata service when creating new instance templates from the API
{: #imd-enable-instance-template-api}

When you create an instance template, you can set this value by making a `POST /instance/templates` call. By default, the  `enabled` property is set to `false`. To enable it, set it to `true`.

For example:

```
curl -X POST "$vpc_api_endpoint/v1/instance/templates?version=2022-01-12&generation=2"\
    -H "Authorization: Bearer $iam_token"\ 
    -d '{
         "image": {
           "id": "3f9a2d96-830e-4100-9b4c-663225a3f872"
         },
         "keys": [
           {
             "id": "363f6d70-0000-0001-0000-00000013b96c"
           }
         ],
         "name": "my-instance-template",
         "metadata_service": {
             "enabled": true
         },
         "primary_network_interface": {
           "subnet": {
             "id": "0d933c75-492a-4756-9832-1200585dfa79"
           }
         },
         "profile": {
           "name": "bx2-2x8"
         },
         "vpc": {
           "id": "dc201ab2-8536-4904-86a8-084d84582133"
         },
         "zone": {
           "name": "us-south-1"
         }
       }'
```
{: pre}

The API does not allow changing the `metadata-service` setting after creating the instance template. If you disabled it for a template, create a new instance template with the `metadata-service` enabled set to `true`.

## Activity Tracker events for instance metadata
{: imd-at-events}

Activity Tracker events are triggered when you get an [instance access identity token](#imd-json-token) and then [use the service](/docs/vpc?topic=vpc-imd-get-metadata). For information about these events, see [Instance Metadata service events](/docs/vpc?topic=vpc-at-events#events-metadata).

## Next steps
{: #imd-token-next}

After creating an access token and enabling the metadata service, you can retrieve metadata for the instance, SSH keys, and placement groups. For information, see [Use the instance metadata service](/docs/vpc?topic=vpc-imd-get-metadata).
