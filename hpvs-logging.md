---

copyright:
  years: 2022, 2024
lastupdated: "2024-09-24"

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

Logs are comprised of events that are typically human-readable and have different formats, for example, unstructured text, JSON, delimiter-separated values, key-value pairs, and so on. The {{site.data.keyword.logs_full_notm}} service can manage general purpose application logs, platform logs, or structured audit events. {{site.data.keyword.logs_full_notm}} can be used with logs from both IBM Cloud services and customer applications.

For information about {{site.data.keyword.logs_full_notm}}, see [{{site.data.keyword.logs_full_notm}}](https://cloud.ibm.com/docs/cloud-logs).

### Access to IBM Cloud Logs:
{: #cloud-logs-access}

Access to {{site.data.keyword.logs_full_notm}} instances for users in vpc account is controlled by {{site.data.keyword.iamlong}} (IAM). Every user that accesses the {{site.data.keyword.logs_full_notm}} in vpc account must be assigned an access policy with an IAM role.

#### Access policies to be added:
{: #cloud-logs-accesspolicies}

 - Cloud Logs
  
To add relevant roles for these policies, refer to [Managing IAM access for {{site.data.keyword.logs_routing_full_notm}}](https://cloud.ibm.com/docs/logs-router?topic=logs-router-iam&interface=api).


### Provisioning an ICL instance
{: #cloud-logs-provision-instance-ui}

To provision an `ICL` instance from the Observability dashboard in the IBM Cloud, complete the following steps:

1. [Log in to your IBM Cloud account.](https://cloud.ibm.com/login)
2. [Provision an ICL instance](https://cloud.ibm.com/docs/cloud-logs?topic=cloud-logs-instance-provision&interface=ui). Choose a plan according to your requirements.


Input parameters to be provided in `env` section of contract are:

- [required] iamApiKey key for the service id. Generate and retrieve the API key from the service ID in IAM.
  
- [required] hostname of the service instance. Retrieve the Hostname from the ICL instance in the Endpoints section, labeled as the Public Ingress endpoint.
  
- [required] port of the service instance. That is 443.

Add these values in the `env` `logging` subsection of the contract as the following example shows. For more information, see the [logging subsection](https://cloud.ibm.com/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_icl).

```yaml
 env:
   logging:
     logRouter:
       hostname: <host name of the service instance> /
       iamApiKey: <iamApiKey of the service instance> / xxxx
       port: <port of the service instance(443).
```

**Note:** Custom tags are not supported for ICL.

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

For more information, see [Managing {{site.data.keyword.logs_full_notm}} logging instances](https://cloud.ibm.com/docs/cloud-logs?topic=cloud-logs-observe&interface=ui#observe_manage).

#### Steps to migrate for existing customers:
{: #cloud-logs-migrate}

- Create an [ICL](https://cloud.ibm.com/catalog/services/cloud-logs) instance
- Prepare a contract modifying the logging section from LogDNA to ICL
- Bring down the current HPVS, without deleting the data volumes
- Create new HPVS attaching the existing volumes with the new contract

## Syslog
{: #syslog}

You can also configure logging with a generic [syslog backend](https://datatracker.ietf.org/doc/html/rfc5424) such as an [rsyslog](https://www.rsyslog.com/) server or a [Logstash](https://www.elastic.co/logstash/) server. The {{site.data.keyword.hpvs}} for VPC instance uses TLS with [mutual authentication](https://en.wikipedia.org/wiki/Mutual_authentication) to connect to the logging backend. Find the following pieces of information to configure logging:

- Syslog hostname and port
- Certificate Authority (CA) - the certificate used to verify the certificate chain both for client and server authentication. Note that the same CA has to be used for both the client and server certificates.
- Client certificate - used to prove the client to the server, signed by the CA
- Client key - private key used by the virtual server instance to establish trust

Fill in the following parts of the contract with the information. The certificates and the key have to be in PEM format.

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

   Create the key and certificate. Make sure the server certificate `server.crt` contains a SAN for the IP or the hostname, depending on whether the server is accessed via IP or hostname.

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

`${HOSTNAME}`, `${CA}`, `${CLIENT_CERTIFICATE}`, and `${CLIENT_PRIVATE_KEY}` are strings without extra encoding or escaping. Regardless of the way you format them, make sure you use **valid YAML** (see [Scalars](https://yaml.org/spec/1.2.2/#scalars){: external}). In the following example, the new lines are replaced with `\n` and carriage returns are deleted to make sure the content fits in one line between the inverted commas (see [scalars in double-quoted style](https://yaml.org/spec/1.2.2/#double-quoted-style){: external}). You can also use other valid YAML variations.

Example:
```yaml
env: 
    logging:
        syslog:
            hostname: 9.20.7.92
            port: 16999
            server: "-----BEGIN CERTIFICATE-----\nMIIFCTCCAvECFEp7wJLz4jNStIsVH2dUeHDN26ZyMA0GCSqGSIb3DQEBCwUAMEEx\nCzAJBgNVBAYTAlVTMRkwFwYDVQQKDBBMb2dzdGFzaCBUZXN0IENBMRcwFQYDVQQD\nDA5jYS5leGFtcGxlLm9yZzAeFw0yMzAxMDUxNjU0MTNaFw0yNDAxMDUxNjU0MTNa\nMEExCzAJBgNVBAYTAlVTMRkwFwYDVQQKDBBMb2dzdGFzaCBUZXN0IENBMRcwFQYD\nVQQDDA5jYS5leGFtcGxlLm9yZzCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCCAgoC\nggIBANe7PR4XaTXtF6h3FhWe/R4BSTVylXWopA51+ppcJ3BOMPjmRMNJ3tFAFE3h\nF4d0RHBNJOZF0+ogT0ZEseTe4mqJXk3RgfMSrLaymNgzaefD67uhQ9ZzznE3kIXe\nmzh/A8aDwhaUMifIKxekisrmpvjDwUJtaSs3pb27W+cOmzAPZ3cmOs09tELLY134\nf52sp0ZqFSOgvCwcdt88PFVMm2rrFgwxP2gLgOkZL4OsM9sQykYEPR28unS+P90V\nqnYPy27xqJNss4OdZCJrjkS7lv2PbBxSoFjDq/yLjnDV8khW2+6w0MFRvamoKL34\nn/XoXN6VCathSxcwvXg0x3wwTxa5Hevb0iziNGXHjZ9bXt+8bnu/Bhsa7KwaoUt9\nrJJeMy0KNsdQyWhMJE904YKm9Eo/S92rrcNWzmzBIV0iecOHc24iw3SIXOnoAKNY\n1GtDOQChSEeb7en25s1fjWTqIDyDOktWjp9DXu1ips9YDb7GKZ7raOoQnsPkGrRE\nKOWClkWQ4qIXJ9LH73ytR1h8+AsGyInaan5ehnz7JC5SFhE96wPzJDaXCKNHBP/e\ntfwQ0BTbgO6z8gPE8JlPGXTmdf9YF5NxMd4oJA7u7Y6x2y4KIRYacrcevDxe/lFk\n843MwiYU2atYgqgFK07BIOHNvqv93WiqXy8WAolSmMoJ/eqdAgMBAAEwDQYJKoZI\nhvcNAQELBQADggIBAFkQpmW3T50eI5AhAOzN6duxQtjDuIE4AhcQaejIVFu8R9H4\nGKw8WQo1DO7jaefRK7BFy68u8Cacgyn6btCoA0AMuKYyt1StM4Jzf2ZxWrox0Tl+\nUW5RJFP8HoIBQutMtgHaY3hWZJ4Jvcg6y7kroMynZnsV3jbK0/GmthtUYonjCpCH\nuC1rEp/0Gkp9BPnrY6cgyRdDbgmDo3YMqmUh8BqTGLEi+F45K/PEN502kUBcMJTY\nvpWVfgMz7nQhN4temIlQQDs8gu3LBt9lxomuMXtYkTq245LfXdtbPPkrwjvbIKzM\nFasa1PkTmK23cXLpRWfNUu/JHChpCl27Yg8ScTm6GV/eKhJbtku8ExvLWgHAITGi\n5Rhh2Pl//Jh4szzTL34IY5bPqSXMrqUB3vFzND5ybmWrwo0i2CyLS+gKgGqzz0Xt\nmvQ6XCiq7EsTdNLlX1ZDWjias12EIyCVbWrmxzFsR/Ji8XQqUoKK+QXhdsQ/uA4H\nn72jGuWsjhRAKE7WyI2i1H+TPRZ+K6VaKeS2aikC3p2JCU4VxrP2jSdWhdYYwEgL\nC/mjDXOjxbr6TIOtrxQQSBplTuRz8yNabrPB6G2xN+e70qpB1KT5w2ee1RH7M1o+\nUoeDoSwqneVvONAjQn/0KKL0Y7P2BURHjJeWsLtFmyUZVlkqlosg8P7Nabrj\n-----END CERTIFICATE-----\n"
            cert: "-----BEGIN CERTIFICATE-----\nMIIFETCCAvkCFBhx5DuYtRzCxRx8Bo+WIS2LFI2uMA0GCSqGSIb3DQEBCwUAMEEx\nCzAJBgNVBAYTAlVTMRkwFwYDVQQKDBBMb2dzdGFzaCBUZXN0IENBMRcwFQYDVQQD\nDA5jYS5leGFtcGxlLm9yZzAeFw0yMzAxMDUxNjU1MzZaFw0yNDAxMDUxNjU1MzZa\nMEkxCzAJBgNVBAYTAlVTMR0wGwYDVQQKDBRMb2dzdGFzaCBUZXN0IENsaWVudDEb\nMBkGA1UEAwwSY2xpZW50LmV4YW1wbGUub3JnMIICIjANBgkqhkiG9w0BAQEFAAOC\nAg8AMIICCgKCAgEArY+N+3IEYrIQpdMPT6xMqksSS43g2+44EdYiPNonP2KjEUdG\n/g57CBnIOaUfZyvg3Fc9ROBMhqYa6CGKCd8Yec7mL4c97tS9wBtc6I0eEBmXAeZz\nCGy6/1HtScZ/mAHw9rshgwF1Si/j9R4NA6ZepmGvoQMdUOGJJHhSsEfovVoRR5D/\nVO1Urpu4LYnz7Zwo+/QEzJbUmSVN52/tNWjgTlHFCOdQ6aZCkwBK2DCa3b2NFWSO\nvfhhl/4GLLeYZ2hG2f9+W+L6slLMWwpMqywY9bUXn+NROcYPoLX222saH6nY74+0\n+B9Qk4BOlNqhN493FCoXq2uzGmYb0igOx+o8UVc7gcozbhvREgW4RRa4XgamonSW\nCccM5sQFHFSxaGZXokpE2GJtQT0wv/pim4Ku0XEIQKmZGejC1dw26fbG+CWDWPXs\nYEmf4S5Z5exjSiQCPL2QawCXgGEJNkiUMj5ld3jeb2kY221IKb+uSE6waNTts4nO\nRa9DveHrUKrNqq33yoEZvj4K/QZ9px9km7o0BhR+oMPAYI/YODgNOhQLSR3q2n1C\njd+baFfg4Sb8L2OUTyW4Kd2Ok9rkCk9W/8T/YKlrBFoSrNHtPhKIi6FZLkrodn1g\n8+lPo3E80Gn5hUZdgsIZ7Y+c2qcUAVu8X/otFaWm8FCmIDyz+ZKm/YgaX4UCAwEA\nATANBgkqhkiG9w0BAQsFAAOCAgEACg4PxboxM01cO3pmhTfvwetBvICz8GOuAq3f\nLWYFXcZmnMHqwDZKOx20a03XfcBaWhyF9XHdCugziEMXTdfKxGwFsIUxQIbDBT4N\nBNCcLTXiTEdtjvXxm0TnM5QdPOE36EsI+O4YT8w+C5nlKuNMtsxsJe+bxEfBi2PS\nJ0vU1bO+4m9p0SDc3h39jb+FLrAnqez2QbT2maby8A8wahunAMWY+ZUkQYoWpilf\nRkGpiLKlkJ95HCYzmt7IeddH5+ZBuG+Sx4SMwCynn64J/UafNW0XV36dzeLSla59\nvQCmWAurjAqa8fqepdvNI4I/JxVfeCQwkrZEos0gec+D7qOupfHk3Zyr6G5Zn8kS\nYRU8HpRIRH4KvsObTNIrW5Z/qbfWAFSTC3q0eflaVLWsrXfSGvBDVqlxO3arhv2q\nra6tcD7MQOBO226i+v3aL9qJ3viWhIQTvONm7D8U+/WryrChBOHVCQ2M3AZQQLeC\nqSkJ60wFx8jEqLj9ELWuTMuHYg5lhMZFyLI8iWOvGRPmgTUZKNH74LF1ujIEuMBx\nE7LBWRGNx2lD0f2aYUdv+qWA8m1ETPyKYme6oUM+kDlf6sstMgahN7zT8jj/W2KD\ndG+yzHk5G06lSQzXGbec3bi2WOpWHJ1J/kTQ8Af1HuJr4UjQmin8fLW6n06diySA\nBSHYGWk=\n-----END CERTIFICATE-----\n"
            key: "-----BEGIN PRIVATE KEY-----\n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\nhJITxResgCTR2aD0nMzF8egEKJoY\n-----END PRIVATE KEY-----\n"
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

   The example config will log the received logs to its own journal. In a production setup, you might want to forward the logs to a database, but this is outside of the scope of this documentation.

   The `gnutls` package poses [requirements on the signatures](https://www.gnutls.org/manual/html_node/Digital-signatures.html) for the client certificate. Make sure to meet them.
   {: note}

   In this configuration, we accept any client certificate that is signed by the certificate authority via the `x509/certvalid` mode. This may change depending on the `StreamDriver.Authmode` setting. See [StreamDriver.Authmode](https://www.rsyslog.com/doc/configuration/modules/imtcp.html#streamdriver-authmode).
   {: note}
   
4. Restart the syslog service.
   
   ```sh
   service syslog restart
   ```
   {: codeblock}

## {{site.data.keyword.loganalysislong_notm}}
{: #log-analysis}

Logging to {{site.data.keyword.loganalysisshort}} is dependent on the state and health of the {{site.data.keyword.loganalysisshort}} service. Service outages might lead to a loss of log data. If you want to log data for audit purposes, it's suggested that you employ your own logging service.
{: note}

Deprecation of {{site.data.keyword.loganalysisshort_notm}} and {{site.data.keyword.at_full_notm}} services 
: Effective 28 March 2024, the IBM {{site.data.keyword.loganalysisshort}} and {{site.data.keyword.at_full_notm}} services are deprecated and will no longer be supported as of 30 March 2025. Customers will need to migrate to {{site.data.keyword.logs_full_notm}}, which replaces these two services, prior to 30 March 2025. {{site.data.keyword.logs_full_notm}} will become generally available during the summer of 2024 in Frankfurt and Madrid with day-one support for EU-managed controls. The service will continue its worldwide multizone region (MZR) roll-out through 3Q2024. For information about {{site.data.keyword.logs_full_notm}}, see the [{{site.data.keyword.logs_full_notm}}](https://cloud.ibm.com/docs/cloud-logs) documentation.

1. [Log in to your IBM Cloud account](/login){: external}.
2. [Provision a {{site.data.keyword.loganalysisshort}} instance](/docs/log-analysis?topic=log-analysis-provision). Choose a plan according to your requirements.  
3. After you create the {{site.data.keyword.loganalysisshort}} instance, click it open and click **Open dashboard**.
4. Click the question mark icon (**Install Instructions**) at the lower left of the page. On the **Add Log Source** page, under **Via Syslog**, click **rsyslog**.
5. Make a note of the ingestion key value at the upper right of the page, and the endpoint value. In the following example, the endpoint value is `syslog-u.au-syd.logging.cloud.ibm.com`.
   ```sh
   ### START Log Analysis rsyslog logging directives ###

   ## TCP TLS only ##
   $DefaultNetstreamDriverCAFile /etc/ld-root-ca.crt # trust these CAs
   $ActionSendStreamDriver gtls # use gtls netstream driver
   $ActionSendStreamDriverMode 1 # require TLS
   $ActionSendStreamDriverAuthMode x509/name # authenticate by hostname
   $ActionSendStreamDriverPermittedPeer *.au-syd.logging.cloud.ibm.com
   ## End TCP TLS only ##

   $template LogDNAFormat,"<%PRI%>%protocol-version% %timestamp:::date-rfc3339% %HOSTNAME% %app-name% %procid% %msgid% [logdna@48950 key=\"bc8a5ba9aa5c0c12b26c6c45089228a4\"] %msg%"

   # Send messages to Log Analysis over TCP using the template.
   *.* @@syslog-u.au-syd.logging.cloud.ibm.com:6514;LogDNAFormat

   ### END Log Analysis rsyslog logging directives ###
   ```
   {: codeblock}

6. Add these values in the `env` `logging` subsection of the contract as the following example shows. For more information, see the [`logging` subsection](/docs/vpc?topic=vpc-about-contract_se&interface=ui#hpcr_contract_logdna).

    ```yaml
    env:
      logging:
        logDNA:
          hostname: ${RSYSLOG_LOGDNA_HOSTNAME}
          ingestionKey: ${LOGDNA_INGESTION_KEY}
    ```
    {: codeblock}

   When the {{site.data.keyword.hpvs}} for VPC instance boots, it extracts the {{site.data.keyword.loganalysisshort}} information from the contract and configures accordingly so that all the logs are routed to the endpoint specified. Then, you can open the {{site.data.keyword.loganalysisshort}} dashboard and view the logs from the virtual server instance.
7. If the LogDNA is used for collecting logs from multiple systems, you can utilise custom tags to isolate logs optionally. 
   Following is an example of a custom tag:
  
   ```yaml
   env:
      logging:
         logDNA:
               hostname: ${RSYSLOG_LOGDNA_HOSTNAME}
               ingestionKey: ${LOGDNA_INGESTION_KEY}
               tags: ["custom tag name 1", "custom tag name 2"]
   ```
**Note:** Special characters and escape sequences are not supported. The instance will remove these characters from the tags.
