---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-24"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an IAM access group and granting the role to connect to the VPN server
{: #create-iam-access-group}

FOR USER ID AND PASSCODE AUTHENTICATION ONLY

To create an IAM access group and grant the user role to connect to the VPN server, follow these steps:

1. In the IBM Cloud console, go to the [Access groups](/iam/groups){: external} page (**Manage > Access (IAM) > Access groups**) and click **Create**.
1. Type a name for your access group and an optional description, then click **Create**.
1. Click the **Access** tab, then click **Assign access**.
1. From the menu, select **VPC Infrastructure Services**. Then, click **Next**.
1. For Resources, select **All resources**, then click **Next**.
1. For Roles and actions, select **Users of the VPN server need this role to connect to the VPN server**, then click **Review**.
1. Review the Create policy summary, and click **Add**.
1. In the Access summary side panel, click **Assign**.
3. Add users to your group.

   * For existing users:
      * Click the **Users** tab, then click **Add users**.
      * Select the checkboxes next to each user that requires a VPN Client for VPC access, then click **Add to group**.
   * For new users:
      * Click **Manage > Access (IAM)**, then click **Invite users** in the upper right.
      * Enter the email address of each user that needs to be invited in the "Enter email address" box. Separate emails by commas, spaces, or line breaks. You can enter up to 100 email addresses.
      * In the group table, click the **Add** link next to the new IAM group that you created, then click **Invite**.

        Each user receives an email with a request for them to join an account in IBM Cloud.

For more information, see [Setting up access groups](/docs/account?topic=account-groups). For IAM required permissions and the minimum IAM role to perform a task, see [VPN Client for VPC](/docs/account?topic=account-iam-service-roles-actions#is.vpn-server-roles).
{: note}
