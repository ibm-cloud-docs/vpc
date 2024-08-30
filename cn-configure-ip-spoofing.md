---

copyright:
  years: 2024
lastupdated: "2024-08-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring IP spoofing on a cluster network
{: #configure-ip-spoofing-cluster-network}

This service is available only to customers with special approval. Contact your IBM Support representative if you are interested in getting early access to this offering.
{: preview}

[Need to circle back w/ Simon/Henry - Eran/Amitabha/Research discussion -- Need to know if we still need this topic]{: tag-green}
[LINK TO SLACK CHANNEL CHAT](https://ibm-cloudplatform.slack.com/archives/C06EQCA95ND/p1724100395051779?thread_ts=1722264782.119259&cid=C06EQCA95ND){: external}

Configure IP spoofing on a cluster network.
{: shortdesc}

## Before you begin
{: #config-ip-spoofing-prerequisites}

* Before you configure IP spoofing on a cluster network, review [Planning considerations](/docs/vpc?topic=vpc-planning-cluster-network&interface=ui) and [Known issues and limitations](/docs/vpc?topic=vpc-limitations-cluster-network&interface=ui).

You can configure IP spoofing on a cluster network with the UI, CLI, API, or Terraform.

## Configuring IP spoofing on a cluster network in the UI
{: #configure-ip-spoofing-cluster-network-ui}
{: ui}

[To be documented]{: tag-red}

## Configuring IP spoofing on a cluster network in the CLI
{: #configure-ip-spoofing-cluster-network-cli}
{: cli}

To configure IP spoofing on a cluster network in the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your account with the CLI. After you enter the password, the system prompts for the account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. Enable the following feature flag:

    ```sh
   export IBMCLOUD_IS_FEATURE_CLUSTER_NETWORK=true
   ```
   {: pre}

1. To configure IP spoofing on a cluster network, enter the following command:

   ```bash
   ibmcloud is XXXX
   ```
   {: pre}

   Where:

   `--xx`
   :    Description

   `--xx`
   :    Description

## Configuring IP spoofing on a cluster network with the API
{: #configure-ip-spoofing-cluster-network-api}
{: api}

To configure IP spoofing on a cluster network with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

    * `version` (string): The API version, in format `YYYY-MM-DD`.
    * _Any others?_

1. When all variables are initiated, run the following command to configure IP spoofing on a cluster network:

```sh
NEED CURL EXAMPLE
```
{: codeblock}

## Configuring IP spoofing on a cluster network with Terraform
{: #configure-ip-spoofing-cluster-network-terraform}
{: terraform}

Terraform will support this feature after it reaches General Availability (GA) and is officially released.
{: note}
