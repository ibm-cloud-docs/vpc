---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-03"

keywords: confidential computing, secure execution, logging for hyper protect virtual server for vpc

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Logging for {{site.data.keyword.hpvs}} for VPC
{: #logging-for-hyper-protect-virtual-servers-for-vpc}

To launch a {{site.data.keyword.hpvs}} for VPC instance, you (as the deployer) need to set up logging first by adding the logging configuration in the `env` section of the [contract](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_env). The instance reads the configuration and configures logging accordingly. All other services start only after logging is configured. If the logging configuration is incorrect, the instance will not start and an error message will be displayed in the serial console.
{: shortdesc}

The logs include startup logs, service logs issued by the {{site.data.keyword.hpvs}} for VPC instance, and container logs.

If your workload produces sensitive information, you can [encrypt your log messages](/docs/vpc?topic=vpc-encrypt-log-messages-for-hyper-protect-virtual-servers-for-vpc).
{: tip}

The following logging services are supported.

## {{site.data.keyword.logs_full_notm}} (ICL)
{: #cloud-logs}

{{site.data.keyword.logs_full}} is a scalable logging service that persists logs and provides users with capabilities for querying, tailing, and visualizing logs.

Logs are comprised of events that are typically human-readable and have different formats, for example, unstructured text, JSON, delimiter-separated values, key-value pairs, and so on. The {{site.data.keyword.logs_full_notm}} service can manage general-purpose application logs, platform logs, or structured audit events. {{site.data.keyword.logs_full_notm}} can be used with logs from both IBM Cloud services and customer applications.

For information about {{site.data.keyword.logs_full_notm}}, see [{{site.data.keyword.logs_full_notm}}](/docs/cloud-logs).

### Access to IBM Cloud Logs:
{: #cloud-logs-access}

Access to {{site.data.keyword.logs_full_notm}} instances for users in VPC account is controlled by {{site.data.keyword.iamlong}} (IAM). Every user that accesses the {{site.data.keyword.logs_full_notm}} in VPC account must be assigned an access policy with an IAM role.

#### Access policies to be added:
{: #cloud-logs-accesspolicies}

 - Cloud Logs



To add relevant roles for these policies, refer to [Managing IAM access for IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-iam-assign-access&interface=ui)



### Provisioning an ICL instance
{: #cloud-logs-provision-instance-ui}

To provision an `ICL` instance from the Observability dashboard in the IBM Cloud, complete the following steps:

1. [Log in to your IBM Cloud account.](https://cloud.ibm.com/login)
2. [Provision an ICL instance](/docs/cloud-logs?topic=cloud-logs-instance-provision&interface=ui). Choose a plan according to your requirements.

#### The `logging` subsection
{: #cloud-logs-provision-instance-ui_exmpl}

```yaml
env: |
   type: env
   logging:
     logRouter:
       hostname: <host name of the service instance> /
       iamApiKey: <iamApiKey of the service instance> / xxxx
       port: <port of the service instance(443)>
```
{: codeblock}

Input parameters to be provided in the `env` section are:

- `hostname`: Hostname of the service instance. Use the endpoint under the ICL instance's Endpoint Section.

   - For public network ICL access: Select the **Public ingress endpoint** section as the hostname in the contract.

   - For private network ICL access: Select the **Private ingress endpoint** section as the hostname in the contract.


   You must create a virtual private endpoint (VPE) gateway for ICL to access {{site.data.keyword.logs_full_notm}} privately. For more information, see [Using virtual private endpoints for VPC to privately connect to {{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-vpe-connection&interface=cli).
   {: note}

- `iamApiKey`: The IAM API key for the service ID. Generate and retrieve the API key from the service ID in IAM.

- `port`: (Optional) The port of the service instance that is 443.

For more information, see the [logging subsection](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_icl).

Custom tags support characters from A-Z, a-z, 0-9, and a hyphen (-).
{: Note}

To see the logs, open ICL [instance](https://cloud.ibm.com/observability/logging).

### Applying Filters in an ICL Instance
{: #cloud-logs-filters}

**Add Columns for Log Messages: Include the following columns to display detailed log messages:**
- Timestamp | _HOSTNAME | Syslog_Identifier | Severity | Message

**Filter by Hostname:**
- Use the _HOSTNAME filter and select the desired hostname to view logs from a specific source.

**Filter by Log Level:**
- Apply the Severity filter and choose the appropriate log level to filter messages based on severity.

**Filter By Service:**
- Add the Syslog_Identifier filter to see logs from a specific service.

**Filter by Image Name, Container Name, and Container ID:**
- Add filters for IMAGE_NAME, CONTAINER_NAME, and CONTAINER_ID to refine the logs further.

**View Additional Log Details:**
- Select the Text option in the column to see more details about each log entry.

**Explore Other Filters:**
- Additional filters are available and can be applied based on specific requirements.

For more information, see [Managing {{site.data.keyword.logs_full_notm}} logging instances](/docs/cloud-logs?topic=cloud-logs-observe&interface=ui#observe_manage).

#### Steps to migrate for existing customers:
{: #cloud-logs-migrate}

- Create an [ICL](https://cloud.ibm.com/catalog/services/cloud-logs) instance
- Prepare a contract ensuring that the logging section is set to ICL
- Bring down the current HPVS, without deleting the data volumes
- Create new HPVS attaching the existing volumes with the new contract

## Syslog
{: #syslog}

You can also configure logging with a generic [syslog backend](https://datatracker.ietf.org/doc/html/rfc5424) such as an [rsyslog](https://www.rsyslog.com/) server or a [Logstash](https://www.elastic.co/logstash/) server. The {{site.data.keyword.hpvs}} for VPC instance uses TLS with [mutual authentication](https://en.wikipedia.org/wiki/Mutual_authentication) to connect to the logging backend. Find the following pieces of information to configure logging:

- Syslog hostname
- [Optional] port and it defaults to 6514
- Certificate Authority (CA) - the certificate used to verify the certificate chain both for client and server authentication. Note that the same CA must be used for both the client and server certificates.
- Client certificate - used to prove the client to the server, signed by the CA
- Client key - private key used by the virtual server instance to establish trust

Fill in the following parts of the contract with the information. The certificates and the key must be in PEM format.

```yaml
env:
  logging:
    syslog:
      hostname: ${HOSTNAME}
      port: ${PORT}
      server: ${CA}
      cert: ${CLIENT_CERTIFICATE}
      key: ${CLIENT_PRIVATE_KEY}
```
{: codeblock}

Make sure to use a strong digest algorithm for the certificates, otherwise the syslog server might reject the certificates.
{: note}

### Example
{: #syslog-example}

You can follow the following procedure to create the required certificates and keys. The example uses [openssl](https://www.openssl.org/) and shows [bash](https://www.gnu.org/software/bash/) syntax.

#### Preparation
{: #syslog-preparation}

1. Create a CA private key and a certificate signing request (CSR).

   Prepare the `ca.cnf` configuration file:

   ```toml
   [ req ]
   default_bits = 2048
   default_md = sha256
   prompt = no
   encrypt_key = no
   distinguished_name = dn

   [ dn ]
   C = US
   O = Logstash Test CA
   CN = ca.example.org
   ```
   {: codeblock}

   Make sure to update `dn` with your values. The actual values can be freely selected and they do not play a role for the subsequent processing.
   {: note}

   Create the key and certificate.

   ```bash
   # create private key
   openssl genrsa -out ca-key.pem 4096
   # create CSR
   openssl req -config ca.cnf -key ca-key.pem -new -out ca-req.csr
   # create self-signed CA
   openssl x509 -signkey ca-key.pem -in ca-req.csr -req -days 365 -out ca.crt
   ```
   {: codeblock}

2. Create files used on the **server side** (the rsyslog server).

   Prepare the `server.cnf` configuration file. It's important to set the `default_md` value to at least `sha256`. Make sure to fill in the correct information for the `dn` field. It's preferred to use a domain name for `CN` but an IP address works too. For more information, see the OpenSSL documentation on [Subject Alternative Name](https://docs.openssl.org/master/man5/x509v3_config/#subject-alternative-name){: external}.

   Example that uses a hostname:
   ```toml
   [ req ]
   default_bits = 2048
   default_md = sha256
   prompt = no
   encrypt_key = no
   distinguished_name = dn

   [ server ]
   subjectAltName = DNS:${HOSTNAME}
   extendedKeyUsage = serverAuth

   [ dn ]
   C = US
   O = Rsyslog Test Server
   CN = ${HOSTNAME}
   ```
   {: codeblock}

   Example that uses an IP address:
   ```toml
   [ req ]
   default_bits = 2048
   default_md = sha256
   prompt = no
   encrypt_key = no
   distinguished_name = dn

   [ server ]
   subjectAltName = IP:${IP}
   extendedKeyUsage = serverAuth

   [ dn ]
   C = US
   O = Rsyslog Test Server
   CN = ${IP_OR_HOSTNAME}
   ```
   {: codeblock}

   Create the key and certificate. Make sure the server certificate `server.crt` contains a SAN for the IP or the hostname, depending on whether the server is accessed through IP or hostname.

   ```bash
   # create private key
   openssl genrsa -out server-key.pem 4096
   # create CSR for the server certificate
   openssl req -config server.cnf -key server-key.pem -new -out server-req.csr
   # have the CA created in (1) sign the certificate
   openssl x509 -req -in server-req.csr -days 365 -CA ca.crt -CAkey ca-key.pem -CAcreateserial -extfile server.cnf -extensions server -out server.crt
   ```
   {: codeblock}

3. Create files used on the **client side** (the {{site.data.keyword.hpvs}} for VPC instance).

   Prepare the `client.cnf` configuration file:

   ```toml
   [ req ]
   default_bits = 2048
   default_md = sha256
   prompt = no
   encrypt_key = no
   distinguished_name = dn

   [ dn ]
   C = US
   O = Logstash Test Client
   CN = client.example.org
   ```

   Make sure to update `dn` with your values. Whether the actual values play a role depends on the `StreamDriver.Authmode` setting (which appears in the following documentation). In this example, we use the setting `StreamDriver.Authmode="x509/certvalid"` and in this case, the value of `dn` does **not** play a role (since all valid client certificates are accepted). Adjust this according to your needs. For more information, see [StreamDriver.Authmode](https://www.rsyslog.com/doc/configuration/modules/imtcp.html#streamdriver-authmode){: external}.
   {: note}

   Create the key and certificate:

   ```bash
   # create private key
   openssl genrsa -out client-key.pem 4096
   # create CSR for client auh
   openssl req -config client.cnf -key client-key.pem -new -out client-req.csr
   # have the CA created in (2) sign the certificate
   openssl x509 -req -in client-req.csr -days 365 -CA ca.crt -CAkey ca-key.pem -CAcreateserial -out client.crt
   # export key to PKCS#8 format
   openssl pkcs8 -topk8 -inform PEM -outform PEM -nocrypt -in client-key.pem -out client-key-pkcs8.pem
   ```

#### Client setup
{: #syslog-client-setup}

Configure the contract with the following template.

```yaml
env:
  logging:
    syslog:
      hostname: ${HOSTNAME}
      port: ${PORT}
      server: ${CA}
      cert: ${CLIENT_CERTIFICATE}
      key: ${CLIENT_PRIVATE_KEY}
```
{: codeblock}

Use the content of the following files in preparation to fill in the placeholders:
- `${CA}` - `ca.crt` from preparation step 1
- `${CLIENT_CERTIFICATE}` - `client.crt` from preparation step 3
- `${CLIENT_PRIVATE_KEY}` - `client-key-pkcs8.pem` from preparation step 3

`${HOSTNAME}`, `${CA}`, `${CLIENT_CERTIFICATE}`, and `${CLIENT_PRIVATE_KEY}` are strings without extra encoding or escaping. Regardless of the way you format them, make sure that you use **valid YAML** (see [Scalars](https://yaml.org/spec/1.2.2/#scalars){: external}). In the following example, the new lines are replaced with `\n` and carriage returns are deleted to make sure the content fits in one line between the inverted commas (see [scalars in double-quoted style](https://yaml.org/spec/1.2.2/#double-quoted-style){: external}). You can also use other valid YAML variations.

Example:
```yaml
env:
  logging:
    syslog:
      hostname: ${HOSTNAME}
      port: 6514
      server: "-----BEGIN CERTIFICATE-----\nMIIFCTCCAvECFEp7wJLz4jNStIsVH2dUeHDN26ZyMA0GCSqGSIb3DQEBCwUAMEEx\nCzAJBgNVBAYTAlVTMRkwFwYDVQQKDBBMb2dzdGFzaCBUZXN0IENBMRcwFQYDVQQD\nDA5jYS5leGFtcGxlLm9yZzAeFw0yMzAxMDUxNjU0MTNaFw0yNDAxMDUxNjU0MTNa\nMEExCzAJBgNVBAYTAlVTMRkwFwYDVQQKDBBMb2dzdGFzaCBUZXN0IENBMRcwFQYD\nVQQDDA5jYS5leGFtcGxlLm9yZzCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCCAgoC\nggIBANe7PR4XaTXtF6h3FhWe/R4BSTVylXWopA51+ppcJ3BOMPjmRMNJ3tFAFE3h\nF4d0RHBNJOZF0+ogT0ZEseTe4mqJXk3RgfMSrLaymNgzaefD67uhQ9ZzznE3kIXe\nmzh/A8aDwhaUMifIKxekisrmpvjDwUJtaSs3pb27W+cOmzAPZ3cmOs09tELLY134\nf52sp0ZqFSOgvCwcdt88PFVMm2rrFgwxP2gLgOkZL4OsM9sQykYEPR28unS+P90V\nqnYPy27xqJNss4OdZCJrjkS7lv2PbBxSoFjDq/yLjnDV8khW2+6w0MFRvamoKL34\nn/XoXN6VCathSxcwvXg0x3wwTxa5Hevb0iziNGXHjZ9bXt+8bnu/Bhsa7KwaoUt9\nrJJeMy0KNsdQyWhMJE904YKm9Eo/S92rrcNWzmzBIV0iecOHc24iw3SIXOnoAKNY\n1GtDOQChSEeb7en25s1fjWTqIDyDOktWjp9DXu1ips9YDb7GKZ7raOoQnsPkGrRE\nKOWClkWQ4qIXJ9LH73ytR1h8+AsGyInaan5ehnz7JC5SFhE96wPzJDaXCKNHBP/e\ntfwQ0BTbgO6z8gPE8JlPGXTmdf9YF5NxMd4oJA7u7Y6x2y4KIRYacrcevDxe/lFk\n843MwiYU2atYgqgFK07BIOHNvqv93WiqXy8WAolSmMoJ/eqdAgMBAAEwDQYJKoZI\nhvcNAQELBQADggIBAFkQpmW3T50eI5AhAOzN6duxQtjDuIE4AhcQaejIVFu8R9H4\nGKw8WQo1DO7jaefRK7BFy68u8Cacgyn6btCoA0AMuKYyt1StM4Jzf2ZxWrox0Tl+\nUW5RJFP8HoIBQutMtgHaY3hWZJ4Jvcg6y7kroMynZnsV3jbK0/GmthtUYonjCpCH\nuC1rEp/0Gkp9BPnrY6cgyRdDbgmDo3YMqmUh8BqTGLEi+F45K/PEN502kUBcMJTY\nvpWVfgMz7nQhN4temIlQQDs8gu3LBt9lxomuMXtYkTq245LfXdtbPPkrwjvbIKzM\nFasa1PkTmK23cXLpRWfNUu/JHChpCl27Yg8ScTm6GV/eKhJbtku8ExvLWgHAITGi\n5Rhh2Pl//Jh4szzTL34IY5bPqSXMrqUB3vFzND5ybmWrwo0i2CyLS+gKgGqzz0Xt\nmvQ6XCiq7EsTdNLlX1ZDWjias12EIyCVbWrmxzFsR/Ji8XQqUoKK+QXhdsQ/uA4H\nn72jGuWsjhRAKE7WyI2i1H+TPRZ+K6VaKeS2aikC3p2JCU4VxrP2jSdWhdYYwEgL\nC/mjDXOjxbr6TIOtrxQQSBplTuRz8yNabrPB6G2xN+e70qpB1KT5w2ee1RH7M1o+\nUoeDoSwqneVvONAjQn/0KKL0Y7P2BURHjJeWsLtFmyUZVlkqlosg8P7Nabrj\n-----END CERTIFICATE-----\n"
      cert: "-----BEGIN CERTIFICATE-----\nMIIFETCCAvkCFBhx5DuYtRzCxRx8Bo+WIS2LFI2uMA0GCSqGSIb3DQEBCwUAMEEx\nCzAJBgNVBAYTAlVTMRkwFwYDVQQKDBBMb2dzdGFzaCBUZXN0IENBMRcwFQYDVQQD\nDA5jYS5leGFtcGxlLm9yZzAeFw0yMzAxMDUxNjU1MzZaFw0yNDAxMDUxNjU1MzZa\nMEkxCzAJBgNVBAYTAlVTMR0wGwYDVQQKDBRMb2dzdGFzaCBUZXN0IENsaWVudDEb\nMBkGA1UEAwwSY2xpZW50LmV4YW1wbGUub3JnMIICIjANBgkqhkiG9w0BAQEFAAOC\nAg8AMIICCgKCAgEArY+N+3IEYrIQpdMPT6xMqksSS43g2+44EdYiPNonP2KjEUdG\n/g57CBnIOaUfZyvg3Fc9ROBMhqYa6CGKCd8Yec7mL4c97tS9wBtc6I0eEBmXAeZz\nCGy6/1HtScZ/mAHw9rshgwF1Si/j9R4NA6ZepmGvoQMdUOGJJHhSsEfovVoRR5D/\nVO1Urpu4LYnz7Zwo+/QEzJbUmSVN52/tNWjgTlHFCOdQ6aZCkwBK2DCa3b2NFWSO\nvfhhl/4GLLeYZ2hG2f9+W+L6slLMWwpMqywY9bUXn+NROcYPoLX222saH6nY74+0\n+B9Qk4BOlNqhN493FCoXq2uzGmYb0igOx+o8UVc7gcozbhvREgW4RRa4XgamonSW\nCccM5sQFHFSxaGZXokpE2GJtQT0wv/pim4Ku0XEIQKmZGejC1dw26fbG+CWDWPXs\nYEmf4S5Z5exjSiQCPL2QawCXgGEJNkiUMj5ld3jeb2kY221IKb+uSE6waNTts4nO\nRa9DveHrUKrNqq33yoEZvj4K/QZ9px9km7o0BhR+oMPAYI/YODgNOhQLSR3q2n1C\njd+baFfg4Sb8L2OUTyW4Kd2Ok9rkCk9W/8T/YKlrBFoSrNHtPhKIi6FZLkrodn1g\n8+lPo3E80Gn5hUZdgsIZ7Y+c2qcUAVu8X/otFaWm8FCmIDyz+ZKm/YgaX4UCAwEA\nATANBgkqhkiG9w0BAQsFAAOCAgEACg4PxboxM01cO3pmhTfvwetBvICz8GOuAq3f\nLWYFXcZmnMHqwDZKOx20a03XfcBaWhyF9XHdCugziEMXTdfKxGwFsIUxQIbDBT4N\nBNCcLTXiTEdtjvXxm0TnM5QdPOE36EsI+O4YT8w+C5nlKuNMtsxsJe+bxEfBi2PS\nJ0vU1bO+4m9p0SDc3h39jb+FLrAnqez2QbT2maby8A8wahunAMWY+ZUkQYoWpilf\nRkGpiLKlkJ95HCYzmt7IeddH5+ZBuG+Sx4SMwCynn64J/UafNW0XV36dzeLSla59\nvQCmWAurjAqa8fqepdvNI4I/JxVfeCQwkrZEos0gec+D7qOupfHk3Zyr6G5Zn8kS\nYRU8HpRIRH4KvsObTNIrW5Z/qbfWAFSTC3q0eflaVLWsrXfSGvBDVqlxO3arhv2q\nra6tcD7MQOBO226i+v3aL9qJ3viWhIQTvONm7D8U+/WryrChBOHVCQ2M3AZQQLeC\nqSkJ60wFx8jEqLj9ELWuTMuHYg5lhMZFyLI8iWOvGRPmgTUZKNH74LF1ujIEuMBx\nE7LBWRGNx2lD0f2aYUdv+qWA8m1ETPyKYme6oUM+kDlf6sstMgahN7zT8jj/W2KD\ndG+yzHk5G06lSQzXGbec3bi2WOpWHJ1J/kTQ8Af1HuJr4UjQmin8fLW6n06diySA\nBSHYGWk=\n-----END CERTIFICATE-----\n"
      key: "-----BEGIN PRIVATE KEY-----\n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\nhJITxResgCTR2aD0nMzF8egEKJoY\n-----END PRIVATE KEY-----\n"
```

You can give the syslog certificates and the key in Base64 format as well.
To generate it in Base64, use the following command:

```sh
cat ${CLIENT_PRIVATE_KEY} | base64 -w0
```

Configure the contract with Base64 encoded certificates:
```yaml
env:
  logging:
    syslog:
      hostname: ${HOSTNAME}
      port: 16999
      server: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUZDVENDQXZFQ0ZFcDd3Skx6NGpOU3RJc1ZIMmRVZUhETjI2WnlNQTBHQ1NxR1NJYjNEUUVCQ3dVQU1FRXgKQ3pBSkJnTlZCQVlUQWxWVE1Sa3dGd1lEVlFRS0RCQk1iMmR6ZEdGemFDQlVaWE4wSUVOQk1SY3dGUVlEVlFRRApEQTVqWVM1bGVHRnRjR3hsTG05eVp6QWVGdzB5TXpBeE1EVXhOalUwTVROYUZ3MHlOREF4TURVeE5qVTBNVE5hCk1FRXhDekFKQmdOVkJBWVRBbFZUTVJrd0Z3WURWUVFLREJCTWIyZHpkR0Z6YUNCVVpYTjBJRU5CTVJjd0ZRWUQKVlFRRERBNWpZUzVsZUdGdGNHeGxMbTl5WnpDQ0FpSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnSVBBRENDQWdvQwpnZ0lCQU5lN1BSNFhhVFh0RjZoM0ZoV2UvUjRCU1RWeWxYV29wQTUxK3BwY0ozQk9NUGptUk1OSjN0RkFGRTNoCkY0ZDBSSEJOSk9aRjArb2dUMFpFc2VUZTRtcUpYazNSZ2ZNU3JMYXltTmd6YWVmRDY3dWhROVabylp6em5FM2tJWGUKbXpoL0E4YUR3aGFVTWlmSUt4ZWtpc3JtcHZqRHdVSnRhU3MzcGIyN1crY09tekFQWjNjbU9zMDl0RUxMWTEzNApmNTJzcDBacUZTT2d2Q3djZHQ4OFBGVk1tMnJyRmd3eFAyZ0xnT2taTDRPc005c1F5a1lFUFIyOHVuUytQOTBWCnFuWVB5Mjd4cUpOc3M0T2RaQ0pyamtTN2x2MlBiQnhTb0ZqRHEveUxqbkRWOGtoVzIrNncwTUZSdmFtb0tMMzQKbi9Yb1hONlZDYXRoU3hjd3ZYZzB4M3d3VHhhNUhldmIwaXppTkdYSGpaOWJYdCs4Ym51L0Joc2E3S3dhb1V0OQpySkplTXkwS05zZFF5V2hNSkU5MDRZS205RW8vUzkycnJjTld6bXpCSVYwaWVjT0hjMjRpdzNTSVhPbm9BS05ZCjFHdERPUUNoU0VlYjdlbjI1czFmaldUcUlEeURPa3RXanA5RFh1MWlwczlZRGI3R0taN3JhT29RbnNQa0dyUkUKS09XQ2xrV1E0cUlYSjlMSDczeXRSMWg4K0FzR3lJbmFhbjVlaG56N0pDNVNGaEU5NndQekpEYVhDS05IQlAvZQp0ZndRMEJUYmdPNno4Z1BFOEpsUEdYVG1kZjlZRjVOeE1kNG9KQTd1N1k2eDJ5NEtJUllhY3JjZXZEeGUvbEZrCjg0M013aVlVMmF0WWdxZ0ZLMDdCSU9ITnZxdjkzV2lxWHk4V0FvbFNtTW9KL2VxZEFnTUJBQUV3RFFZSktvWkkKaHZjTkFRRUxCUUFEZ2dJQkFGa1FwbVczVDUwZUk1QWhBT3pONmR1eFF0akR1SUU0QWhjUWFlaklWRnU4UjlINApHS3c4V1FvMURPN2phZWZSSzdCRnk2OHU4Q2FjZ3luNmJ0Q29BMEFNdUtZeXQxU3RNNEp6ZjJaeFdyb3gwVGwrClVXNVJKRlA4SG9JQlF1dE10Z0hhWTNoV1pKNEp2Y2c2eTdrcm9NeW5abnNWM2piSzAvR210aHRVWW9uakNwQ0gKdUMxckVwLzBHa3A5QlBuclk2Y2d5UmREYmdtRG8zWU1xbVVoOEJxVEdMRWkrRjQ1Sy9QRU41MDJrVUJjTUpUWQp2cFdWZmdNejduUWhONHRlbUlsUVFEczhndTNMQnQ5bHhvbXVNWHRZa1RxMjQ1TGZYZHRiUFBrcndqdmJJS3pNCkZhc2ExUGtUbUsyM2NYTHBSV2ZOVXUvSkhDaHBDbDI3WWc4U2NUbTZHVi9lS2hKYnRrdThFeHZMV2dIQUlUR2kKNVJoaDJQbC8vSmg0c3p6VEwzNElZNWJQcVNYTXJxVUIzdkZ6TkQ1eWJtV3J3bzBpMkN5TFMrZ0tnR3F6ejBYdAptdlE2WENpcTdFc1RkTkxsWDFaRFdqaWFzMTJFSXlDVmJXcm14ekZzUi9KaThYUXFVb0tLK1FYaGRzUS91QTRICm43MmpHdVdzamhSQUtFN1d5STJpMUgrVFBSWitLNlZhS2VTMmFpa0MzcDJKQ1U0VnhyUDJqU2RXaGRZWXdFZ0wKQy9takRYT2p4YnI2VElPdHJ4UVFTQnBsVHVSejh5TmFiclBCNkcyeE4rZTcwcXBCMUtUNXcyZWUxUkg3TTFvKwpVb2VEb1N3cW5lVnZPTkFqUW4vMEtLTDBZN1AyQlVSSGpKZVdzTHRGbXlVWlZsa3Fsb3NnOFA3TmFicmoKLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQ==
      cert: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUZFVENDQXZrQ0ZCaHg1RHVZdFJ6Q3hSeDhCbytXSVMyTEZJMnVNQTBHQ1NxR1NJYjNEUUVCQ3dVQU1FRXgKQ3pBSkJnTlZCQVlUQWxWVE1Sa3dGd1lEVlFRS0RCQk1iMmR6ZEdGemFDQlVaWE4wSUVOQk1SY3dGUVlEVlFRRApEQTVqWVM1bGVHRnRjR3hsTG05eVp6QWVGdzB5TXpBeE1EVXhOalUxTXpaYUZ3MHlOREF4TURVeE5qVTFNelphCk1Fa3hDekFKQmdOVkJBWVRBbFZUTVIwd0d3WURWUVFLREJSTWIyZHpkR0Z6YUNCVVpYTjBJRU5zYVdWdWRERWIKTUJrR0ExVUVBd3dTWTJ4cFpXNTBMbVY0WVcxd2JHVXViM0puTUlJQ0lqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQwpBZzhBTUlJQ0NnS0NBZ0VBclkrTiszSUVZcklRcGRNUFQ2eE1xa3NTUzQzZzIrNDRFZFlpUE5vblAyS2pFVWRHCi9nNTdDQm5JT2FVZlp5dmczRmM5Uk9CTWhxWWE2Q0dLQ2Q4WWVjN21MNGM5N3RTOXdCdGM2STBlRUJtWEFlWnoKQ0d5Ni8xSHRTY1ovbUFIdzlyc2hnd0YxU2kvajlSNE5BNlplcG1Hdm9RTWRVT0dKSkhoU3NFZm92Vm9SUjVELwpWTzFVcnB1NExZbno3WndvKy9RRXpKYlVtU1ZONTIvdE5XamdUbEhGQ09kUTZhWkNrd0JLMkRDYTNiMk5GV1NPCnZmaGhsLzRHTExlWVoyaEcyZjkrVytMNnNsTE1Xd3BNcXl3WTliVVhuK05ST2NZUG9MWDIyMnNhSDZuWTc0KzAKK0I5UWs0Qk9sTnFoTjQ5M0ZDb1hxMnV6R21ZYjBpZ094K284VVZjN2djb3piaHZSRWdXNFJSYTRYZ2Ftb25TVwpDY2NNNXNRRkhGU3hhR1pYb2twRTJHSnRRVDB3di9waW00S3UwWEVJUUttWkdlakMxZHcyNmZiRytDV0RXdgfkuUFhzCllFbWY0UzVaNWV4alNpUUNQTDJRYXdDWGdHRUpOa2lVTWo1bGQzamViMmtZMjIxSUtiK3VTRTZ3YU5UdHM0bk8KUmE5RHZlSHJVS3JOcXEzM3lvRVp2ajRLL1FaOXB4OWttN28wQmhSK29NUEFZSS9ZT0RnTk9oUUxTUjNxMm4xQwpqZCtiYUZmZzRTYjhMMk9VVHlXNEtkMk9rOXJrQ2s5Vy84VC9ZS2xyQkZvU3JOSHRQaEtJaTZGWkxrcm9kbjFnCjgrbFBvM0U4MEduNWhVWmRnc0laN1krYzJxY1VBVnU4WC9vdEZhV204RkNtSUR5eitaS20vWWdhWDRVQ0F3RUEKQVRBTkJna3Foa2lHOXcwQkFRc0ZBQU9DQWdFQUNnNFB4Ym94TTAxY08zcG1oVGZ2d2V0QnZJQ3o4R091QXEzZgpMV1lGWGNabW5NSHF3RFpLT3gyMGEwM1hmY0JhV2h5RjlYSGRDdWd6aUVNWFRkZkt4R3dGc0lVeFFJYkRCVDROCkJOQ2NMVFhpVEVkdGp2WHhtMFRuTTVRZFBPRTM2RXNJK080WVQ4dytDNW5sS3VOTXRzeHNKZStieEVmQmkyUFMKSjB2VTFiTys0bTlwMFNEYzNoMzlqYitGTHJBbnFlejJRYlQybWFieThBOHdhaHVuQU1XWStaVWtRWW9XcGlsZgpSa0dwaUxLbGtKOTVIQ1l6bXQ3SWVkZEg1K1pCdUcrU3g0U013Q3lubjY0Si9VYWZOVzBYVjM2ZHplTFNsYTU5CnZRQ21XQXVyakFxYThmcWVwZHZOSTRJL0p4VmZlQ1F3a3JaRW9zMGdlYytEN3FPdXBmSGszWnlyNkc1Wm44a1MKWVJVOEhwUklSSDRLdnNPYlROSXJXNVovcWJmV0FGU1RDM3EwZWZsYVZMV3NyWGZTR3ZCRFZxbHhPM2FyaHYycQpyYTZ0Y0Q3TVFPQk8yMjZpK3YzYUw5cUozdmlXaElRVHZPTm03RDhVKy9XcnlyQ2hCT0hWQ1EyTTNBWlFRTGVDCnFTa0o2MHdGeDhqRXFMajlFTFd1VE11SFlnNWxoTVpGeUxJOGlXT3ZHUlBtZ1RVWktOSDc0TEYxdWpJRXVNQngKRTdMQldSR054MmxEMGYyYVlVZHYrcVdBOG0xRVRQeUtZbWU2b1VNK2tEbGY2c3N0TWdhaE43elQ4amovVzJLRApkRyt5ekhrNUcwNmxTUXpYR2JlYzNiaTJXT3BXSEoxSi9rVFE4QWYxSHVKcjRValFtaW44ZkxXNm4wNmRpeVNBCkJTSFlHV2s9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0=
      key: LS0tLS1CRUdJTiBQUklWQVRFIEtFWS0tLS0tCk1JSUpRUUlCQURBTkJna3Foa2lHOXcwQkFRRUZBQVNDQ1Nzd2dna25BZ0VBQW9JQ0FRQ3RqNDM3Y2dSaXNoQ2wKMHc5UHJFeXFTeEpMamVEYjdqZ1IxaUk4MmljL1lxTVJSMGIrRG5zSUdjZzVwUjluSytEY1Z6MUU0RXlHcGhybwpJWW9KM3hoNXp1WXZoejN1MUwzQUcxem9qUjRRR1pjQjVuTUliTHIvVWUxSnhuK1lBZkQydXlHREFYVktMK1AxCkhnMERwbDZtWWEraEF4MVE0WWtrZUZLd1IraTlXaEZIa1A5VTdWU3VtN2d0aWZQdG5Dajc5QVRNbHRTWkpVM24KYiswMWFPQk9VY1VJNTFEcHBrS1RBRXJZTUpyZHZZMFZaSTY5K0dHWC9nWXN0NWhuYUViWi8zNWI0dnF5VXN4YgpDa3lyTEJqMXRSZWY0MUU1eGcrZ3RmYmJheG9mcWRqdmo3VDRIMUNUZ0U2VTJxRTNqM2NVS2hlcmE3TWFaaHZTCktBN0g2anhSVnp1QnlqTnVHOUVTQmJoRkZyaGVCcWFpZEpZSnh3em14QVVjVkxGb1psZWlTa1RZWW0xQlBUQy8KK21LYmdxN1JjUWhBcVprWjZNTFYzRGJwOXNiNEpZTlk5ZXhnU1ovaExsbmw3R05LSkFJOHZaQnJBSmVBWVFrMgpTSlF5UG1WM2VONXZhUmpiYlVncHY2NUlUckJvMU8yemljNUZyME85NGV0UXdjfdkFzMnFyZmZLZ1JtK1BncjlCbjJuCkgyU2J1alFHRkg2Z3c4QmdqOWc0T0EwNkZBdEpIZXJhZlVLTjM1dG9WK0RoSnZ3dlk1UlBKYmdwM1k2VDJ1UUsKVDFiL3hQOWdxV3NFV2hLczBlMCtFb2lMb1ZrdVN1aDJmV0R6NlUramNUelFhZm1GUmwyQ3dobnRqNXphcHhRQgpXN3hmK2kwVnBhYndVS1lnUExQNWtxYjlpQnBmaFFJREFRQUJBb0lDQUNzb3ZJemZnSG11Zi9kTWNjMUZNbGRTCmpiMGVEZUdDN294NDdGQ25pd1QzR1VmTnFyaTRqeDJuazZQS0RQSVI5anUwc2ZhenREUHprRk5USzhsaW9lcUEKYWJzOTdVZTd2V2ZOSmlCcUh5U3Z5RjVmbVJGcVFHSUhWSE41R2ZlSjNBcnU0OWw0L2xxeGFBVm5NS05NdHRLMwpEZjZERU1JeEkzSmZQV2k2cVFTVkppRGV6SytveU5zV3ZBa08rZ3FIUDZYUHUzWEl1QnRSTEhzMTJRM2tBNHRXClNDSDdxNkkraHVXWk9BTmtxczRqT2JjdEoxWFVNeWloc1pWakhsSHdtMVhRYy9LVGtmWFFJeU1zZjM0OVhBT1YKd2NjdnR0NGdBM2phWndXUEw1TGFJS2tKMmwydEk5TmFIN0JpWVo2NFhVczFZR2R2UTcxMzBNbEV6dEFsemxPZQo5TTR0a3ZkRUxMY3Z5QnlFc1kzSmFPYldlL04vUlBrM3ZvbTRFUC9YRitkVG5JWFJPMG5MRFA1S3d6ajFDcHdiClJoN0pwNmRtZk9wQk1MS3RiNWlFS1VST1BqR0pUK2pLT1JoV2FUd280em1xajZFSHAxWjJjVWVaZHZab21RV00KYjc1eE5vSnlCS29vTEFhZmRHV08wQURSMW5iSzU2K1JwYkYwNy94SEhkV1IxU3VKRTZ2cHBrUTMzV09zTE1KYgpDbzE0MUFHKzVOUlBlNWJuNUtIOUtyZ3NKVE5QcGxoTmZxNUtHRSt4YitnZklINzFLdXhNZ0hhZkJwMk5nNDMyCmpHcjRaZkJKeS93OGNTMGpMV3J6QVBFdnoxWm1oSUVHTmVpT3M3OFFPNmVmZVQ0ZkNBb2h3N3FRdXIyM0s4WUIKVHlWRmRVYXE2M25kRnEza3FCR3RBb0lCQVFEWlBzODZTeUR3dkx0dGFMbGdwRTh6K1hneExSV1pSd1BDUSt5VgpBaEpLYWVoeHlhdkFQa3ArazVmMUVhQUw5ZzIwWkN6TUEzTjhpbXNFZTh6dmJyRHVSL3hCQ0lVR3BwbVFuRC9FCkNVUWs0VW42M3puTlpkSitoN2NuL2t5c2k3RDBvZDlvSGdZeEkzb2owVmhQTmtkd200R1NKOGx2WHJMN1JEL2YKY1hGS3J6SU9tZGtPSlkxRHlkdFJBUzc3Mk1LWFZVUnhhRloza2VQRkVUbG9xaXFnQklhS3QxZ3c5dVNydGtnOAplY1czTlZRREk1MjFRL3VOWVFhcUEyQUtHbUxYQzlDZzRHQ2Z4c2VhaUx4V2FFc2QrY09BenpvVTh2KzhuUFJvCnRWZEtQRWcvL2IwVEpBcnhoNFpRVk94b2dMVmJnVzJKb1k5Z3Nhc2xxWnBRYVJiTEFvSUJBUURNaGNCVkc4dk8KcXNtcXYycVYwKzI1MUtMdDVZMmhmVTVrM09PTnNJQVFsMWE3c0tPWTRlWGlWcEt6VDNKL2VtWmVMc1FtSG53MgpSZElYcWphVmpINWpOQURWOXRIc1FaMktBMHFWMkpPL1ZLN2ZRdGpWOFhJYUhoL2dJQVhYZXJTeUxkaDdyZEhwCmcreGZIYURCMWtLVW1aUjZNdEI4N2hsUTZuZ0JJNDkvN3hnSlJlVGVlZTJvajRzTjEwQUxWaTUxWHlOZlorRlYKUXU4RDJWUDZzc243MHF2ZW1wekxhUDcwWk5qMmVFUzdLSzhYRW03eDdTdHYwcHRabDduK0UrT3FaMWk0T1ZjRgo3VUNSQkNybURZV1lDTG1ham1SMDR6eUJlcHBKZlNSYkgzeTNtWEJlTndtbEZxbVF6NU5WTkRnL1lrY1BiTTRPCmFrQ1g3WlhQSDBqdkFvSUJBRmhaeC9OZ0xJUmJiU3BBeGV0OHgwMU83c2VwR3ppYi9mWmFvM095UlBnSWZHVVMKYkl3aGlUQlRIQ0NweTFveDlqN2Y0cXdSMXp6V0dsSFhlM0FBcDJvdzBuRXNZdFZpbWQrSy9BL2c2TnJLMk1oegpVbEdyVUdEdkZ0am4vZ3pLUHV3dWpPb09FOXlXSGcxRkRWSWh0QW9pNUI0cG1pMTE2UHB4Tmp6TUtSUURqaXNMCi9JOVpURXMrWTdoYzc5dXl1dWpLMzZ2emovN08wVUFMRWpyendhUVVVeGRGRzFQR2hSY2thZHBXZDhkYm85QW4KQXZOK00yYTdCL2ZLcVp0U1FkSk5Wc3FtbGdWRTFWYU90M0c0dHB2NVFMNDVDTmtPUGwxWnc3aDF6NHM4V3ZIVApZcnJQRkxoSHNxTW05b0pGbmZ3WjlnOWNLakJiOFV1KzN5aEdwT01DZ2dFQWVIemZad1JlR0IyemV2MFR2THJDCmxUUzQyNi9kdFdLTjJZdnNIdC81UWt6MkV0S29QbnZ1bzEzZlJQV3IvWC9OYVBUaUo1YlVGR0VqdVQ5VXN0dTIKNVppUVdYejBCTnhQQkN5V054c0ZSN1dLNUFxTWxkV05JK2ZWWFlOZ0RhYkRaeWp0SFVlMG4zNVJ0V05OL29QTQphNkRpd083SXRxREtsMG5hY3NsUlU4dzJlOWdLVWlyQW9Rb1hvSXJMdHlJSmNxb2V1NmtHTGVXbHk3MnY1TVNKCmkrcDd5RU9MMWFYQWRaZ24zV1BURWZPUTJ1WElLSXhSaDZvcVRTaStzUGxrcVZJRENWejJjSTVwK0VUZFJQUjQKWEszZk1qZHE1Uld0NHBXbzZWeHBHNm04SHFtdGNrTzRVZUs4K0l2aFAxUHBReVlSdVB1ZmxReHhpMCt6YnZiKwpUd0tDQVFBdHhVQVM4citBUDNVdWZpOUR2dWpJNXozK21XcVppTTVNeGc4T0pxMHFOUEU4VjZnZnJTc3BFZ0R0CkhXRjhUVU5vQVRXTENDYWsxdTlJbUJxaVBaTUg5V2ZSWGFMU29mckZKc1ZURnQrNVplVDZRTW5jMFJuQlpha0wKdkpNWDlyS2tiOThsZUlSZkN3emxuQlE4NElGTTQxZTBGMTUrODUzYUlpYnBCQUk3QkVmVHZKOEVnL20yMHcxSApyUFJQMWo2R1locGtBSW0yK1RWeDZERlkvSk82Sk0xaTB0ekh2N3ppaFNlamkwbHdCTUtKN00wVFJYejFkSmVSCjNHc0RsRDdtS3dMVmFCQktRMVV4aDF6WWJpYVV6VnN0MVMyV2R2dDEzZjg5SVY0TW1tdXEydjFVejRqZTdwREIKaEpJVHhSZXNnQ1RSMmFEMG5NekY4ZWdFS0pvWQotLS0tLUVORCBQUklWQVRFIEtFWS0tLS0t
```

Use the content of the following files in preparation to fill in the placeholders:

- ${Base64 encoded CA} - Get ca.crt from preparation step 1 and run:

   ```bash
   cat ca.crt | Base64 -w0 
   ```
   {: codeblock}

- ${CLIENT_CERTIFICATE} - Get client.crt from preparation step 3 and run:

   ```bash
   cat client.crt | Base64 -w0 
   ```
   {: codeblock}

- ${CLIENT_PRIVATE_KEY} - Get client-key-pkcs8.pem from preparation step 3 and run:

   ```bash
   cat client-key-pkcs8.pem | Base64 -w0
   ```
   {: codeblock}

For example:

```yaml
   "server": LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUZVVENDQXprQ0ZESkM2Mm4rUWFaZWRyQjF4K0JCSzVQMmF0ZVZNQTBHQ1NxR1NJYjNEUUVCQ3dVQU1HVXgKQ3pBSkJnTlZCQVlUQWtSRk1SSXdFQVlEVlFRSURBbFRkSFYwZEdkaGNuUXhFakFRQmdOVkJBY01DVk4wZFhSMApaMkZ5ZERFTU1Bb0dBMVVFQ2d3RFNVSk5NUXd3Q2dZRFZRUUxEQU5KUWsweEVqQVFCZ05WQkFNTUNXeHZZMkZzCmFHOXpkREFlRncweU1qQTRNVFV3T0RRMU1UQmFGdzB5TXpBNE1UVXdPRFExTVRCYU1HVXhDekFKQmdOVkJBWVQKQWtSRk1SSXdFQVlEVlFRSURBbFRkSFYwZEdkaGNuUXhFakFRQmdOVkJBY01DVk4wZFhSMFoyRnlkREVNTUFvRwpBMVVFQ2d3RFNVSk5NUXd3Q2dZRFZRUUxEQU5KUWsweEVqQVFCZ05WQkFNTUNXeHZZMkZzYUc5emREQ0NBaUl3CkRRWUpLb1pJaHZjTkFRRUJCUUFEZ2dJUEFEQ0NBZ29DZ2dJQkFKZFMyS3ZwMEo3ZGlXNytSbWh2ZFIxbjNNa0kKTXVnbldrQjJMM1RpNm1aNUZhcGhlQlZYS3EwV2RMRCt0Rm0ydFhXcUxmb09henFQSmpYSkVwR1BFNUk4Mk9nNwovNndMbmJla3RFRExpT2hleGZ0Smpkb0NtQ1RwMEhwWVhqV3RIMzl0cWdCTHNLVmZ6aEoySFByWlA1RUpDOHQ5CnVFdjQ2dzBaaVJ6d2tRYjdhZ3pZdWNpK0RFVXhjc2dvTHB6Z3R6VC9yWFlxOGlwNUhLb2ZQYyszSnBaeGRXZ00KSDE4MVFHalduZ1FRSXVrZTR0cXAyc3FjZTZWNkZLWktLTk9NeVZsczgvcEYrTlZzck1na2VZSFpzaUU1Um4wQgpiU2RXSTh1VWx6YjhoZnBxcXBNeDhhclYxNnRoT1JHZ0NrOVZ1RndiL0k4cVU3OVY0ZTVQcjB3SXgxTG9qV1BzCnUxM1Z3RnFka1c1TlVuV2lPNlBXN00yM0cvWVZUQ1Q1WFF5dC9XQWFOQmtSRlZ6SCtVV1o1ZTVsME1MTW5ibDkKWmc3ZFdBZWgxckNiTnIzS1NCa2MzdXJEQmtCelhJUXpiSWY3ZkUwT2Uxam9pVXlsRUk1dlVlK0g2K2JQcHFuawpQa3hKWW5ER3Y4QkVwWUZSRHg0emNQLzVtaGFQNHB2MHBTNGJaVjdrdExnQUVrUmxLT0M4blZ4YjBrdDJFMXZ2CjBLN2ZSZjc0OVdPdGM3cjdQQ2dvUXBNd21DNzNtZmhsaFRJVXk5VDB5UkJ6M1c5c0xqbFZ4ZVEzNHpnb2pmeXYKV2ZGRzcwZWxrZ3N3Zdjndfdk1lPL0JhaDNJYzBYa2NxQWhkRnhtbFJrYldwaVpPMXJKWk9OVnJWQ2RZZ0NtWEVTcTZyZwowRTA1RGF2eDlSZU9XcTZQQWdNQkFBRXdEUVlKS29aSWh2Y05BUUVMQlFBRGdnSUJBQ3hGNy9YV0NpMHdTa3VPCmJjRHhWQ2NEQVRPeEdlYzlNU1duaUw4ajlleFY3Wm1tWW1ETjVBUytiMERQd0ExMlBCL2FLMTh4S0MxQzV0T0YKNW1lY2VEL0dNKzZTY1RYS0EwajZidmh0amw3UmdUdStVbSsrVkFnbk13MnkrTEVVbVQwRG5qdy9zYWlXa1hEVgpMeGI3cytodWdQSTZra1lWNElHMVJjTHZCa2M5UkFyZWxLVlNqbVFOblZTYVZIcVg4NlpBeEl1eGR0RHdRcFh3CjJycStLM09qSCtSa0FXWGtHbGo3ZUNxZGNyYzVjRks3a1gvbTcxdGVSM0pQdWpLYmp0SlYxNzA4WTV4UUhPVjcKUGF3L3dvYXoycmRITkMwMXI4Z2pkODhvNXFoaURxd2JsNU1RVHNPb2N4MjdSYU4yUFV2QVg0SlpvS0NnMVVIUgo4TzZyNVhvZXNvY1pkRGhUYy9xU3J3YU42VGxDb2JhKzhJSjZqSE9KMW9DWjA5ZHJUS29RdEJyVlFlYnhMY2dOCjMyWEpKd01ub2RyS3hqN0tLdnNTaDNGV0ZYQU82UlJGUGV6M2hSSU9PTU56dk5mejdpb0xaeHppWis2TWYyVzYKSHBrdlg2QjNzcnh5SGN1M3hOb0VpOS81V255UlJHZ2hMZzcrc0VpbllkaURmWk52b1RUY0xYUDZPVmxCQWl4bgpzTWN3anVIeUdVc2RaTTZlSTJ5UUF1TVBaQ3NZRWRXVDFmMjN4ZWhjUWpnL0dFSml2d3VVbEFjK3VuNTAzOVNnCjRrQWNCZFZQc3FXV0o2dXZQS01zWUFZdGRWUkdNZ1FHY1VvbE1YNCtTcENoUzRkcEZhOG4xNlUvdE9vVEpqSSsKSU1ncEt3Z0VWRGpubFAzcXB0T2xrUm9wallWVgotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0t,
   "cert": LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNZVENDQVVtZ0F3SUJBZ0lCQVRBTkJna3Foa2lHOXcwQkFRc0ZBREJBTVFzd0NRWURWUVFHRXdKVlV6RVkKTUJZR0ExVUVDZ3dQVW5ONWMyeHZaeUJVWlhOMElFTkJNUmN3RlFZRFZRUUREQTVqWVM1bGVHRnRjR3hsTG05eQpaekFlRncweU1qRXdNVEl4TmpFMU1qZGFGdzB6TWpFd01Ea3hOakUxTWpkYU1Cb3hHREFXQmdOVkJBTVREMFJGClUwdFVUMUF0T0RsTE9Fd3lSREJaTUJNR0J5cUdTTTQ5QWdFR0NDcUdTTTQ5QXdFSEEwSUFCRnc0QWFhQ1hESE8Ka0hGVTBJNHM3RUYweFQvYk1aZTUxbGFCVDcyL1VPcm9aVEZncEtFSDJWTzdQampQU3dZUDdCL1JreWhlUm1acwp0V0ZnK09qV3NFK2pWekJWTUE4R0ExVWRFd0VCL3dRRk1BTUJBUUF3RGdZRFZSMFBBUUgvQkFRREFnR0lNQllHCkExVWRKUUVCL3dRTU1Bb0dDQ3NHQVFVRkJ3TUJNQm9HQTFVZEVRUVRNQkdDRDBSRlUwdFVUMUF0T0RsTE9Fd3kKUkRBTkJna3Foa2lHOXcwQkFRc0ZBereojoQU9DQVFFQXRLYzViN2wxNWY4Z1hKZXA2SjRvS2dLWWI0YTlJeWZNMnNGUgppYmprMmFKSkExYkJzMkRNbHZNbTlhVHFVNStFeURSUDNEVEd3bG8vNUlKYmVQV290Y2pwN21reklyN3lhcFlTCld5SVRtMnhQZTdQd2MxVk5GNWhmQ2NsSTI1OEFPbW1ZQUxDZWx0YmJCQUd1WDRrMzZSWlF6YjRIK3BkVmI2NXAKTkxLYVYvaGtxUy9DZ1AwbkdXWEc1UExCeFJ3dTN5Y0hXeFgzRTdGc1JXMUsxZC9oczdrMHdoaE1adndZSHZnegpYVWZ0Q2RrYkp0R3l0YlNxa2NvcjVtZDRSbTBuelpPdVVaY1phVTRXUnNLc0ZUY2lsZ1VlOVlYZkFUNUdpZ3lqClllRGJZTXhQWWFSS1QzR01ybUowOW9rY0VIaTdqRjNhWVoxU2s4b2c0VEJWV2YvYlpBPT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQ==,
   "key": LS0tLS1CRUdJTiBQUklWQVRFIEtFWS0tLS0tCk1JR0hBZ0VBTUJNR0J5cUdTTTQ5QWdFR0NDcUdTTTQ5QXdFSEJHMHdhd0lCQVFRZ1NQTkc0Z0U0TVREQm5lMXIKa0VPQldFM0NGa0NuS2lXVXQ5T3FTcVJHa3ZTaFJBTkNBQVJjT0FHbWdsd3h6cEJ4Vk5DT0xPeEJkTVUvMnpHWkdfkdAp1ZFpXZ1UrOXYxRHE2R1V4WUtTaEI5bFR1ejQ0ejBzR0Qrd2YwWk1vWGtabWJMVmhZUGpvMXJCUAotLS0tLUVORCBQUklWQVRFIEtFWS0tLS0t
```
{: codeblock}

#### Server setup
{: #syslog-server-setup}

There are many ways to set up a compatible server endpoint. The following example shows a simple setup of an [rsyslog](https://www.rsyslog.com/) server.

1. Install the required server packages (example shows Ubuntu).

   ```bash
   apt-get install rsyslog rsyslog-gnutls
   ```
   {: codeblock}

2. Get certificates and keys from the preparation steps.

   - `ca.crt` - from step 1, copy to `/certs/ca.crt`
   - `server.crt` - from step 2, copy to `/certs/server.crt`
   - `server-key.pem` - from step 2, copy to `/certs/server-key.pem`

3. Configure the rsyslog server in the `/etc/rsyslog.d/server.conf` file.

   ```toml
   # output to journal
   module(load="omjournal")
   template(name="journal" type="list") {
   # can add other metadata here
   property(outname="PRIORITY" name="pri")
   property(outname="SYSLOG_FACILITY" name="syslogfacility")
   property(outname="SYSLOG_IDENTIFIER" name="app-name")
   property(outname="HOSTNAME" name="hostname")
   property(outname="MESSAGE"  name="msg")
   }

   ruleset(name="journal-output") {
   action(type="omjournal" template="journal")
   }

   # make gtls driver the default and set certificate files
   $DefaultNetstreamDriver "gtls"
   $DefaultNetstreamDriverCAFile /certs/ca.crt
   $DefaultNetstreamDriverCertFile /certs/server.crt
   $DefaultNetstreamDriverKeyFile /certs/server-key.pem

   # load TCP listener
   module(
   load="imtcp"
   StreamDriver.Name="gtls"
   StreamDriver.Mode="1"
   StreamDriver.Authmode="x509/certvalid"
   )

   # start up listener at port ${PORT}
   input(
   type="imtcp"
   port="${PORT}"
   ruleset="journal-output"
   )
   ```
   {: codeblock}

   Make sure to fill in the `${PORT}` placeholder. It must match the `${PORT}` setting in the contract.

   The example config logs the received logs to its own journal. In a production setup, you might want to forward the logs to a database, but this is outside of the scope of this documentation.

   The `gnutls` package poses [requirements on the signatures](https://www.gnutls.org/manual/html_node/Digital-signatures.html) for the client certificate. Make sure to meet them.
   {: note}


   In this configuration, we accept any client certificate that is signed by the certificate authority through the `x509/certvalid` mode. This can change depending on the `StreamDriver.Authmode` setting. See [StreamDriver.Authmode](https://www.rsyslog.com/doc/configuration/modules/imtcp.html#streamdriver-authmode){: external}.
   {: note}

4. Restart the syslog service.

   ```sh
   service syslog restart
   ```
   {: codeblock}

## Certificate chain support for Syslog
{: #syslog-chainsupport}

The same CA must be used for both the client and server certificates.
   * **CA** – Root CA, Intermediate CA, which signed the client and server
   * **Client certificate** – Used to prove the client to the server, signed by the CA
   * **Client key** – Private key used by the virtual server instance to establish trust

Fill in the contract with the following information. The certificates and the key must be in **PEM** format:

```yaml
env:
  logging:
    syslog:
      hostname: ${HOSTNAME}
      port: 6514
      server: ${CA} # Include both RootCA and IntermediateCA that signed the client.
      cert: ${CLIENT_CERTIFICATE}
      key: ${CLIENT_PRIVATE_KEY}
```
{: codeblock}

Make sure to use a strong digest algorithm for the certificates; otherwise, the syslog server might reject them.
{: note}

### Example
{: #cert_syslog2}

Perform the following steps to create the required certificates and keys. This example uses 
`openssl` and shows `bash` syntax.

1. Create a `RootCA` private key and a certificate signing request (CSR).

   1. Prepare the `ca.cnf` configuration file:

      ```bash
      [ req ]
      default_bits = 2048
      default_md = sha256
      prompt = no
      encrypt_key = no
      distinguished_name = dn

      [ dn ]
      C = US
      O = Logstash Test CA
      CN = ca.example.org

      [ ext ]
      subjectKeyIdentifier = hash
      keyUsage = critical, keyCertSign, cRLSign
      basicConstraints = critical, CA:TRUE, pathlen:1
      ```
      {: codeblock}

      Update `dn` with your values. The actual values can be freely selected and do not affect subsequent processing.
      {: note}

   1. Create a self-signed RootCA key and certificate:

      ```bash
      # create private key
      openssl genrsa -out ca-key.pem 4096
      # create CSR
      openssl req -config ca.cnf -key ca-key.pem -new -out ca-req.csr
      # create self-signed CA
      openssl x509 -signkey ca-key.pem -in ca-req.csr -req -days 365 -out ca.crt -extfile ca.cnf -extensions ext
      ```
      {: codeblock}

   1. Prepare the `intermediate-ca.cnf` configuration file:

      ```toml
      [ req ]
      default_bits = 2048
      default_md = sha256
      prompt = no
      encrypt_key = no
      distinguished_name = dn

      [ dn ]
      C = US
      O = Logstash Intermediate CA
      CN = intermediate.example.org

      [ ext ]
      subjectKeyIdentifier = hash
      authorityKeyIdentifier = keyid,issuer
      basicConstraints = critical, CA:TRUE, pathlen:0
      keyUsage = critical, keyCertSign, cRLSign
      ```
      {: codeblock}

   1. Create the intermediate CA key and certificate:

      ```bash
      # create private key
      openssl genrsa -out intermediate-ca-key.pem 4096
      # create CSR
      openssl req -config intermediate-ca.cnf -key intermediate-ca-key.pem -new -out intermediate-ca-req.csr
      # issue Intermediate CA certificate using Root CA
      openssl x509 -req -in intermediate-ca-req.csr -CA ca.crt -CAkey ca-key.pem -CAcreateserial -out intermediate-ca.crt -days 365 -extfile intermediate-ca.cnf -extensions ext
      ```
      {: codeblock}

1. Create files used on the server side (the `rsyslog` server).

   1. Prepare the `server.cnf` configuration file. It's important to set the `default_md` value to at least `sha256`. Make sure to fill in the correct information for the `dn` field. It's preferred to use a domain name for `CN` but an IP address works too. For more information, see the OpenSSL documentation on [Subject Alternative Name](https://docs.openssl.org/master/man5/x509v3_config/#subject-alternative-name){: external}.

      Example that uses a hostname:

      ```toml
      [ req ]
      default_bits = 2048
      default_md = sha256
      prompt = no
      encrypt_key = no
      distinguished_name = dn

      [ server ]
      subjectAltName = DNS:${HOSTNAME}
      extendedKeyUsage = serverAuth

      [ dn ]
      C = US
      O = Rsyslog Test Server
      CN = ${HOSTNAME}
      ```
      {: codeblock}

      Example that uses an IP address:

      ```toml
      [ req ]
      default_bits = 2048
      default_md = sha256
      prompt = no
      encrypt_key = no
      distinguished_name = dn

      [ server ]
      subjectAltName = IP:${IP}
      extendedKeyUsage = serverAuth

      [ dn ]
      C = US
      O = Rsyslog Test Server
      CN = ${IP_OR_HOSTNAME}
      ```
      {: codeblock}

   1. Create the key and certificate.
      Ensure the `server.crt` contains a SAN for the IP or hostname.

      ```bash
      # create private key
      openssl genrsa -out server-key.pem 4096
      # create CSR
      openssl req -config server.cnf -key server-key.pem -new -out server-req.csr
      # have the IntermediateCA sign the certificate
      openssl x509 -req -in server-req.csr -days 365 -CA intermediate-ca.crt -CAkey intermediate-ca-key.pem -CAcreateserial -extfile server.cnf -extensions server -out server.crt
      ```
      {: codeblock}

1. Create files used on the client side (for the Hyper Protect Virtual Servers instance).

   1. Prepare the `client.cnf` configuration file:

      ```toml
      [ req ]
      default_bits = 2048
      default_md = sha256
      prompt = no
      encrypt_key = no
      distinguished_name = dn

      [ dn ]
      C = US
      O = Logstash Test Client
      CN = client.example.org
      ```
      {: codeblock}

      Update `dn` with your values. Whether these values matter depends on the `StreamDriver.Authmode` setting.
      In this example, `StreamDriver.Authmode="x509/certvalid"`, so all valid client certificates are accepted.
      For details, see [StreamDriver.Authmode](https://www.rsyslog.com/doc/configuration/modules/imtcp.html#streamdriver-authmode){: external}.
      {: note}

   1. Create the key and certificate:

      ```bash
      # create private key
      openssl genrsa -out client-key.pem 4096
      # create CSR for client auth
      openssl req -config client.cnf -key client-key.pem -new -out client-req.csr
      # have the IntermediateCA sign the certificate
      openssl x509 -req -in client-req.csr -days 365 -CA intermediate-ca.crt -CAkey intermediate-ca-key.pem -CAcreateserial -out client.crt
      # export key to PKCS#8 format
      openssl pkcs8 -topk8 -inform PEM -outform PEM -nocrypt -in client-key.pem -out client-key-pkcs8.pem
      ```
      {: codeblock}

### Client setup
{: #syslog-chainsupport_cs}

Configure the contract with the following template:

```yaml
env:
  logging:
    syslog:
      hostname: ${HOSTNAME}
      port: 6514
      server: ${CA} # Include both RootCA and IntermediateCA that signed the client.
      cert: ${CLIENT_CERTIFICATE}
      key: ${CLIENT_PRIVATE_KEY}
```
{: codeblock}

Use the following files to fill in placeholders:

* `${CA}` → `ca.crt`, `intermediate-ca.crt` (from preparation step 1)
* `${CLIENT_CERTIFICATE}` → `client.crt` (from preparation step 3)
* `${CLIENT_PRIVATE_KEY}` → `client-key-pkcs8.pem` (from preparation step 3)

`${HOSTNAME}`, `${CA}`, `${CLIENT_CERTIFICATE}`, and `${CLIENT_PRIVATE_KEY}` are strings without extra encoding or escaping. Regardless of the way you format them, make sure that you use valid YAML (see ​​​​​[​Scalars](https://yaml.org/spec/1.2.2/#scalars)​​​​). In the following example, the new lines are replaced with `\n` and carriage returns are deleted to make sure the content fits in one line between the inverted commas (see [​​​​​​scalars in double-quoted style](https://yaml.org/spec/1.2.2/#double-quoted-style)​​​​). You can also use other valid YAML variations.​​​​

Example:

```yaml
env:
    logging:
        syslog:
            hostname: 9.20.*.**
            port: 6514
            server: "-----BEGIN CERTIFICATE-----\nMIIFCTCCAvECFEp7wJLz4jNStIsVH2dUeHDN26ZyMA0GCSqGSIb3DQEBCwUAMEEx\nCzAJBgNVBAYTAlVTMRkwFwYDVQQKDBBMb2dzdGFzaCBUZXN0IENBMRcwFQYDVQQD\nDA5jYS5leGFtcGxlLm9yZzAeFw0yMzAxMDUxNjU0MTNaFw0yNDAxMDUxNjU0MTNa\nMEExCzAJBgNVBAYTAlVTMRkwFwYDVQQKDBBMb2dzdGFzaCBUZXN0IENBMRcwFQYD\nVQQDDA5jYS5leGFtcGxlLm9yZzCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCCAgoC\nggIBANe7PR4XaTXtF6h3FhWe/R4BSTVylXWopA51+ppcJ3BOMPjmRMNJ3tFAFE3h\nF4d0RHBNJOZF0+ogT0ZEseTe4mqJXk3RgfMSrLaymNgzaefD67uhQ9ZzznE3kIXe\nmzh/A8aDwhaUMifIKxekisrmpvjDwUJtaSs3pb27W+cOmzAPZ3cmOs09tELLY134\nf52sp0ZqFSOgvCwcdt88PFVMm2rrFgwxP2gLgOkZL4OsM9sQykYEPR28unS+P90V\nqnYPy27xqJNss4OdZCJrjkS7lv2PbBxSoFjDq/yLjnDV8khW2+6w0MFRvamoKL34\nn/XoXN6VCathSxcwvXg0x3wwTxa5Hevb0iziNGXHjZ9bXt+8bnu/Bhsa7KwaoUt9\nrJJeMy0KNsdQyWhMJE904YKm9Eo/S92rrcNWzmzBIV0iecOHc24iw3SIXOnoAKNY\n1GtDOQChSEeb7en25s1fjWTqIDyDOktWjp9DXu1ips9YDb7GKZ7raOoQnsPkGrRE\nKOWClkWQ4qIXJ9LH73ytR1h8+AsGyInaan5ehnz7JC5SFhE96wPzJDaXCKNHBP/e\ntfwQ0BTbgO6z8gPE8JlPGXTmdf9YF5NxMd4oJA7u7Y6x2y4KIRYacrcevDxe/lFk\n843MwiYU2atYgqgFK07BIOHNvqv93WiqXy8WAolSmMoJ/eqdAgMBAAEwDQYJKoZI\nhvcNAQELBQADggIBAFkQpmW3T50eI5AhAOzN6duxQtjDuIE4AhcQaejIVFu8R9H4\nGKw8WQo1DO7jaefRK7BFy68u8Cacgyn6btCoA0AMuKYyt1StM4Jzf2ZxWrox0Tl+\nUW5RJFP8HoIBQutMtgHaY3hWZJ4Jvcg6y7kroMynZnsV3jbK0/GmthtUYonjCpCH\nuC1rEp/0Gkp9BPnrY6cgyRdDbgmDo3YMqmUh8BqTGLEi+F45K/PEN502kUBcMJTY\nvpWVfgMz7nQhN4temIlQQDs8gu3LBt9lxomuMXtYkTq245LfXdtbPPkrwjvbIKzM\nFasa1PkTmK23cXLpRWfNUu/JHChpCl27Yg8ScTm6GV/eKhJbtku8ExvLWgHAITGi\n5Rhh2Pl//Jh4szzTL34IY5bPqSXMrqUB3vFzND5ybmWrwo0i2CyLS+gKgGqzz0Xt\nmvQ6XCiq7EsTdNLlX1ZDWjias12EIyCVbWrmxzFsR/Ji8XQqUoKK+QXhdsQ/uA4H\nn72jGuWsjhRAKE7WyI2i1H+TPRZ+K6VaKeS2aikC3p2JCU4VxrP2jSdWhdYYwEgL\nC/mjDXOjxbr6TIOtrxQQSBplTuRz8yNabrPB6G2xN+e70qpB1KT5w2ee1RH7M1o+\nUoeDoSwqneVvONAjQn/0KKL0Y7P2BURHjJeWsLtFmyUZVlkqlosg8P7Nabrj\n-----END CERTIFICATE-----\n-----BEGIN CERTIFICATE-----\nMIIFIzCCAwugAwIBAgIUbRa/FnYm8juUlFt9pU8EKjpWBL4wDQYJKoZIhvcNAQEL\nBQAwFTETMBEGA1UEAwwKTXkgUm9vdCBDQTAeFw0yNTA2MzAwNzU0MzBaFw0zMDA2\nMjkwNzU0MzBaMBoxGDAWBgNVBAMMD0ludGVybWVkaWF0ZSBDQTCCAiIwDQYJKoZI\nhvcNAQEBBQADggIPADCCAgoCggIBAK2+eElAqggO+7UaPsZXUL1dbhH54me93/Dc\nXcbm8gXYPoRAaDR7U6reIVTsIZPlrK/sNLikAGyf9N2NOXNdr920zRpFAsLGvlRO\nnBXsErAp4SSH+soUR2QLrel6LNmdjymQVQlQjVseiZ6j6f6g+wDZNDc4BG+vsfO8\n7FlDYQ6esqvSA/CKaxdAHjC3jbAPs1tufE3bLwJiNEl7B2ClLYL7qNsfU6HkxOvM\nMVG00Q99PcSIjVlxgu2daXbZL5M8t6ROIlu9uhKv5Ee23+XG8STCDMX9cUBNRa6l\nML4Ier2VtvMMHIsusyUE7Up6X0dNn+ZJjSs296YYbggfaqeeDD8lfpt/2BDi/Qv6\neZPBiOl3jsDOUYAn47bJEcG9LlCZWTFKfO5KcHl9nUHFQERVVw96LkyVdMCsowhu\nqTNp8b44AmPBfFkCjzPIwNUK7q+HhV1oHrYKyzysoNztgbcYO+SrWmjtuR4vlhWx\nqYOFq5yq/M7oxrDIVdbz7VsjSxIGwB2Q5zq1jSAGWb6lZ/4nCEirAlEE3x53eJmB\njthrt8Y3pCjNGh/qFzpETuk9ah3FSb/aWSxN1+aJasRhE7ucGZjWxMPLdmrlxvio\nV2YsZuNCghG6iwBBK6wh5BWav67jDbXILTu2RJ8w9u1WhY0dgcv7M6C8vBr0C+Dm\ngWjlMeCVAgMBAAGjZjBkMB0GA1UdDgQWBBTppYu+rVEkhfNT0mHa/NuacgDgkzAf\nBgNVHSMEGDAWgBTB+r0CFdVzie0l1AM+7hgDxLsp6zASBgNVHRMBAf8ECDAGAQH/\nAgEAMA4GA1UdDwEB/wQEAwIBBjANBgkqhkiG9w0BAQsFAAOCAgEApnZNjom2Jkdv\n6XBYAKG4JpEh4SEknTgj4hfu9VlTnwLvsJkcOIlk/1j0sVJTQlqKah9zrlHZiGUt\ngscTaXuK7qexqCa+/SkFY2mTiHHarJcms2nagCeCzsLhwoM4bP9De7Xy/74/0wGe\nf08cyb4C2km7NRV42FqIAB1jqkXHin1ZRpYZG7MxPNbIC/wCBIJ3McQMjVUqScQr\ndOnbiW0Uw1lgyD8R4jb3bmYAz8MVtohJQbyqqIDDRWj2bG/EDpYESn54EXbxwI3O\n0IE6uC6BELr1RjlNq1tKib2wyuIyT3M9ZQoJFhHomKKZvnXnDnVMmuoZXh8vxUcA\nmD1LbWlUF4qlJ+MUFTExux71eOd/6CvvDA2+eVkrI9iqMtqpWetRP1b0DAjQUZc3\ndtGnyMni/7IgRErvPutVYnVHlsPUB9iwNRAI3xBLPF4PUiaG36eIiUfNTXCinB/+\nf9M+rf93xk5AN9Y8pVPqLSiNvK1yqNsDncRHcweP2A/ri8zHjTjLTJ7ZLKAVnzMS\n42xdnmma51V0WoJ7rS1aHBV7DAmH1IGUMXGWpEnMnT4PIs7AV3xkIuXreuI0j6uK\nYseZOcUvwV4cwdWyflWdQ+37nmeztOksc5wQElKAe/h3IqkpDM7bw6TEzUGQWHkZ\nhGnq7CSY+GvV0/YIzXaXjSaX/GJqKfc=\n-----END CERTIFICATE-----\n"
            cert: "-----BEGIN CERTIFICATE-----\nMIIFETCCAvkCFBhx5DuYtRzCxRx8Bo+WIS2LFI2uMA0GCSqGSIb3DQEBCwUAMEEx\nCzAJBgNVBAYTAlVTMRkwFwYDVQQKDBBMb2dzdGFzaCBUZXN0IENBMRcwFQYDVQQD\nDA5jYS5leGFtcGxlLm9yZzAeFw0yMzAxMDUxNjU1MzZaFw0yNDAxMDUxNjU1MzZa\nMEkxCzAJBgNVBAYTAlVTMR0wGwYDVQQKDBRMb2dzdGFzaCBUZXN0IENsaWVudDEb\nMBkGA1UEAwwSY2xpZW50LmV4YW1wbGUub3JnMIICIjANBgkqhkiG9w0BAQEFAAOC\nAg8AMIICCgKCAgEArY+N+3IEYrIQpdMPT6xMqksSS43g2+44EdYiPNonP2KjEUdG\n/g57CBnIOaUfZyvg3Fc9ROBMhqYa6CGKCd8Yec7mL4c97tS9wBtc6I0eEBmXAeZz\nCGy6/1HtScZ/mAHw9rshgwF1Si/j9R4NA6ZepmGvoQMdUOGJJHhSsEfovVoRR5D/\nVO1Urpu4LYnz7Zwo+/QEzJbUmSVN52/tNWjgTlHFCOdQ6aZCkwBK2DCa3b2NFWSO\nvfhhl/4GLLeYZ2hG2f9+W+L6slLMWwpMqywY9bUXn+NROcYPoLX222saH6nY74+0\n+B9Qk4BOlNqhN493FCoXq2uzGmYb0igOx+o8UVc7gcozbhvREgW4RRa4XgamonSW\nCccM5sQFHFSxaGZXokpE2GJtQT0wv/pim4Ku0XEIQKmZGejC1dw26fbG+CWDWPXs\nYEmf4S5Z5exjSiQCPL2QawCXgGEJNkiUMj5ld3jeb2kY221IKb+uSE6waNTts4nO\nRa9DveHrUKrNqq33yoEZvj4K/QZ9px9km7o0BhR+oMPAYI/YODgNOhQLSR3q2n1C\njd+baFfg4Sb8L2OUTyW4Kd2Ok9rkCk9W/8T/YKlrBFoSrNHtPhKIi6FZLkrodn1g\n8+lPo3E80Gn5hUZdgsIZ7Y+c2qcUAVu8X/otFaWm8FCmIDyz+ZKm/YgaX4UCAwEA\nATANBgkqhkiG9w0BAQsFAAOCAgEACg4PxboxM01cO3pmhTfvwetBvICz8GOuAq3f\nLWYFXcZmnMHqwDZKOx20a03XfcBaWhyF9XHdCugziEMXTdfKxGwFsIUxQIbDBT4N\nBNCcLTXiTEdtjvXxm0TnM5QdPOE36EsI+O4YT8w+C5nlKuNMtsxsJe+bxEfBi2PS\nJ0vU1bO+4m9p0SDc3h39jb+FLrAnqez2QbT2maby8A8wahunAMWY+ZUkQYoWpilf\nRkGpiLKlkJ95HCYzmt7IeddH5+ZBuG+Sx4SMwCynn64J/UafNW0XV36dzeLSla59\nvQCmWAurjAqa8fqepdvNI4I/JxVfeCQwkrZEos0gec+D7qOupfHk3Zyr6G5Zn8kS\nYRU8HpRIRH4KvsObTNIrW5Z/qbfWAFSTC3q0eflaVLWsrXfSGvBDVqlxO3arhv2q\nra6tcD7MQOBO226i+v3aL9qJ3viWhIQTvONm7D8U+/WryrChBOHVCQ2M3AZQQLeC\nqSkJ60wFx8jEqLj9ELWuTMuHYg5lhMZFyLI8iWOvGRPmgTUZKNH74LF1ujIEuMBx\nE7LBWRGNx2lD0f2aYUdv+qWA8m1ETPyKYme6oUM+kDlf6sstMgahN7zT8jj/W2KD\ndG+yzHk5G06lSQzXGbec3bi2WOpWHJ1J/kTQ8Af1HuJr4UjQmin8fLW6n06diySA\nBSHYGWk=\n-----END CERTIFICATE-----\n"
            key: "-----BEGIN PRIVATE KEY-----\n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\nhJITxResgCTR2aD0nMzF8egEKJoY\n-----END PRIVATE KEY-----\n"
```
{: codeblock}

The syslog certificates and the key can be given in Base64 format as well.

To generate in Base64, use the following command:

```bash
cat ${CLIENT_PRIVATE_KEY} | base64 -w0
```
{: codeblock}

Configure the contract with base64 encoded certificates.

```yaml
​env:
    logging:
        syslog:
            hostname: ${HOSTNAME}
            port: 16999
            server: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUZDVENDQXZFQ0ZFcDd3Skx6NGpOU3RJc1ZIMmRVZUhETjI2WnlNQTBHQ1NxR1NJYjNEUUVCQ3dVQU1FRXgKQ3pBSkJnTlZCQVlUQWxWVE1Sa3dGd1lEVlFRS0RCQk1iMmR6ZEdGemFDQlVaWE4wSUVOQk1SY3dGUVlEVlFRRApEQTVqWVM1bGVHRnRjR3hsTG05eVp6QWVGdzB5TXpBeE1EVXhOalUwTVROYUZ3MHlOREF4TURVeE5qVTBNVE5hCk1FRXhDekFKQmdOVkJBWVRBbFZUTVJrd0Z3WURWUVFLREJCTWIyZHpkR0Z6YUNCVVpYTjBJRU5CTVJjd0ZRWUQKVlFRRERBNWpZUzVsZUdGdGNHeGxMbTl5WnpDQ0FpSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnSVBBRENDQWdvQwpnZ0lCQU5lN1BSNFhhVFh0RjZoM0ZoV2UvUjRCU1RWeWxYV29wQTUxK3BwY0ozQk9NUGptUk1OSjN0RkFGRTNoCkY0ZDBSSEJOSk9aRjArb2dUMFpFc2VUZTRtcUpYazNSZ2ZNU3JMYXltTmd6YWVmRDY3dWhROVp6em5FM2tJWGUKbXpoL0E4YUR3aGFVTWlmSUt4ZWtpc3JtcHZqRHdVSnRhU3MzcGIyN1crY09tekFQWjNjbU9zMDl0RUxMWTEzNApmNTJzcDBacUZTT2d2Q3djZHQ4OFBGVk1tMnJyRmd3eFAyZ0xnT2taTDRPc005c1F5a1lFUFIyOHVuUytQOTBWCnFuWVB5Mjd4cUpOc3M0T2RaQ0pyamtTN2x2MlBiQnhTb0ZqRHEveUxqbkRWOGtoVzIrNncwTUZSdmFtb0tMMzQKbi9Yb1hONlZDYXRoU3hjd3ZYZzB4M3d3VHhhNUhldmIwaXppTkdYSGpaOWJYdCs4Ym51L0Joc2E3S3dhb1V0OQpySkplTXkwS05zZFF5V2hNSkU5MDRZS205RW8vUzkycnJjTld6bXpCSVYwaWVjT0hjMjRpdzNTSVhPbm9BS05ZCjFHdERPUUNoU0VlYjdlbjI1czFmaldUcUlEeURPa3RXanA5RFh1MWlwczlZRGI3R0taN3JhT29RbnNQa0dyUkUKS09XQ2xrV1E0cUlYSjlMSDczeXRSMWg4K0FzR3lJbmFhbjVlaG56N0dfdfpDNVNGaEU5NndQekpEYVhDS05IQlAvZQp0ZndRMEJUYmdPNno4Z1BFOEpsUEdYVG1kZjlZRjVOeE1kNG9KQTd1N1k2eDJ5NEtJUllhY3JjZXZEeGUvbEZrCjg0M013aVlVMmF0WWdxZ0ZLMDdCSU9ITnZxdjkzV2lxWHk4V0FvbFNtTW9KL2VxZEFnTUJBQUV3RFFZSktvWkkKaHZjTkFRRUxCUUFEZ2dJQkFGa1FwbVczVDUwZUk1QWhBT3pONmR1eFF0akR1SUU0QWhjUWFlaklWRnU4UjlINApHS3c4V1FvMURPN2phZWZSSzdCRnk2OHU4Q2FjZ3luNmJ0Q29BMEFNdUtZeXQxU3RNNEp6ZjJaeFdyb3gwVGwrClVXNVJKRlA4SG9JQlF1dE10Z0hhWTNoV1pKNEp2Y2c2eTdrcm9NeW5abnNWM2piSzAvR210aHRVWW9uakNwQ0gKdUMxckVwLzBHa3A5QlBuclk2Y2d5UmREYmdcdtRG8zWU1xbVVoOEJxVEdMRWkrRjQ1Sy9QRU41MDJrVUJjTUpUWQp2cFdWZmdNejduUWhONHRlbUlsUVFEczhndTNMQnQ5bHhvbXVNWHRZa1RxMjQ1TGZYZHRiUFBrcndqdmJJS3pNCkZhc2ExUGtUbUsyM2NYTHBSV2ZOVXUvSkhDaHBDbDI3WWc4U2NUbTZHVi9lS2hKYnRrdThFeHZMV2dIQUlUR2kKNVJoaDJQbC8vSmg0c3p6VEwzNElZNWJQcVNYTXJxVUIzdkZ6TkQ1eWJtV3J3bzBpMkN5TFMrZ0tnR3F6ejBYdAptdlE2WENpcTdFc1RkTkxsWDFaRFdqaWFzMTJFSXlDVmJXcm14ekZzUi9KaThYUXFVb0tLK1FYaGRzUS91QTRICm43MmpHdVdzamhSQUtFN1d5STJpMUgrVFBSWitLNlZhS2VTMmFpa0MzcDJKQ1U0VnhyUDJqU2RXaGRZWXdFZ0wKQy9takRYT2p4YnI2VElPdHJ4UVFTQnBsVHVSejh5TmFiclBCNkcyeE4rZTcwcXBCMUtUNXcyZWUxUkg3TTFvKLS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUZJekNDQXd1Z0F3SUJBZ0lVYlJhL0ZuWW04anVVbEZ0OXBVOEVLanBXQkw0d0RRWUpLb1pJaHZjTkFRRUwKQlFBd0ZURVRNQkVHQTFVRUF3d0tUWGtnVW05dmRDQkRRVEFlRncweU5UQTJNekF3TnpVME16QmFGdzB6TURBMgpNamt3TnpVME16QmFNQm94R0RBV0JnTlZCQU1NRDBsdWRHVnliV1ZrYVdGMFpTQkRRVENDQWlJd0RRWUpLb1pJCmh2Y05BUUVCQlFBRGdnSVBBRENDQWdvQ2dnSUJBSzIrZUVsQXFnZ08rN1VhUHNaWFVMMWRiaEg1NG1lOTMvRGMKWGNibThnWFlQb1JBYURSN1U2cmVJVlRzSVpQbHJLL3NOTGlrQUd5ZjlOMk5PWE5kcjkyMHpScEZBc0xHdmxSTwpuQlhzRXJBcDRTU0grc29VUjJRTHJlbDZMTm1kanltUVZRbFFqVnNlaVo2ajZmNmcrd0RaTkRjNEJHK3ZzZk84CjdGbERZUTZlc3F2U0EvQ0theGRBSGpDM2piQVBzMXR1ZkUzYkx3SmlORWw3QjJDbExZTDdxTnNmVTZIa3hPdk0KTVZHMDBROTlQY1NJalZseGd1MmRhWGJaTDVNOHQ2Uk9JbHU5dWhLdjVFZTIzK1hHOFNUQ0RNWDljVUJOUmE2bApNTDRJZXIyVnR2TU1ISXN1c3lVRTdVcDZYMGRObitaSmpTczI5NllZYmdnZmFxZWVERDhsZnB0LzJCRGkvUXY2CmVaUEJpT2wzanNET1VZQW40N2JKRWNHOUxsQ1pXVEZLZk81S2NIbDluVUhGUUVSVlZ3OTZMa3lWZE1Dc293aHUKcVROcDhiNDRBbVBCZkZrQ2p6UEl3TlVLN3ErSGhWMW9IcllLeXp5c29OenRnYmNZTytTcldtanR1UjR2bGhXeApxWU9GcTV5cS9NN294ckRJVmRiejdWc2pTeElHd0IyUTV6cTFqU0FHV2I2bFovNG5DRWlyQWxFRTN4NTNlSm1CCmp0aHJ0OFkzcENqTkdoL3FGenBFVHVrOWFoM0ZTYi9hV1N4TjErYUphc1JoRTd1Y0daald4TVBMZG1ybHh2aW8KVjJZc1p1TkNnaEc2aXdCQks2d2g1QldhdjY3akRiWElMVHUyUko4dzl1MVdoWTBkZ2N2N002Qzh2QnIwQytEbQpnV2psTWVDVkFnTUJBQUdqWmpCa01CMEdBMVVkRGdRV0JCVHBwWXUrclZFa2hmTlQwbUhhL051YWNnRGdrekFmCkJnTlZIU01FR0RBV2dCVEIrcjBDRmRWemllMGwxQU0rN2hnRHhMc3A2ekFTQmdOVkhSTUJBZjhFQ0RBR0FRSC8KQWdFQU1BNEdBMVVkRHdFQi93UUVBd0lCQmpBTkJna3Foa2lHOXcwQkFRc0ZBQU9DQWdFQXBuWk5qb20ySmtkdgo2WEJZQUtHNEpwRWg0U0VrblRnajRoZnU5VmxUbndMdnNKa2NPSWxrLzFqMHNWSlRRbHFLYWg5enJsSFppR1V0CmdzY1RhWHVLN3FleHFDYSsvU2tGWTJtVGlISGFySmNtczJuYWdDZUN6c0xod29NNGJQOURlN1h5Lzc0LzB3R2UKZjA4Y3liNEMya203TlJWNDJGcUlBQjFqcWtYSGluMVpScFlaRzdNeFBOYklDL3dDQklKM01jUU1qVlVxU2NRcgpkT25iaVcwVXcxbGd5RDhSNGpiM2JtWUF6OE1WdG9oSlFieXFxSUREUldqMmJHL0VEcFlFU241NEVYYnh3STNPCjBJRTZ1QzZCRUxyMVJqbE5xMXRLaWIyd3l1SXlUM005WlFvSkZoSG9tS0tadm5YbkRuVk1tdW9aWGg4dnhVY0EKbUQxTGJXbFVGNHFsSitNVUZURXh1eDcxZU9kLzZDdnZEQTIrZVZrckk5aXFNdHFwV2V0UlAxYjBEQWpRVVpjMwpkdEdueU1uaS83SWdSRXJ2UHV0VlluVkhsc1BVQjlpd05SQUkzeEJMUEY0UFVpYUczNmVJaVVmTlRYQ2luQi8rCmY5TStyZjkzeGs1QU45WThwVlBxTFNpTnZLMXlxTnNEbmNSSGN3ZVAyQS9yaTh6SGpUakxUSjdaTEtBVm56TVMKNDJ4ZG5tbWE1MVYwV29KN3JTMWFIQlY3REFtSDFJR1VNWEdXcEVuTW5UNFBJczdBVjN4a0l1WHJldUkwajZ1SwpZc2VaT2NVdndWNGN3ZFd5ZmxXZFErMzdubWV6dE9rc2M1d1FFbEtBZS9oM0lxa3BETTdidzZURXpVR1FXSGtaCmhHbnE3Q1NZK0d2VjAvWUl6WGFYalNhWC9HSnFLZmM9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0KwpVb2VEb1N3cW5lVnZPTkFqUW4vMEtLTDBZN1AyQlVSSGpKZVdzTHRGbXlVWlZsa3Fsb3NnOFA3TmFicmoKLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQ==
            cert: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUZFVENDQXZrQ0ZCaHg1RHVZdFJ6Q3hSeDhCbytXSVMyTEZJMnVNQTBHQ1NxR1NJYjNEUUVCQ3dVQU1FRXgKQ3pBSkJnTlZCQVlUQWxWVE1Sa3dGd1lEVlFRS0RCQk1iMmR6ZEdGemFDQlVaWE4wSUVOQk1SY3dGUVlEVlFRRApEQTVqWVM1bGVHRnRjR3hsTG05eVp6QWVGdzB5TXpBeE1EVXhOalUxTXpaYUZ3MHlOREF4TURVeE5qVTFNelphCk1Fa3hDekFKQmdOVkJBWVRBbFZUTVIwd0d3WURWUVFLREJSTWIyZHpkR0Z6YUNCVVpYTjBJRU5zYVdWdWRERWIKTUJrR0ExVUVBd3dTWTJ4cFpXNTBMbVY0WVcxd2JHVXViM0puTUlJQ0lqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQwpBZzhBTUlJQ0NnS0NBZ0VBclkrTiszSUVZcklRcGRNUFQ2eE1xa3NTUzQzZzIrNDRFZFlpUE5vblAyS2pFVWRHCi9nNTdDQm5JT2FVZlp5dmczRmM5Uk9CTWhxWWE2Q0dLQ2Q4WWVjN21MNGM5N3RTOXdCdGM2STBlRUJtWEFlWnoKQ0d5Ni8xSHRTY1ovbUFIdzlyc2hnd0YxU2kvajlSNE5BNlplcG1Hdm9RTdfdfWRVT0dKSkhoU3NFZm92Vm9SUjVELwpWTzFVcnB1NExZbno3WndvKy9RRXpKYlVtU1ZONTIvdE5XamdUbEhGQ09kUTZhWkNrd0JLMkRDYTNiMk5GV1NPCnZmaGhsLzRHTExlWVdeftewreoyaEcyZjkrVytMNnNsTE1Xd3BNcXl3WTliVVhuK05ST2NZUG9MWDIyMnNhSDZuWTc0KzAKK0I5UWs0Qk9sTnFoTjQ5M0ZDb1hxMnV6R21ZYjBpZ094K284VVZjN2djb3piaHZSRWdXNFJSYTRYZ2Ftb25TVwpDY2NNNXNRRkhGU3hhR1pYb2twRTJHSnRRVDB3di9waW00S3UwWEVJUUttWkdlakMxZHcyNmZiRytDV0RXUFhzCllFbWY0UzVaNWV4alNpUUNQTDJRYXdDWGdHRUpOa2lVTWo1bGQzamViMmtZMjIxSUtiK3VTRTZ3YU5UdHM0bk8KUmE5RHZlSHJVS3JOcXEzM3lvRVp2ajRLL1FaOXB4OWttN28wQmhSK29NUEFZSS9ZT0RnTk9oUUxTUjNxMm4xQwpqZCtiYUZmZzRTYjhMMk9VVHlXNEtkMk9rOXJrQ2s5Vy84VC9ZS2xyQkZvU3JOSHRQaEtJaTZGWkxrcm9kbjFnCjgrbFBvM0U4MEduNWhVWmRnc0laN1krYzJxY1VBVnU4WC9vdEZhV204RkNtSUR5eitaS20vWWdhWDRVQ0F3RUEKQVRBTkJna3Foa2lHOXcwQkFRc0ZBQU9DQWdFQUNnNFB4Ym94TTAxY08zcG1oVGZ2d2V0QnZJQ3o4R091QXEzZgpMV1lGWGNabW5NSHF3RFpLT3gyMGEwM1hmY0JhV2h5RjlYSGRDdWd6aUVNWFRkZkt4R3dGc0lVeFFJYkRCVDROCkJOQ2NMVFhpVEVkdGp2WHhtMFRuTTVRZFBPRTM2RXNJK080WVQ4dytDNW5sS3VOTXRzeHNKZStieEVmQmkyUFMKSjB2VTFiTys0bTlwMFNEYzNoMzlqYitGTHJBbnFlejJRYlQybWFieThBOHdhaHVuQU1XWStaVWtRWW9XcGlsZgpSa0dwaUxLbGtKOTVIQ1l6bXQ3SWVkZEg1K1pCdUcrU3g0U013Q3lubjY0Si9VYWZOVzBYVjM2ZHplTFNsYTU5CnZRQ21XQXVyakFxYThmcWVwZHZOSTRJL0p4VmZlQ1F3a3JaRW9zMGdlYytEN3FPdXBmSGszWnlyNkc1Wm44a1MKWVJVOEhwUklSSDRLdnNPYlROSXJXNVovcWJmV0FGU1RDM3EwZWZsYVZMV3NyWGZTR3ZCRFZxbHhPM2FyaHYycQpyYTZ0Y0Q3TVFPQk8yMjZpK3YzYUw5cUozdmlXaElRVHZPTm03RDhVKy9XcnlyQ2hCT0hWQ1EyTTNBWlFRTGVDCnFTa0o2MHdGeDhqRXFMajlFTFd1VE11SFlnNWxoTVpGeUxJOGlXT3ZHUlBtZ1RVWktOSDc0TEYxdWpJRXVNQngKRTdMQldSR054MmxEMGYyYVlVZHYrcVdBOG0xRVRQeUtZbWU2b1VNK2tEbGY2c3N0TWdhaE43elQ4amovVzJLRApkRyt5ekhrNUcwNmxTUXpYR2JlYzNiaTJXT3BXSEoxSi9rVFE4QWYxSHVKcjRValFtaW44ZkxXNm4wNmRpeVNBCkJTSFlHV2s9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0=
            key: LS0tLS1CRUdJTiBQUklWQVRFIEtFWS0tLS0tCk1JSUpRUUlCQURBTkJna3Foa2lHOXcwQkFRRUZBQVNDQ1Nzd2dna25BZ0VBQW9JQ0FRQ3RqNDM3Y2dSaXNoQ2wKMHc5UHJFeXFTeEpMamVEYjdqZ1IxaUk4MmljL1lxTVJSMGIrRG5zSUdjZzVwUjluSytEY1Z6MUU0RXlHcGhybwpJWW9KM3hoNXp1WXZoejN1MUwzQUcxem9qUjRRR1pjQjVuTUliTHIvVWUxSnhuK1lBZkQydXlHREFYVktMK1AxCkhnMERwbDZtWWEraEF4MVE0WWtrZUZLd1IraTlXaEZIa1A5VTdWU3VtN2d0aWZQdG5Dajc5QVRNbHRTWkpVM24KYiswMWFPQk9VY1VJNTFEcHBrS1RBRXJZTUpyZHZZMFZaSTY5K0dHWC9nWXN0NWhuYUViWi8zNWI0dnF5VXN4YgpDa3lyTEJqMXRSZWY0MUU1eGcrZ3RmYmJheG9mcWRqdmo3VDRIMUNUZ0U2VTJxRTNqM2NVS2hlcmE3TWFaaHZTCktBN0g2anhSVnpfefw1QnlqTnVHOUVTQmJoRkZyaGVCcWFpZEpZSnh3em14QVVjVkxGb1psZWlTa1RZWW0xQlBUQy8KK21LYmdxN1JjUWhBcVprWjZNTFYzRGJwOXNiNEpZTlk5ZXhnU1ovaExsbmw3R05LSkFJOHZaQnJBSmVBWVFrMgpTSlF5UG1WM2VONXZhUmpiYlVncHY2NUlUckJvMU8yemljNUZyME85NGV0UXFzMnFyZmZLZ1JtK1BncjlCbjJuCkgyU2J1alFHRkg2Z3c4QmdqOWc0T0EwNkZBdEpIZXJhZlVLTjM1dG9WK0RoSnZ3dlk1UlBKYmdwM1k2VDJ1UUsKVDFiL3hQOWdxV3NFV2hLczBlMCtFb2lMb1ZrdVN1aDJmV0R6NlUramNUelFhZm1GUmwyQ3dobnRqNXphcHhRQgpXN3hmK2kwVnBhYndVS1lnUExQNWtxYjlpQnBmaFFJREFRQUJBb0lDQUNzb3ZJemZnSG11Zi9kTWNjMUZNbGRTCmpiMGVEZUdDN294NDdGQ25pd1QzR1VmTnFyaTRqeDJuazZQS0RQSVI5anUwc2ZhenREUHprRk5USzhsaW9lcUEKYWJzOTdVZTd2V2ZOSmlCcUh5U3Z5RjVmbVJGcVdfdfFHSUhWSE41R2ZlSjNBcnU0OWw0L2xxeGFBVm5NS05NdHRLMwpEZjZERU1JeEkzSmZQV2k2cVFTVkppRGV6SytveU5zV3ZBa08rZ3FIUDZYUHUzWEl1QnRSTEhzMTJRM2tBNHRXClNDSDdxNkkraHVXWk9BTmtxczRqT2JjdEoxWFVNeWloc1pWakhsSHdtMVhRYy9LVGtmWFFJeU1zZjM0OVhBT1YKd2NjdnR0NGdBM2phWndXUEw1TGFJS2tKMmwydEk5TmFIN0JpWVo2NFhVczFZR2R2UTcxMzBNbEV6dEFsemxPZQo5TTR0a3ZkRUxMY3Z5QnlFc1kzSmFPYldlL04vUlBrM3ZvbTRFUC9YRitkVG5JWFJPMG5MRFA1S3d6ajFDcHdiClJoN0pwNmRtZk9wQk1MS3RiNWlFS1VST1BqR0pUK2pLT1JoV2FUd280em1xajZFSHAxWjJjVWVaZHZab21RV00KYjc1eE5vSnlCS29vTEFhZmRHV08wQURSMW5iSzU2K1JwYkYwNy94SEhkV1IxU3VKRTZ2cHBrUTMzV09zTE1KYgpDbzE0MUFHKzVOUlBlNWJuNUtIOUtyZ3NKVE5QcGxoTmZxNUtHRSt4YitnZklINzFLdXhNZ0hhZkJwMk5nNDMyCmpHcjRaZkJKeS93OGNTMGpMV3J6QVBFdnoxWm1oSUVHTmVpT3M3OFFPNmVmZVQ0ZkNBb2h3N3FRdXIyM0s4WUIKVHlWRmRVYXE2M25kRnEza3FCR3RBb0lCQVFEWlBzODZTeUR3dkx0dGFMbGdwRTh6K1hneExSV1pSd1BDUSt5VgpBaEpLYWVoeHlhdkFQa3ArazVmMUVhQUw5ZzIwWkN6TUEzTjhpbXNFZTh6dmJyRHVSL3hCQ0lVR3BwbVFuRC9FCkNVUWs0VW42M3puTlpkSitoN2NuL2t5c2k3RDBvZDlvSGdZeEkzb2owVmhQTmtkd200R1NKOGx2WHJMN1JEL2YKY1hGS3J6SU9tZGtPSlkxRHlkdFJBUzc3Mk1LWFZVUnhhRloza2VQRkVUbG9xaXFnQklhS3QxZ3c5dVNydGtnOAplY1czTlZRREk1MjFRL3VOWVFhcUEyQUtHbUxYQzlDZzRHQ2Z4c2VhaUx4V2FFc2QrY09BenpvVTh2KzhuUFJvCnRWZEtQRWcvL2IwVEpBcnhoNFpRVk94b2dMVmJnVzJKb1k5Z3Nhc2xxWnBRYVJiTEFvSUJBUURNaGNCVkc4dk8KcXNtcXYycVYwKzI1MUtMdDVZMmhmVTVrM09PTnNJQVFsMWE3c0tPWTRlWGlWcEt6VDNKL2VtWmVMc1FtSG53MgpSZElYcWphVmpINWpOQURWOXRIc1FaMktBMHFWMkpPL1ZLN2ZRdGpWOFhJYUhoL2dJQVhYZXJTeUxkaDdyZEhwCmcreGZIYURCMWtLVW1aUjZNdEI4N2hsUTZuZ0JJNDkvN3hnSlJlVGVlZTJvajRzTjEwQUxWaTUxWHlOZlorRlYKUXU4RDJWUDZzc243MHF2ZW1wekxhUDcwWk5qMmVFUzdLSzhYRW03eDdTdHYwcHRabDduK0UrT3FaMWk0T1ZjRgo3VUNSQkNybURZV1lDTG1ham1SMDR6eUJlcHBKZlNSYkgzeTNtWEJlTndtbEZxbVF6NU5WTkRnL1lrY1BiTTRPCmFrQ1g3WlhQSDBqdkFvSUJBRmhaeC9OZ0xJUmJiU3BBeGV0OHgwMU83c2VwR3ppYi9mWmFvM095UlBnSWZHVVMKYkl3aGlUQlRIQ0NweTFveDlqN2Y0cXdSMXp6V0dsSFhlM0FBcDJvdzBuRXNZdFZpbWQrSy9BL2c2TnJLMk1oegpVbEdyVUdEdkZ0am4vZ3pLUHV3dWpPb09FOXlXSGcxRkRWSWh0QW9pNUI0cG1pMTE2UHB4Tmp6TUtSUURqaXNMCi9JOVpURXMrWTdoYzc5dXl1dWpLMzZ2emovN08wVUFMRWpyendhUVVVeGRGRzFQR2hSY2thZHBXZDhkYm85QW4KQXZOK00yYTdCL2ZLcVp0U1FkSk5Wc3FtbGdWRTFWYU90M0c0dHB2NVFMNDVDTmtPUGwxWnc3aDF6NHM4V3ZIVApZcnJQRkxoSHNxTW05b0pGbmZ3WjlnOWNLakJiOFV1KzN5aEdwT01DZ2dFQWVIemZad1JlR0IyemV2MFR2THJDCmxUUzQyNi9kdFdLTjJZdnNIdC81UWt6MkV0S29QbnZ1bzEzZlJQV3IvWC9OYVBUaUo1YlVGR0VqdVQ5VXN0dTIKNVppUVdYejBCTnhQQkN5V054c0ZSN1dLNUFxTWxkV05JK2ZWWFlOZ0RhYkRaeWp0SFVlMG4zNVJ0V05OL29QTQphNkRpd083SXRxREtsMG5hY3NsUlU4dzJlOWdLVWlyQW9Rb1hvSXJMdHlJSmNxb2V1NmtHTGVXbHk3MnY1TVNKCmkrcDd5RU9MMWFYQWRaZ24zV1BURWZPUTJ1WElLSXhSaDZvcVRTaStzUGxrcVZJRENWejJjSTVwK0VUZFJQUjQKWEszZk1qZHE1Uld0NHBXbzZWeHBHNm04SHFtdGNrTzRVZUs4K0l2aFAxUHBReVlSdVB1ZmxReHhpMCt6YnZiKwpUd0tDQVFBdHhVQVM4citBUDNVdWZpOUR2dWpJNXozK21XcVppTTVNeGc4T0pxMHFOUEU4VjZnZnJTc3BFZ0R0CkhXRjhUVU5vQVRXTENDYWsxdTlJbUJxaVBaTUg5V2ZSWGFMU29mckZKc1ZURnQrNVplVDZRTW5jMFJuQlpha0wKdkpNWDlyS2tiOThsZUlSZkN3emxuQlE4NElGTTQxZTBGMTUrODUzYUlpYnBCQUk3QkVmVHZKOEVnL20yMHcxSApyUFJQMWo2R1locGtBSW0yK1RWeDZERlkvSk82Sk0xaTB0ekh2N3ppaFNlamkwbHdCTUtKN00wVFJYejFkSmVSCjNHc0RsRDdtS3dMVmFCQktRMVV4aDF6WWJpYVV6VnN0MVMyV2R2dDEzZjg5SVY0TW1tdXEydjFVejRqZTdwREIKaEpJVHhSZXNnQ1RSMmFEMG5NekY4ZWdFS0pvWQotLS0tLUVORCBQUklWQVRFIEtFWS0tLS0t​​​​

```
{: codeblock}

Use the content of the following files in preparation to fill in the placeholders:

- `${Base64 encoded CA}` – Get `ca.crt` and `intermediate-ca.crt` from preparation step 1.
   Run the following commands and place the output in the server section of the contract:

   ```bash
   cat ca.crt | base64 -w0
   ```
   {: codeblock}

   ```bash
   cat intermediate-ca.crt | base64 -w0
   ```
   {: codeblock}


- `${CLIENT_CERTIFICATE}` – Get client.crt from preparation step 3 and run:

   ```bash
   cat client.crt | base64 -w0
   ```
   {: codeblock}

- ${CLIENT_PRIVATE_KEY} – Get client-key-pkcs8.pem from preparation step 3 and run:

   ```bash
   cat client-key-pkcs8.pem | base64 -w0
   ```
   {: codeblock}

### Server setup
{: #syslog-chainsupport_ss}

There are many ways to set up a compatible server endpoint. The following example shows a simple setup of an **rsyslog** server.

1. Install the required server packages (example shows Ubuntu):

   ```bash
   apt-get install rsyslog rsyslog-gnutls
   ```
   {: codeblock}

1. Get certificates and keys from the preparation steps:
   
   - intermediate-ca.crt, ca.crt - from step 1, copy to /certs/ca.crt [Create a ca.crt having both Root and Intermediate certificate]
   - server.crt - from step 2, copy to /certs/server.crt
   - server-key.pem - from step 2, copy to /certs/server-key.pem

1. Configure the rsyslog server in the `/etc/rsyslog.d/server.conf` file:

   ```bash
   # output to journal
   module(load="omjournal")
   template(name="journal" type="list") {
     # can add other metadata here
     property(outname="SYSLOG_FACILITY" name="syslogfacility")
     property(outname="SYSLOG_IDENTIFIER" name="app-name")
     property(outname="HOSTNAME" name="hostname")
     property(outname="MESSAGE"  name="msg")
   }

   ruleset(name="journal-output") {
     action(type="omjournal" template="journal")
   }

   # make gtls driver the default and set certificate files
   $DefaultNetstreamDriver "gtls"
   $DefaultNetstreamDriverCAFile /certs/ca.crt
   $DefaultNetstreamDriverCertFile /certs/server.crt
   $DefaultNetstreamDriverKeyFile /certs/server-key.pem

   # load TCP listener
   module(
     load="imtcp"
     StreamDriver.Name="gtls"
     StreamDriver.Mode="1"
     StreamDriver.Authmode="x509/certvalid"
   )

   # start up listener at port 6514
   input(
     type="imtcp"
     port="6514"
     ruleset="journal-output"
   )
   ```
   {: codeblock}

   The example configuration logs the received messages to its own journal.
   In a production setup, you might want to forward the logs to a database, but that is outside the scope of this documentation.

   The `gnutls` package poses [requirements on the signatures](https://www.gnutls.org/manual/html_node/Digital-signatures.html) for the client certificate. Make sure that these requirements are met.
   {: note}

   In this configuration, any client certificate that is signed by the certificate authority is accepted through the `x509/certvalid` mode.
   This behavior can vary depending on the `StreamDriver.Authmode` setting.
   See [StreamDriver.Authmode](https://www.rsyslog.com/doc/configuration/modules/imtcp.html#streamdriver-authmode){: external}.

1. Restart the syslog service:

   ```bash
   service syslog restart
   ```
   {: codeblock}
