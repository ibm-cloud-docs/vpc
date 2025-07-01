---

copyright:
  years: 2022, 2025
lastupdated: "2025-07-01"

keywords: confidential computing, secure execution, hpcr, contract, customization, env, workload, encryption, attestation, validating

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Validating the certificates
{: #cert_validate}

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
   | `ibm-hyper-protect-container-runtime-1-0-s390x-23` | [certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-23/ibm-hyper-protect-container-runtime-1-0-s390x-23-intermediate.crt){: external} | 01 September 2026 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-22` | [certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-22/ibm-hyper-protect-container-runtime-1-0-s390x-22-intermediate.crt){: external} | 01 September 2026 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-21` | [certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-21/ibm-hyper-protect-container-runtime-1-0-s390x-21-intermediate.crt){: external} | 01 September 2026 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-20` | [certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-20/ibm-hyper-protect-container-runtime-1-0-s390x-20-intermediate.crt){: external} | 01 September 2026 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-19` | [certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-19/ibm-hyper-protect-container-runtime-1-0-s390x-19-intermediate.crt){: external} | 01 September 2026 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-18` | [certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-18/ibm-hyper-protect-container-runtime-1-0-s390x-18-intermediate.crt){: external} | 03 June 2026 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-17` | [certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-17/ibm-hyper-protect-container-runtime-1-0-s390x-17-intermediate.crt){: external} | 03 June 2026 |
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
   openssl verify -crl_download -crl_check -untrusted DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem ibm-hyper-protect-container-runtime-1-0-s390x-23-intermediate.crt
   ```
   {: pre}

   If the `OpenSSL` command fails to execute, download the [CRL](http://crl3.digicert.com/DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crl) and verify certificate manually using below command:

   ```sh
   openssl verify -untrusted DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem -CRLfile DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crl ibm-hyper-protect-container-runtime-1-0-s390x-23-intermediate.crt
   ```
   {: pre}

3. Complete the following steps to verify the signature of the attestation certificate document:

   1. Extract the public signing key into a file. In the following example, the file is called `pubkey.pem`:

      ```sh
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-23-intermediate.crt -pubkey -noout >  pubkey.pem
      ```
      {: pre}

   2. Extract the encryption key signature from the encryption certificate document.
      The following command returns the offset value of the signature:

      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-23-encrypt.crt | tail -1 | cut -d : -f 1
      ```
      {: pre}

      Consider that the output of the command is `<offset_value>`. Use this `<offset_value>` to extract the encryption key signature into a file called signature:

      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-23-encrypt.crt -out signature -strparse <offset_value> -noout
      ```
      {: pre}

   3. Extract the body of the encryption certificate document into a file called body.

      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-23-encrypt.crt -out body -strparse 4 -noout
      ```
      {: pre}

   4. Verify the signature by using the signature and body files:
      ```sh
      openssl sha512 -verify pubkey.pem -signature signature body
      ```
      {: pre}

4. Verify the certificates issuer. Compare the output of the following two commands. The output should match.

   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-23-encrypt.crt  -issuer -noout
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-23-intermediate.crt -subject -noout
   ```
   {: pre}
   
5. Verify that the encryption certificate document is still valid by checking the output of the following command:

   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-23-encrypt.crt -dates -noout
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
   openssl verify -crl_download -crl_check -untrusted DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem ibm-hyper-protect-container-runtime-1-0-s390x-23-intermediate.crt
   ```
   {: pre}

3. Complete the following steps to verify the signature of the encrypted certificate document:
   1. Extract the public signing key into a file. In the following example, the file is called `pubkey.pem`:

      ```sh
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-23-intermediate.crt -pubkey -noout >  pubkey.pem
      ```
      {: pre}

   2. Extract the attestation key signature from the attestation certificate document.
      The following command returns the offset value of the signature:

      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-23-attestation.crt | tail -1 | cut -d : -f 1
      ```
      {: pre}

      Consider that the output of the command is `<offset_value>`. Use this `<offset_value>` to extract the attestation key signature into a file called signature:

      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-23-attestation.crt -out signature -strparse <offset_value> -noout
      ```
      {: pre}

   3. Extract the body of the attestation certificate document into a file called body.

      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-23-attestation.crt -out body -strparse 4 -noout
       ```
      {: pre}

   4. Verify the signature by using the signature and body files:
      ```sh
      openssl sha512 -verify pubkey.pem -signature signature body
      ```
      {: pre}

4. Verify the certificates issuer. Compare the output of the following two commands. The output should match.

   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-23-attestation.crt -issuer -noout
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-23-intermediate.crt -subject -noout
   ```
   {: pre}

5. Verify that the attestation certificate document is still valid by checking the output of the following command:

   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-23-attestation.crt -dates -noout
   ```
   {: pre}

## Certificate revocation list
{: #certificate-revocation-list}

The certificates contain **Certificate Revocation List (CRL) Distribution Points**. You can use the CRL to verify that your certificates are valid (not revoked).

1. Extract and download the CRL URL from the attestation or encryption certificate:

   ```Sh
   openssl x509 -in "ibm-hyper-protect-container-runtime-1-0-s390x-23-encrypt.crt" -noout -ext crlDistributionPoints
   crl_url= https://ibm.biz/hyper-protect-container-runtime-0b8907-crl-1 # (example)
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
   openssl x509 -in "ibm-hyper-protect-container-runtime-1-0-s390x-23-intermediate.crt" -pubkey -noout -out pubkey
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
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-23-encrypt.crt -noout -serial
      serial=F0E13E14FABECF4BA192C1FE9FE48891 # (example)
      ```
      {: pre}

   2. Export the value of 'serial' by running the following command:

      ```sh
      export serial=F0E13E14FABECF4BA192C1FE9FE48891 # (example)
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
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-23-attestation.crt -noout -serial
      serial=D81F1467D9C543F6688960B8C6BDB578  # (example)
      ```
      {: pre}


   2. Export the value of 'serial' by running the following command:
      ```sh
      export serial=D81F1467D9C543F6688960B8C6BDB578  # (example)
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
