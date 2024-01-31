---

copyright:
  years: 2022, 2024
lastupdated: "2024-01-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Use the instance metadata service
{: #imd-get-metadata}

After you obtained an instance identity access token, you can access the metadata service and retrieve metadata about a virtual server instance. This topic describes how to make calls to the API to access instance metadata such as initialization data, network interfaces, volume attachments, public SSH keys, and placement groups.
{: shortdesc}

When you make API calls to the instance metadata service, events are triggered in the Activity Tracker. For more information, see [Instance Metadata service events](/docs/vpc?topic=vpc-at-events#events-metadata).

## Before you begin
{: #imd-md-prereqs}

To access metadata service, you must have an instance identity access token. If you don't have one, see [Acquire an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service#imd-json-token).

The metadata service is disabled by default. To enable it, see [Enable or disable the metadata service](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-metadata-service-enable).

For more information about these API calls and examples, see the [Metadata service API reference](/apidocs/vpc-metadata).

Windows users have extra requirements to access and use the metadata service. For more information, see [Setting up windows servers for using the metadata service](/docs/vpc?topic=vpc-imd-windows-configuration).
{: note}


## Accessing the metadata service by using the instance identity access token service
{: #imd-get-token-usemd}

To access the instance metadata service, you must first obtain an instance identity access token. You can later generate IAM tokens from the instance identity access token and then use them to access IAM-enabled services.

Windows users have extra requirements to set up the metadata service. For more information, see [Setting up Windows servers for using the metadata service](/docs/vpc?topic=vpc-imd-windows-configuration).
{: note}

### Instance identity access token concepts
{: #imd-token-concepts-usemd}

An instance identity access token provides a security credential for accessing the metadata service. It's a signed token with a set of claims based on information about the instance and information that is passed in the token request.

To access the instance identity, make a `PUT "http://api.metadata.cloud.ibm.com/instance_identity/v1/token` call by using the [Metadata service API](/apidocs/vpc-metadata#create-access-token) that invokes the instance host name. Communication between the instance and metadata service never leaves the host, you acquire the token from within the instance. If secure access to the instance metadata service is enabled on your instance use, the "https" protocol instead of the "http" protocol.

In the request, you specify an expiration time for the token. The default is 5 minutes, but you can specify that it expires sooner or later (5 seconds to 1 hour).

The response (a JSON payload) contains the instance identity access token. Use this token to access the metadata service.

You can also generate an IAM token from this token and use the RIAS API to call IAM-enabled services. For more information, see [Generate an IAM token from an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange).

### Acquire an instance identity access token
{: #imd-json-token-usemd}

Using the Metadata service API, make `PUT "http://api.metadata.cloud.ibm.com/instance_identity/v1/token` call to get an instance identity access token. The following example uses `jq` to parse the JSON API response and then extract the instance identity access token value. You can use your preferred JSON parser.

In the example, the return value of the cURL command is the instance identity access token, which is extracted by `jq` and placed in the `instance_identity_token` environment variable. You use specify this variable in a `GET` call to the metadata service to reach the metadata endpoint. For more information, see [Retrieve metadata from your running instances](/docs/vpc?topic=vpc-imd-get-metadata&interface=api#imd-retrieve-instance-data).

The example uses `jq` as a parser, a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/). `jq` might not come preinstalled on all VPC images available when you create an instance. You might need to install `jq` before use or use any parser of your choice.
{: note}

```json
instance_identity_token=`curl -X PUT "http://api.metadata.cloud.ibm.com/instance_identity/v1/token?version=2022-08-08"\
  -H "Metadata-Flavor: ibm"\
  -d '{
        "expires_in": 3600
      }' | jq -r '(.access_token)'`
```
{: pre}

The following JSON response shows the instance identity access token character string, date, and time that it was created, date, and time that it expires, and expiration time you set. Keep in mind that the token expires in 5 minutes.

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IlZTSS1DUl91cy1lYXN0X2I5...",
  "created_at": "2022-08-08T11:08:39.363Z",
  "expires_at": "2022-08-08T11:13:39.363Z",
  "expires_in": 3600
}
```
{: codeblock}


## Generate an IAM token from an instance identity access token
{: #imd-token-exchange-usemd}
{: api}

To access IBM Cloud IAM-enabled services in the account, you can generate an IAM token from the instance identity access token by using trusted profile information. After you generate the IAM token, you can use it to access IAM-enabled services, such as {{site.data.keyword.cos_full_notm}}, Cloud Database Service, and the VPC APIs. You can reuse the token multiple times.

Make a `POST /instance_identity/v1/iam_token` call and specify the ID of the trusted profile. This request uses the instance identity access token and a trusted profile that is linked to a virtual server instance to generate an IAM access token. The trusted profile can be linked either when you create the instance or provided in the request body.

The IAM API used to pass the instance identity access token and generate an IAM token is being deprecated. Beta users must migrate to the metadata service API to generate an IAM token by using `POST /instance_identity/v1/iam_token`.
{: note}

Example request:

```json
iam_token=`curl -X POST "http://api.metadata.cloud.ibm.com/instance_identity/v1/iam_token?version=2022-08-08"\
   -H "Authorization: Bearer $instance_identity_token"\
   -d '{
       "trusted_profile": {
          "id": "Profile-8dd84246-7df4-4667-94e4-8cede51d5ac5"
       }
      }' | jq -r '(.access_token)'`
```
{: pre}

The JSON response shows the IAM token.

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0aGVfYmVzdCI6I8...",
  "created_at": "2022-08-08T14:10:15Z",
  "expires_at": "2022-08-08T15:10:15Z",
  "expires_in": 3600
}
```
{: codeblock}

For more information about trusted profiles, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).

## Create a trusted profile for the instance
{: #imd-trusted-profile-config-usemd}

Trusted profiles for compute resource identities help you assign an {{site.data.keyword.cloud}} IAM identity to an {{site.data.keyword.cloud}} resource, such as a virtual server instance. You can call any IAM-enabled service from an instance without having to manage and distribute IAM secrets to the instance. You can create a trusted profile when you generate an IAM token from an instance identity access token and link it to the instance. For more information, see [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).


## Retrieve instance metadata from your running virtual server instance
{: #imd-retrieve-instance-data}

Use the instance metadata service to get metadata about the instance, initialization information, SSH keys, and placement groups.

### Retrieve instance initialization information
{: #imd-retrieve-initialization-data}

Make a `GET "http://169.254.169.254/metadata/v1/instance/initialization"` call to retrieve initialization information for the instance.

This request:

* Calls the API to retrieve the instance identity access token.
* Uses the token to access the metadata service.
* Extracts the user data from the JSON payload by using `jq`.

You're making an unsecured request that is then secured by a proxy. The proxy intercepts the request by watching for the instance IP.
{: note}

In the example, the return value of the cURL command is the user data, which is extracted by `jq` and placed in the `user_data` environment variable.

```curl
curl -X GET "http://169.254.169.254/metadata/v1/instance/initialization?version=2021-10-12"\
  -H "Accept: application/json"\
  -H "Authorization: Bearer $access_token" | jq -r
```
{: codeblock}

The response lists public SSH keys that were used at instance initialization and extracts any other user data made available when the instance was provisioned. The response includes the SHA256 value that identifies the SSH key.

```json
{
  "keys": [
    {
      "crn": "crn:[...]",
      "fingerprint": "SHA256:RJ+YWs3rupwGFiJuLqY43tvmdeLOUjcIc9cA6IR8n8E",
      "id": "dadae729-1d81-4a13-966e-f5d92699f103",
      "name": "my-key1",
      "resource_type": "key"
    }
  ],
  "user_data": "Content-Type: multipart/form-data; boundary=3efa30189c9e0e8ebc24a4decbbf4c2be7b26120c1cdd7cb7bc2ecb0c07c\nMIME-Version: 1.0\n\n--3efa30189c9e0e8ebc24a4decbbf4c2be7b26120c1cdd7cb7bc2ecb0c07c\nContent-Type: text/cloud-config\n\n#cloud-config\ndisable_root: false\nchpasswd:\n  list: |\n    root:genes1s\n  expire: false\nusers:\n- default\n- name: root\n  lock-passwd: false\n  ssh_pwauth: true\n\n--3efa30189c9e0e8ebc24a4decbbf4c2be7b26120c1cdd7cb7bc2ecb0c07c--"
}
```
{: codeblock}

### Retrieve instance information
{: #imd-retrieve-instance}

Make a `GET "http://169.254.169.254/metadata/v1/instance"` call to retrieve detailed information about the instance. To tailor information for specific instance details, such as network interfaces, see [Extra instance metadata endpoints](#imd-additional-inst-metadata).


```curl
curl -X GET "http://169.254.169.254/metadata/v1/instance?version=2021-10-12"\
  -H "Accept:application/json"\
  -H "Authorization: Bearer $access_token"
```
{: codeblock}

The response lists all details for an instance, including network interfaces, compute profile, and volume attachments.

```json
{
  "boot_volume_attachment": {
    "id": "a8a15363-a6f7-4f01-af60-715e85b28141",
    "name": "my-boot-volume-attachment",
    "volume": {
      "crn": "crn:[...]",
      "id": "49c5d61b-41e7-4c01-9b7a-1a97366c6916",
      "name": "my-boot-volume"
    }
  },
  "created_at": "2021-10-19T16:11:57Z",
  "crn": "crn:[...]",
  "disks": [],
  "id": "eb1b7391-2ca2-4ab5-84a8-b92157a633b0",
  "image": {
    "crn": "crn:[...]",
    "id": "9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366",
    "name": "my-image"
  },
  "memory": 8,
  "name": "my-instance",
  "network_interfaces": [
    {
      "id": "7ca88dfb-8962-469d-b1de-1dd56f4c3275",
      "name": "my-network-interface",
      "primary_ipv4_address": "10.0.0.32",
      "resource_type": "network_interface",
      "subnet": {
        "crn": "crn:[...]",
        "id": "bea6a632-5e13-42a4-b4b8-31dc877abfe4",
        "name": "my-subnet",
        "resource_type": "subnet"
      }
    }
  ],
  "primary_network_interface": {
    "id": "7ca88dfb-8962-469d-b1de-1dd56f4c3275",
    "name": "my-network-interface",
    "primary_ipv4_address": "10.0.0.32",
    "resource_type": "network_interface",
    "subnet": {
      "crn": "crn:[...]",
      "id": "bea6a632-5e13-42a4-b4b8-31dc877abfe4",
      "name": "my-subnet",
      "resource_type": "subnet"
    }
  },
  "profile": {
    "name": "bx2-2x8"
  },
  "resource_type": "instance",
  "vcpu": {
    "architecture": "amd64",
    "count": 2
  },
  "volume_attachments": [
    {
      "id": "a8a15363-a6f7-4f01-af60-715e85b28141",
      "name": "my-boot-volume-attachment",
      "volume": {
        "crn": "crn:[...]",
        "id": "49c5d61b-41e7-4c01-9b7a-1a97366c6916",
        "name": "my-boot-volume"
      }
    },
    {
      "id": "e77125cb-4df0-4988-a878-531ae0ae0b70",
      "name": "my-volume-attachment-1",
      "volume": {
        "crn": "crn:[...]",
        "id": "2cc091f5-4d46-48f3-99b7-3527ae3f4392",
        "name": "my-data-volume"
      }
    }
  ],
  "vpc": {
    "crn": "crn:[...]",
    "id": "f0aae929-7047-46d1-92e1-9102b07a7f6f",
    "name": "my-vpc",
    "resource_type": "vpc"
  },
  "zone": {
    "name": "us-south-1"
  }
}
```
{: codeblock}

### Extra instance metadata endpoints
{: #imd-additional-inst-metadata}

Table 1 shows more endpoints for API GET calls that you can make to get specific metadata for an instance. (The well-known URL is omitted in the list.)

| API endpoint | Description |
|--------------|-------------|
| `/metadata/v1/instance/network_interfaces` | List metadata for all network interfaces for an instance. |
| `/metadata/v1/instance/network_interfaces/{id}` | Retrieve metadata for a network interface by ID. |
| `/metadata/v1/instance/volume_attachments` | List metadata for all volume attachments for an instance. |
| `/metadata/v1/instance/volume_attachment/{id}` | Retrieve metadata for a volume attachment by ID. |
{: caption="Table 1. Instance metadata endpoints" caption-side="bottom"}

For more information about these APIs, including required parameters and examples, see the [Metadata service API reference guide](/apidocs/vpc-metadata).

## Retrieve metadata about SSH keys
{: #imd-retrieve-key-data}

Make a `GET "http://169.254.169.254/metadata/v1/keys"` call to retrieve information about all SSH keys configured for the instance.
The output is parsed by `jq`.

```curl
curl -X GET "http://169.254.169.254/metadata/v1/keys?version=2021-10-12"\
  -H "Accept:application/json"\
  -H "Authorization: Bearer $access_token" | jq -r
```
{: codeblock}

Example output shows one SSH key.

```json
{
  "keys": [
    {
      "created_at": "2021-10-19T16:39:23.000Z",
      "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::key:r006-44e5e06b-9450-4f5f-a9be-96feebf770d8",
      "fingerprint": "SHA256:lZmocJFsWfJcIl8Jdp8r6Ak8gzMqxrFb9UtwWCk27CM",
      "id": "r006-44e5e06b-9450-4f5f-a9be-96feebf770d8",
      "length": 4096,
      "name": "my-key1",
      "public_key": "ssh-rsa AAAAB...n",
      "resource_type": "key",
      "type": "rsa"
    }
  ]
}
```
{: codeblock}

If you have more than one SSH key, you can make a `GET "http://169.254.169.254/metadata/v1/keys/{id}"` call and provide the SSH key ID.

## Retrieve metadata about placement groups
{: #imd-retrieve-pg-data}

Make a `GET "http://169.254.169.254/metadata/v1/placement_groups"` call to retrieve information about placement groups configured for the instance. In the example, the return value of the cURL command is a list of placement groups, beginning with the first and up to 50. The output is parsed by `jq`.

```curl
curl -X GET "http://169.254.169.254/metadata/v1/placement_groups?version=2021-10-12"\
  -H "Accept:application/json"\
  -H "Authorization: Bearer $access_token"\
  --data-urlencode "version=2021-10-12"\
  -d '{
        "start": "first",
        "limit": 50
     }' | jq -r
```
{: codeblock}

Example return:

```json
{
  "first": {
    "href": "http://169.254.169.254/v1/placement_groups?limit=50"
  },
  "limit": 50,
  "placement_groups": [
    {
      "created_at": "2021-10-12T19:55:00Z",
      "crn": "crn:[...]",
      "id": "r018-418fe842-a3e9-47b9-a938-1aa5bd632871",
      "lifecycle_state": "stable",
      "name": "my-placement-group",
      "resource_type": "placement_group",
      "strategy": "host_spread"
    }
  ],
  "total_count": 1
}
```
{: codeblock}

You can also specify details for a single placement group that is used by the instance by making a `GET "http://169.254.169.254/metadata/v1/placement_groups/{id}"` call and providing the placement group ID.

## Next steps
{: #imd-next-steps-md}

* [Access metadata from a running instance](/docs/vpc?topic=vpc-imd-access-instance-metadata) and use it to bootstrap new instances.
* [Create and use a trusted profile](/docs/vpc?topic=vpc-imd-trusted-profile-metadata) to access IAM-enabled services.
