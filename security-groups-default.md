---

copyright:
  years: 2018, 2019

lastupdated: "2019-07-29"

keywords: default, security group, asynchronous, rules, vpc, vpc network

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Updating the default security group
{: #updating-the-default-security-group}


The default security group is very similar to any other security group, with the exception that it cannot be deleted.
{:shortdesc}

Each VPC has a default security group, with rules to allow:

* Inbound traffic from all members of the group.
* All outbound traffic.

If you edit the rules of the default security group, those edited rules then apply to all current and future servers in the group.

Inbound rules to allow ping and SSH are not automatically added to the default security group. You can modify the rules of the default security group using the REST API, the `ibmcloud cli`, or the UI.

## Example: Modifying the default security group rules using the CLI
{: #example-modifying-the-default-security-group-rules-using-the-cli}

1. Log in to IBM Cloud.

   If you have a federated account:
   ```
   ibmcloud login -sso
   ```
   {: pre}

   Otherwise, use this command:

   ```
   ibmcloud login
   ```
   {: pre}

2. Get the default security group ID and details for the VPC

   Run the following CLI command to list all VPCs:

   ```
   ibmcloud is vpcs
   ```
   {: pre}

   The default security group name is shown under the column `Default Security Group`. Note the name of it so that you can find the `ID` when you list the security groups (next). 
   
   Now list all the security groups in the VPC:

   ```
   ibmcloud is security-groups
   ```
   {: pre}

   Save the security group ID (for the default security group) in a variable so you can use it later. For example, using the variable name `sg`:

   ```
   sg=0738-2d364f0a-a870-42c3-a554-000001162469
   ```
   {: pre}

   To get details about the security group, run the following CLI command:

   ```
   ibmcloud is security-group $sg
   ```
   {: pre}
   
   Alternatively, you could insert the actual ID value in place of the variable `$sg`.

3. Update the default security group -- add rules allowing SSH and PING

   ```
   ibmcloud is security-group-rule-add $sg inbound tcp --port-min 22 --port-max 22
   ibmcloud is security-group-rule-add $sg inbound icmp --icmp-type 8 --icmp-code 0
   ```
   {: codeblock}


Adding and removing security group rules is an asynchronous operation. It usually takes from 1 to 30 seconds for the change to go into effect.
{: note}
