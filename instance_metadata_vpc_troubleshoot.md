---

copyright:
  years: 2021
lastupdated: "2021-09-10"

keywords: metadata, virtual private cloud, instance, virtual server, troubleshooting, troubleshoot

subcollection: vpc


---

{:support: data-reuse='support'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:beta: .beta}
{:note:.deprecated}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Troubleshooting the Instance Metadata service (Beta)
{: #imd-troubleshoot}

When you configure and use the Instance Metadata service, you might encounter issues. Often, you can recover by following a few steps. Issues, symptoms, likely causes, and resolutions are described in the following sections.
{: shortdesc}

This service is available only to accounts with special approval to preview this beta feature.
{: beta}

## Disabling the metadata service hangs a virtual server instance
{: #imd-ts-1}
{: troubleshoot}
{: support}

You disabled the metadata service using a `POST /instance` call and specify the `metadata_service` parameter in the data section of the request, setting `enabled` to `false`. The operation succeeded but the instance would not reach a running state.
{: tsSymptoms}

For Beta, the metadata service is enabled by default. Trying to disable the service in an allow-listed account is not permitted.
{: tsCauses}

To disable the service, use an account not on the Beta allow-list.
{: tsResolve}
