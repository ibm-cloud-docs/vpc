---

copyright:
  years: 2023
lastupdated: "2023-12-05"

keywords: Backup for VPC, api migration, versioned change

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to the `2023-12-05` version (backup policies)
{: #2023-12-05-migration-backup-policy}

As described in the [VPC API](/apidocs/vpc/latest) reference [versioning](/apidocs/vpc#api-versioning) policy, most changes to the VPC APIs are fully backward compatible and are made available to all clients, regardless of the API version the client requests. However, the 2023-12-05 release of the VPC API necessitated incompatible changes in support of backup policy methods.

Before you adopt the release version `2023-12-05` or later, be aware of the following changes that might require you to update your client:

- Backup policy methods supports only one type of resource for a backup policy. In version `2023-12-05`, the property `match_resource_types` changed to `match_resource_type`. This change applies when [creating](/apidocs/vpc/latest#list-backup-policies), [updating](/apidocs/vpc/latest#update-backup-policy), [listing](/apidocs/vpc/latest#list-backup-policies), [retrieving](/apidocs/vpc/latest#get-backup-policy), and [deleting](/apidocs/vpc/latest#delete-backup-policy) a backup policy.
- When initiating a request to [create a backup policy](/apidocs/vpc/latest#create-backup-policy), you must provide a value for the `match_resource_type` property, either `volume` or `instance`. When `volume` is selected the backup policy is applied to individual {{site.data.keyword.block_storage_is_short}} volumes separately. When `instance` is specified, the {{site.data.keyword.block_storage_is_short}} volumes that are attached to the same instances are treated as a consistency group and the backup snapshots of the volumes are created together in a loose-knit group.

## Action needed
{: #action-needed-backup-policy}

Before specifying `version` query parameter of `2023-12-05` or later, follow these actions to avoid regressions in client functionality.

If your clients continue to specify version `2023-12-04` or earlier, no changes are required.

### Client migration
{: #client-migration-backup-policy}

Before you migrate a client to API version `2023-12-05` or later, review your code for use of the `POST /backup_policies`, `PATCH /backup_policies` and `GET /backup_policies` methods. Verify that your code includes the `match_resource_type` and  `included_content` properties for backup policies in the manner that is appropriate for your programming language. For more information, see the [API change log](/docs/vpc?topic=vpc-api-change-log#version-2023-12-05).

## Examples
{: #examples-create-backup-policy}

These examples compare differences between before and after the `2023-12-05` versioned change.

### Creating a backup policy
{: #migration-backup-policy-examples}

The following example uses API version `2023-12-04` or earlier to create a backup policy for individual volumes. The `data` object specifies `match_resource_types` as `array`. It's not possible to create an `instance` type backup policy with an API version `2023-12-04` or earlier.

```sh
curl -X POST "$vpc_api_endpoint/v1/backup_policies?version=2023-12-04&generation=2"    
-H "Authorization: $iam_token"   
 -d '{
   "match_resource_types": "array",
   "match_user_tags": ["my-daily-backup-policy"],
   "name": "my-backup-policy",
   "plans": [
      {
        "attach_user_tags": ["my-daily-backup-plan"],
         "copy_user_tags": true,
         "cron_spec": "*/5 1,2,3 * * *",
         "deletion_trigger": {"delete_after": 20},
         "name": "my-backup-plan"
       }
     ],
     "resource_group": {}
   }'
```
{: screen}

The following example uses API version `2023-12-05` or later to create a backup policy for individual volumes. The `data` object specifies `match_resource_type` as `volume`.

```sh
curl -X POST "$vpc_api_endpoint/v1/backup_policies?version=2023-12-05&generation=2"\
   -H "Authorization: $iam_token"\
   -d '{
     "match_resource_type": "volume",
     "match_user_tags": ["my-daily-backup-policy"],
     "name": "my-backup-policy",
     "plans": [
      {
        "attach_user_tags": ["my-daily-backup-plan"],
         "copy_user_tags": true,
         "cron_spec": "*/5 1,2,3 * * *",
         "deletion_trigger": {"delete_after": 20},
         "name": "my-backup-plan"
       }
     ],
     "resource_group": {}
   }'
```
{: screen}

The following example uses the API version `2023-12-05` or later to create a backup policy for a consistency group of {{site.data.keyword.block_storage_is_short}} volumes that are attached to the same virtual server instance. The data object specifies `match_resource_type` as `instance` and the `included_content` as `data_volumes`.

```sh
curl -X POST "$vpc_api_endpoint/v1/backup_policies?version=2023-12-05&generation=2"\
   -H "Authorization: $iam_token"\
   -d '{
      "match_resource_type": "instance",
      "included_content": "data_volumes"
      "match_user_tags": ["my-daily-backup-policy"],
      "name": "my-backup-policy",
      "plans": [
       {
         "attach_user_tags": ["my-daily-backup-plan"],
         "copy_user_tags": true,
         "cron_spec": "*/5 1,2,3 * * *",
         "deletion_trigger": {"delete_after": 20},
         "name": "my-backup-plan"
       }
     ],
     "resource_group": {}
   }'
```
{: screen}
