---

copyright:
  years: 2025, 2026
lastupdated: "2025-07-08"

keywords: image, VPC, API, virtual private cloud, support

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to the `2025-06-30` version (image ownership property and filter change)
{: #2025-06-30-image-ownership-property-change}

As described in the VPC API reference [versioning](/apidocs/vpc/latest#api-versioning) policy, most changes to the VPC APIs are fully backward compatible and are made available to all clients, regardless of the API version the client requests. However, the `2025-06-30` release of the VPC API necessitated incompatible changes in support of improved modeling of the ownership property of image resources.

Before you adopt the release version `2025-06-30` or later, review the changes described in this migration guidance that might require you to update your client.

## Changed image properties
{: #changed-properties-image-api-version-2025-06-30}
  
The following properties have changed for API requests that use a `version` query parameter of `2025-06-30` or later.

When [retrieving](/apidocs/vpc/2025-06-30#get-image), [listing](/apidocs/vpc/2025-06-30#list-images), [creating](/apidocs/vpc/2025-06-30#create-image), or [updating](/apidocs/vpc/2025-06-30#update-image) images, the following resulting properties have changed for API requests that use a `version` query parameter of `2025-06-30` or later.

| Old property           |        New property      |
|------------------------|--------------------------|
|   `owner_type`         | `remote.account`         |
{: caption="Old and new properties resulting when retrieving, listing, creating, or updating images." caption-side="bottom"}

When [listing](/apidocs/vpc/2025-06-30#list-images) images, the following query parameters have changed for API requests that use a `version` query parameter of `2025-06-30` or later.

| Old parameter          |        New parameter     |
|------------------------|--------------------------|
|   `owner_type`         | `remote.account.id`      |
{: caption="Old and new query parameters when retrieving images." caption-side="bottom"}

## Action needed
{: #action-needed-image-api-version-2025-06-30}

Before you specify the `version` query parameter of `2025-06-30` or later, follow these actions to avoid regressions in client functionality.

If your clients continue to specify version `2025-06-29` or earlier, no changes are required.
{: note}

### Client migration
{: #client-migration-image-api-version-2025-06-30}

Before you migrate a client to API version `2025-06-30` or later, review your code that uses the following methods:

- `POST /images`
- `GET /images/{id}`
- `GET /images`
- `PATCH /images/{id}`

Review the changes that were announced in the [API change log](/docs/vpc?topic=vpc-api-change-log#version-2025-06-30), and verify that your code adopts these changes in a manner that is appropriate for your programming language.

## Examples
{: #example-image-api-version-2025-06-30}

These examples compare differences between before and after the `2025-06-30` versioned change.

### Properties of images owned by the requesting account
{: #example-properties-images-owned-by-requesting-account}

The following example image is from a response to a request made using API version `2025-06-29` or earlier.  The image is owned by the requesting account, so it includes the `owner_type` property set to `user`.

```sh
curl -X GET "$vpc_api_endpoint/v1/images/$image_id?version=2025-06-29&generation=2" -H "Authorization: $iam_token"
```
{: pre}

```json
{
  "id": "r006-426418bb-a89c-42fa-9ece-ac2d1b73ebe6",
  "crn": "crn:v1:bluemix:public:is:us-south:a/3f728115e2294567898f7212ed0b1418::image:r006-426418bb-a89c-42fa-9ece-ac2d1b73ebe6",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/images/r006-426418bb-a89c-42fa-9ece-ac2d1b73ebe6",
  "name": "my-image",
  "minimum_provisioned_size": 100,
  "resource_group": {
    "id": "111fdd0ccab94cdeb0ef8a41183e132a",
    "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/111fdd0ccab94cdeb0ef8a41183e132a",
    "name": "Default"
  },
  "created_at": "2025-05-28T10:00:14Z",
  "file": {
    "checksums": {
      "sha256": "ff3c40900eb47230a89533c5feb4e090c6328b7b41b308b64242cab475b60121"
    },
    "size": 2
  },
  "operating_system": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/red-8-amd64",
    "name": "red-8-amd64",
    "architecture": "amd64",
    "display_name": "Red Hat Enterprise Linux 8.x - Minimal Install (amd64)",
    "family": "Red Hat Enterprise Linux",
    "vendor": "Red Hat",
    "version": "8.x - Minimal Install",
    "dedicated_host_only": false,
    "allow_user_image_creation": true,
    "user_data_format": "cloud_init"
  },
  "status": "available",
  "visibility": "private",
  "encryption": "none",
  "status_reasons": [],
  "owner_type": "user",
  "catalog_offering": {
    "managed": false
  },
  "resource_type": "image",
  "user_data_format": "cloud_init",
  "allowed_use": {
    "instance": "true",
    "bare_metal_server": "true",
    "api_version": "2025-05-15"
  }
}
```
{: pre}

The following example image is from a response to a request made using API version `2025-06-30` or later.  The image is owned by the requesting account, so it omits `remote.account` because the owning account is not remote.

```sh
curl -X GET "$vpc_api_endpoint/v1/images/$image_id?version=2025-06-30&generation=2" -H "Authorization: $iam_token"
```
{: pre}

```json
{
  "id": "r006-426418bb-a89c-42fa-9ece-ac2d1b73ebe6",
  "crn": "crn:v1:bluemix:public:is:us-south:a/3f728115e2294567898f7212ed0b1418::image:r006-426418bb-a89c-42fa-9ece-ac2d1b73ebe6",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/images/r006-426418bb-a89c-42fa-9ece-ac2d1b73ebe6",
  "name": "my-image",
  "minimum_provisioned_size": 100,
  "resource_group": {
    "id": "111fdd07fab94cdeb0ef8a47b83e132a",
    "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/111fdd0ccab94cdeb0ef8a47b83e132a",
    "name": "Default"
  },
  "created_at": "2025-05-28T10:00:14Z",
  "file": {
    "checksums": {
      "sha256": "ff3c40900eb47230a89533c5feb4e090c6328b7b41b308b64242cab475b60121"
    },
    "size": 2
  },
  "operating_system": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/red-8-amd64",
    "name": "red-8-amd64",
    "architecture": "amd64",
    "display_name": "Red Hat Enterprise Linux 8.x - Minimal Install (amd64)",
    "family": "Red Hat Enterprise Linux",
    "vendor": "Red Hat",
    "version": "8.x - Minimal Install",
    "dedicated_host_only": false,
    "allow_user_image_creation": true,
    "user_data_format": "cloud_init"
  },
  "status": "available",
  "visibility": "private",
  "encryption": "none",
  "status_reasons": [],
  "catalog_offering": {
    "managed": false
  },
  "resource_type": "image",
  "user_data_format": "cloud_init",
  "allowed_use": {
    "instance": "true",
    "bare_metal_server": "true",
    "api_version": "2025-05-15"
  }
}
```
{: pre}

### Properties of images not owned by the requesting account
{: #example-properties-images-not-owned-by-requesting-account}

The following example image is from a response to a request made using API version `2025-06-29` or earlier.  The image is not owned by the requesting account, so it includes the `owner_type` property set to `provider`.

```sh
curl -X GET "$vpc_api_endpoint/v1/images/$image_id?version=2025-06-29&generation=2" -H "Authorization: $iam_token"
```
{: pre}

```json
{
  "id": "r006-6795c5a0-d10f-4a89-aa93-a911e90f6cb8",
  "crn": "crn:v1:bluemix:public:is:us-south:a/811f8abfbd32425597dc7ba40da98fa6::image:r006-6795c5a0-d10f-4a89-aa93-a911e90f6cb8",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/images/r006-6795c5a0-d10f-4a89-aa93-a911e90f6cb8",
  "name": "ibm-redhat-8-8-minimal-amd64-10",
  "minimum_provisioned_size": 100,
  "resource_group": {
    "id": "4cdc4448a0a148159685b9b0dc54ed19",
    "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/4cdc4448a0a148159685b9b0dc54ed19",
    "name": "Default"
  },
  "created_at": "2025-04-11T04:35:45Z",
  "file": {
    "checksums": {
      "sha256": "5c356e409f6fea99afd93477c1c54a78e118e454c993874339884e9096978fe5"
    },
    "size": 2
  },
  "operating_system": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/red-8-amd64",
    "name": "red-8-amd64",
    "architecture": "amd64",
    "display_name": "Red Hat Enterprise Linux 8.x - Minimal Install (amd64)",
    "family": "Red Hat Enterprise Linux",
    "vendor": "Red Hat",
    "version": "8.x - Minimal Install",
    "dedicated_host_only": false,
    "allow_user_image_creation": true,
    "user_data_format": "cloud_init"
  },
  "status": "available",
  "visibility": "public",
  "encryption": "none",
  "status_reasons": [],
  "owner_type": "provider",
  "catalog_offering": {
    "managed": false
  },
  "resource_type": "image",
  "user_data_format": "cloud_init",
  "allowed_use": {
    "instance": "true",
    "bare_metal_server": "true",
    "api_version": "2025-04-11"
  }
}
```
{: pre}

The following example image is from a response to a request made using API version `2025-06-30` or later.  The image is not owned by the requesting account, so it includes the `remote.account` property with `remote.account.id` set to the account identifier of the image owner.

```sh
curl -X GET "$vpc_api_endpoint/v1/images/$image_id?version=2025-06-30&generation=2" -H "Authorization: $iam_token"
```
{: pre}

```json
{
  "id": "r006-6795c5a0-d10f-4a89-aa93-a911e90f6cb8",
  "crn": "crn:v1:bluemix:public:is:us-south:a/811f8abfbd32425597dc7ba40da98fa6::image:r006-6795c5a0-d10f-4a89-aa93-a911e90f6cb8",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/images/r006-6795c5a0-d10f-4a89-aa93-a911e90f6cb8",
  "name": "ibm-redhat-8-8-minimal-amd64-10",
  "minimum_provisioned_size": 100,
  "resource_group": {
    "id": "4cdc4448a0a148159685b9b0dc549d19",
    "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/4cdc4448a0a148159685b9b0dc549d19",
    "name": "Default"
  },
  "created_at": "2025-04-11T04:35:45Z",
  "file": {
    "checksums": {
      "sha256": "5c356e455f6fea99afd93477c1c54a78e118e454c993874339884e9096978fe5"
    },
    "size": 2
  },
  "operating_system": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/red-8-amd64",
    "name": "red-8-amd64",
    "architecture": "amd64",
    "display_name": "Red Hat Enterprise Linux 8.x - Minimal Install (amd64)",
    "family": "Red Hat Enterprise Linux",
    "vendor": "Red Hat",
    "version": "8.x - Minimal Install",
    "dedicated_host_only": false,
    "allow_user_image_creation": true,
    "user_data_format": "cloud_init"
  },
  "status": "available",
  "visibility": "public",
  "encryption": "none",
  "status_reasons": [],
  "catalog_offering": {
    "managed": false
  },
  "resource_type": "image",
  "user_data_format": "cloud_init",
  "allowed_use": {
    "instance": "true",
    "bare_metal_server": "true",
    "api_version": "2025-04-11"
  },
  "remote": {
    "account": {
      "id": "811f8abfbd32425597dc7ba40da98fa6",
      "resource_type": "image"
    }
  }
}
```
{: pre}

### Listing images owned by the requesting account
{: #example-listing-images-owned-by-requesting-account}

The following example uses API version `2025-06-29` or earlier and query parameter `owner_type=user` to list images owned by the requesting account.

```sh
curl -X GET "$vpc_api_endpoint/v1/images?version=2025-06-29&generation=2&owner_type=user" -H "Authorization: $iam_token"
```
{: pre}

The response includes only images with `owner_type` set to `user`:

```json
{
  "first": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/images?limit=50"
  },
  "limit": 50,
  "total_count": 1,
  "images": [
    {
      "id": "r006-426418bb-a89c-42fa-9ece-ac2d1b73ebe6",
      "crn": "crn:v1:bluemix:public:is:us-south:a/3f728115e2294567898f7212ed0b1418::image:r006-426418bb-a89c-42fa-9ece-ac2d1b73ebe6",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/images/r006-426418bb-a89c-42fa-9ece-ac2d1b73ebe6",
      "name": "my-image",
      "minimum_provisioned_size": 100,
      "resource_group": {
        "id": "111fdd07fab94cdeb0ef8a47b83e132a",
        "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/111fdd0ccab94cdeb0ef8a47b83e132a",
        "name": "Default"
      },
      "created_at": "2025-05-28T10:00:14Z",
      "file": {
        "checksums": {
          "sha256": "ff3c40900eb47230a89533c5feb4e090c6328b7b41b308b64242cab475b60121"
        },
        "size": 2
      },
      "operating_system": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/red-8-amd64",
        "name": "red-8-amd64",
        "architecture": "amd64",
        "display_name": "Red Hat Enterprise Linux 8.x - Minimal Install (amd64)",
        "family": "Red Hat Enterprise Linux",
        "vendor": "Red Hat",
        "version": "8.x - Minimal Install",
        "dedicated_host_only": false,
        "allow_user_image_creation": true,
        "user_data_format": "cloud_init"
      },
      "status": "available",
      "visibility": "private",
      "encryption": "none",
      "status_reasons": [],
      "owner_type": "user",
      "catalog_offering": {
        "managed": false
      },
      "resource_type": "image",
      "user_data_format": "cloud_init",
      "allowed_use": {
        "instance": "true",
        "bare_metal_server": "true",
        "api_version": "2025-05-15"
      }
    }
  ]
}
```
{: pre}

The following example uses API version `2025-06-30` or later and query parameter `remote.account.id=null` to list images owned by the requesting account.

```sh
curl -X GET "$vpc_api_endpoint/v1/images?version=2025-06-30&generation=2&remote.account.id=null" -H "Authorization: $iam_token"
```
{: pre}

The response includes only images without `remote.account` set because the owning account is not remote:

```json
{
  "first": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/images?limit=50"
  },
  "limit": 50,
  "total_count": 1,
  "images": [
    {
      "id": "r006-426418bb-a89c-42fa-9ece-ac2d1b73ebe6",
      "crn": "crn:v1:bluemix:public:is:us-south:a/3f728115e2294567898f7212ed0b1418::image:r006-426418bb-a89c-42fa-9ece-ac2d1b73ebe6",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/images/r006-426418bb-a89c-42fa-9ece-ac2d1b73ebe6",
      "name": "my-image",
      "minimum_provisioned_size": 100,
      "resource_group": {
        "id": "111fdd07fab94cdeb0ef8a47b83e132a",
        "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/111fdd0ccab94cdeb0ef8a47b83e132a",
        "name": "Default"
      },
      "created_at": "2025-05-28T10:00:14Z",
      "file": {
        "checksums": {
          "sha256": "ff3c40900eb47230a89533c5feb4e090c6328b7b41b308b64242cab475b60121"
        },
        "size": 2
      },
      "operating_system": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/red-8-amd64",
        "name": "red-8-amd64",
        "architecture": "amd64",
        "display_name": "Red Hat Enterprise Linux 8.x - Minimal Install (amd64)",
        "family": "Red Hat Enterprise Linux",
        "vendor": "Red Hat",
        "version": "8.x - Minimal Install",
        "dedicated_host_only": false,
        "allow_user_image_creation": true,
        "user_data_format": "cloud_init"
      },
      "status": "available",
      "visibility": "private",
      "encryption": "none",
      "status_reasons": [],
      "catalog_offering": {
        "managed": false
      },
      "resource_type": "image",
      "user_data_format": "cloud_init",
      "allowed_use": {
        "instance": "true",
        "bare_metal_server": "true",
        "api_version": "2025-05-15"
      }
    }
  ]
}
```
{: pre}

### Listing images not owned by the requesting account
{: #example-listing-images-not-owned-by-requesting-account}

The following example uses API version `2025-06-29` or earlier and query parameter `owner_type=provider` to list images not owned by the requesting account.

```sh
curl -X GET "$vpc_api_endpoint/v1/images?version=2025-06-29&generation=2&owner_type=provider" -H "Authorization: $iam_token"
```
{: pre}

The response includes only images with `owner_type` set to `provider`:

```json
{
  "first": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/images?limit=50"
  },
  "limit": 50,
  "total_count": 1,
  "images": [
    {
      "id": "r006-6795c5a0-d10f-4a89-aa93-a911e90f6cb8",
      "crn": "crn:v1:bluemix:public:is:us-south:a/811f8abfbd32425597dc7ba40da98fa6::image:r006-6795c5a0-d10f-4a89-aa93-a911e90f6cb8",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/images/r006-6795c5a0-d10f-4a89-aa93-a911e90f6cb8",
      "name": "ibm-redhat-8-8-minimal-amd64-10",
      "minimum_provisioned_size": 100,
      "resource_group": {
        "id": "4cdc4448a0a148159685b9b0dc54ed19",
        "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/4cdc4448a0a148159685b9b0dc54ed19",
        "name": "Default"
      },
      "created_at": "2025-04-11T04:35:45Z",
      "file": {
        "checksums": {
          "sha256": "5c356e409f6fea99afd93477c1c54a78e118e454c993874339884e9096978fe5"
        },
        "size": 2
      },
      "operating_system": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/red-8-amd64",
        "name": "red-8-amd64",
        "architecture": "amd64",
        "display_name": "Red Hat Enterprise Linux 8.x - Minimal Install (amd64)",
        "family": "Red Hat Enterprise Linux",
        "vendor": "Red Hat",
        "version": "8.x - Minimal Install",
        "dedicated_host_only": false,
        "allow_user_image_creation": true,
        "user_data_format": "cloud_init"
      },
      "status": "available",
      "visibility": "public",
      "encryption": "none",
      "status_reasons": [],
      "owner_type": "provider",
      "catalog_offering": {
        "managed": false
      },
      "resource_type": "image",
      "user_data_format": "cloud_init",
      "allowed_use": {
        "instance": "true",
        "bare_metal_server": "true",
        "api_version": "2025-04-11"
      }
    }
  ]
}
```
{: pre}

The following example uses API version `2025-06-30` or later and query parameter `remote.account.id=not:null` to list images not owned by the requesting account.

```sh
curl -X GET "$vpc_api_endpoint/v1/images?version=2025-06-30&generation=2&remote.account.id=not:null" -H "Authorization: $iam_token"
```
{: pre}

The response includes only images with `remote.account.id` set because the owning account is remote:

```json
{
  "first": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/images?limit=50"
  },
  "limit": 50,
  "total_count": 1,
  "images": [
    {
      "id": "r006-6795c5a0-d10f-4a89-aa93-a911e90f6cb8",
      "crn": "crn:v1:bluemix:public:is:us-south:a/811f8abfbd32425597dc7ba40da98fa6::image:r006-6795c5a0-d10f-4a89-aa93-a911e90f6cb8",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/images/r006-6795c5a0-d10f-4a89-aa93-a911e90f6cb8",
      "name": "ibm-redhat-8-8-minimal-amd64-10",
      "minimum_provisioned_size": 100,
      "resource_group": {
        "id": "4cdc4448a0a148159685b9b0dc549d19",
        "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/4cdc4448a0a148159685b9b0dc549d19",
        "name": "Default"
      },
      "created_at": "2025-04-11T04:35:45Z",
      "file": {
        "checksums": {
          "sha256": "5c356e455f6fea99afd93477c1c54a78e118e454c993874339884e9096978fe5"
        },
        "size": 2
      },
      "operating_system": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/red-8-amd64",
        "name": "red-8-amd64",
        "architecture": "amd64",
        "display_name": "Red Hat Enterprise Linux 8.x - Minimal Install (amd64)",
        "family": "Red Hat Enterprise Linux",
        "vendor": "Red Hat",
        "version": "8.x - Minimal Install",
        "dedicated_host_only": false,
        "allow_user_image_creation": true,
        "user_data_format": "cloud_init"
      },
      "status": "available",
      "visibility": "public",
      "encryption": "none",
      "status_reasons": [],
      "catalog_offering": {
        "managed": false
      },
      "resource_type": "image",
      "user_data_format": "cloud_init",
      "allowed_use": {
        "instance": "true",
        "bare_metal_server": "true",
        "api_version": "2025-04-11"
      },
      "remote": {
        "account": {
          "id": "811f8abfbd32425597dc7ba40da98fa6",
          "resource_type": "image"
        }
      }
    }
  ]
}
```
{: pre}
