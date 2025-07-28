---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-28"

keywords: file share, file storage, virtual network interface, accessor share, de-auth

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Removing access to a file share from other accounts
{: #file-storage-accessor-delete}

When you no longer want to allow another account or service to have access to your file share data, you can revoke their access at any time. To revoke access, first remove the IAM authorization. Removal of the IAM authorization prevents the other account or service from creating another accessor share and mount target. Then, delete the binding that connects the origin file share to the accessor shares. This action severs the network path, and puts the accessor share and the mount target that is attached to the accessor share in a failed state.

A failed accessor share and its associated mount targets can't become stable again and can't be reused. The accessor account can and must delete these resources.
{: note}

During the beta phase of the regional file share offering, sharing access to a share with other accounts is not supported.
{: beta}

## Removing authorization
{: #file-storage-xaccount-deauth}

[Removing an authorization in the console](/docs/account?topic=account-serviceauth&interface=ui#remove-auth){: ui}[Removing an authorization by using the CLI](/docs/account?topic=account-serviceauth&interface=cli#remove-auth-cli){: cli}[Removing an authorization by using the API](/docs/account?topic=account-serviceauth&interface=api#remove-auth-api){: api}[Removing an authorization by using Terraform](/docs/account?topic=account-serviceauth&interface=terraform#remove-auth-tf){: terraform}

## Deleting the bindings
{: #file-storage-delete-bindings}

1. Select a file share from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view#file-storage-view-shares-targets-ui).
1. On the File share details page, scroll to the Accessor share bindings section to locate the binding that you want to delete.
1. At the end of the row of the binding, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Delete**.
{: ui}

You can delete a share binding by using the `is share-binding-delete` command. See the following example.
{: cli}

```sh
ibmcloud is share-accessor-binding-delete SHARE ACCESSOR_BINDING
```
{: pre}
{: cli}

You can programmatically delete a share binding by calling the `/shares` method in the [VPC API](/apidocs/vpc/latest){: external} as shown in the following sample request.
{: api}

```sh
curl -X DELETE "$vpc_api_endpoint/v1/shares/v1/shares/$share_id/bindings/$binding_id?version=2024-06-21&generation=2"
```
{: pre}
{: api}

If you want to remove a binding by using Terraform, you need to delete the `ibm_is_share_binding` resource. You can use the `terraform destroy` command to conveniently delete a remote object such as a single binding resource.
{: terraform}

```terraform
terraform destroy --target ibm_is_share_binding.example.id
```
{: pre}
{: terraform}
