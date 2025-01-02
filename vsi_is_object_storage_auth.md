---

copyright:
  years: 2019, 2025
lastupdated: "2024-02-22"

keywords: create authorization for {{site.data.keyword.cos_full_notm}}, import image to vpc infrastructure, migrate virtual server, migrate instance

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Granting access to {{site.data.keyword.cos_full_notm}} to import and export images
{: #object-storage-prereq}

To import a custom image to {{site.data.keyword.vpc_short}}, or to export a custom image from {{site.data.keyword.vpc_short}}, you must have an instance of {{site.data.keyword.cos_full}}
available. You must also create a bucket in {{site.data.keyword.cos_full_notm}} to store your images. Finally, you must
create an authorization so that the Image Service for VPC can access {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

## Creating an {{site.data.keyword.cos_full_notm}} service instance
{: #migrate-prereq-icos-instance}

If you need to create an instance of {{site.data.keyword.cos_full_notm}}, see
[Getting started with {{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage).

From IBM {{site.data.keyword.iamshort}}, you must create an authorization so that the Image Service for VPC can access {{site.data.keyword.cos_full_notm}}.

## Creating an authorization
{: #migrate-prereq-create-service-authorization}

To authorize the Image Service for VPC to access the target service, {{site.data.keyword.cos_full_notm}}, you must create an [authorization](/docs/account?topic=account-serviceauth). To import an image, you must specify the Reader service access role for {{site.data.keyword.cos_full_notm}}. To export an image, you must specify the Writer service access role for {{site.data.keyword.cos_full_notm}}. With both Reader and Writer service access roles for {{site.data.keyword.cos_full_notm}}, you can both import and export images.

### Granting Reader and Writer access with the UI
{: #custom-image-service-authorization-rw-ui}
{: ui}

Complete the following steps to create an authorization for the Image Service for VPC to both import images from an {{site.data.keyword.cos_full_notm}} service instance and export images to an {{site.data.keyword.cos_full_notm}} service instance.

1. From the [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com){: external} menu bar, click **Manage > Access (IAM)**, and select **Authorizations**.
2. On the Manage authorizations page, click **Create**.
3. Make your selection for the **Source account**. By default **This account** is selected. Click **Next**. 
4. Select a source service for the authorization. Specify **VPC Infrastructure Services** as the source service. Click **Next**.
5. For Resources, make your selection for how you want to scope the access. 
    1. Select **Specific resources**.
    2. For Specific resources, select **Resource type** and **Image service for VPC**.
    3. Click **Next**.
6. For the Target service, select **Cloud Object Storage**. Click **Next**.
7. For Resources, specify how you want to scope the access. You can use the default selection **All resources**. Click **Next**.

    If you want to scope the access to a specific resource within {{site.data.keyword.cos_full_notm}}, you can select **Resources based on selected attributes**. Then make selections to narrow the access according to your preferences.
    {: tip}

9. For Roles, select both service access roles, **Reader** and **Writer**.
10. Click **Review** to ensure your selections look correct.
11. Click **Authorize**.

For more information, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth#serviceauth).

### Granting Reader and Writer access to all buckets from the CLI
{: #custom-image-service-authorization-rw}
{: cli}

To grant both `Reader` and `Writer` access to all buckets in {{site.data.keyword.cos_full_notm}}, run the `iam authorization-policy-create` command. The following command authorizes the Image Service for VPC to both import images from any bucket in an {{site.data.keyword.cos_full_notm}} service instance and export images to any bucket in an {{site.data.keyword.cos_full_notm}} service instance.

```sh
ibmcloud iam authorization-policy-create is cloud-object-storage Reader,Writer --source-resource-type image
```
{: pre}

For more information, see [`ibmcloud iam authorization-policy-create`](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_authorization_policy_create).

### Granting access to a specific bucket from the CLI
{: #custom-image-service-authorization-bucket}
{: cli}

You can choose to grant authorization to a specific bucket in {{site.data.keyword.cos_full_notm}}. The following example describes how to grant `Reader` access to a specific bucket for importing images from {{site.data.keyword.cos_full_notm}}. To export an image to {{site.data.keyword.cos_full_notm}}, you must also grant `Writer` access.

Before you run the command you need to know the GUID for the {{site.data.keyword.cos_full_notm}} service instance.
1. Use the `ibmcloud resource service-instance` command to obtain the GUID. See the following example:

   ```sh
   $ ibmcloud resource service-instance cos-fs-cloud-us-south
   Retrieving service instance cos-fs-cloud-us-south in all resource groups under account Test Account as test.user@ibm.com...
   OK

   Name:                  cos-fs-cloud-us-south
   ID:                    crn:v1:bluemix:public:cloud-object-storage:global:a/a1234567:0e4a33e6-973e-42b6-bea4-ce1b3aebe163::
   GUID:                  0e4a33e6-973e-42b6-bea4-ce1b3aebe163
   Location:              global
   Service Name:          cloud-object-storage
   Service Plan Name:     standard
   Resource Group Name:   defaults
   State:                 active
   Type:                  service_instance
   Sub Type:
   Locked:                false
   Created at:            2021-07-27T14:40:45Z
   Created by:            IBMid-12345678
   Updated at:            2021-07-27T14:40:47Z
   Last Operation:
                          Status    create succeeded
                          Message   Completed create instance operation
    ```
    {: screen}

1. Run the following command and replace `$COS_INSTANCE_GUID` with the `GUID` value.

   ```sh
   ibmcloud iam authorization-policy-create is cloud-object-storage Reader --source-resource-type image --target-service-instance-id $COS_INSTANCE_GUID
   ```
   {: pre}

   A successful response looks like the following example:

   ```sh
   $ ibmcloud iam authorization-policy-create is cloud-object-storage Reader --source-resource-type image --target-service-instance-id 0e4a33e6-973e-42b6-bea4-ce1b3aebe163
   Creating authorization policy under account a1234567 as test.user@ibm.com...
   OK
   Authorization policy 0bbcb168-bf7b-4ebf-9684-769f1d7e80e7 was created.

   ID:                        0bbcb168-bf7b-4ebf-9684-769f1d7e80e7
   Source service name:       is
   Source service instance:   All instances
   Source resource type:      image
   Target service name:       cloud-object-storage
   Target service instance:   0e4a33e6-973e-42b6-bea4-ce1b3aebe163
   Roles:                     Reader
   ```
   {: screen}

For more information about all of the parameters that are available for this command, see [`ibmcloud iam authorization-policy-create`](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_authorization_policy_create).

### Granting access with the API
{: #auth-api}
{: api}

To authorize a source service access to a target service, use the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy). See the following API example for Create a policy method with the `type=authorization` specified. All of the possible attributes are listed.

The supported attributes for creating an authorization policy depend on what each service supports. For more information about the supported attributes for each service, refer to the documentation for the services that you're using.
{: note}

The example shows an authorization policy for the Image Service for VPC to access {{site.data.keyword.cos_full_notm}}.

```sh
curl --location --request POST 'https://iam.cloud.ibm.com/v1/policies' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <iam token>' \
--data-raw '{
    "type": "authorization",
    "subjects": [
        {
            "attributes": [
                {
                    "name": "accountId",
                    "value": "$ACCOUNT_ID"
                },
                {
                    "name": "serviceName",
                    "value": "is"
                },
                {
                    "name": "resourceType",
                    "value": "image"
                }
            ]
        }
    ],
    "roles": [
        {
            "role_id": "crn:v1:bluemix:public:iam::::serviceRole:Reader"
        },
        {
            "role_id": "crn:v1:bluemix:public:iam::::serviceRole:Writer"
        }
    ],
    "resources": [
        {
            "attributes": [
                {
                    "name": "accountId",
                    "value": "$ACCOUNT_ID"
                },
                {
                    "name": "serviceName",
                    "value": "cloud-object-storage"
                }
            ]
        }
    ]
}'
```
{: codeblock}


## Next steps
{: #next-grant-icos-auth}

When you've completed these steps so that Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}} or export images to {{site.data.keyword.cos_full_notm}}, continue with one of the following topics:
 * [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image)
 * [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image)
 * [Importing a custom image](/docs/vpc?topic=vpc-importing-custom-images-vpc)
 * [Exporting a custom image](/docs/vpc?topic=vpc-managing-custom-images#custom-image-export-to-cos)
