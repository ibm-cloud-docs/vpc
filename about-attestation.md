---

copyright:
  years: 2022, 2025
lastupdated: "2025-08-25"

keywords: confidential computing, enclave, secure execution, hpcr, hyper protect virtual server for vpc

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attestation
{: #about-attestation}

Attestation is a process that starts by default at virtual instance creation, ensures that the virtual server instance image is indeed built by IBM, and that it was not modified. This process also provides information and allows validation of any data that is provided to the instance at the time of deployment.
{: shortdesc}

When you create a virtual server instance by using the IBM Hyper Protect Container Runtime image, the image uses an initial file system that is protected by encryption and signed by IBM Secure Execution. For more information, see [Confidential computing with LinuxONE](/docs/vpc?topic=vpc-about-se). To know more about the attestation process, see this [video](https://mediacenter.ibm.com/media/IBM+Cloud+Hyper+Protect+Virtual+Server+for+VPC.mp4/1_mj9ksaob).

The boot process creates a unique root disk encryption key to ensure protection of the root disk. To perform attestation, the virtual server instance image contains an attestation-signing key and the hash of the root partition at build time. The boot process validates the root partition. If the hash of the root partition does not match, the boot process does not continue because it assumes that the image was modified before boot. The attestation signing key is a random RSA 4 K key that is signed by an IBM root key that is maintained in Hyper Protect Crypto Services. The IBM root key is signed by Digicert.

During deployment of the virtual server instance in the cloud, an attestation record is created. It contains hashes of the following items:

* The original base image
* The root partition at the moment of the first boot
* The root partition at build time
* The cloud initialization options

The attestation record is signed by the attestation key. As an extra protection layer, you can provide a public key during deployment, against which the attestation record is encrypted. The hash of this public key is added to the attestation record so that the record can be viewed only by the compliance authority. The expected authority can be easily identified through that hash.

Before you upload any workload to your instance, you need to validate the attestation record. After an instance is created, you can validate the attestation record within the created instance. Your instance must have access to the `/var/hyperprotect` directory. If so, follow these procedures:

* The attestation record is signed by the attestation signing key.
* The attestation signing key can be confirmed by the IBM intermediate certificate. The IBM intermediate certificate is signed by DigiCert, which is proven by the root certificate of DigiCert, thus completing the chain of trust.

The encryption and attestation certificates are signed by the IBM intermediate certificate, which are signed by the IBM Digicert intermediate cert. The IBM Digicert intermediate cert is signed by DigiCert Trusted Root G4. For more information about the certificates, see [DigiCert Trusted Root Authority Certificates](https://www.digicert.com/kb/digicert-root-certificates.htm).

Use the following procedure to validate the attestation record and hashes:

* Obtain the attestation record `se-checksums.txt` and the signature file `se-signature.bin` from your {{site.data.keyword.hpvs}} for VPC instance. To do so, you can implement your container to provide the attestation record and the signature file. The attestation record and the signature file are made available to your container in the `/var/hyperprotect` directory.
* Get the IBM attestation certificate. The following table lists the expiry dates for the attestation certificates based on the version of the image.

From 25 March 2025, the certificate links are changed.
{: note}

   | Image version| Certificate link | Expiry date |
   |--------------|------------------|-------------|
   | `ibm-hyper-protect-container-runtime-1-0-s390x-23` | [Certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-23/ibm-hyper-protect-container-runtime-1-0-s390x-23-attestation.crt){: external} | 25 April 2026 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-22` | [Certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-22/ibm-hyper-protect-container-runtime-1-0-s390x-22-attestation.crt){: external} | 21 March 2026 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-21` | [Certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-21/ibm-hyper-protect-container-runtime-1-0-s390x-21-attestation.crt){: external} | 04 March 2026 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-20` | [Certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-20/ibm-hyper-protect-container-runtime-1-0-s390x-20-attestation.crt){: external} | 20 November 2025 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-19` | [Certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-19/ibm-hyper-protect-container-runtime-1-0-s390x-19-attestation.crt){: external} | 19 September 2025 |
   {: caption="Attestation certificate expiry dates" caption-side="bottom"}

* Validate the attestation certificate by following the instructions [here](/docs/vpc?topic=vpc-cert_validate#validate_attest_cert).
* Extract the attestation public key from the attestation certificate by using the following command:

   ```sh
   openssl x509 -pubkey -noout -in ibm-hyper-protect-container-runtime-1-0-s390x-23-attestation.crt > contract-public-key.pub
   ```
   {: pre}


* Verify the signature of the attestation record:

   ```sh
   openssl sha256 -verify contract-public-key.pub -signature se-signature.bin se-checksums.txt
   ```
   {: pre}

   Signature verification must be done on a [decrypted attestation file](#decrypt_attest_record).
   {: note}

* You can now use the hashes from the attestation record for validation.

In case you provided a public key for encrypting the attestation record, the following script might help in decrypting the record.

```sh
#!/bin/bash
#
# Example script to decrypt attestation document.
#
# Usage:
#   ./decrypt-attestation.sh <rsa-priv-key.pem> [file]
#
# Token Format:
#   hyper-protect-basic.<ENC_AES_KEY_BASE64>.<ENC_MESSAGE_BASE64>


RSA_PRIV_KEY="$1"
if [ -z "$RSA_PRIV_KEY" ]; then
    echo "Usage: $0 <rsa-priv-key.pem>"
    exit 1
fi
INPUT_FILE="${2:-se-checksums.txt.enc}"
TMP_DIR="$(mktemp -d)"
#trap 'rm -r $TMP_DIR' EXIT


PASSWORD_ENC="${TMP_DIR}/password_enc"
MESSAGE_ENC="${TMP_DIR}/message_enc"


# extract encrypted AES key and encrypted message
cut -d. -f 2 "$INPUT_FILE"| base64 -d > "$PASSWORD_ENC"
cut -d. -f 3 "$INPUT_FILE"| base64 -d > "$MESSAGE_ENC"

# decrypt password
PASSWORD=$(openssl pkeyutl -decrypt -inkey "$RSA_PRIV_KEY" -in "$PASSWORD_ENC")

# decrypt message
echo -n "$PASSWORD" | openssl aes-256-cbc -d -pbkdf2 -in "$MESSAGE_ENC" -pass stdin --out se-checksums.txt
```
{: codeblock}

In the case of a docker container, the `decrypt-attestation.sh` file can be accessed by mounting `/var/hyperprotect` in the docker container. For example,

```sh
 volumes:
      - "/var/hyperprotect/:/var/hyperprotect/:ro"
```
{: codeblock}

In the case of a Podman container, the `decrypt-attestation.sh` file can be accessed by mounting `/var/hyperprotect` in the Podman container. For example,

```sh
 volumeMounts:
     - name: attestation
       readOnly: true
       mountPath: /var/hyperprotect:Z,U
```
{: codeblock}

## The attestation document
{: #attestation_doc}

The attestation document is available at `/var/hyperprotect/se-checksums.txt`, within the {{site.data.keyword.hpvs}} for VPC instance. The other related files are also located in the same directory.

The following information is available at the `/var/hyperprotect/` directory:

```sh
/var/hyperprotect/
|-- cidata
|   |-- meta-data
|   |-- vendor-data
|-- se-checksums.txt
|-- se-signature.bin
|-- se-version
|-- user-data.decrypted
```
{: codeblock}

Checksums are the SHA256 of the message digest and can be calculated by using the following Linux command-line utility:

```sh
sha256sum <file>
```
{: pre}

The following snippet is an example of an attestation document:

```text
25.4.0
Machine Type/Plant/Serial: 3932/02/8A018
Image age: 11 days since creation.
Ultravisor CUID: 0x580f2286ec628711ec828b17b31a572c
Host HKD: HKD-3932-028A018.crt (HKD-3932-028A018.crt)
HKD is valid until: Feb 27 18:11:39 2025 GMT
AP device present: true
Number of configured APs: 1
e23a548070908ae09eb8b42df1865c2bc03c2f7135ddbae56b9064ec015fc867 AP(1):secret
84ae048bc5d88e99f6ec13b4c4ba3e2ffe5f10285f7dd71a65ea99eaa1838ce0 root.tar.gz
3e13f7658ef790dbc040e90ff4f8d537c9c10da879b0b16df9e98265c7b5170a baseimage
348f4e1c9ff891956ab23f4dbec91b3cc46a761afc1187d06c2d77ef87ed44b9 /dev/disk/by-label/cidata
58d327a44a3a13a679b153900ab215c4a163c7f9ca8f4152074027f9d1d2d33a cidata/meta-data
c2530f161027a25e948a82b50e1b7fd4a0df74449d0317c315e30d9e089f2f23 cidata/user-data
5a2b8897d00e4f03436494a36304776a637bf3f134ae10107f2a47e9859ed0fb cidata/vendor-data
a17a6ea7d8fdd9831c345510c1ceb4823c929ccef89bfab3d43fdf1b6a87871f contract:attestationPublicKey 
23364d8558c5d151e15963543c82c8127d05f2635720fd559a76e5cc20290ab7 contract:env 
e568d46f563a0afc41059f71e4c9e9eab26c4f4dffde24701dc3e7d5b14a16ae contract:workload
```
{: codeblock}

### `Machine Type/Plant/Serial`
{: #machine_type_plant_serial}

`Machine Type/Plant/Serial` is the information required to obtain a Host Key Document for the secure execution VM. It reflects on which machine the secure execution VM is currently running.

### `baseimage`
{: #base_image}

The `baseimage` is the IBM internal QEMU Copy On Write Version 2 (QCOW2) file, which is used as the source for most of the operating system files of the Hyper Protect Container Runtime image. It is used only at image build time by the enabler process. The enabler uses this source together with other Debian packages to create the `root.tar.gz` and the encrypted secure execution kernel or 'initrd' image.

Following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-23 `baseimage`:

```sh
3e13f7658ef790dbc040e90ff4f8d537c9c10da879b0b16df9e98265c7b5170a baseimage
```
{: pre}

Following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-22 `baseimage`:

```sh
538170f79b7bd44553847e81afce7ae14c8ea8857df243e4f8656c9d06d42c18 baseimage
```
{: pre}

Following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-21 `baseimage`:

```sh
538170f79b7bd44553847e81afce7ae14c8ea8857df243e4f8656c9d06d42c18 baseimage
```
{: pre}

Following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-20 `baseimage`:

```sh
080f817231fe4bc40021d24e20af9f1135a36711047212f9374664b86ab406ac baseimage
```
{: pre}

The following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-19 `baseimage`:

```sh
b3fb54a3bb57fcbba070c456d7394fb5e7a4f9b1febaec0efc32c5aa0e325071 baseimage
```
{: pre}

The following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-18 `baseimage`:

```sh
f9940321d043d133ddf2f22cd4794bc56be9d54b2e9c7893a6d6a4813635024c baseimage
```
{: pre}

The following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-17 and ibm-hyper-protect-container-runtime-1-0-s390x-16 `baseimage`:

```sh
f9940321d043d133ddf2f22cd4794bc56be9d54b2e9c7893a6d6a4813635024c baseimage
```
{: pre}



### `root.tar.gz`
{: #root_tarfile}

The `root.tar.gz` is part of the final secure execution enabled IBM Hyper Protect Container Runtime image and contains all operating system files. It is stored on the image's first partition (boot partition) as `/boot/root.tar.gz`.

Following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-23 `root.tar.gz`.

```sh
84ae048bc5d88e99f6ec13b4c4ba3e2ffe5f10285f7dd71a65ea99eaa1838ce0 root.tar.gz
```
{: pre}

Following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-22 `root.tar.gz`.

```sh
ff09f53f19d0f82ca24d4f2d5277c851516734c3d55ae7f8db47cde378a51ec9 root.tar.gz
```
{: pre}

Following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-21 `root.tar.gz`.

```sh
024ff109be23e1e4e7b9f07dc553afc60a5a93645939eedf2a936930cc8a44ae root.tar.gz
```
{: pre}

Following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-20 `root.tar.gz`.

```sh
ad65a3820d4a233c84e6d201ce537b8020435ccefe26682809da5ef9b176b8ae root.tar.gz
```
{: pre}

Following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-19 `root.tar.gz`.

```sh
d8e069345618143ce0948044cab27ba52ffc53eaba987d353bb5ab59f3ad2390 root.tar.gz
```
{: pre}


Following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-18 `root.tar.gz`.

```sh
4802242b82dce78d11c4b02129a9b9c1e62598b1cb8073f8e7b097449644bbb4 root.tar.gz
```
{: pre}

Following is the shasum of the ibm-hyper-protect-container-runtime-1-0-s390x-17 and ibm-hyper-protect-container-runtime-1-0-s390x-16 `root.tar.gz`.

```sh
4802242b82dce78d11c4b02129a9b9c1e62598b1cb8073f8e7b097449644bbb4 root.tar.gz
```
{: pre}



### `/dev/disk/by-label/cidata`
{: #block_device_cidata}

The `/dev/disk/by-label/cidata` is a block device that is attached to the running instance that contains the cloud-init files as provided by {{site.data.keyword.vpc_full}} (VPC). For more information about Cloud-Init, see [User data](/docs/vpc?topic=vpc-user-data), or [cloud-init documentation](https://cloudinit.readthedocs.io/en/latest/).

### `cidata`
{: #cidata}

```sh
58d327a44a3a13a679b153900ab215c4a163c7f9ca8f4152074027f9d1d2d33a cidata/meta-data
c2530f161027a25e948a82b50e1b7fd4a0df74449d0317c315e30d9e089f2f23 cidata/user-data
5a2b8897d00e4f03436494a36304776a637bf3f134ae10107f2a47e9859ed0fb cidata/vendor-data
```
{: codeblock}

### `attestationPublicKey`
{: #attest_pubkey}

The `attestationPublicKey` is the public key that you provide which is used to encrypt the attestation document. The `attestationPublicKey` is part of the user-data file. Encrypting the attestation document is optional.

```sh
a17a6ea7d8fdd9831c345510c1ceb4823c929ccef89bfab3d43fdf1b6a87871f contract:attestationPublicKey
```
{: pre}

### Decrypting the attestation document
{: #decrypt_attest_record}

If user data contains a public RSA key (attribute: attestationPublicKey), then the attestation document (se-checksums.txt) is encrypted with the given key. The encryption is done by the same process as that of contract encryption. For more information, see [Contract encryption](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_encrypt). The public RSA key itself can also be encrypted like the contract.

The encrypted attestation document is then named `se-checksums.txt.enc`.

In the case of a docker container, the `decrypt-attestation.sh` file can be accessed by mounting `/var/hyperprotect` in the docker container. For example,

```sh
 volumes:
      - "/var/hyperprotect/:/var/hyperprotect/:ro"
```
{: codeblock}

In the case of a Podman container, the `decrypt-attestation.sh` file can be accessed by mounting `/var/hyperprotect` in the Podman container. For example,

```sh
 volumeMounts:
     - name: attestation
       readOnly: true
       mountPath: /var/hyperprotect:Z,U
```
{: codeblock}

## Understanding attestation flows
{: #attestation_flow}

The following diagram shows two scenarios for attestation from the point of view of the auditor to validate that the deployment is the expected one. The left side of the diagram shows the establishment of trust by the auditor who is rooted on a third-party certificate authority. Any key used is kept in a Hyper Protect Crypto Service and signed in a certificate chain based on the third-party authority. The Build environment that is used by Hyper Protect is running in a trusted execution environment by using IBM Secure Execution Technology.

The result is a secure execution image that is seen at the end of the diagram, which is an encrypted secure execution image. To the right of the diagram, the validation of the deployment is outlined. For this, the auditor includes into the encrypted workload contract of the IBM Hyper Protect instance, the public key of a secret, which only the auditor has control over. Such secrets might be protected by appropriate means, like a Hyper Protect Crypto Service, HSM or just a random key. Only the Hyper Protect bootloader that is executed in the trusted execution environment provided through IBM Secure Execution for Linux on IBM LinuxONE, can run the secure execution image of {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}}. The bootloader contains the secret to decrypt the contract.

During boot several hashes of components and measures of code are taken and added to the attestation record. To further protect this attestation record, the record is encrypted with the public key that the auditor provided. By doing so, only the auditor is in the position to decrypt the attestation record and can validate that the workload that is deployed in the enclave is the expected and untampered version of the workload that is expected to be deployed into the {{site.data.keyword.hpvs}} for VPC instance.

![Figure showing the attestation process](images/vsi_se_attestationrecord.png "Figure showing the attestation process"){: caption="Attestation process" caption-side="bottom"}

## Next steps
{: #next-steps-attestation}

* [Creating an instance by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers)
* [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli)
