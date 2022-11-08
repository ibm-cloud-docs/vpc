---

copyright:
  years: 2022
lastupdated: "2022-11-08"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting File Storage for VPC
{: #troubleshooting-file-storage}

When you create or manage File Storage for VPC, you might encounter issues. Often, you can recover by following a few steps. Issues, symptoms, likely causes, and resolutions are described in the following sections.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Frankfurt, London, Dallas, Toronto, Washington, Sao Paulo, Sydney, Tokyo, and Osaka regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## CBR error when performing file share operations
{: #fs-troubleshoot-1}
{: troubleshoot}
{: support}

A `shares_access_forbidden` error occurs when using the context-based restriction (CBR) feature when performing a file share operations, such as creating or updating a file share. In this case, the user request is forbidden, which is not the expected behavior.
{: tsSymptoms}

An issue exists with the [IAM CBR feature](/docs/account?topic=account-context-restrictions-whatis). Because the file service depends on the IAM CBR, performing share operations results in an error.
{: tsCauses}

An error occurs after you set up CBR by [creating a network zone](/docs/vpc?topic=vpc-cbr&interface=cli#creating-network-zones) and [context-based rule](/docs/vpc?topic=vpc-cbr&interface=cli#creating-rules), and then performing a file share operation.

For example, first, you would add `shares` service to a network zone.

    ```sh
    ibmcloud cbr zone-create --name network-zone-1 --description "Example zone 1" --addresses 198.51.100.0  --vpc VPC-1 --service-ref service_name=shares
    ```
    {: screen}



To complete setting up CBR, you would then create a context-based rule and specify `shares` as the service name and share ID for resource attributes.

    ```sh
    ibmcloud cbr rule-create --zone-id a7eeb5dd8e6bdce670eba1afce18e37f --description "Test CBR for file share" --service-name shares --resource-attributes "shareId=UUID-OF-SHARE"
    ```
    {: screen}

See the [CBR CLI reference](/docs/account?topic=cli-cbr-plugin) for more information about these commands.

The error occurs when you attempt to create a file share after enabling CBR. For example, using the [VPC API](/docs/vpc?topic=vpc-file-storage-create&interface=api#fs-create-file-share-api) to create the share, you'll get this error in the response:

```json
{
    "errors": {
      {
          "code": "shares_access_forbidden",
          "message": "The user request is forbidden",
          "more_info": "The user is forbidden to access the requested resource. Check permissions and try again."
      }
   }
}
```
{: codeblock}

This error requires that you contact [IBM support](/docs/vpc?topic=vpc-getting-help&interface=cli#support-cases). Provide the error logs for their reference.
{: tsResolve}
