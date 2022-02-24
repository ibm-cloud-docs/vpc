---

copyright:
  years: 2022
lastupdated: "2022-02-24"

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
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Use the instance metadata service
{: #imd-get-metadata}

After obtaining an instance identity access token, you can access the metadata service and retrieve metadata about a virtual server instance. This topic describes how to make calls to the API to access instance metadata such as initialization data, network interfaces, volume attachments, public SSH keys, and placement groups.
{: shortdesc}

When you make API calls to the instance metadata service, events are triggered in the Activity Tracker. For more information, see [Instance Metadata service events](/docs/vpc?topic=vpc-at-events#events-metadata).

## Before you begin
{: #imd-md-prereqs}

To access metadata service, you must have an instance identity access token. If you haven't already obtained one, see [Aquire an access token](/docs/vpc?topic=vpc-imd-configure-service#imd-json-token).

The metadata service is disabled by default. To enable it, see [Enable or disable the metadata service](docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-metadata-service-enable).

For detailed information and examples of the API calls described in this topic, see the [Metadata service Beta API reference](/apidocs/vpc-metadata-beta).

Windows users have additional requirements to access and use the metadata service. For information, see [Setting up windows servers for using the metadata service](/docs/vpc?topic=vpc-imd-windows-configuration).
{: note}

## Retrieve instance metadata from your running virtual server instance
{: #imd-retrieve-instance-data}

Use the instance metadata service to get metadata about the instance, initialization information, SSH keys, and placement groups.

### Retrieve instance initialization information
{: #imd-retrieve-initialization-data}

Make a `GET "http://169.254.169.254/metadata/v1/instance/initialization"` call to retrieve initialization information for the instance.

This request:

* Invokes the API to retrieve the access token
* Uses the token to access the metadata service 
* Extracts the user data from the JSON payload using `jq`

You're making an unsecured request that is then secured by a proxy. The proxy intercepts the request by watching for the instance IP.
{: note}

In the example, the return value of the cURL command is the user data, which is extracted by `jq` and placed in the `user_data` evironment variable.

```
curl -X GET "http://169.254.169.254/metadata/v1/instance/initialization?version=2021-10-12"\
  -H "Accept: application/json"\
  -H "Authorization: Bearer $access_token" | jq -r
```
{: pre}

The response lists public SSH keys used at instance initialization and extracts any other user data made available when provisioning the instance. The response includes the SHA256 value which identifies the SSH key.

```
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
{: code_block}

### Retrieve instance information
{: imd-retrieve-instance}

Make a `GET "http://169.254.169.254/metadata/v1/instance"` call to retrieve detailed information about the instance. To tailor information for specific instance details, such as network interfaces, see [Additional instance metadata](#imd-additional-inst-metadata).


```
curl -X GET "http://169.254.169.254/metadata/v1/instance?version=2021-10-12"\
  -H "Accept:application/json"\
  -H "Authorization: Bearer $access_token"
```
{: pre}

The response lists all details for an instance, including network interfaces, compute profile, and volume attachments.

```
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

### Additional instance metadata endpoints
{: #imd-additional-inst-metadata}

Table 1 shows additional endpoints for API GET calls that you can make to get specific metadata for an instance. (The well-known URL is omitted in the list.)

| API endpoint | Description |
|--------------|-------------|
| /metadata/v1/instance/network_interfaces | List metadata for all network interfaces for an instance. |
| /metadata/v1/instance/network_interfaces/{id} | Retrieve metadata for a network interface by ID. |
| /metadata/v1/instance/volume_attachments | List metadata for all volume attachments for an instance. |
| /metadata/v1/instance/volume_attachment/{id} | Retrieve metadata for a volume attachment by ID. |
{: caption="Table 1. Instance metadata endpoints" caption-side="bottom"}

For more information about these APIs, including required parameters and examples, see the [Metadata service Beta API reference guide](/apidocs/vpc-metadata-beta).

## Retrieve metadata about SSH keys
{: #imd-retrieve-key-data}

Make a `GET "http://169.254.169.254/metadata/v1/keys"` call to retrieve information about all SSH keys configured for the instance.
The output is parsed by `jq`.

```
curl -X GET "http://169.254.169.254/metadata/v1/keys?version=2021-10-12"\
  -H "Accept:application/json"\
  -H "Authorization: Bearer $access_token" | jq -r
```
{: pre}

Example output, showing one SSH key:

```
{
  "keys": [
    {
      "created_at": "2021-10-19T16:39:23.000Z",
      "crn": "crn:v1:staging:public:is:us-south:a/2c91d5e9ecee442189fb07cd7a8776c4::key:r134-44e5e06b-9450-4f5f-a9be-96feebf770d8",
      "fingerprint": "SHA256:lZmocJFsWfJcIl8Jdp8r6Ak8gzMqxrFb9UtwWCk27CM",
      "id": "r134-44e5e06b-9450-4f5f-a9be-96feebf770d8",
      "length": 4096,
      "name": "my-key1",
      "public_key": "ssh-rsa AAAAB...n",
      "resource_type": "key",
      "type": "rsa"
    }
  ]
}
```
{: code_block}

If you have more than one SSH key, you can make a `GET "http://169.254.169.254/metadata/v1/keys/{id}"` call and provide the SSH key ID.

## Retrieve metadata about placement groups
{: #imd-retrieve-pg-data}

Make a `GET "http://169.254.169.254/metadata/v1/placement_groups"` call to retrieve information about placement groups configured for the instance. In the example, the return value of the cURL command is a list of placement groups, beginning with the first and up to 50. The output is parsed by `jq`. 

```
curl -X GET "http://169.254.169.254/metadata/v1/placement_groups?version=2021-10-12"\
  -H "Accept:application/json"\
  -H "Authorization: Bearer $access_token"\
  --data-urlencode "version=2021-10-12"\
  -d '{ 
        "start": "first",
        "limit": 50 
     }' | jq -r
```
{: code_block}

Example return:

```
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

You can also specify details for a single placement group used by the instance by making a `GET "http://169.254.169.254/metadata/v1/placement_groups/{id}"` call and providing the placement group ID.

## Next steps
{: #imd-next-steps-md}

* [Access metadata from a running instance](/docs/vpc?topic=vpc-imd-access-instance-metadata) and use it to bootstrap new instances.
* [Create and use a trusted profile](/docs/vpc?topic=vpc-imd-trusted-profile-metadata) to access IAM-enabled services.
