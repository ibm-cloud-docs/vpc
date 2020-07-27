---

copyright:
  years: 2018, 2020
lastupdated: "2020-07-27"

keywords: vpc, known issues, bugs, defects

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Known issues
{: #known-issues}

Known issues might change over time, so check back occasionally.
{:shortdesc}


## API-1144: Virtual server instances (VSIs) must be stopped before they can be deleted
{: #API-1144}
- **Symptom:** The VSI cannot be deleted.
- **Workaround:** Stop the instance before you attempt to delete it.


## RIOS-129: Inconsistent image names between VPC generation 1 and 2
{: #RIOS-129}
- **Symptom:** The names of stock images are different in VPC generation 1 and VPC generation 2 environments. The expected behavior is that names are consistent across VPC unified images.
- **Fix:** The image service is being rolled out in VPC generation 1 first. When the service is deployed in VPC generation 2, unified image names will match, as the same image service will be running in both environments.
