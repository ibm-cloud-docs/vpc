---

copyright:
  years: 2022, 2025
lastupdated: "2025-06-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Setting up client-to-site authentication
{: #client-to-site-authentication}

Configure your authentication settings for the VPN server and VPN clients. Certificates are managed through {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

## Creating an IAM service-to-service authorization
{: #creating-iam-service-to-service}

To create an IAM service-to-service authorization for your VPN server and IBM Cloud Secrets Manager, follow these steps:

   You can also set up IAM service-to-service authorization on the VPN server provisioning page, or use the **Edit** authorization side panel.
   {: note}

1. From the IBM Cloud console, go to the [Manage authorizations](/iam/authorizations){: external} page and click **Create**.
1. Select **VPC Infrastructure Services** from the menu. Then, select **Resource based on selected attributes**.
1. Select **Resource type** > **Client VPN for VPC**.
1. For Target service, select **Secrets Manager**.
1. Keep **All resources** selected. Then, select the **SecretsReader** checkbox.
1. Click **Authorize**.

## Managing VPN server and client certificates
{: #creating-cert-manager-instance-import}

### Importing a certificate into Secrets Manager
{: #import-certificate}

The following procedure uses [OpenVPN easy-rsa](https://github.com/OpenVPN/easy-rsa){: external} to generate the VPN server and client certificates, and then imports these certificates to Secrets Manager. For more information, see the [Easy-RSA 3 Quickstart README](https://github.com/OpenVPN/easy-rsa/blob/master/README.quickstart.md){: external}.

1. Clone the Easy-RSA 3 repository into your local folder:

   ```sh
   git clone https://github.com/OpenVPN/easy-rsa.git
   cd easy-rsa/easyrsa3
   ```
   {: pre}

1. Create a Public Key Infrastructure (PKI) and Certificate Authority (CA):

   ```sh
   ./easyrsa init-pki
   ./easyrsa build-ca nopass
   ```
   {: pre}

   Check that the CA certificate is generated at path `./pki/ca.crt`. As described [in the next section](#import-vpn-server-certificates-sm), when importing the server or client certificates into Secrets Manager, the certificate used to sign other certificates will be required as the intermediate certificate.

1. Generate a VPN server certificate:

   ```sh
   ./easyrsa build-server-full vpn-server.vpn.ibm.com nopass
   ```
   {: pre}

   Check that the VPN server public key is generated at path `./pki/issued/vpn-server.vpn.ibm.com.crt`, and that the private key is at path `./pki/private/vpn-server.vpn.ibm.com.key`.

1. Generate a VPN client certificate:

   ```sh
   ./easyrsa build-client-full client1.vpn.ibm.com nopass
   ```
   {: pre}

   Check that the VPN client public key is generated at path `./pki/issued/client1.vpn.ibm.com.crt`, and that the private key is at path `./pki/private/client1.vpn.ibm.com.key`.

1. If you need to create more VPN client certificates, repeat step 4.

[Important considerations:]{: tag-purple}

* In the preceding example, the VPN server and client certificates are signed by the same CA. To use different CAs to sign the client certificate, copy the `easyrsa3` folder into a new path and follow steps 2 and 4.
* If you already have the VPN server certificate from other CAs, make sure that the certificate has the Extended key usage: `TLS Web Client Authentication`. You can use the following command to check the certificate information based on content of the encoded certificate file:  `cat YOUR-CERTIFICATE-FILE | openssl x509 -noout -text`.
* If the certificate is used as the VPN client certificate to authenticate the client, you must upload the **Certificate file** and the **Intermediate certificate file**. For example, if you use different client certificates to authenticate different users, make sure that these certificates are signed by the same CA and that you uploaded one of the client certificates (**Certificate file** and **Intermediate certificate file** only) to Secrets Manager.

   You can use a CA chain to bundle the certificates (Intermediate CA 1, Intermediate CA 2, and root CA) into a single file and upload to the intermediate certificate file. Also, keep in mind that you can create multiple client certificates offline using the same CA without having to upload the certificates to Secrets Manager.
   {: note}

### Importing VPN server certificates into Secrets Manager
{: #import-vpn-server-certificates-sm}

To import VPN server certificates into Secrets Manager, follow these steps:

1. If you do not have a Secrets Manager instance already, go to the [Secrets Manager](/catalog/services/secrets-manager){: external} page. Then, complete the information and click **Create** to create a Secrets Manager instance.

   For more information, see ordering certificates for [Secrets Manager](/docs/secrets-manager?topic=secrets-manager-certificates&interface=ui).
   {: note}

1. In the console, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) > **Resource List > Security**
1. From the list of services, select your instance of Secrets Manager.
1. In the **Secrets** table, click **Add**.
1. Click **Imported certificate**, then click **Next**.
1. Follow these steps to import the certificate:
   1. Add a name and description to easily identify your secret.
   1. Select the secret group that you want to assign to the secret.
   1. Optional: Add labels to help you to search for similar secrets in your instance.
   1. Optional: Add metadata to your secret or to a specific version of your secret.
   1. Click **Next** and select the **Import a certificate** tile.
   1. Click **Browse** and select `./pki/issued/vpn-server.vpn.ibm.com.crt` as the certificate file.
   1. Click **Browse** and select `./pki/private/vpn-server.vpn.ibm.com.key` as the certificate's private key.
   1. Click **Browse** and select `./pki/ca.crt` as the intermediate certificate.
   1. Click **Next**.
   1. Review the details of your certificate and click **Add**.

For more information, see [Importing your existing certificates](/docs/secrets-manager?topic=secrets-manager-certificates&interface=ui#import-certificates).
{: note}

[Important considerations:]{: tag-purple}

* In this example, the VPN server and client certificates are signed by the same CA, so you need to upload only the VPN server certificate. You must also use the certificate as a VPN server certificate and authenticate the VPN client. If your VPN server and client certificate are signed by different CAs, you must upload them separately.
* If you updated the certificate in Secrets Manager, the VPN server is not aware of the certificate update. You must reimport the certificate with a different CRN, then update the VPN server with the new certificate CRN.
* If the certificate is used as the VPN server certificate, you must upload the **Certificate file**, **Private key file**, and **Intermediate certificate file**. If the certificate is used as the VPN client certificate to authenticate the client, you must upload the **Certificate file** and **Intermediate certificate file**.

   You can use a CA chain to bundle the certificates (Intermediate CA 1, Intermediate CA 2, and root CA) into a single file and upload to the intermediate certificate file.
   {: note}

### Ordering a public certificate by using Secrets Manager
{: #order-certificate}

You can use Secrets Manager to order a public SSL/TLS certificate as the VPN server certificate. Because the public CA root certificate is not stored in Secrets Manager, Secrets Manager stores only the intermediate certificates. You need the root certificates from Let's Encrypt, saved as a `.pem` file. The two files that you require are located in [https://letsencrypt.org/certs/lets-encrypt-r3.pem](https://letsencrypt.org/certs/lets-encrypt-r3.pem){: external} and [https://letsencrypt.org/certs/isrgrootx1.pem](https://letsencrypt.org/certs/isrgrootx1.pem){: external}. These files were concatenated for your convenience; however, for security reasons, it is recommended that you download and concatenate your own root certificate into a file. Also, when you download and update the VPN client profile, use this root certificate to replace the `<ca>` section in the client profile.

For more information about the Let's Encrypt certificate chain, see [https://letsencrypt.org/certificates/](https://letsencrypt.org/certificates/){: external}.
{: note}

```text
-----BEGIN CERTIFICATE-----
MIIFFjCCAv6gAwIBAgIRAJErCErPDBinU/bWLiWnX1owDQYJKoZIhvcNAQELBQAw
TzELMAkGA1UEBhMCVVMxKTAnBgNVBAoTIEludGVybmV0IFNlY3VyaXR5IFJlc2Vh
cmNoIEdyb3VwMRUwEwYDVQQDEwxJU1JHIFJvb3QgWDEwHhcNMjAwOTA0MDAwMDAw
WhcNMjUwOTE1MTYwMDAwWjAyMQswCQYDVQQGEwJVUzEWMBQGA1UEChMNTGV0J3Mg
RW5jcnlwdDELMAkGA1UEAxMCUjMwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEK
AoIBAQC7AhUozPaglNMPEuyNVZLD+ILxmaZ6QoinXSaqtSu5xUyxr45r+XXIo9cP
R5QUVTVXjJ6oojkZ9YI8QqlObvU7wy7bjcCwXPNZOOftz2nwWgsbvsCUJCWH+jdx
sxPnHKzhm+/b5DtFUkWWqcFTzjTIUu61ru2P3mBw4qVUq7ZtDpelQDRrK9O8Zutm
NHz6a4uPVymZ+DAXXbpyb/uBxa3Shlg9F8fnCbvxK/eG3MHacV3URuPMrSXBiLxg
Z3Vms/EY96Jc5lP/Ooi2R6X/ExjqmAl3P51T+c8B5fWmcBcUr2Ok/5mzk53cU6cG
/kiFHaFpriV1uxPMUgP17VGhi9sVAgMBAAGjggEIMIIBBDAOBgNVHQ8BAf8EBAMC
AYYwHQYDVR0lBBYwFAYIKwYBBQUHAwIGCCsGAQUFBwMBMBIGA1UdEwEB/wQIMAYB
Af8CAQAwHQYDVR0OBBYEFBQusxe3WFbLrlAJQOYfr52LFMLGMB8GA1UdIwQYMBaA
FHm0WeZ7tuXkAXOACIjIGlj26ZtuMDIGCCsGAQUFBwEBBCYwJDAiBggrBgEFBQcw
AoYWaHR0cDovL3gxLmkubGVuY3Iub3JnLzAnBgNVHR8EIDAeMBygGqAYhhZodHRw
Oi8veDEuYy5sZW5jci5vcmcvMCIGA1UdIAQbMBkwCAYGZ4EMAQIBMA0GCysGAQQB
gt8TAQEBMA0GCSqGSIb3DQEBCwUAA4ICAQCFyk5HPqP3hUSFvNVneLKYY611TR6W
PTNlclQtgaDqw+34IL9fzLdwALduO/ZelN7kIJ+m74uyA+eitRY8kc607TkC53wl
ikfmZW4/RvTZ8M6UK+5UzhK8jCdLuMGYL6KvzXGRSgi3yLgjewQtCPkIVz6D2QQz
CkcheAmCJ8MqyJu5zlzyZMjAvnnAT45tRAxekrsu94sQ4egdRCnbWSDtY7kh+BIm
lJNXoB1lBMEKIq4QDUOXoRgffuDghje1WrG9ML+Hbisq/yFOGwXD9RiX8F6sw6W4
avAuvDszue5L3sz85K+EC4Y/wFVDNvZo4TYXao6Z0f+lQKc0t8DQYzk1OXVu8rp2
yJMC6alLbBfODALZvYH7n7do1AZls4I9d1P4jnkDrQoxB3UqQ9hVl3LEKQ73xF1O
yK5GhDDX8oVfGKF5u+decIsH4YaTw7mP3GFxJSqv3+0lUFJoi5Lc5da149p90Ids
hCExroL1+7mryIkXPeFM5TgO9r0rvZaBFOvV2z0gp35Z0+L4WPlbuEjN/lxPFin+
HlUjr8gRsI3qfJOQFy/9rKIJR0Y/8Omwt/8oTWgy1mdeHmmjk7j1nYsvC9JSQ6Zv
MldlTTKB3zhThV1+XWYp6rjd5JW1zbVWEkLNxE7GJThEUG3szgBVGP7pSWTUTsqX
nLRbwHOoq7hHwg==
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
MIIFazCCA1OgAwIBAgIRAIIQz7DSQONZRGPgu2OCiwAwDQYJKoZIhvcNAQELBQAw
TzELMAkGA1UEBhMCVVMxKTAnBgNVBAoTIEludGVybmV0IFNlY3VyaXR5IFJlc2Vh
cmNoIEdyb3VwMRUwEwYDVQQDEwxJU1JHIFJvb3QgWDEwHhcNMTUwNjA0MTEwNDM4
WhcNMzUwNjA0MTEwNDM4WjBPMQswCQYDVQQGEwJVUzEpMCcGA1UEChMgSW50ZXJu
ZXQgU2VjdXJpdHkgUmVzZWFyY2ggR3JvdXAxFTATBgNVBAMTDElTUkcgUm9vdCBY
MTCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCCAgoCggIBAK3oJHP0FDfzm54rVygc
h77ct984kIxuPOZXoHj3dcKi/vVqbvYATyjb3miGbESTtrFj/RQSa78f0uoxmyF+
0TM8ukj13Xnfs7j/EvEhmkvBioZxaUpmZmyPfjxwv60pIgbz5MDmgK7iS4+3mX6U
A5/TR5d8mUgjU+g4rk8Kb4Mu0UlXjIB0ttov0DiNewNwIRt18jA8+o+u3dpjq+sW
T8KOEUt+zwvo/7V3LvSye0rgTBIlDHCNAymg4VMk7BPZ7hm/ELNKjD+Jo2FR3qyH
B5T0Y3HsLuJvW5iB4YlcNHlsdu87kGJ55tukmi8mxdAQ4Q7e2RCOFvu396j3x+UC
B5iPNgiV5+I3lg02dZ77DnKxHZu8A/lJBdiB3QW0KtZB6awBdpUKD9jf1b0SHzUv
KBds0pjBqAlkd25HN7rOrFleaJ1/ctaJxQZBKT5ZPt0m9STJEadao0xAH0ahmbWn
OlFuhjuefXKnEgV4We0+UXgVCwOPjdAvBbI+e0ocS3MFEvzG6uBQE3xDk3SzynTn
jh8BCNAw1FtxNrQHusEwMFxIt4I7mKZ9YIqioymCzLq9gwQbooMDQaHWBfEbwrbw
qHyGO0aoSCqI3Haadr8faqU9GY/rOPNk3sgrDQoo//fb4hVC1CLQJ13hef4Y53CI
rU7m2Ys6xt0nUW7/vGT1M0NPAgMBAAGjQjBAMA4GA1UdDwEB/wQEAwIBBjAPBgNV
HRMBAf8EBTADAQH/MB0GA1UdDgQWBBR5tFnme7bl5AFzgAiIyBpY9umbbjANBgkq
hkiG9w0BAQsFAAOCAgEAVR9YqbyyqFDQDLHYGmkgJykIrGF1XIpu+ILlaS/V9lZL
ubhzEFnTIZd+50xx+7LSYK05qAvqFyFWhfFQDlnrzuBZ6brJFe+GnY+EgPbk6ZGQ
3BebYhtF8GaV0nxvwuo77x/Py9auJ/GpsMiu/X1+mvoiBOv/2X/qkSsisRcOj/KK
NFtY2PwByVS5uCbMiogziUwthDyC3+6WVwW6LLv3xLfHTjuCvjHIInNzktHCgKQ5
ORAzI4JMPJ+GslWYHb4phowim57iaztXOoJwTdwJx4nLCgdNbOhdjsnvzqvHu7Ur
TkXWStAmzOVyyghqpZXjFaH3pO3JLF+l+/+sKAIuvtd7u+Nxe5AW0wdeRlN8NwdC
jNPElpzVmbUq4JUagEiuTDkHzsxHpFKVK7q4+63SM1N95R1NbdWhscdCb+ZAJzVc
oyi3B43njTOQ5yOf+1CceWxG1bQVs5ZufpsMljq4Ui0/1lvh+wjChP4kqKOJ2qxq
4RgqsahDYVvTH9w7jXbyLeiNdd8XM2w9U/t7y0Ff/9yi0GE44Za4rF2LN9d11TPA
mRGunUHBcnWEvgJBQl9nJEiU0Zsnvgc/ubhPgXRR4Xq37Z0j4r7g1SgEEzwxA57d
emyPxgcYxn/eR44/KJ4EBs+lVDR3veyJm+kXQ99b21/+jh5Xos1AnX5iItreGCc=
-----END CERTIFICATE-----
```

The ordered certificates are public SSL/TLS certificates and must be used as a VPN server certificate only. You cannot use the ordered certificates to authenticate the VPN clients because of the following reasons:

* Anyone can order a new certificate from a public CA. Then, they can pass the authentication if you use public SSL/TLS certificates to authenticate your VPN client.
* You cannot create a certificate revocation list (CRL) to revoke a VPN client certificate with a public CA.

You must create your own CA and import the CA certificate into Secrets Manager to authenticate your VPN client.
{: important}

### Using a private certificate
{: #using-private-certificate}

You can use a Secrets Manager private certificate in your VPN server. Review the following important considerations when doing so:

* All CAs in a CA chain must be contained in a Secrets Manager instance. Also, you must create the root CA inside the Secrets Manager. When you select an intermediate CA, the following options are _required_ to make sure that every CA can be found while the CA chain of a private certificate in your VPN server is verified:

   * In the Type section, you must select **Internal signing**.
   * In the Type section, you must enable the **Encode URL** switch when you create a CA to encode the issuing certificate URL in end-entity certificates. It doesn't work if you enable the **Encode URL** switch after creating the CA.
* The maximum number of CAs in a private certificate CA chain is 11 (a root CA and 10 intermediate CAs at most).
* In the Certificate revocation list section, enable both **CRL building** and **CRL distribution points** switches if you want to use the CA CRL of a private certificate. A revoked certificate is no longer trusted by the applications after one hour.
* The CA CRL works only if you don't upload a certificate revocation list when you create a VPN server. You don't need to enable a CA CRL if the VPN server's CRL is already uploaded.
* While creating a template for an intermediate CA, in the Certificate roles section:
   * Select the **Use certificates for server** checkbox if that created CA will sign the server certificate for the VPN server.
   * Select the **Use certificates for client** checkbox if that created CA will sign the client certificate for the VPN server.
   * Select both **Use certificates for server** and **Use certificates for client** checkboxes if that created CA will sign both server and client certificates for the VPN server.
* Make sure that every CA has a unique **Common name** in the CA chain while creating it in the Secrets Manager instance.

For more information, see [Creating private certificates](/docs/secrets-manager?topic=secrets-manager-certificates&interface=ui) in the Secrets Manager documentation.
{: note}

### Locating the certificate CRN
{: #locating-cert-crn}

When you configure authentication for a client-to-site VPN server during provisioning using the UI, you can choose to specify the Secrets Manager and SSL certificate, or the certificate's CRN. You might want to do this if you cannot view the Secrets Manager in the menu, which means you don't have access to the Secrets Manager instance. Keep in mind that you must enter the CRN if using the API to create a client-to-site VPN server.

To obtain the CRN, you must have permission to access the Secrets Manager instance.
{: note}

To find a certificate's CRN, follow these steps:

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) > Resource list**.
1. Click to expand **Security**, then select the Secrets Manager that you want to find the CRN for.
1. Select anywhere in the table row of the certificate to open the Certificate details side panel. The certificate CRN is listed in the side panel.

## Configuring user IDs and passcodes
{: #client-to-site-configuration-passcode}

To configure authentication for VPN client users, follow these steps:

1. The VPN administrator invites the user of the VPN client to the account that the VPN server resides in.
1. The VPN administrator assigns the VPN client user an IAM permission. This permission allows this user to connect with the VPN server. For more information, see [Creating an IAM access group and granting the role to connect to the VPN server](/docs/vpc?topic=vpc-create-iam-access-group).
1. The VPN client user opens the following website to generate a passcode for their user ID:

   ```sh
   https://iam.cloud.ibm.com/identity/passcode
   ```
   {: pre}

1. The VPN client user inputs the passcode on their openVPN client and starts the connection to the VPN server. For more information, see [Setting up a client VPN environment and connecting to a VPN server](/docs/vpc?topic=vpc-vpn-client-environment-setup).
