---

copyright:
  years: 2022
lastupdated: "2022-11-22"

keywords: troubleshoot, troubleshoot hyper protect virtual servers for vpc, debug hyper protect virtual servers for vpc, questions about hyper protect virtual servers for vpc, hyper protect virtual servers decryption failure

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I see decryption failure errors in the serial console?
{: #hyper-protect-virtual-server-decryption-failure}
{: troubleshoot}
{: support}

You see decryption failure errors in the serial console of your {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} for VPC instance.
{: tsSymptoms}

The contract isn't encrypted by the correct key.
{: tsCauses}

If you opt to use contract encryption, you must take the [correct key](/docs/vpc?topic=vpc-about-contract_se#encrypt_downloadcert). The contract is decrypted by the bootloader and if it's not encrypted by the correct key, you see decryption failures errors in the serial console of the instance.
{: tsResolve}
