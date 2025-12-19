---

copyright:
  years: 2025, 2025
lastupdated: "2025-12-19"

keywords: load balancer, network, faqs

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I create an HTTPS listener on a load balancer with a public certificate that is stored in IBM Cloud Secrets Manager?
{: #troubleshoot-lb-https-listener}
{: troubleshoot}
{: support}

Use the following information to troubleshoot issues with setting up an HTTPS listener on an IBM Cloud load balancer by using a public SSL certificate that is stored in Secrets Manager.
{: shortdesc}

Error encountered in the console:
{: tsSymptoms}

       ```sh
       Error object: {
         "name": "AxiosError",
         "code": "ERR_BAD_RESPONSE",
         "message": "Request failed with status code 500",
         "statusCode": 500,
         "error": {
           "errors": [
             {
               "code": "internal_error",
               "message": "Operation Failed. An internal error has occurred.",
               "more_info": "https://cloud.ibm.com/docs/vpc?topic=vpc-rias-error-messages#internal_error"
             }
           ]
       ```
       {: codeblock}

Error encountered from the CLI:

       ```sh
       FAILED
       Failed to create Listener listener.
 
       FAILED
       Error code: internal_error
       Error message: Operation Failed. An internal error has occurred.
       ```
       {: codeblock}

The Secrets Manager instance that is used by the customer is configured with **Private only** endpoint access. Hence, the load balancer (which requires access to Secrets Manager to retrieve the certificate) can't validate or retrieve the public certificate because it operates outside the private VPC. The result is a 500 internal error when you try to add the HTTPS listener.
{: tsCauses}

To resolve this issue, use a Secrets Manager instance that supports both public and private endpoints.
{: tsResolve}

Follow these steps to resolve this issue:

1. Provision a new Secrets Manager instance. During provisioning, select **Public and private** for the Endpoints option.
1. Migrate the certificate to the new instance if needed.
1. Retry the HTTPS listener creation with the new Secrets Manager CRN.
