---

copyright:
  years: 2022
lastupdated: "2022-07-26"

keywords: confidential computing, secure execution, hpcr, contract, customization, env, workload, encryption, attestation, validating

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Validating the certificates
{: #cert_validate}

You can validate the certificates that you download for contract encryption and attestation.
{: shortdesc}


## Downloading the certificates
{: #download_cert}

Download the following certificates:
* Get the DigiCert certificates. The DigiCert Trusted Root G4 certificate can be downloaded [here](https://cacerts.digicert.com/DigiCertTrustedRootG4.crt.pem), and the Digicert G4 intermediate certificate can be downloaded [here](https://cacerts.digicert.com/DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem).
* Get the [IBM intermediate certificate](https://cloud.ibm.com/media/docs/downloads/hyper-protect-container-runtime/ibm-hyper-protect-container-runtime-1-0-s390x-3-intermediate.crt).

## Validating the contract encryption certificate
{: #validate_encrypt_cert}

Complete the following steps on an Ubuntu system, to validate the encryption certificate:
1. Use the following command to verify the CA certificate:
   ```sh
   openssl verify -crl_download -crl_check DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem
   ```
   {: pre}

2. Use the following command to verify the signing key certificate:
   ```sh
   openssl verify -crl_download -crl_check -untrusted DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem ibm-hyper-protect-container-runtime-1-0-s390x-3-intermediate.crt
   ```
   {: pre}

3. Complete the following steps to verify the signature of the encrypted certificate document:
   1. Extract the public signing key into a file. In the following example, the file is called `pubkey.pem`:
      ```sh
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-3-intermediate.crt -pubkey -noout >  pubkey.pem
      ```
      {: pre}

   2. Extract the encrypt key signature from the encrypt certificate document.
      The following command returns the offset value of the signature:
      ```
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-3-encrypt.crt | tail -1 | cut -d : -f 1
      ```
      Consider that the output of the command is <offset_value>. Use this <offset_value> to extract the encrypt key signature into a file called signature:
      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-3-encrypt.crt -out signature -strparse <offset_value> -noout
      ```
      {: pre}

   3. Extract the body of the encrypt key document into a file called body.
      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-3-encrypt.crt -out body -strparse 4 -noout
      ```
      {: pre}

   4. Verify the signature by using the signature and body files:
      ```sh
      openssl sha512 -verify pubkey.pem -signature signature body
      ```
      {: pre}

4. Verify the host key document issuer. Compare the output of the following two commands. The output should match.
   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-3-encrypt.crt  -issuer -noout
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-3-intermediate.crt -subject -noout
   ```
   {: pre}

5. Verify that the encrypt document is still valid by checking the output of the following command:
   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-3-encrypt.crt -dates -noout
   ```
   {: pre}

## Validating the attestation certificate
{: #validate_attest_cert}

Complete the following steps on an Ubuntu system, to validate the encryption certificate:
1. Use the following command to verify the CA certificate:
   ```sh
   openssl verify -crl_download -crl_check DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem
   ```
   {: pre}

2. Use the following command to verify the signing key certificate:
   ```sh
   openssl verify -crl_download -crl_check -untrusted DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem ibm-hyper-protect-container-runtime-1-0-s390x-3-intermediate.crt
   ```
   {: pre}

3. Complete the following steps to verify the signature of the encrypted certificate document:
   1. Extract the public signing key into a file. In the following example, the file is called `pubkey.pem`:
      ```sh
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-3-intermediate.crt -pubkey -noout >  pubkey.pem
      ```
      {: pre}

   2. Extract the encrypt key signature from the encrypt certificate document.
      The following command returns the offset value of the signature:
      ```
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-3-attestation.crt | tail -1 | cut -d : -f 1
      ```
      Consider that the output of the command is <offset_value>. Use this <offset_value> to extract the encrypt key signature into a file called signature:
      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-3-attestation.crt -out signature -strparse <offset_value> -noout
      ```
      {: pre}

   3. Extract the body of the encrypt key document into a file called body.
      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-3-attestation.crt -out body -strparse 4 -noout
      ```
      {: pre}

   4. Verify the signature by using the signature and body files:
      ```sh
      openssl sha512 -verify pubkey.pem -signature signature body
      ```
      {: pre}

4. Verify the host key document issuer. Compare the output of the following two commands. The output should match.
   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-3-attestation.crt -issuer -noout
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-3-intermediate.crt -subject -noout
   ```
   {: pre}

5. Verify that the encrypt document is still valid by checking the output of the following command:
   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-3-attestation.crt -dates -noout
   ```
   {: pre}

   You can now use the hashes from the attestation record for validation.
