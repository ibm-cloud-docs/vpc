---

copyright:
  years: 2018, 2021
lastupdated: "2021-01-27"

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

## RIOS-1410: Checksum not available for some public images
{: #RIOS-1410}

- **Symptom:** When you use the API or CLI to list images, some public stock images might not include a checksum. 
- **Fix:** The checksum is for informational purposes only for stock images. No fix is available at this time.

## API-1144: Virtual server instances must be stopped before they can be deleted
{: #API-1144}
- **Symptom:** The virtual server instances cannot be deleted.
- **Workaround:** Stop the instance before you attempt to delete it.
