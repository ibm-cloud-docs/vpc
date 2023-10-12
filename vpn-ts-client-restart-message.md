---

copyright:
  years: 2021, 2023
lastupdated: "2023-01-31"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I see a VPN client restart message when certificate authentication is used?
{: #troubleshoot-client-restart}
{: troubleshoot}
{: support}

When certificate authentication is enabled on the VPN server, and the certificate that is presented by the VPN client cannot be verified, the VPN server directs the VPN client to restart.
{: shortdesc}

The VPN client logs the following message `Connection reset, restarting`.
{: tsSymptoms}

The client certificate cannot pass verification.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Use the Linux command `openssl x509 -noout -text -in certificate-file-name` to get the certificate information.
1. Check that the issuer of the client certificate is matched with the client root certificate that you chose when you provisioned the VPN server.
1. Check that the certification extension `X509v3 Extended Key Usage` is `TLS Web Client Authentication`.
