---

copyright:
  years: 2022, 2026
lastupdated: "2026-02-20"

keywords: dynamic registry reference for hyper protect virtual server for vpc

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Using a dynamic registry reference
{: #hyper-protect-virtual-server-use-dynamic-registry-reference}

The {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for VPC is deprecated. As of 28 February 2026, you can't create new instances. Existing instances are supported until 20 February 2027. Any instances that still exist on that date will be deleted. You can redeploy your workloads by using [IBM Confidential Computing Container Runtime (formerly known as Hyper Protect Virtual Servers)](https://www.ibm.com/docs/en/hpvs) or [IBM Confidential Computing Container Runtime for Red Hat Virtualization Solutions (formerly known as Hyper Protect Container Runtime for Red Hat Virtualization Solutions)](https://www.ibm.com/docs/en/hpcr/1.1.x). For information about data migration, see the [Migration guide](/docs/vpc?topic=vpc-migration_guide). For more information, see the [Service deprecation announcement](/docs/vpc?topic=vpc-ichpcs_deprecated_anmt).
{: deprecated}

You can follow this tutorial to learn how to use a dynamic registry reference in the [contract](/docs/vpc?topic=vpc-about-contract_se).
{: shortdesc}

## Explicit registry reference
{: #explicit-registry-reference}

Typically, the docker registry is referenced through the full docker URL in the compose file. See the following example:

```yaml
services:
  helloworld:
    image: docker.io/library/hello-world@sha256:53f1bbee2f52c39e41682ee1d388285290c5c8a76cc92b42687eecf38e0af3f0
```
{: codeblock}

In the example, `docker.io/library/` is the registry prefix, `hello-world` is the identifier of the OCI image in that registry, and `sha256:53f1bbee2f52c39e41682ee1d388285290c5c8a76cc92b42687eecf38e0af3f0` is the unique identifier of the version of the image.

In such a case, the workload provider role decides about the registry (and the associated pull credentials), since both the registry reference and the pull credentials are part of the workload section of the contract.

## Dynamic registry reference
{: #dynamic-registry-reference}

In certain use cases, the registry is **not known** when the workload section is pre-encrypted. For example, when the workload provider wants to allow the deployer to use a registry mirror or a private container registry.

In that case, it's possible to dynamically override the registry and the pull credentials. This effort has to be coordinated between the workload provider and the deployer.

The templating approach works only for a [compose](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_compose)-based workload, and only for images that are referenced through a digest. DCT-based workloads are not supported.
{: note}

### Workload provider
{: #workload-provider}

The workload provider marks the registry as dynamic by using a replacement variable in the docker compose file:

```yaml
services:
  helloworld:
    image: ${REGISTRY}/hpse-docker-hello-world-s390x@sha256:43c500c5f85fc450060b804851992314778e35cadff03cb63042f593687b7347

```
{: codeblock}

The digest of the image is identical across registries, so the workload provider can lock down a specific version of the image by setting the key, independent of the registry being used. Using tokens in the compose file is a native feature of the [compose specification](https://docs.docker.com/reference/compose-file/#interpolation){: external}.

Now the workload provider can prepare (encrypt) the workload section **without** specifying the pull secrets for that registry.

### Deployer
{: #deployer}

The deployer enters the missing information about the registry and the associated pull secrets.

The registry is set as an environment variable. Both the deployer and the workload provider can provide pieces to the overall environment and these pieces are overlaid with workload taking precedence.

The pull credentials are passed in through an `auth` section in the environment part of the contract. Just as environment variables, these `auth` sections are overlaid with the workload section taking precedence.

```yaml
---
  ---
  env:
    type: env
    auths:
      de.icr.io:
        username: xxx
        password: yyy
    env:
      REGISTRY: de.icr.io
```
{: codeblock}
