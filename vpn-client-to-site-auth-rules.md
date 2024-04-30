---

copyright:
  years: 2021
lastupdated: "2021-06-07"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Using VPN client authorization rules
{: #client-vpn-auth-rules}

Each network must have an authorization rule that grants access to the network.
{: shortdesc}

## Adding authorization rules in the UI
{: #ui-add-auth-rule}
{: ui}

To add authorization rules and grant access to a network to a Client VPN endpoint, take the following steps:
1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account
2. From the dashboard, click the Menu icon ![Menu icon](../icons/icon_hamburger.svg) and select **Client VPN**
3. Select the Client VPN endpoint where you want to add the authorization rule (DEPENDS ON UI—steps might be more complex)
4. Select Authorize ingress (may be different name)
5. Enter an IP address in CIDR format for your destination network
6. Choose whether to grant access to:
   - All users
   - Users in a specific access group
7. Enter a brief description of the authorization rule
8. Click **Add authorization rule**

## Adding authorization rules fronm the CLI
{: #cli-add-auth-rule}
{: cli}

Add an authorization rule using the `authorize-client-vpn-ingress` command.

## Removing authorization rules in the UI
{: #ui-remove-auth-rule}
{: ui}

When you remove authorization rules, you remove access to the network.

To remove authorization rules and grant access to a network to a Client VPN endpoint, take the following steps:
1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account
2. From the dashboard, click the Menu icon ![Menu icon](../icons/icon_hamburger.svg) and select **Client VPN**
3. Select the Client VPN endpoint where the authorization rule is applied (DEPENDS ON UI—steps might be more complex)
4. Select Revoke ingress (may be different name)

## Removing authorization rules by using the CLI
{: #cli-remove-auth-rule}
{: cli}

Remove an authorization rule using the `revoke-client-vpn-ingress` command.

## Viewing authorization rules in the UI
{: #ui-view-auth-rule}
{: ui}

To view authorization rules and grant access to a network to a Client VPN endpoint, take the following steps:
1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account
2. From the dashboard, click the Menu icon ![Menu icon](../icons/icon_hamburger.svg) and select **Client VPN**
3. Select the Client VPN endpoint where the authorization rule is applied (DEPENDS ON UI—steps might be more complex)
4. Select Authorization (may be different name)to see a list of authorization rules for the Client VPN

## Viewing authorization rules from the CLI
{: #cli-view-auth-rule}
{: cli}

View authorizations rule using the `list-client-vpn-authorization-rules` command.
