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


# Configure the metadata service (Beta)
{: #imd-configure-service}

Configure the metadata service by obtaining an access token from the metadata service. Optionally, exchange this token with an IAM token to access IAM-enabled services in the account. Create a trusted profile with these access rights to allow the instance to call the metadata service.
{: shortdesc}

This service is available only to accounts with special approval to preview this beta feature.
{: beta}

## Accessing the metadata service using the instance identity token service
{: #imd-get-token}

To access the instance metadata service, you must first obtain an instance identity access token (a JSON web token). You can later exchange it for an IAM token, which you can use to access all IAM-enabled services.

Windows users have additional requirements to set up the metadata service. For information, see [Setting up windows servers for using the metadata service](/docs/vpc?topic=vpc-imd-windows-configuration).
{: note}

### Access token concepts
{: #imd-token-concepts}

An instance identity access token provides your security credential for accessing the metadata service. It's a signed token with a set of claims based on information about the instance and information passed in the token request.

To interact with the token server, you make a REST API `PUT "http://169.254.169.254/instance_identity/v1/token` call that invokes a well-known, non-routable IP address. You access the token from within the instance. Communication between the instance and metadata service never leaves the host.

In the data section of the request, you specify an expiration time for the token. The default is five minutes, but you can specify that it expire sooner or later (5 seconds to one hour).

The response (a JSON payload) contains the instance identity access token. Use this token to access the metadata service. You can also exchange this token for an IAM token and use the RIAS API to call IAM-enabled services. For more information, see [Exchange an instance identity access token for an IAM token](#imd-token-exchange).

### Acquire an access token
{: #imd-json-token}

Make `PUT "http://169.254.169.254/instance_identity/v1/token` call to get an instance identity access token from the token service. The following example uses `jq` to parse the JSON API response and then extract the access token value. You can use your preferred parser.

In the example, the return value of the cURL command is the instance identity access token, which is extracted by `jq` and placed in the `access_token` evironment variable. You use specify this variable in the `GET` call to the metadata service, to reach the metadata endpoint. For more information, see [Retrieve metadata from your running instances](/docs/vpc?topic=vpc-imd-get-metadata#imd-retrieve-instance-data).

The example uses `jq` as a parser, a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/). `jq` may not come preinstalled on all VPC images available when creating an instance. You might need to install `jq` prior to use or use any parser of your choice.
{: note}

```
access_token=`curl -X PUT "http://169.254.169.254/instance_identity/v1/token?version=2021-08-03" \
  -H "Metadata-Flavor: IBM" \
  -H "Accept: application/json" \
  -d '{ \
        "expires_in": 30 \
      }' | jq -r '(.access_token)'`
```
{: codeblock}

The JSON response shows the access token character string, date and time it was created, date and time it expires, and expiration time you set.  Note that the token expires in 30 seconds. For example:

```
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInD3cCI6IkpXCVI9...MNx2KPoYj8YibyB1jO4o8",
  "created_at": "2021-07-30T15:09:45Z",
  "expires_at": "2021-07-30T15:10:15Z",
  "expires_in": 3600
}
```
{: codeblock}

For convenience, create an environment variable for the access token:

```
ACCESS_TOKEN = <token_string>
```
{: pre}

## Exchange an instance identity access token for an IAM token
{: #imd-token-exchange}

To access IBM Cloud IAM-enabled services in the account, you can exchange the instance identity access token for an IAM token using trusted profile information. After you exchange the token, you can use it to access IAM-enabled services, such as Cloud Object Storage, Cloud Database Service, as well as the VPC APIs. You can re-use the token multiple times.

You exchange the access token with an IAM token by invoking a `POST` request within the virtual machine. The request specifies the token variable and creates a [trusted profile](/docs/vpc?topic=vpc-imd-trusted-profile-metadata) within IAM. This exchanges the access token with an IAM token linked to the trusted profile.

To exchange a token, make a call like this:

```
curl – X POST \
-H "Content-Type: application/www.form.urlencoded" \
-H "Accept: application/json" \
-d “grant_type=urn.ibm.params.0auth.grant-type:cr-token&cr-token= \
    {$access_token}&profile_id= \
    Profile-b12b1517-aadc-48d4-9f6e-ae069a8ad318” \
    https:iam.cloud.ibm.com.com/identity/token -s \
    | jq -r
```
{: codeblock}

The JSON response shows the IAM token. For more information on exchanging tokens, see [Generating an IAM token for a compute resource](/docs/account?topic=account-trusted-profile-iam-token#token-exchange).

## Create a trusted profile for the instance
{: #imd-trusted-profile-config}

Trusted profiles for compute resource identities is a feature that lets you assign an IBM Cloud IAM identity to an IBM Cloud resource, such as a virtual server instance. You can call any IAM-enabled service from an instance without having to manage and distribute IAM secrets to the instance. Trusted profiles can be created within the token exchange process and linked to the instance. For more information, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).

## Enable or disable the instance metadata service
{: #imd-metadata-service-enable}

To retrieve metadata from an instance, you must first enable the service. You can do this for new instances and existing instances. 

From the VPC API, make a `POST /instance` call and specify the `metadata_service` parameter in the data section of the request, setting `enabled` to `true`. To disable the metadata service when you're creating a new instance, you'd set it to `false`.

For Beta, allow-listed user accounts have the metadata service enabled by default. If you want to disable the service, you must disable it from an account not on the allow list. For more information, see [Troubleshooting the Instance Metadata service](/docs/vpc?topic=vpc-imd-troubleshoot).
{: note}

This example shows enabling the metadata service at instance creation:

```
curl -X POST "$vpc_api_endpoint/v1/instances?version=2021-07-30&generation=2" \
-H "Authorization: $iam_token" \
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
    }
  },
  .
  .
  .
}
```
{: code_block}

To enable or disable the service from an existing instance, you'd do the same in a `PATCH /instances` request.

This example shows disabling the metadata service for an existing instance:

```
curl -X PATCH "$vpc_api_endpoint/v1/instances?version=2021-06-28&generation=2" -H "Authorization: $iam_token" 
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
    "enabled": false
    }
  },
  .
  .
  .
}
```
{: code_block}

The response in the case of a `POST` or `PATCH` call will show the metadata with the appropriate toggle. For example, enabled would show this:

```
  "metadata_service": {
    "enabled": true
  }
```
{: code_block}

You can also verify the metadata service setting by making a `GET /instance/{id}` call.

If you use instance templates, you can set this value by making a `POST /instance/template` call and set `enabled` to true or false.

## Next steps
{: #imd-token-next}

After creating an access token and enabling the metadata service, you can retrieve metadata for the instance, SSH keys, and placement groups. For information, see [Use the instance metadata service](/docs/vpc?topic=vpc-imd-get-metadata).
