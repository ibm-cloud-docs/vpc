---

copyright:
  years: 2022, 2025
lastupdated: "2025-08-06"

keywords: troubleshooting, file storage for vpc, CBR errors

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do my file share operations fail with a 'shares_access_forbidden' error?
{: #troubleshooting-file-storage}
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
