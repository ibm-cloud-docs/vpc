---

copyright:
  years: 2022, 2026
lastupdated: "2026-02-20"

keywords: confidential computing, secure execution, hpcr, contract, customization, env, workload, encryption, attestation, validating

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Validating the certificates
{: #cert_validate}

The {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for VPC is deprecated. As of 28 February 2026, you can't create new instances. Existing instances are supported until 20 February 2027. Any instances that still exist on that date will be deleted. You can redeploy your workloads by using [IBM Confidential Computing Container Runtime (formerly known as Hyper Protect Virtual Servers)](https://www.ibm.com/docs/en/hpvs) or [IBM Confidential Computing Container Runtime for Red Hat Virtualization Solutions (formerly known as Hyper Protect Container Runtime for Red Hat Virtualization Solutions)](https://www.ibm.com/docs/en/hpcr/1.1.x). For information about data migration, see the [Migration guide](/docs/vpc?topic=vpc-migration_guide). For more information, see the [Service deprecation announcement](/docs/vpc?topic=vpc-ichpcs_deprecated_anmt).
{: deprecated}

You can validate the certificates that you download for contract encryption and attestation.
{: shortdesc}

## Downloading the certificates
{: #download_cert}

Download the following certificates:
* Get the DigiCert certificates. The DigiCert Trusted Root G4 certificate can be downloaded [here](https://cacerts.digicert.com/DigiCertTrustedRootG4.crt.pem){: external}, and the Digicert G4 intermediate certificate can be downloaded [here](https://cacerts.digicert.com/DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem){: external}.
* Get the IBM intermediate certificate. The following table lists the expiry dates for the intermediate certificates based on the version of the image.

From 25 March 2025, the certificate links are changed.
{: note}

   | Image version| Certificate link | Expiry date |
   | -------- | ----------- | ----------- |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-25` | [certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-25/ibm-hyper-protect-container-runtime-1-0-s390x-25-intermediate.crt){: external} | 03 August 2027 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-24` | [certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-24/ibm-hyper-protect-container-runtime-1-0-s390x-24-intermediate.crt){: external} | 03 August 2027 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-23` | [certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-23/ibm-hyper-protect-container-runtime-1-0-s390x-23-intermediate.crt){: external} | 01 September 2026 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-22` | [certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-22/ibm-hyper-protect-container-runtime-1-0-s390x-22-intermediate.crt){: external} | 01 September 2026 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-21` | [certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-21/ibm-hyper-protect-container-runtime-1-0-s390x-21-intermediate.crt){: external} | 01 September 2026 |
   {: caption="Intermediate certificate expiry dates" caption-side="bottom"}


* Ensure to use the certificates corresponding to the hyper protect container runtime image for contract encryption and attestation.

## Validating the contract encryption certificate
{: #validate_encrypt_cert}

Complete the following steps on an Ubuntu system to validate the encryption certificate:

1. Use the following command to verify the CA certificate:

   ```sh
   openssl verify -crl_download -crl_check DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem
   ```
   {: pre}

2. Use the following command to verify the signing key certificate:

   ```sh
   openssl verify -crl_download -crl_check -untrusted DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem ibm-hyper-protect-container-runtime-1-0-s390x-25-intermediate.crt
   ```
   {: pre}

   If the `OpenSSL` command fails to execute, download the [CRL](http://crl3.digicert.com/DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crl) and verify certificate manually by using the following command:

   ```sh
   openssl verify -untrusted DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem -CRLfile DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crl ibm-hyper-protect-container-runtime-1-0-s390x-25-intermediate.crt
   ```
   {: pre}

3. Complete the following steps to verify the signature of the encryption certificate document:

   1. Extract the public signing key into a file. In the following example, the file is called `pubkey.pem`:

      ```sh
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-25-intermediate.crt -pubkey -noout >  pubkey.pem
      ```
      {: pre}

   2. Extract the encryption key signature from the encryption certificate document. To download the encryption certificate, visit [Downloading the encryption certificate and extracting the public key](/docs/vpc?topic=vpc-about-contract_se#encrypt_downloadcert).
      The following command returns the offset value of the signature:

      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt | tail -1 | cut -d : -f 1
      ```
      {: pre}

      Consider that the output of the command is `<offset_value>`. Use this `<offset_value>` to extract the encryption key signature into a file called signature:

      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt -out signature -strparse <offset_value> -noout
      ```
      {: pre}

   3. Extract the body of the encryption certificate document into a file called body.

      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt -out body -strparse 4 -noout
      ```
      {: pre}

   4. Verify the signature by using the signature and body files:
      ```sh
      openssl sha512 -verify pubkey.pem -signature signature body
      ```
      {: pre}

4. Verify the certificates issuer. Compare the output of the following two commands. The output should match.

   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt  -issuer -noout
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-25-intermediate.crt -subject -noout
   ```
   {: pre}
   
5. Verify that the encryption certificate document is still valid by checking the output of the following command:

   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt -dates -noout
   ```
   {: pre}

## Validating the attestation certificate
{: #validate_attest_cert}

Complete the following steps on an Ubuntu system to validate the attestation certificate:

1. Use the following command to verify the CA certificate:

   ```sh
   openssl verify -crl_download -crl_check DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem
   ```
   {: pre}

2. Use the following command to verify the signing key certificate:

   ```sh
   openssl verify -crl_download -crl_check -untrusted DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem ibm-hyper-protect-container-runtime-1-0-s390x-25-intermediate.crt
   ```
   {: pre}

   If the `OpenSSL` command fails to execute, download the [CRL](http://crl3.digicert.com/DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crl) and verify certificate manually by using the following command:

   ```sh
   openssl verify -untrusted DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem -CRLfile DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crl ibm-hyper-protect-container-runtime-1-0-s390x-25-intermediate.crt
   ```
   {: pre}

3. Complete the following steps to verify the signature of the attestation certificate document:
   1. Extract the public signing key into a file. In the following example, the file is called `pubkey.pem`:

      ```sh
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-25-intermediate.crt -pubkey -noout >  pubkey.pem
      ```
      {: pre}

   2. Extract the attestation key signature from the attestation certificate document. For downloading the attestation certificate, visit [Attestation](/docs/vpc?topic=vpc-about-attestation).
      The following command returns the offset value of the signature:

      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-25-attestation.crt | tail -1 | cut -d : -f 1
      ```
      {: pre}

      Consider that the output of the command is `<offset_value>`. Use this `<offset_value>` to extract the attestation key signature into a file called signature:

      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-25-attestation.crt -out signature -strparse <offset_value> -noout
      ```
      {: pre}

   3. Extract the body of the attestation certificate document into a file called body.

      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-25-attestation.crt -out body -strparse 4 -noout
       ```
      {: pre}

   4. Verify the signature by using the signature and body files:
      ```sh
      openssl sha512 -verify pubkey.pem -signature signature body
      ```
      {: pre}

4. Verify the certificates issuer. Compare the output of the following two commands. The output should match.

   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-25-attestation.crt -issuer -noout
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-25-intermediate.crt -subject -noout
   ```
   {: pre}

5. Verify that the attestation certificate document is still valid by checking the output of the following command:

   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-25-attestation.crt -dates -noout
   ```
   {: pre}

## Certificate revocation list
{: #certificate-revocation-list}

The certificates contain **Certificate Revocation List (CRL) Distribution Points**. You can use the CRL to verify that your certificates are valid (not revoked).

1. Extract and download the CRL URL from the attestation or encryption certificate:

   ```Sh
   openssl x509 -in "ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt" -noout -ext crlDistributionPoints
   crl_url=https://ibm.biz/hyper-protect-container-runtime-0b8907-crl-1 # (example)
   curl --location --silent "$crl_url" --output "ibm-hyper-protect-container-runtime.crl"
   ```
   {: pre}

2. Verify that the CRL is valid (check valid dates and issuer):

   ```sh
   openssl crl -text -noout -in "ibm-hyper-protect-container-runtime.crl"
   ```
   {: pre}

3. Verify the CRL signature:

   ```sh
   openssl x509 -in "ibm-hyper-protect-container-runtime-1-0-s390x-25-intermediate.crt" -pubkey -noout -out pubkey
   bbegin="$(openssl asn1parse -in "ibm-hyper-protect-container-runtime.crl" | head -2 | tail -1 | cut -d : -f 1)"
   bend="$(openssl asn1parse -in "ibm-hyper-protect-container-runtime.crl" | tail -1 | cut -d : -f 1)"
   openssl asn1parse -in "ibm-hyper-protect-container-runtime.crl" -out signature -strparse $bend -noout
   openssl asn1parse -in "ibm-hyper-protect-container-runtime.crl" -out body -strparse $bbegin -noout
   openssl sha512 -verify pubkey -signature signature body
   ```
   {: codeblock}

4. Verify that the encryption certificate document is valid:
   1. Extract the serial from the encryption certificate:

      ```sh
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt -noout -serial
      ```
      {: pre}
      
      Example output:

      ```sh
      serial=4B956E1568B55D4C971203F34409BAD5
      ```
      {: pre}

   2. Export the value of 'serial' by running the following command:

      ```sh
      export serial=4B956E1568B55D4C971203F34409BAD5
      ```
      {: pre}

      You can verify if the value is set by running the following command:
      ```sh
      echo $serial
      ```
      {: pre}

   3. Verify that the certificate is not listed within the CRL:
      ```sh
      openssl crl -text -noout -in "ibm-hyper-protect-container-runtime.crl" | grep -q "$serial" && echo REVOKED || echo OK
      ```
      {: pre}

   A revoked encryption certificate document must not be used for further encryptions.

5. Verify that the attestation certificate document is valid:
   1. Extract the serial from the attestation certificate:
   
      ```sh
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-25-attestation.crt -noout -serial
      
      ```
      {: pre}
      
      Example output:

      ```sh
      serial=60FCA6CFF60A3F1A8C62C387E6C63781
      ```
      {: pre}

   2. Export the value of 'serial' by running the following command:
      ```sh
      export serial=60FCA6CFF60A3F1A8C62C387E6C63781
      ```
      {: pre}

      You can verify if the value is set by running the following command:
      ```sh
      echo $serial
      ```
      {: pre}

   3. Verify that the certificate is not listed within the CRL:
      ```sh
      openssl crl -text -noout -in "ibm-hyper-protect-container-runtime.crl" | grep -q "$serial" && echo REVOKED || echo OK
      ```
      {: pre}

   An image with a revoked attestation certificate document must not be started.
