---

copyright:
  years: 2018, 2021

lastupdated: "2021-12-13"

keywords:  

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Updating the default security group
{: #updating-the-default-security-group}

The default security group is similar to any other security group, with the exception that it cannot be deleted.
{: shortdesc}

Each VPC has a default security group, with rules to allow:

* Inbound traffic from all members of the group (that is, all other virtual server instances that are attached to this security group)
* All outbound traffic

If you edit the rules of the default security group, those edited rules then apply to all current and future servers in the group.

Inbound rules to allow pinging and SSH are not automatically added to the default security group. You can modify the rules of the default security group by using the UI, CLI, or API.

## Updating the default security group by using the CLI
{: #example-modifying-the-default-security-group-rules-using-the-cli}
{: cli}

1. Log in to {{site.data.keyword.vpc_full}}.

   If you have a federated account:

   ```sh
   ibmcloud login -sso
   ```
   {: pre}

   Otherwise, use this command:

   ```sh
   ibmcloud login
   ```
   {: pre}

2. Get the default security group ID and details for the VPC

   Run the following command to list all VPCs:

   ```sh
   ibmcloud is vpcs
   ```
   {: pre}

   The default security group name is shown under the column `Default Security Group`. Note the name of it so that you can find the `ID` when you list the security groups (next).

   Now list all the security groups in the VPC:

   ```sh
   ibmcloud is security-groups
   ```
   {: pre}

   Save the security group ID (for the default security group) in a variable so that you can use it later. For example, use the variable name `sg`:

   ```sh
   sg=0738-2d364f0a-a870-42c3-a554-000001162469
   ```
   {: pre}

   To get details about the security group, run the following command:

   ```sh
   ibmcloud is security-group $sg
   ```
   {: pre}

   Alternatively, you can insert the actual ID value in place of the variable `$sg`.

3. Update the default security group to add rules that allow SSH and PING.

   Disabling SSH connections prohibits the license registration for RedHat Enterprise Linux. This can result in provisioning failures.
   {: important}

   ```sh
   ibmcloud is security-group-rule-add $sg inbound tcp --port-min 22 --port-max 22
   ibmcloud is security-group-rule-add $sg inbound icmp --icmp-type 8 --icmp-code 0
   ```
   {: codeblock}

Adding and removing security group rules is an asynchronous operation. It usually takes 1 - 30 seconds for the change to go into effect.
{: note} 
