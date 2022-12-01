---

copyright:
  years: 2022
lastupdated: "2022-11-28"

keywords: confidential computing, secure execution, logging for hyper protect virtual server for vpc

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Logging for {{site.data.keyword.hpvs}} for VPC
{: #logging-for-hyper-protect-virtual-servers-for-vpc}

To launch a {{site.data.keyword.hpvs}} for VPC instance, you (as the deployer) need to set up logging first by adding the logging configuration in the `env` section of the [contract](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_env). The instance will read the configuration and configure logging accordingly. All other services will start only after logging is configured. If the logging configuration is incorrect, the instance will not start and an error message will be displayed in the serial console.
{: shortdesc}

The logs include startup logs, service logs issued by the {{site.data.keyword.hpvs}} for VPC instance, and container logs.

The following logging services are supported.

## IBM Log Analysis
{: #log-analysis}

1. [Log in to your IBM Cloud account](/login){: external}.
2. [Provision a Log Analysis instance](/docs/log-analysis?topic=log-analysis-provision). Choose a plan according to your requirements.  
3. After you create the Log Analysis instance, click it open and click **Open dashboard**.
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

6. Add these values in the `env` `logging` subsection of the contract as the following example shows. For more information, see the [`logging` subsection](/docs/vpc?topic=vpc-about-contract_se&interface=ui#hpcr_contract_env_log).

    ```yaml
    env:
      logging:
        logDNA:
          hostname: ${RSYSLOG_LOGDNA_HOSTNAME}
          ingestionKey: ${LOGDNA_INGESTION_KEY}
    ```
    {: codeblock}

   When the {{site.data.keyword.hpvs}} for VPC instance boots, it extracts the Log Analysis information from the contract and configures accordingly so that all the logs are routed to the endpoint specified. Then, you can open the Log Analysis dashboard and view the logs from the virtual server instance.


## Syslog
{: #syslog}

You can also configure logging with a generic [syslog backend](https://datatracker.ietf.org/doc/html/rfc5424) such as an [rsyslog](https://www.rsyslog.com/) server or a [Logstash](https://www.elastic.co/logstash/) server. The IBM Hyper Protec Virtual Servers instance uses TLS with [mutual authentication](https://en.wikipedia.org/wiki/Mutual_authentication) to connect to the logging backend. Find the following pieces of information to configure logging:

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

   Create the key and certificate:

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

   Prepare the `server.cnf` configuration file. It's important to set the `default_md` value to at least `sha256`. Make sure to fill in the correct information for the `dn` field. It's preferred to use a domain name for `CN` but an IP address works too.

   ```toml
   [ req ]
   default_bits = 2048
   default_md = sha256
   prompt = no
   encrypt_key = no
   distinguished_name = dn

   [ dn ]
   C = US
   O = Rsyslog Test Server
   CN = ${IP_OR_HOSTNAME}
   ```
   {: codeblock}

   Create the key and certificate:

   ```bash
   # create private key
   openssl genrsa -out server-key.pem 4096
   # create CSR for the server certificate
   openssl req -config server.cnf -key server-key.pem -new -out server-req.csr
   # have the CA created in (1) sign the certificate
   openssl x509 -req -in server-req.csr -days 365 -CA ca.crt -CAkey ca-key.pem-CAcreateserial -out server.crt    
   ```
   {: codeblock}

3. Create files used on the **client side** (the IBM Hyper Protect Virtual Servers instance).

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

   Make sure to update `dn` with your values. Whether the actual values play a role depends on the `StreamDriver.Authmode` setting (which will appear later in the following documentation). In this example, we use the setting `StreamDriver.Authmode="x509/certvalid"` and in this case, the value of `dn` does **not** play a role (since all valid client certificates are accepted). Adjust this according to your needs. For more information, see [StreamDriver.Authmode](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#streamdriver-authmode).
   {: note}

   Create the key and certificate:

   ```bash
   # create private key
   openssl genrsa -out client-key.pem 4096
   # create CSR for client auh
   openssl req -config client.cnf -key client-key.pem -new -out client-req.csr
   # have the CA created in (2) sign the certificate
   openssl x509 -req -in client-req.csr -days 365 -CA ca.crt -CAkey ca-key.pem-CAcreateserial -out client.crt
   # export key to PKCS#8 format
   openssl pkcs8 -topk8 -inform PEM -outform PEM -nocrypt -in client-key.pem -outclient-key-pkcs8.pem
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

#### Server setup
{: #syslog-server-setup}

There are many ways to set up a compatible server endpoint. The following example shows a simple setup of an [rsyslog](https://www.rsyslog.com/) server.

1. Install the required server packages (example shows ubuntu).

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

   In this configuration, we accept any client certificate that is signed by the certificate authority via the `x509/certvalid` mode. This may change depending on the `StreamDriver.Authmode` setting. See [StreamDriver.Authmode](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#streamdriver-authmode).
   {: note}
