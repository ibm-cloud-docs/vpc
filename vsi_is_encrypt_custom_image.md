---

copyright:
  years: 2019, 2026
lastupdated: "2026-02-03"

keywords: creating an encrypted custom image, qcow2

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an encrypted custom image
{: #create-encrypted-custom-image}

When you have a qcow2 custom image that meets the requirements for {{site.data.keyword.vpc_full}} infrastructure, you can encrypt it. The following procedure describes how to encrypt your custom image with LUKS encryption by using QEMU and your own unique passphrase. After you encrypt the image, you wrap the passphrase with your customer root key (CRK). The wrapped (or encrypted) data encryption key is stored with your image metadata when you import it to {{site.data.keyword.vpc_short}}.
{: shortdesc}

A quick way to create an encrypted custom image is by using image from volume. You can use this feature to create a custom image from the boot volume of an instance and specify customer-managed encryption. For more information, see [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc).

You can't use an encrypted custom image in a private catalog or on a bare metal server.
{: note}

For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys).

## How encrypted custom images work
{: #encrypted-images-work}

After you encrypt a custom image with your own passphrase, you upload it to {{site.data.keyword.cos_full_notm}}. Before you import the encrypted image to {{site.data.keyword.vpc_short}}, you need to set up a key management service (KMS) and create a customer root key (CRK). Then, use your CRK to protect the passphrase that you use to encrypt your image. By wrapping your secret passphrase with your CRK, you create what the KMS refers to as a wrapped data encryption key (WDEK). Wrapping your passphrase data encrypts it and secures it so that you never need to share your passphrase in plain text.

When you import the image, you must specify the cloud resource name (CRN) for your customer root key (CRK) that is stored in your KMS. You must also specify the ciphertext for your wrapped data encryption key (WDEK). The passphrase is stored encrypted in the WDEK always. It is only unwrapped when a virtual server that uses the encrypted image is started. 

When you're ready to provision a virtual server with the encrypted image, the encryption information isn't needed. The WDEK and the CRN of the CRK are stored as metadata with the image. For more information, see [About encrypted custom images](/docs/vpc?topic=vpc-vpc-encryption-about#byok-about-encrypted-images).  

## Overview of encrypted image process
{: #overview-encrypted-image-process}

The following steps summarize the high-level process that you need to complete to be successful with creating and importing an encrypted image. Subsequent sections provide details on how to complete the steps. 

1. Create an encrypted image by using QEMU and a passphrase of your choice to encrypt a qcow2 file with LUKS encryption. 
2. Upload the encrypted image file to {{site.data.keyword.cos_full_notm}}. 
3. Provision a key management service, create a customer root key (CRK), and then wrap your passphrase with the CRK to generate a wrapped data encryption key (WDEK).  
4. Make sure that you have the required IBM {{site.data.keyword.iamshort}} authorizations so that you can import the image from {{site.data.keyword.cos_full_notm}} and so that your WDEK can be used for data encryption.
5. Import the image to {{site.data.keyword.vpc_short}}. You must specify {{site.data.keyword.cos_full_notm}} location, the CRK that is stored in your KMS, and your WDEK ciphertext.

## Encrypting the image
{: #manually-encrypt-image}

In this step, you use QEMU to specify your own passphrase and encrypt your custom image with LUKS encryption.

The custom image that you plan to encrypt must meet the custom image requirements for {{site.data.keyword.vpc_short}} infrastructure. Confirm that you completed the image preparation requirements in the following topics:

* [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images)
* [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image)
* [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image)

To complete the encryption task, you must have [QEMU](https://www.qemu.org/){: external} version 2.12 or later installed. 

You must use a Linux or Windows operating system to complete the QEMU encryption instructions. The Mac operating system is not supported. 
{: important}

Complete the following steps by using QEMU to create a second encrypted qcow2 file of your custom image. 

1. Determine your own passphrase to use for encrypting your image. The sample commands in this task use the passphrase `abc123`. Keep your passphrase available. Later you need to wrap the passphrase to create a wrapped data encryption key (WDEK). The WDEK is required when you import the image to {{site.data.keyword.vpc_short}}.

2. Verify your current qcow2 custom image by using the following command:

   ```sh
   qemu-img info my_100G_custom_image.qcow2
   ```
   {: pre}
      
   For this example, you'd see a response similar to the following output:

   ```text
   image: my_100G_custom_image.qcow2
   file format: qcow2
   virtual size: 100 GiB (107374182400 bytes)
   disk size: 1.28 GiB
   cluster_size: 65536
   Format specific information:
       compat: 1.1
       lazy refcounts: false
       refcount bits: 16
       corrupt: false
   ```
   {: screen}
   
3. Create a new, empty qcow2 file of the exact same size and encrypt it with LUKS encryption. Use the passphrase that you choose, for example, `abc123`, to encrypt the file:

   ```sh
   qemu-img create --object secret,id=sec0,data=abc123 -f qcow2 -o encrypt.format=luks,encrypt.key-secret=sec0 my_100G_custom_image-encrypted.qcow2 100G
   ```
   {: pre}
   
4. Convert your qcow2 image, `my_100G_custom_image.qcow2` to the encrypted image, `my_100G_custom_image-encrypted.qcow2`.

   ```sh
   qemu-img convert --object secret,id=sec0,data=abc123 --image-opts driver=qcow2,file.filename=my_100G_custom_image.qcow2 --target-image-opts driver=qcow2,encrypt.key-secret=sec0,file.filename=my_100G_custom_image-encrypted.qcow2 -n -p
   ```
   {: pre}

5. Compare the two files to verify that they are identical. 

   ```sh
   qemu-img compare --object secret,id=sec0,data=abc123 --image-opts driver=qcow2,file.filename=my_100G_custom_image.qcow2  driver=qcow2,encrypt.key-secret=sec0,file.filename=my_100G_custom_image-encrypted.qcow2 -p
   ```
   {: pre}
   
6. Check the file for errors. 

   ```sh
   qemu-img check --object secret,id=sec0,data=abc123 --image-opts driver=qcow2,encrypt.key-secret=sec0,file.filename=my_100G_custom_image-encrypted.qcow2 
   ```
   {: pre}
   
   For this example, you'd see a response similar to the following output:

   ```text
   No errors were found on the image.
   16343/1638400 = 1.00% allocated, 0.00% fragmented, 0.00% compressed clusters
   Image end offset: 1074790400
   ```
   {: screen}
   
7. Run `info` on your new, encrypted file to verify that it is the size and encryption level you expect.

   ```sh
   qemu-img info my_100G_custom_image-encrypted.qcow2
   ```
   {: pre}

   For this example, you'd see a response similar to the following output:

   ```text
   image: my_100G_custom_image-encrypted.qcow2
   file format: qcow2
   virtual size: 100 GiB (107374182400 bytes)
   disk size: 1.27 GiB
   cluster_size: 65536
   Format specific information:
       compat: 1.1
       lazy refcounts: false
       refcount bits: 16
       corrupt: false
   ```
   {: screen}

## Upload the encrypted image to {{site.data.keyword.cos_full_notm}}
{: #upload-encrypted-image-to-cos}

When your image file is encrypted with LUKS encryption and your unique passphrase, you can upload it to {{site.data.keyword.cos_full_notm}} by completing the following steps: 

1. Make sure that your customized image file has a descriptive name so that you can easily identify it later. 
2. On the **Objects** page of your {{site.data.keyword.cos_full_notm}} bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB. 

## Setting up your key management service and keys
{: #kms-prereqs}

To import an encrypted custom image to {{site.data.keyword.vpc_short}}, you need a key management service. You also need a customer root key (CRK) and a wrapped data encryption key (WDEK). The WDEK is the passphrase that you used to encrypt your image wrapped with your CRK, so that your passphrase remains known only to you. The WDEK is used to access the encrypted image when a virtual server instance that uses the encrypted image is started.

The following list is a summary of the key management prerequisites:
* Provision a supported key management service, either [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-getting-started-tutorial) or [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started).
* Import a customer root key (CRK) to the key management service or create one in the key management service.
* Wrap (protect) the passphrase that you used to encrypt your image with your customer root key to create a wrapped data encryption key (WDEK).

The following example steps are specific to {{site.data.keyword.keymanagementserviceshort}}, but the general flow also applies to {{site.data.keyword.hscrypto}}. If you're using {{site.data.keyword.hscrypto}}, see the [{{site.data.keyword.hscrypto}} information](/docs/hs-crypto?topic=hs-crypto-get-started) for corresponding instructions.
{: note}

1. Provision the [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-provision) service.
   
   Provisioning a {{site.data.keyword.keymanagementserviceshort}} service instance makes sure that it includes the most recent updates that are required for customer-managed encryption.
   {: tip}
   
2. [Create](/docs/key-protect?topic=key-protect-create-root-keys) or [import](/docs/key-protect?topic=key-protect-import-root-keys) a customer root key (CRK) in {{site.data.keyword.keymanagementservicelong_notm}}.

   Plan ahead for importing keys by [reviewing your options for creating and encrypting key material](/docs/key-protect?topic=key-protect-importing-keys#plan-ahead). For added security, you can enable the secure import of the key material by using an [import token](/docs/key-protect?topic=key-protect-importing-keys#using-import-tokens) to encrypt your key material before you bring it to the cloud.
   {: tip}

3. Use your customer root key (CRK) to wrap, or protect, the unique passphrase that you used to encrypt your image with LUKS encryption. In the image encryption example, we used the passphrase `abc123`. 
 
   1. Make sure that the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in is installed so that you can generate a WDEK. For more information, see [Installing the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in](/docs/key-protect?topic=key-protect-set-up-cli#install-cli).  
   
   2. Encode your passphrase with Base64 encoding to prepare it for wrapping with your CRK. Run the following command, substituting your passphrase for `abc123`. The *-n* parameter is required so that you don't encode a new line character, which causes the wrapped passphrase to not work.   

      ```sh
      echo -n "abc123"|base64
      ```
      {: pre}

      For this example, you'd see a response similar to the following output:

      ```text
      YWJjMTIz
      ```
      {: screen}

   3. Wrap your encoded passphrase with your CRK by running the `ibmcloud kp key wrap` command. While the passphrase that is used to generate the encrypted image is not technically a data encryption key, it is the terminology that {{site.data.keyword.keymanagementserviceshort}} uses for data in wraps and unwraps. The data that is returned from {{site.data.keyword.keymanagementserviceshort}} is referred to as the WDEK. For more information, see [kp key wrap](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-key-wrap). 

      ```sh
      ibmcloud kp key wrap KEY_ID -i INSTANCE_ID -p PLAINTEXT
      ```
      {: pre}

      Where *KEY_ID* is the ID of the root key that you want to use for wrapping, *INSTANCE_ID* is the instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance, and *PLAINTEXT* is your encoded passphrase. For example, *YWJjMTIz*. 

      For this example, you'd see a response similar to the following output:

      ```text
      Wrapping key...
      SUCCESS
      Ciphertext
      eyJjaXBoZXJ0ZXh0IjoiKzhjbHVqcUNP ...<redacted>... NmY3MTJjNGViIn0=
      ```
      {: screen}

   4. Save, or persist to storage, the ciphertext for the WDEK. You must specify the WDEK ciphertext when you import your encrypted image to {{site.data.keyword.vpc_short}}.

      
## IAM authorization prerequisites
{: #iam-auth-prereqs}

Make sure that you created the required authorizations in IBM {{site.data.keyword.iamshort}}. 

1. From IBM {{site.data.keyword.iamshort}} (IAM), [create an authorization](/docs/account?topic=account-serviceauth) between **Cloud Block Storage** (source service) and **your key management service** (target service). The authorization permits the {{site.data.keyword.cloud_notm}} backplane services to use your WDEK for data encryption.
2. Make sure that you created an IAM authorization between the **Image Service for VPC** and **{{site.data.keyword.cos_full_notm}}**. Specify **Infrastructure Services** as the source service. Specify **Image Service for VPC** as the resource type. Specify **{{site.data.keyword.cos_full_notm}}** as the target service. The authorization is so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}. For more information, see [Granting access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq).


## Next steps
{: #encrypt-next-steps}

When your image is successfully encrypted, your KMS is set up, and you created the required keys, you can [import](/docs/vpc?topic=vpc-importing-custom-images-vpc) the image to {{site.data.keyword.vpc_short}}. When the image is available in {{site.data.keyword.vpc_short}}, you can use it to provision instances. Make sure that you have [Granted access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq).  

When you are ready to provision a new virtual server instance with the encrypted image, no encryption information is needed. The wrapped data encryption key (WDEK) and the CRN of the customer root key (CRK) are stored as metadata with the image.
