---

copyright:
  years: 2022, 2025
lastupdated: "2025-06-10"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Use the instance metadata service
{: #imd-get-metadata}

After you obtained an instance identity access token, you can access the metadata service and retrieve metadata about a virtual server instance. Instance metadata includes information such as initialization data, network interfaces, volume attachments, public SSH keys, and placement groups.
{: shortdesc}

When you make API requests to the instance metadata service, events are triggered that can be used for auditing. For more information, see [Instance metadata service events](/docs/vpc?topic=vpc-at_events#events-metadata).

## Before you begin
{: #imd-md-prereqs}

The metadata service is disabled by default. To enable it, follow the steps in the [Enable the metadata service](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-metadata-service-enable) topic.

Windows users have extra requirements to access and use the metadata service. For more information, see [Setting up windows servers for using the metadata service](/docs/vpc?topic=vpc-imd-windows-configuration).
{: note}

## Accessing the metadata service to create an instance identity access token
{: #imd-get-token-usemd}

To access the instance metadata service, you must first obtain an instance identity access token. Make a `PUT /instance_identity/v1/token` request to the [Metadata service API](/apidocs/vpc-metadata#create-access-token) from the virtual server.

Communication between the instance and the metadata service never leaves the host. You acquire the token from within the instance. If secure access to the instance metadata service is enabled on your instance, use the "https" protocol instead of the "http" protocol.
{: important}

In the request, you can specify an expiration time for the token. The default expiration value is 5 minutes, but you can specify any value between 5 seconds to 1 hour.

```sh
curl -X PUT "http://api.metadata.cloud.ibm.com/instance_identity/v1/token?version=2025-04-22" -H "Metadata-Flavor: ibm" -d '{"expires_in": 3600}'
```
{: pre}

The API response contains the instance identity access token. It shows the instance identity access token character string, date, and time that it was created, date, and time that it expires, and expiration time you set. You can use this token to access the metadata service and to generate IAM tokens that your application can use to access IAM-enabled services. For more information, see [Generate an IAM token from an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-token-exchange).

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IlZTSS1DUl91cy1lYXN0X2I5...",
  "created_at": "2025-04-22T11:08:39.363Z",
  "expires_at": "2025-04-22T11:13:39.363Z",
  "expires_in": 3600
}
```
{: codeblock}

You can create an environment variable to be used in the next request to create an IAM token. See the following example. 

```json
instance_identity_token=`curl -X PUT "http://api.metadata.cloud.ibm.com/instance_identity/v1/token?version=2024-11-12" -H "Metadata-Flavor: ibm" -d '{"expires_in": 3600}' | jq -r '(.instance_identity_token)'`
```
{: pre}

The example uses `jq` as a parser, a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/). `jq` might not come preinstalled on all VPC images available when you create an instance. You might need to install `jq` before use or use any other parser of your choice.
{: note}

## Generating an IAM token from an instance identity access token
{: #imd-token-exchange-usemd}
{: api}

To access IBM Cloud IAM-enabled services in the account, you can generate an IAM token from the instance identity access token by using trusted profile information. You can use [trusted profiles](/docs/account?topic=account-create-trusted-profile) for compute resource identities to assign an {{site.data.keyword.cloud}} IAM identity to a virtual server instance.

After you generate the IAM token, you can use it to access IAM-enabled services without having to manage and distribute IAM secrets to the instance. You can reuse the token multiple times. The trusted profile can be linked either when you create the instance or provided in the request body.

Make a `POST /instance_identity/v1/iam_token` request and specify the ID of the trusted profile. See the following example request:

```json
iam_token=`curl -X POST "$vpc_metadata_api_endpoint/instance_identity/v1/iam_token?version=2025-04-22" -H "Authorization: Bearer $instance_identity_token" -d '{
      "trusted_profile": {
        "id": "Profile-8dd84246-7df4-4667-94e4-8cede51d5ac5"
      }
    }' | jq -r '(.iam_token)'`
```
{: pre}

The API response shows the IAM token.

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0aGVfYmVzdCI6I8...",
  "created_at": "2025-04-22T14:10:15Z",
  "expires_at": "2025-04-22T15:10:15Z",
  "expires_in": 3600
}
```
{: codeblock}

For more information, see [Using a trusted profile to request IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).

## Retrieving instance metadata from your running virtual server instance
{: #imd-retrieve-instance-data}

Use the instance metadata service to get metadata about the instance, initialization information, SSH keys, and placement groups.

### Retrieving instance information
{: #imd-retrieve-instance}

Make a `GET "/metadata/v1/instance"` request to retrieve detailed information about the instance.

```sh
curl -X GET "$vpc_metadata_api_endpoint/metadata/v1/instance?version=2025-04-22" -H "Authorization: Bearer $instance_identity_token"
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

### Retrieving instance initialization information
{: #imd-retrieve-initialization-data}

You can retrieve initialization information for the instance programmatically by making a `GET /metadata/v1/instance/initialization` request. See the following example.

```sh
curl -X GET "$vpc_metadata_api_endpoint/metadata/v1/instance/initialization?version=2025-04-22" -H "Authorization: Bearer $instance_identity_token" | jq -r '(.user_data)'
```
{: codeblock}

In the example, the returned value of the command is the user data, which is extracted by `jq` and placed in the `user_data` environment variable.

The response lists public SSH keys that were used at instance initialization and extracts any other user data that was made available when the instance was provisioned. The response includes the SHA256 value that identifies the SSH key.

```json
{
  "keys": [
    {
      "crn": "crn:[...]",
      "fingerprint": "SHA256:RJ+YWs3rupwGFiJuLqY43tvmdeLOUjcIc9cA6IR8n8E",
      "id": "dadae729-1d81-4a13-966e-f5d92699f103",
      "name": "my-key-1",
      "resource_type": "key"
    }
  ],
  "user_data": "Content-Type: multipart/form-data; boundary=3efa30189c9e0e8ebc24a4decbbf4c2be7b26120c1cdd7cb7bc2ecb0c07c\nMIME-Version: 1.0\n\n--3efa30189c9e0e8ebc24a4decbbf4c2be7b26120c1cdd7cb7bc2ecb0c07c\nContent-Type: text/cloud-config\n\n#cloud-config\ndisable_root: false\nchpasswd:\n  list: |\n    root:genes1s\n  expire: false\nusers:\n- default\n- name: root\n  lock-passwd: false\n  ssh_pwauth: true\n\n--3efa30189c9e0e8ebc24a4decbbf4c2be7b26120c1cdd7cb7bc2ecb0c07c--"
}
```
{: screen}

For more information, see the API reference: [Retrieve initialization information](/apidocs/vpc-metadata#get-instance-initialization).

### Other instance metadata methods
{: #imd-additional-inst-metadata}

The following table shows more methods for API GET requests that you can make to get specific information about the instance.

| API endpoint | Description |
|--------------|-------------|
| `/metadata/v1/instance/cluster_network_attachments`| List cluster network attachments |
| `/metadata/v1/instance/cluster_network_attachments/{id}` | Retrieve a cluster network attachment |
| `/metadata/v1/instance/network_attachments` | List network attachments |
| `/metadata/v1/instance/network_attachments/{id}`| Retrieve a network attachment |
| `/metadata/v1/instance/network_interfaces` | List metadata for all network interfaces for an instance. |
| `/metadata/v1/instance/network_interfaces/{id}` | Retrieve metadata for a network interface by ID. |
| `/metadata/v1/instance/volume_attachments` | List metadata for all volume attachments for an instance. |
| `/metadata/v1/instance/volume_attachment/{id}` | Retrieve metadata for a volume attachment by ID. |
{: caption="Instance metadata endpoints" caption-side="bottom"}

For more information about required parameters and examples, see the [Metadata service API reference guide](/apidocs/vpc-metadata).

## Retrieving metadata about SSH keys
{: #imd-retrieve-key-data}

Make a `GET /metadata/v1/keys` request to retrieve information about all the SSH keys that are configured for the instance.

```sh
curl -X GET "$vpc_metadata_api_endpoint/metadata/v1/keys?version=2025-04-22" -H "Authorization: Bearer $instance_identity_token"
```
{: codeblock}

The example output shows one SSH key.

```json
{
  "keys": [
    {
      "created_at": "2024-10-19T16:39:23.000Z",
      "crn": "crn:bluemix:public:is:us-south:a/a1234567::key:r006-44e5e06b-9450-4f5f-a9be-96feebf770d8",
      "fingerprint": "SHA256:lZmocJFsWfJcIl8Jdp8r6Ak8gzMqxrFb9UtwWCk27CM",
      "id": "r006-44e5e06b-9450-4f5f-a9be-96feebf770d8",
      "length": 4096,
      "name": "my-key-1",
      "public_key": "ssh-rsa AAAAB...n",
      "resource_type": "key",
      "type": "rsa"
    }
  ]
}
```
{: codeblock}

If you have more than one SSH key, you can make a `GET /metadata/v1/keys/{id}` request with the ID of one of the SSH keys. The response provides information about the specified SSH key.

## Retrieving metadata about placement groups
{: #imd-retrieve-pg-data}

Make a `GET /metadata/v1/placement_groups` request to retrieve information about placement groups configured for the instance. In the example, the return value of the command is a list of placement groups, beginning with the first and up to 50.

```sh
curl -X GET "$vpc_metadata_api_endpoint/metadata/v1/placement_groups?version=2025-04-22" -H "Authorization: Bearer $instance_identity_token" -d '{"start": "first","limit": 50}'
```
{: codeblock}

The example output shows only one placement group.

```json
{
  "first": {
    "href": "http://169.254.169.254/v1/placement_groups?limit=50"
  },
  "limit": 50,
  "placement_groups": [
    {
      "created_at": "2022-10-12T19:55:00Z",
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

You can also request details for a specific placement group that is used by the instance. To retrieve information of a specific placement group, make a `GET "/metadata/v1/placement_groups/{id}"` request and replace the `{id}` with actual ID of the placement group.

## Next steps
{: #imd-next-steps-md}

* [Access metadata from a running instance](/docs/vpc?topic=vpc-imd-access-instance-metadata) and use it to bootstrap new instances.
* [Create and use a trusted profile](/docs/vpc?topic=vpc-imd-trusted-profile-metadata) to access IAM-enabled services.
