---

copyright:
  years: 2019, 2021
lastupdated: "2021-05-14"

keywords: create authorization for IBM Cloud Object storage, import image to vpc infrastructure, migrate virtual server, migrate instance

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Granting access to {{site.data.keyword.cos_full_notm}} to import images
{: #object-storage-prereq}

To import a custom image to {{site.data.keyword.vpc_short}}, you must have an instance of {{site.data.keyword.cos_full}} 
available. You must also create a bucket in {{site.data.keyword.cos_full_notm}} to store your images. Finally, you must 
create an authorization so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}.
{:shortdesc}

## Creating an {{site.data.keyword.cos_full_notm}} service instance
{: #migrate-prereq-icos-instance}

If you need to create an instance of {{site.data.keyword.cos_full_notm}}, see 
[Getting started with {{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage).

From IBM {{site.data.keyword.iamshort}}, you must create an authorization so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}. 

### Granting access with the CLI
{: #migrate-prereq-create-service-authorization}
{: cli}

To authorize the image source service to access the target service, a specific instance of {{site.data.keyword.cos_full_notm}}, run the `ibmcloud iam authorization-policy-create` command. 

Before you run the command you need to know the GUID for the {{site.data.keyword.cos_full_notm}}. 
1. Go to Resource list > Storage. 
1. Select the {{site.data.keyword.cos_full_notm}} instance where your images are stored. 
1. From the right panel, copy the GUID. For example, `f7d4676f-f298-4cb3-8390-2fe258a5d6df`.
1. Run the following command and replace $COS_INSTANCE_CRN with the GUID. 
```
ibmcloud iam authorization-policy-create is cloud-object-storage Reader --source-resource-type image --target-service-instance-id $COS_INSTANCE_CRN
```

For more information about all of the parameters that are available for this command, see [ibmcloud iam authorization-policy-create](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_authorization_policy_create).

### Granting access with the API
{: #auth-api}
{: api}

To authorize a source service access to a target service, use the [IAM Policy Management API](/apidocs/iam-policy-management#create-a-policy). See the following API example for Create a policy method with the `type=authorization` specified. All of the possible attributes are listed.

The supported attributes for creating an authorization policy depend on what each service supports. For more information about the supported attributes for each service, refer to the documentation for the services that you're using.
{: note}

The example shows an authorization policy for the image service to access IBM Cloud Object Storage.

```
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

  
<!--- ### Granting access with the UI

1. From the [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc){: external} menu bar, click **Manage** &gt; **Access (IAM)**, and select **Authorizations**.
2. Click **Create**.
3. Select a source and target service for the authorization. Specify **VPC Infrastructure Services** as the source service. Specify **Image Service for VPC** as the resource type. Specify **Cloud Object Storage** as the target service.
4. Select a role to assign access to the source service that accesses the target service.
5. Click **Authorize**.

For more information, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth#serviceauth). -->


## Next steps
{: #next-grant-icos-auth}

When you've completed these steps so that Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}, continue with one of the following topics:
 * [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image)
 * [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image)
 * [Importing a custom image](/docs/vpc?topic=vpc-managing-images#import-custom-image)
