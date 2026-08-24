---

copyright:
  years: 2021, 2026
lastupdated: "2026-08-24"

keywords: snapshots, Block Storage snapshots, share snapshots, cross-account, service-to-service authorization

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Sharing {{site.data.keyword.block_storage_is_short}} snapshots across accounts
{: #snapshots-vpc-share}

You can share snapshots with other {{site.data.keyword.cloud_notm}} accounts by creating service-to-service authorizations. This feature enables cross-account snapshot restore capabilities.
{: shortdesc}

## Sharing a snapshot with another account in the console
{: #snapshots-vpc-s2s-ui}
{: ui}

You can share a snapshot with another account in the console.

1. Go to the list of snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Snapshots**.
2. From the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Share snapshot**.
3. Enter the account ID of the account that you want to share the snapshot with.
4. Click **Create a custom IAM authorization**.

Alternatively, you can create a service-to-service authorization through the **Manage > Access (IAM) > Authorizations** menu. For more information, see [Creating service-to-service authorization for cross-account restore in the console](/docs/vpc?topic=vpc-block-s2s-auth&interface=ui#block-s2s-auth-xaccountrestore-ui).

## Removing sharing permissions for a snapshot in the console
{: #snapshots-vpc-s2s-update-ui}
{: ui}

1. Go to the list of snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Snapshots**.
2. From the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Manage share permission**.
3. The side-panel displays the list of accounts that you shared your snapshot with.

   The list shows all the authorization that were set up for the snapshot. For example, if an account has an authorization for this specific snapshot and an authorization for all snapshots in your account, that account is listed twice.
   {: note}

4. Click **Manage IAM Authorization** to go to the Authorization page to modify or revoke the authorization.

Alternatively, you can manage a service-to-service authorization policy directly through the **Manage > Access (IAM) > Authorizations** menu. For more information, see [Using authorizations to grant access between services](/docs/iam?topic=iam-serviceauth&interface=ui).

## Sharing a snapshot with another account from the CLI
{: #snapshots-vpc-s2s-cli}
{: cli}

You can create a service-to-service authorization for a specific snapshot from the CLI by using the `ibmcloud iam authorization-policy-create` command. For more information, see [Creating service-to-service authorization for cross-account restore from the CLI](/docs/vpc?topic=vpc-block-s2s-auth&interface=cli#block-s2s-auth-xaccountrestore-cli).

## Removing sharing permissions for a snapshot from the CLI
{: #snapshots-vpc-s2s-update-cli}
{: cli}

You can remove a service-to-service authorization for a specific snapshot from the CLI by using the `authorization-policy-delete` command. For more information, see [Removing an authorization by using the CLI](/docs/iam?topic=iam-serviceauth&interface=cli#remove-auth-cli).

## Sharing a snapshot with another account with the API
{: #snapshots-vpc-s2s-api}
{: api}

You can programmatically create a service-to-service authorization for a specific snapshot by calling the `policies` method in the [IAM Policy Management API](/docs/apis/iam-policy-management#create-policy). For more information, see [Creating service-to-service authorization for cross-account restore with the API](/docs/vpc?topic=vpc-block-s2s-auth&interface=api#block-s2s-auth-xaccountrestore-api).

## Removing sharing permissions for a snapshot with the API
{: #snapshots-vpc-s2s-update-api}
{: api}

You can programmatically revoke a service-to-service authorization for a specific snapshot by calling the `policies` method in the [IAM Policy Management API](/docs/apis/iam-policy-management#delete-policy). For more information, see [Removing an authorization by using the API](/docs/iam?topic=iam-serviceauth&interface=api#remove-auth-api).

## Sharing a snapshot with another account with Terraform
{: #snapshots-vpc-s2s-terraform}
{: terraform}

You can create a service-to-service authorization for a specific snapshot by using Terraform. For more information, see [Creating service-to-service authorization for cross-account restore with Terraform](/docs/vpc?topic=vpc-block-s2s-auth&interface=terraform#block-s2s-auth-xaccountrestore-terraform).

## Removing sharing permissions for a snapshot with Terraform
{: #snapshots-vpc-s2s-update-terraform}
{: terraform}

You can remove a service-to-service authorization for a specific snapshot by using Terraform. For more information, see [Removing an authorization by using Terraform](/docs/iam?topic=iam-serviceauth&interface=terraform#remove-auth-tf).

## Next steps
{: #snapshots-vpc-share-next-steps}

* [Manage snapshot tags](/docs/vpc?topic=vpc-snapshots-vpc-tags)
* [Manage fast restore clones](/docs/vpc?topic=vpc-snapshots-vpc-fast-restore)
* [Create remote region copies](/docs/vpc?topic=vpc-snapshots-vpc-remote-copy)
