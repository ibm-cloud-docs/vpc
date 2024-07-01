---

copyright:
  years: 2022, 2024
lastupdated: "2024-07-01"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Establishing service-to-service authorizations for the Backup service
{: #backup-s2s-auth}

Before you can create backup policies, you need to establish service-to-service authorizations and specify user roles. This authorization enables the Backup for VPC service to detect the tags, create backup snapshots and store them in {{site.data.keyword.cos_short}}.
{: shortdesc}

## Overview
{: #backup-s2s-auth-overview}

For IBM Cloud Backup for VPC service to work, you need to provide an authorization for the service. In an authorization, the source service is the service that is granted access to the target service. The roles that you select define the level of access for the source service. The target service is the service that you are granting permission to be accessed by the source service based on the roles that you assign. A source service can be in the same account where the authorization is created or in another account. The target service is always in the account where the authorization is created.

To create a backup policy and for the backup jobs to run correctly, the Backup service needs to be authorized to work with {{site.data.keyword.block_storage_is_short}}, Snapshots for VPC, and Virtual Server for VPC services. 

If you are an Enterprise account administrator who wants to create a backup policy for your enterprise account and subaccounts, you also need to have authorization for the Backup for VPC service in the enterprise account to work with the Backup for VPC service in the subaccounts.



For more information about authorizations, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth).

If you set up service authorizations incorrectly, the backup service cannot create the backup policies. For more information, see the troubleshooting topic [Backup policy not created due to incorrect authorizations](/docs/vpc?topic=vpc-baas-troubleshoot&interface=ui#baas-ts-3).
{: note}

## Creating authorization policies in the UI
{: #backup-s2s-auth-procedure-ui}
{: ui}

### Enabling service-to-service authorization for volume backups at the account level
{: #backup-s2s-auth-procedure-ui-account}

To create a service-to-service authorization policy, follow this procedure:

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM)**. The **Manage access and users** page is displayed.
1. From the side panel, select **Authorizations**.
1. On the **Manage authorizations** page, click **Create**. 
1. In the **Source** section, select the **Source account**. As you're setting up authorization for the Backup service in your account, select **This account**. Click **Next**.
1. For the source service, select **VPC Infrastructure Services** from the list. Click **Next**.
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**. 
   1. From the list, select **Resource type**. 
   1. In the next field, select **IBM Cloud Backup for VPC**.
   1. Click **Next**.
1. For the target service, select **VPC Infrastructure Services** from the list. Click **Next**.
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**.
   1. Click **Resource type**. Select one of the following services. You need to create authorization for all of them.
   
   | Source service - resource type | Target service - resource type  | Dependent service user role |
   |--------------------------------|---------------------------------|-----------|
   | IBM Cloud Backup for VPC       | Block Storage for VPC           | Operator |
   | IBM Cloud Backup for VPC       | Block Storage Snapshots for VPC | Editor   |
   | IBM Cloud Backup for VPC       | Multi Volume Snapshots for VPC  | Editor   |
   | IBM Cloud Backup for VPC       | Virtual Server for VPC          | Operator | 
   {: caption="Table 1. Service-to-service authorizations" caption-side="bottom"}

1. Click **Next**.
1. Select the role. See Table 1 for the appropriate role.
1. Click **Review** and inspect your choices.
1. Click **Authorize**.
1. When you are returned to the **Manage authorizations** page, click **Create** again and follow the same steps to set up authorizations for the remaining services.

### Creating cross-account authorization for volume backups for the Enterprise
{: #backup-s2s-auth-procedure-ui-enterprise}

To allow an Enterprise administrator to manage backups centrally, the subaccounts must provide authorization for the Backup service of the Enterprise account to interact with the resources of the child accounts. 

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM)**. The **Manage access and users** page is displayed.
1. From the side panel, select **Authorizations**.
1. On the **Manage authorizations** page, click **Create**. 
1. In the **Source** section, select the **Source account**. As you're setting up authorization for the Backup service of the enterprise account, select **Specific account**, and enter the Enterprise account's ID. Click **Next**.
1. For the source service, select **VPC Infrastructure Services** from the list. Click **Next**.
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**. 
   1. From the list, select **Resource type**. 
   1. In the next field, select **IBM Cloud Backup for VPC**.
   1. Click **Next**.
1. For the target service, select **VPC Infrastructure Services** from the list. 
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**.
   1. Click **Resource type**. Select one of the services in Table 2. You need to create authorization for all of them.

   | Source service - resource type | Target service - resource type  | Dependent service user role |
   |--------------------------------|---------------------------------|-----------|
   | IBM Cloud Backup for VPC       | Block Storage for VPC           | Operator  |
   | IBM Cloud Backup for VPC       | Block Storage Snapshots for VPC | Editor    |
   | IBM Cloud Backup for VPC       | Multi Volume Snapshots for VPC  | Editor    |
   | IBM Cloud Backup for VPC       | Virtual Server for VPC          | Operator  | 
   | IBM Cloud Backup for VPC       | IBM Cloud Backup for VPC        | Editor    |
   {: caption="Table 2. Service-to-service authorizations for the Enterprise" caption-side="bottom"}

1. Click **Next**.
1. Select the role. See Table 2 for the appropriate role.
1. Click **Review** and inspect your choices.
1. Click **Authorize**.
1. When you are returned to the **Manage authorizations** page, click **Create** again and follow the same steps to set up authorizations for the remaining services.



## Creating authorization policies from the CLI
{: #backup-s2s-auth-procedure-cli}
{: cli}

### Enabling service-to-service authorization for volume backups at the account level
{: #backup-s2s-auth-procedure-cli-account}

To use Backup for VPC in your account to create policies, plans and run backup jobs for block storage volumes, create the following service-to-service authorizations:

* `backup-policy` (source) to `instance` (target) with _Operator_ role
* `backup-policy` (source) to `volume` (target) with _Operator_ role
* `backup-policy` (source) to `snapshot` (target) with _Editor_ role
* `backup-policy` (source) to `snapshot-consistency-group` (target) with _Editor_ role

1. Create four JSON files with the following information for the authorization policies.
   * Instance service:
     ```json
     {
       "type": "authorization",
       "subjects": [
           {
            "attributes": [
                {
                    "name": "accountId",
                    "value": "ACCOUNT_ID"
                },
                {
                    "name": "serviceName",
                    "value": "is"
                },
                {
                    "name": "resourceType",
                    "value": "backup-policy"
                }
               ]
           }
       ],
       "roles": [
           {
            "role_id": "crn:v1:bluemix:public:iam::::role:Operator"
           }
       ],
       "resources": [
           {
            "attributes": [
                {
                    "name": "accountId",
                    "value": "ACCOUNT_ID"
                },
                {
                    "name": "serviceName",
                    "operator": "stringEquals",
                    "value": "is"
                },
                {
                    "name": "instanceId",
                    "value": "*",
                    "operator": "stringEquals"
                }
            ]
          }
       ]
     }
     ```
     {: codeblock}

   * Block Storage volume service:
     ```json
     {
       "type": "authorization",
       "subjects": [
           {
            "attributes": [
                {
                    "name": "accountId",
                    "value": "ACCOUNT_ID"
                },
                {
                    "name": "serviceName",
                    "value": "is"
                },
                {
                    "name": "resourceType",
                    "value": "backup-policy"
                }
               ]
           }
       ],
       "roles": [
           {
            "role_id": "crn:v1:bluemix:public:iam::::role:Operator"
           }
       ],
       "resources": [
           {
            "attributes": [
                {
                    "name": "accountId",
                    "value": "ACCOUNT_ID"
                },
                {
                    "name": "serviceName",
                    "operator": "stringEquals",
                    "value": "is"
                },
                {
                    "name": "volumeId",
                    "value": "*",
                    "operator": "stringEquals"
                }
            ]
           }
       ]
     }
     ```
     {: codeblock}

   * Block Storage snapshot service:
     ```json
     {
       "type": "authorization",
       "subjects": [
           {
            "attributes": [
                {
                    "name": "accountId",
                    "value": "ACCOUNT_ID"
                },
                {
                    "name": "serviceName",
                    "value": "is"
                },
                {
                    "name": "resourceType",
                    "value": "backup-policy"
                }
               ]
           }
       ],
       "roles": [
           {
            "role_id": "crn:v1:bluemix:public:iam::::role:Editor"
           }
       ],
       "resources": [
           {
            "attributes": [
                {
                    "name": "accountId",
                    "value": "ACCOUNT_ID"
                },
                {
                    "name": "serviceName",
                    "operator": "stringEquals",
                    "value": "is"
                },
                {
                    "name": "snapshotId",
                    "value": "*",
                    "operator": "stringEquals"
                }
            ]
           }
          ]
     }
     ```
     {: codeblock}

   * Snapshot consistency group:
    ```json
      {
       "type": "authorization",
       "subjects": [
        {
            "attributes": [
                {
                    "name": "accountId",
                    "value": "ACCOUNT_ID"
                },
                {
                    "name": "serviceName",
                    "value": "is"
                },
                {
                    "name": "resourceType",
                    "value": "backup-policy"
                }
            ]
        }
       ],
       "roles": [
        {
            "role_id": "crn:v1:bluemix:public:iam::::role:Editor"
        }
       ],
       "resources": [
        {
            "attributes": [
                {
                    "name": "accountId",
                    "value": "ACCOUNT_ID"
                },
                {
                    "name": "serviceName",
                    "operator": "stringEquals",
                    "value": "is"
                },
                {
                    "name": "snapshotConsistencyGroupId",
                    "value": "*",
                    "operator": "stringEquals"
                }
            ]
        }
       ]
      }
      ```
      {: codeblock}

1. Then, use the JSON files to run the following CLI command.
   ```sh
   ibmcloud iam authorization-policy-create --file ~/Documents/policy.json
   ```
   {: pre}



For more information about all of the parameters that are available for this command, see [ibmcloud iam authorization-policy-create](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_authorization_policy_create).

### Creating cross-account authorization for volume backups for the Enterprise
{: #backup-s2s-auth-procedure-cli-enterprise}

To allow an Enterprise administrator to manage backups centrally, the subaccounts must provide authorization for the Backup service of the Enterprise account to interact with the resources of the child accounts. 

Run the `ibmcloud iam authorization-policy-create` command with one of the following options: `--source-service-account`, `--source-service-instance-name`, or `--source-service-instance-id` to identify the enterprise account as the source. To get the enterprise account ID, you can run the following command.

```sh
ibmcloud enterprise show
```
{: pre}

Then, use the account ID to authorize the Enterprise account's backup service instance to interact with the child account's backup, snapshot, volume, and instance services.

```sh
ibmcloud iam authorization-policy-create is is Editor --source-resource-type backup-policy --target-resource-type backup-policy --source-service-account ACCOUNT_ID
```
{: pre}

```sh
ibmcloud iam authorization-policy-create is is Editor --source-resource-type backup-policy --target-resource-type snapshot --source-service-account ACCOUNT_ID
```
{: pre}

```sh
ibmcloud iam authorization-policy-create is is Editor --source-resource-type backup-policy --target-resource-type volume --source-service-account ACCOUNT_ID
```
{: pre}

```sh
ibmcloud iam authorization-policy-create is is Editor --source-resource-type backup-policy --target-resource-type snapshot-consistency-group --source-service-account ACCOUNT_ID
```
{: pre}

```sh
ibmcloud iam authorization-policy-create is is Editor --source-resource-type backup-policy --target-resource-type instance --source-service-account ACCOUNT_ID
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud iam authorization-policy-create](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_authorization_policy_create).



## Creating authorization policies with the API
{: #backup-s2s-auth-procedure-api}
{: api}

### Enabling service-to-service authorization for volume backups at the account level
{: #backup-s2s-auth-procedure-api-account}

To use Backup for VPC in your account to create policies, plans and run backup jobs for block storage volumes, create the following service-to-service authorizations:

* `is.backup-policy` (source) to `is.instance` (target) with _operator_ role.
* `is.backup-policy` (source) to `is.volume` (target) with _operator_ role.
* `is.backup-policy` (source) to `is.snapshot` (target) with _editor_ role.
* `is.backup-policy` (source) to `is.snapshot-consistency-group` with _editor_ role

Make the request to the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy), similar to the following examples.

```json
curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 
'Authorization: Bearer $TOKEN' -H 
'Content-Type: application/json' -d 
'{
   "type": "access",
   "description": "Operator role for the Backup service to the Virtual Server service",
   "subjects": [
    {"attributes": [
       {"name": "serviceName","value": "is"},
       {"name": "accountId","value": "$ACCOUNT_ID"},
       {"name": "resourceType","value": "backup-policy"}]
    }
   ],
  "roles":[
    {"role_id": "crn:v1:bluemix:public:iam::::role:Operator"}
   ],
   "resources":[
    {"attributes":[
      {"name": "accountId","value": "$ACCOUNT_ID"},
      {"name": "serviceName","value": "is","operator": "stringEquals"},
      {"name": "instanceId","value": "*","operator": "stringEquals"}]
    }
  ]
}'
```

```json
curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 
'Authorization: Bearer $TOKEN' -H 
'Content-Type: application/json' -d 
'{
   "type": "access",
   "description": "Operator role for the Backup service to the Cloud Block Storage",
   "subjects":[
    {"attributes":[
      {"name": "serviceName","value": "is"},
      {"name": "accountId","value": "$ACCOUNT_ID"},
      {"name": "resourceType","value": "backup-policy"}]
    }],
   "roles":[
    {"role_id": "crn:v1:bluemix:public:iam::::role:Operator"}
    ],
   "resources":[
    {"attributes": [
      {"name": "accountId","value": "$ACCOUNT_ID"},
      {"name": "serviceName","value": "is.volume","operator": "stringEquals"},
      {"name": "volumeId","value": "*","operator": "stringEquals"}
     ]
    }
   ]
}'
```
{: pre}

```json
curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 
'Authorization: Bearer $TOKEN' -H 
'Content-Type: application/json' -d 
'{
   "type": "access",
   "description": "Editor role for the Backup service to Block Storage Snapshots",
   "subjects": [
    {"attributes": [
      {"name": "serviceName","value": "is"},
      {"name": "accountId","value": "$ACCOUNT_ID"},
      {"name": "resourceType","value": "backup-policy"}]
    }
   ],
   "roles":[
    {"role_id": "crn:v1:bluemix:public:iam::::role:Editor"}
   ],
   "resources":[
    {"attributes": [
      {"name": "accountId","value": "$ACCOUNT_ID"},
      {"name": "serviceName","value": "is","operator": "stringEquals"},
      {"name": "snapshotId","value": "*","operator": "stringEquals"}]
    }
   ]
}'
```
{: pre}

```json
curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 
'Authorization: Bearer $TOKEN' -H 
'Content-Type: application/json' -d 
'{
   "type": "access",
   "description": "Editor role for the Backup service to the Snapshot consistency groups",
   "subjects": [
    {"attributes": [
       {"name": "serviceName","value": "is"},
       {"name": "accountId","value": "$ACCOUNT_ID"},
       {"name": "resourceType","value": "backup-policy"}]
    }
   ],
  "roles":[
    {"role_id": "crn:v1:bluemix:public:iam::::role:Editor"}
   ],
   "resources":[
    {"attributes":[
      {"name": "accountId","value": "$ACCOUNT_ID"},
      {"name": "serviceName","value": "is","operator": "stringEquals"},
      {"name": "snapshotConsistencyGroupId","value": "*","operator": "stringEquals"}]
    }
  ]
}'
```
{: pre}

For more information, see the api spec for [IAM Policy Management](/apidocs/iam-policy-management#create-policy).

### Creating cross-account authorization for volume backups for the Enterprise
{: #backup-s2s-auth-procedure-api-enterprise}

To allow an Enterprise administrator to manage backups centrally, the subaccounts must provide authorization for the Backup service of the Enterprise account to interact with the resources of the child accounts.

1. Make an API request to the [Enterprise Management API](https://cloud.ibm.com/apidocs/enterprise-apis/enterprise#list-enterprises) to get the account ID of the parent enterprise account.

   ```sh
   curl -X GET "https://enterprise.cloud.ibm.com/v1/enterprises" -H 
   "Authorization: Bearer <IAM_Token>" -H 
   'Content-Type: application/json'
   ```
   {: pre}

1. Then, make the requests to the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy) to create the service-to-service authorizations for the `is.backup-policy` of enterprise account to interact with the child account's `is.backup`, `is.snapshot`, `is.volume`, `is.snapshot-consistency-group`, and `is.instance` services.

   * Authorize `is.backup-policy` (source) to interact with `is.backup-policy` (target) with the _editor_ role.

   ```json
   curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 
   'Authorization: Bearer $TOKEN' -H 
   'Content-Type: application/json' -d 
   '{
     "type": "access",
     "description": "Editor role for the Enterprise account's backup service to interact with this account's backup service.",
     "subjects": [
       {"attributes": [
          {"name": "serviceName","value": "is"},
          {"name": "accountId","value": "$ENTERPRISE_ACCOUNT_ID"},
          {"name": "resourceType","value": "backup-policy"}]
        }
      ],
     "roles":[
       {"role_id": "crn:v1:bluemix:public:iam::::role:Editor"}
     ],
     "resources":[
       {"attributes":[
          {"name": "accountId","value": "$SUB_ACCOUNT_ID","operator": "stringEquals"},
          {"name": "serviceName","value": "is","operator": "stringEquals"},
          {"name": "backupPolicyId","value": "*","operator": "stringEquals"}]
       }
      ]
     }'
     ```
     {: pre}

   * Authorize `is.backup-policy` (source) to interact with `is.volume` (target) with the _operator_ role.

   ```json
   curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 
   'Authorization: Bearer $TOKEN' -H 
   'Content-Type: application/json' -d 
   '{
     "type": "access",
     "description": "Operator role for the Enterprise account's backup service to interact with this account's volume service",
     "subjects": [
       {
        "attributes": [
          {"name": "serviceName","value": "is"},
          {"name": "accountId","value": "$ENTERPRISE_ACCOUNT_ID"},
          {"name": "resourceType","value": "backup-policy"}]
        }
      ],
     "roles":[
       {"role_id" "crn:v1:bluemix:public:iam::::role:Operator"}
      ],
     "resources":[
       {"attributes": [
          {"name": "accountId","value": "$SUB_ACCOUNT_ID"},
          {"name": "serviceName","value": "is.volume","operator": "stringEquals"},
          {"name": "volumeId","value": "*","operator": "stringEquals"}]
       } 
      ]
   }'
   ```
   {: pre}

   * Authorize `is.backup-policy` (source) to interact with `is.snapshot` (target) with the _editor_ role.

   ```json
   curl -X POST 'https://iam.test.cloud.ibm.com/v1/policies' -H 
   'Authorization: Bearer $TOKEN' -H 
   'Content-Type: application/json' -d 
    '{
      "type": "access",
      "description": "Editor role for the Enterprise account's backup service to interact with this account's snapshots",
      "subjects":[
       {
        "attributes":[
          {"name": "serviceName","value": "is"},
          {"name": "accountId","value": "$ENTERPRISE_ACCOUNT_ID"},
          {"name": "resourceType","value": "backup-policy"}]
        }
       ],
      "roles":[
         {"role_id": "crn:v1:bluemix:public:iam::::role:Editor"}
       ],
      "resources":[
         {"attributes": [
          {"name": "accountId","value": "$SUB_ACCOUNT_ID"},
          {"name": "serviceName","value": "is","operator": "stringEquals"},
          {"name": "snapshotId","value": "*","operator": "stringEquals"}]
       }
      ]
    }'
    ```
    {: pre}

   * Authorize `is.backup-policy` (source) to interact with `is.instance` (target) with the _operator_ role.
  
   ```json
   curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 
   'Authorization: Bearer $TOKEN' -H 
   'Content-Type: application/json' -d 
   '{
     "type": "access",
     "description": "Operator role for the Enterprise account's backup service to interact with this account's virtual server instance service",
     "subjects": [
       {"attributes": [
          {"name": "serviceName","value": "is"},
          {"name": "accountId","value": "$ENTERPRISE_ACCOUNT_ID"},
          {"name": "resourceType","value": "backup-policy"}]
        }
      ],
     "roles":[
       {"role_id" "crn:v1:bluemix:public:iam::::role:Operator"}
      ],
     "resources":[
       {"attributes": [
          {"name": "accountId","value": "$SUB_ACCOUNT_ID"},
          {"name": "serviceName","value": "is.volume","operator": "stringEquals"},
          {"name": "instanceId","value": "*","operator": "stringEquals"}]
       }
      ]
   }'
   ```
   {: pre}
      
For more information, see the api spec for [IAM Policy Management](/apidocs/iam-policy-management#create-policy).



## Creating authorization policies with Terraform
{: #backup-s2s-auth-procedure-terraform}
{: terraform}

### Enabling service-to-service authorization for volume backups at the account level
{: #backup-s2s-auth-procedure-terraform-account}

Create an authorization policy between services by using the `ibm_iam_authorization_policy` resource argument in your `main.tf` file.



```terraform
resource "ibm_iam_authorization_policy" "policy1" {
  subject_attributes {
    name  = "accountId"
    value = data.ibm_iam_account_settings.iam.account_id
  }
  subject_attributes {
    name  = "serviceName"
    value = "is"
  }
  subject_attributes {
    name  = "resourceType"
    value = "backup-policy"
  }
  resource_attributes {
    name  = "accountId"
    operator = "stringEquals"
    value = data.ibm_iam_account_settings.iam.account_id
  }
  resource_attributes {
    name  = "serviceName"
    operator = "stringEquals"
    value = "is"
  }
  resource_attributes {
    name  = "volumeId"
    operator = "stringExists"
    value = "true"
  }
  roles   = ["Operator"]
}

