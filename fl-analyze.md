---

copyright:
  years: 2019, 2020
lastupdated: "2020-04-22"

keywords: flow logs, configure, viewing records

subcollection: vpc
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank_" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Viewing flow log objects
{: #fl-analyze}

Flow log objects summarize the traffic sent and received by a VNIC/VPE in a given time interval. Each object contains individual flow logs.

To view these flow logs:

1. Download the objects from your designated Cloud Object Storage (COS) bucket using the COS UI, CLI, or API.
1. Extract and view the downloaded flow log objects in JSON format.
1. Leverage command line tools, such as **jq** or **grep** to search the downloaded flow log objects.
