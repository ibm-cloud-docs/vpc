---

copyright:
  years: 2022, 2025
lastupdated: "2025-04-25"

keywords: troubleshooting, file storage for vpc, CBR errors

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting {{site.data.keyword.filestorage_vpc_short}}
{: #troubleshooting-file-storage}

When you create or manage {{site.data.keyword.filestorage_vpc_short}}, you might encounter issues. Often, you can recover by following a few steps. Issues, symptoms, likely causes, and resolutions are described in the following sections.
{: shortdesc}

## CBR error during file share operations
{: #fs-troubleshoot-1}
{: troubleshoot}
{: support}

A `shares_access_forbidden` error occurs when the context-based restriction (CBR) feature is used during file share operations, such as creating or updating a file share. In this case, the user request is forbidden, which is not the expected behavior.
{: tsSymptoms}

An issue exists with the [IAM CBR feature](/docs/account?topic=account-context-restrictions-whatis). Because the file service depends on the {{site.data.keyword.iamshort}} CBR, share operations result in an error.
{: tsCauses}

An error occurs after you set up CBR by [creating a network zone](/docs/vpc?topic=vpc-cbr&interface=ui#network-zone) and a [context-based rule](/docs/vpc?topic=vpc-cbr&interface=ui#cbr-rules), and then try to perform a file share operation.

1. Add `shares` service to a network zone.
    ```sh
    ibmcloud cbr zone-create --name network-zone-1 --description "Example zone 1" --addresses 198.51.100.0  --vpc VPC-1 --service-ref service_name=shares
    ```
    {: pre}

2. Create a context-based rule and specify `shares` as the service name and share ID for resource attributes.
    ```sh
    ibmcloud cbr rule-create --zone-id a7eeb5dd8e6bdce670eba1afce18e37f --description "Test CBR for file share" --service-name shares --resource-attributes "shareId=UUID-OF-SHARE"
    ```
    {: pre}

    For more information about these commands, see the [CBR CLI reference](/docs/account?topic=account-cbr-plugin).

3. The error occurs when you attempt to create a file share after CBR is enabled. For example, when you use the [VPC API](/docs/vpc?topic=vpc-file-storage-create&interface=api#fs-create-file-share-api) to create the share, you get the following error message in the response:

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
{: screen}

This error requires that you contact [IBM support](/docs/account?topic=account-open-case). Provide the error logs for their reference.
{: tsResolve}

## Bulk migration of file share profiles is not supported
{: #fs-troubleshoot-profiles}
{: troubleshoot}
{: support}

{{site.data.keyword.filestorage_vpc_short}} does not support changing file storage profiles of multiple shares at the same time. You can update the profile for a single share but bulk migration is not supported.
{: tsSymptoms}

You cannot use the UI, CLI, or API to update the profiles of multiple file shares in a single operation.
 {: tsCauses}

For migrating multiple shares, you must create your own script. The script can list the shares that you want to update, and then go through the list of shares to update each individual share profile.
{: tsResolve}

Create a script file to list your file shares, parse the list, and then individually update the profile for each file share. Ensure that you set execute permission on your script file. In your script:

1. List your file shares in your region by specifying a `GET /shares` request. To keep the operation manageable, you might want to set a limit on the number of shares.

   ```sh
   curl -X GET \
   "$vpc_api_endpoint/v1/shares?limit=20?version=2023-07-17&generation=2"\
   -H "Authorization: $iam_token"
   ```
   {: codeblock}

2. Parse each file in the list and update each file share by making separate `PATCH/ shares/{share_id}` requests.

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/shares/432f1a4d-4aac-4ba1-922c-76fdbcbeb1e3?version=2023-07-17&generation=2" -H "Authorization: $iam_token"
   -d '{
        "profile": {
           "name": "dp2"
        },
   }'
   ```
   {: codeblock}

3. Verify that the profile was updated by listing your file shares again.
