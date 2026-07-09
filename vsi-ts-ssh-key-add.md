---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-09"

keywords: troubleshoot virtual server instance, SSH key, can't add SSH key, SSH key parse error, line breaks SSH key

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I add my SSH key?
{: #troubleshooting-cant-add-ssh-key}
{: troubleshoot}
{: support}

You try to add an SSH key to your account, but receive an error that the key can't be parsed.
{: shortdesc}

You try to add an SSH key to your account and receive an error stating that the key can't be parsed.
{: tsSymptoms}

Line breaks were introduced when the SSH key was copied from a terminal.
{: tsCauses}

Make sure that the string contains no line breaks. An SSH key is a continuous string of characters. Sometimes line breaks are introduced when the SSH key is copied from a terminal. To avoid this issue, first paste your SSH key into a text editor and remove any line breaks. Then, copy the SSH key from the text editor and paste it into the VPC UI, CLI, or API.
{: tsResolve}
