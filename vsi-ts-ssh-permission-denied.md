---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-09"

keywords: troubleshoot virtual server instance, SSH key permission denied, known hosts, authorized host, ssh-keyscan

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I receive an SSH key permission denied error?
{: #troubleshooting-ssh-key-permission-denied-error}
{: troubleshoot}
{: support}

You receive a permission denied error when you try to connect to your virtual server instance by using SSH.
{: shortdesc}

You receive an SSH key permission denied error when you try to connect to your virtual server instance.
{: tsSymptoms}

Your host might not be recognized as an authorized host.
{: tsCauses}

Add your host as a known host by running the following command in your terminal.
{: tsResolve}

Replace `[sFTP]` with your host.

```sh
ssh-keyscan -t rsa [sFTP] >> ~/.ssh/known_hosts
```
{: pre}


For troubleshooting information about z/OS virtual server instances, see [{{site.data.keyword.waziaas_full_notm}} documentation](https://www.ibm.com/docs/en/wazi-aas/1.1.0?topic=known-limitations){: external}.