resource "ibm_iam_authorization_policy" "policy2" {
  subject_attributes {
    name  = "accountId"
    value = data.ibm_iam_account_settings.iam.account_id
  }
  subject_attributes {
    name  = "serviceName"
    value = "is"
  }
  subject_attributes {
    name  = "resourceType"
    value = "backup-policy"
  }
  resource_attributes {
    name  = "accountId"
    operator = "stringEquals"
    value = data.ibm_iam_account_settings.iam.account_id
  }
  resource_attributes {
    name  = "serviceName"
    operator = "stringEquals"
    value = "is"
  }
  resource_attributes {
    name  = "snapshotId"
    operator = "stringExists"
    value = "true"
  }
  roles   = ["Editor"]
}

resource "ibm_iam_authorization_policy" "policy3" {
  subject_attributes {
    name  = "accountId"
    value = data.ibm_iam_account_settings.iam.account_id
  }
  subject_attributes {
    name  = "serviceName"
    value = "is"
  }
  subject_attributes {
    name  = "resourceType"
    value = "backup-policy"
  }
  resource_attributes {
    name  = "accountId"
    operator = "stringEquals"
    value = data.ibm_iam_account_settings.iam.account_id
  }
  resource_attributes {
    name  = "serviceName"
    operator = "stringEquals"
    value = "is"
  }
  resource_attributes {
    name  = "snapshotConsistencyGroupId"
    operator = "stringExists"
    value = "true"
  }
  roles   = ["Editor"]
}

resource "ibm_iam_authorization_policy" "policy4" {
  subject_attributes {
    name  = "accountId"
    value = data.ibm_iam_account_settings.iam.account_id
  }
  subject_attributes {
    name  = "serviceName"
    value = "is"
  }
  subject_attributes {
    name  = "resourceType"
    value = "backup-policy"
  }
  resource_attributes {
    name  = "accountId"
    operator = "stringEquals"
    value = data.ibm_iam_account_settings.iam.account_id
  }
  resource_attributes {
    name  = "serviceName"
    operator = "stringEquals"
    value = "is"
  }
  resource_attributes {
    name  = "instanceId"
    operator = "stringExists"
    value = "true"
  }
  roles   = ["Operator"]
}
```
{: screen}

For more information about the arguments and attributes, see the [Terraform documentation for authorization resources](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_authorization_policy){: external}.



## Next Steps
{: #backup-s2s-next-steps}

[Create backup policies](/docs/vpc?topic=vpc-create-backup-policy-and-plan).
