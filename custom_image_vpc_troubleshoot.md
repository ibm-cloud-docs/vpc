---

copyright:
  years: 2021
lastupdated: "2021-08-20"

keywords: custom image, image from volume, troubleshooting, troubleshoot

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
{:note:.deprecated}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Troubleshooting custom images
{: #ifv-troubleshooting-custom-images}

When you create or manage custom images, you might encounter issues. Issues, symptoms, likely causes, and resolutions are described in the following sections.
{: shortdesc}

## Cannot retrieve a volume in a specified region
{: #troubleshoot-ifv1}
{: troubleshoot}
{: support}

When creating a new image from an existing volume, specifying an encryption key or encrypted source volume will result in the new image being encrypted. Volumes used as the source for new encrypted images must currently have 100 GB capacity, but the one specified in the image does not.
{: tsSymptoms}

Custom images can be between 10 - 250 GB but volumes used as source for new encrypted images must be 100 GB capacity.
{: tsCauses}

Specify a source volume with 100 GB capacity. Optionally, resize the source volume and try the request again.
{: tsResolve}
