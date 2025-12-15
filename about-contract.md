---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-15"

keywords: confidential computing, enclave, secure execution, hpcr, contract, customization, schema, contract schema, env, workload, encryption

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About the contract
{: #about-contract_se}

When you create an {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for VPC instance with the IBM Hyper Protect Container Runtime (HPCR) image, you must specify a contract as part of the **User Data** field.
{: shortdesc}

## What is a contract?
{: #hpcr_contract}

The contract is a definition file in the YAML format that is specific to the {{site.data.keyword.hpvs}} for VPC instance. This file must be created by the cloud user as a prerequisite for creating an instance. After this file is created, it must be passed as an input as part of the **User Data** field when an instance is created. You cannot create an instance without a valid contract. If you create an instance without a contract, the deployment starts and then fails, and the instance goes into a shutdown state. The contract is specific to creating a {{site.data.keyword.hpvs}} for VPC instance and is an extension of the IBM Secure Execution technology by Hyper Protect.

If the workload discloses the decrypted tokens either through SSH or REST APIs, then the decrypted data contains both the workload and the environment secrets. However, it does not contain the seeds that were used for volume encryption.
{: note}

## Contract sections
{: #hpcr_contract_sections}

A contract file can have the following four valid high-level sections, of which the `workload` and `env` sections are mandatory.
* [`workload`](#hpcr_contract_workload) is a mandatory section.
* [`env`](#hpcr_contract_env) is a mandatory section.
* [`attestationPublicKey`](/docs/vpc?topic=vpc-about-attestation#attest_pubkey) is an optional section. You can provide a public RSA key as part of the contract, which is used to encrypt the attestation document and the attribute must be named as `attestationPublicKey`.
* `envWorkloadSignature` is an optional section and contains the signature of the other sections of the contract.

The two primary sections in a contract are the `workload` and `env` sections. These two sections are needed because the information that is added into the contract comes from two different personas, namely the "workload" and the "deployer" persona.

The workload persona provides information about the container (or workload) that needs to be brought up on the {{site.data.keyword.hpvs}} for VPC instance. It includes information about the name of the container, the Container Registry where it resides, credentials of the Container Registry, the image digest, the notary server information (required for image validation), the environment variables that need to be passed to the container, and the docker compose file or Pod descriptors with the container information.

If you use a docker compose file, only one container is supported. Pod descriptors support one or multiple containers.
{: note}

The deployer persona works closely with IBM Cloud. This persona receives the workload information (preferably an encrypted workload section) from the workload persona. The deployer then creates the `env` section of the contract. The `env` section has information that is specific to the IBM Cloud environment. Usually, it is information that the workload persona does not have and does not need to know. An example is information about the IBM Cloud Logging instance, which the deployer persona creates, before adding information to the `env` section of the contract.

## The workload section
{: #hpcr_contract_workload}

This section is one of the most important sections of the contract. The `workload` section can have multiple subsections and the purpose of the subsections is to provide information that is required for bringing up the workload. The `workload` section is the parent section that can have the following subsections:
* `type`: workload. This subsection is mandatory.
* [`auths`](#hpcr_contract_auths). This subsection is optional.
* [`compose`](#hpcr_contract_compose) (for single container) or [`play`](#hpcr_contract_play) (for single container or multiple containers). They are mutually exclusive; one of the sections must exist.
* [`images`](#hpcr_contract_images). This subsection is optional.
* [`volumes`](#hpcr_contract_volumes). This subsection is optional.

The following snippet shows a high-level sample of the workload section of the contract. The minimum that a workload section needs is the compose section. The other sections can be added based on the requirement.

```yaml
workload: |
  type: workload
  auths:
    <registry url>:
      password: <password>
      username: <user name>
    <registry url>:
      password: <password>
      username: <user name>

  compose:
    archive: <base64 encoded of tgz of docker-compose.yaml>
  images:
    dct:
      <docker image name (without the tag, an example is docker.io/redbookuser/s390x:)>:
        notary: "<notary URL>"
        publicKey: <docker content trust signed public key>
      <docker image name>:
        notary: "<notary URL>"
        publicKey: <docker content trust signed public key>

  volumes:
    <volume key>:
      mount: "<data volume mount path>"
      seed: "<Passphrase of the LUKS encryption>"
      filesystem: "ext4"
```
{: codeblock}

### The `auths` subsection
{: #hpcr_contract_auths}

The `auths` section consists of information about the container's registry. If a public image is used in the contract, you do not need the `auths` section because no credentials are required. The `auths` subsection is required only if the container images are private. This subsection does not have any image information, as shown in the following sample. This subsection needs to contain the name of the image registry and the credentials such as username-password for the same. The key must be the hostname of the Container Registry or the following string for the default docker registry:

```text
https://index.docker.io/v1/
```
{: screen}

The following snippet shows an example for IBM Cloud Registry. For more information about using the API key, see [Using client software to authenticate in automation](/docs/Registry?topic=Registry-registry_access#registry_access_apikey_auth).

```yaml
auths:
  us.icr.io:
    password: <apikey>
    username: iamapikey
```
{: codeblock}

### The `compose` subsection
{: #hpcr_contract_compose}

It consists of an archive subsection. The archive subsection contains the Base64 encoded TGZ file archive of the `docker-compose.yaml` file. As the Hyper Protect Container Runtime image uses Docker Engine and Docker Compose to start the container, the information about the container must first be created by using a standard docker-compose file. This file is then archived and Base64 encoded, and the output of this process is provided as the value to the archive subsection, within the compose section. For more information, see [Overview of Docker Compose](https://docs.docker.com/compose/).

The mount points specified under the volumes information of the docker-compose file might be aligned with the volume mount point that is specified in the workload section of the contract.

Executing a build as part of a docker compose file is not supported. Make sure your docker compose file doesn't have a `build` section.
{: note}

Both "yaml" and "yml" formats are supported for docker-compose file. See the following example of a docker-compose file.

```yaml
version: '3'
services:
  nginx:
    image: nginx@sha256:e73ba8654ba7fd1834e78a3d4e9d72ffaaa3372d42996af5c34ba3e1abc293e8
    privileged: true
    user: 0:0
    restart: always
    ports:
    - 80:80
```
{: codeblock}

Use cases exist where the registry is **not known** when the workload section is pre-encrypted. For example, when the workload provider wants to allow the deployer to use a registry mirror or a private Container Registry, it's possible to dynamically override the registry and the pull credentials. This effort has to be coordinated between the workload provider and the deployer. For more information, see [Using a dynamic registry reference](/docs/vpc?topic=vpc-hyper-protect-virtual-server-use-dynamic-registry-reference).
{: tip}

Complete the following steps to get the Base64 encoded archive file. The Base64 output is available in the compose.b64 file.
Go to the `<COMPOSE_Folder>` and run the following commands:
```sh
tar czvf compose.tgz docker-compose.yml
```
{: pre}

```sh
base64 -w0 compose.tgz > compose.b64
```
{: pre}

Make sure that the compose tgz file contains only directories and regular files. Links or pipes are not supported.
{: note}

Copy the content of compose.b64 as a value of compose -> archive.
```buildoutcfg
compose:
  archive: <paste the content of compose.b64 >
```

For this example, you'd see a response similar to the following output:
```yaml
compose:
  archive: H4sIAKOFmGIAA+2RTW6DMBBGs84pRuyB8Q8k+DIRwZOGtmBkkyrcvhgnLVVV1EWkqhJv4ZHt8ednWZvqhWxcmaYzjpKhed08HETMpQRfd3k2VeRhPpEJCUxymTPkIuOALBOIG8DHq3zn4vrSjiqdLY/nsv+xb2w7nRZywlPgo/4THNm3uiKntgCWdO1aowmZnwLUTflECpwo8Jpu9NyZ2zvQgdADFEudoXyQzSu+fPPzseSvedo6qjV7mDa2anZbdH8totL6somtUlvX8K4SJshDsFKU2NmFvAZuMc9U37wceeys+Y6BI8Fi6+6vxK5RS+YFDh6RNu//tuVlZWVJd4BcjKckQAIAAA=
```
{: screen}

### The `play` subsection
{: #hpcr_contract_play}

In the `play` subsection, you can define the workload through [Pod descriptors](https://kubernetes.io/docs/concepts/workloads/pods/){: external}. Each pod can contain one or more container definitions. Descriptors can be provided in one of the following ways:

- In plain YAML format in the `resources` subsection of `play`. This section is an array of descriptors and supports two types of descriptors: [Pods](https://kubernetes.io/docs/concepts/workloads/pods/){: external} and [ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/){: external}.

   The following example illustrates how to use the `resources` section:

   ```yaml
   workload: |
     type: workload
     play:
       resources:
         - apiVersion: v1
           kind: Pod
           metadata:
             name: busybox
           spec:
             containers:
             - name: main
               image: ...
               command:
               - printenv
               envFrom:
               - configMapRef:
                   name: contract.config.map
                   optional: false
             restartPolicy: Never
   ```
   {: codeblock}

   The following example illustrates how to use the `initcontainers` with the `resources` section:

   ```yaml
   workload: |
     type: workload
     play:
       resources:
         - apiVersion: v1
           kind: Pod
           metadata:
             name: busybox
           spec:
             initContainers:
             - name: attest
               image: ...
               volumeMounts:
               - mountPath: /var/hyperprotect
                 name: datavolume
             containers:
             - name: main
               image: ...
               command:
               - printenv
               envFrom:
               - configMapRef:
                   name: contract.config.map
                   optional: false
             restartPolicy: Never
    ```
    {: codeblock}
    
- In the `archive` subsection of `play`, the archive is a Base64 encoded, gzipped tar file. The Pods or ConfigMaps are represented as YAML files, at the top level in this tar file. The file might also contain extra files and all the files are extracted to the host file system before the Pods are started. The *current working directory* is the directory in which the files were extracted, so it's possible to use a volume mount with a relative path to mount the files or directories from the YAML file.

   Example:

   ```yaml
   workload: |
     type: workload
     play:
       archive: ${COMPOSE_VALUE}
     auths:
       us.icr.io:
         username: iamapikey
         password: Eqx0TS....
     volumes:
       test-volume:
         mount: /var/hyperprotect
         seed: "workload_phrase"
         filesystem: ext4
   ```
   {: codeblock}

- In a template format in the `templates` subsection of `play`. This section is an array of descriptors in the YAML format. [Pods](https://kubernetes.io/docs/concepts/workloads/pods/){: external} or [ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/){: external} can have points of variability (POV) that are not known at the time of writing the descriptors. These POVs can be represented as [templates](https://pkg.go.dev/text/template){: external} and the values are completed at deployment time from information in the contract. We use [go templates](https://pkg.go.dev/text/template){: external} as the templating syntax, which is the same as used for [helm charts](https://helm.sh/docs/chart_template_guide/getting_started/){: external}, so templates can easily be exchanged with k8s. We support the following `Built-In` objects:
   - *Environment:* this object contains the environment variables as merged between the workload and the environment section. The object is available as `{{ .Env }}`.

   Example:

   ```yaml
   workload: |
     type: workload
     auths:
      docker.io:
       password: <password>
       username: test
     play:
       templates:
         - apiVersion: v1
           kind: Pod
           metadata:
             name: busybox
           spec:
             initContainers:
             - name: initcontainer
               image: docker.io/library/init-config@sha256:b1306efee704017b0e02efadc011d374063a4e9c47b86bdc57744fc3f0666383
             containers:
             - name: main
               image: "{{ .Env.REGISTRY }}/hpse-docker-busybox-s390x@sha256:732efa374f1e6c964caeacab0bcb370385ee386041a14d4a32176462e3f75c7b"
               command:
               - printenv
               envFrom:
               - configMapRef:
                   name: contract.config.map
                   optional: false
             restartPolicy: Never
   env: |
     type: env
     logging:
       logRouter:
         hostname: 34be57c7-6ff2-4685-8839-903921e90ab9.ingress.jp-tok.logs.cloud.ibm.com
         iamApiKey: <iamApiKey of the service instance> / xxxx
         port: 443
     env:
       REGISTRY: docker-io/test
   ```
   {: codeblock}

   The `{{ .Env REGISTRY }}` expression references the `REGISTRY` environment variable that in this example is defined in the `env` section of the contract.

   The templates need to be valid [YAML](https://yaml.org/spec/1.2.2/){: external}, so a replacement expression must be escaped if it appears as the first part of a string. Otherwise, it collides with the [block mapping](https://yaml.org/spec/1.2.2/#822-block-mappings){: external} syntax. This is different from [helm templates](https://helm.sh/docs/helm/helm_template/){: external} where expressions are applied to the textual representation of the document instead of the model representation.
   {: note}

#### Environment Variables
{: #environment_variables}

In the contract, you can define environment variables in the `workload` and `env` sections. Both sets of variables are merged together with `workload` taking precedence. Pods use the concept of a [ConfigMap](https://kubernetes.io/docs/concepts/configuration/configmap/){: external} to define configuration, so HPCR represents the merged environment sections as a special [ConfigMap](https://kubernetes.io/docs/concepts/configuration/configmap/){: external} named `contract.config.map`. The following example mounts all environment variables from the contract into the container:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  containers:
  - name: main
    image: ...
    command:
    - printenv
    envFrom:
    - configMapRef:
        name: contract.config.map
        optional: false
  restartPolicy: Never
```
{: codeblock}

#### Pod communication
{: #pod-communication}

- Container to container

   Containers inside one Pod communicate to each other through `localhost`. Each container needs to listen on a different port because they share the IP address by design.

- Pod to host

   Usually a Pod needs to expose at least one of its containers to the host so that the container is accessible through the IP address on the host through a mapped port. For this use case, use the `hostPort` feature on a container. Note that this is **not** [best practice](https://kubernetes.io/docs/concepts/configuration/overview/#services){: external} in the Kubernetes world, in which a service would be used instead.

   Specify both `hostPort` and `containerPort` explicitly. If you specify only `containerPort`, ports are not bound.
   {: important}

   Example:

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
       name: nginx-with-busybox
   spec:
       containers:
           - image: ...
             name: frontend
             ports:
               - containerPort: 80
                 hostPort: 80
             volumeMounts:
               - mountPath: /etc/nginx
                 name: local-frontend
                 readOnly: true
           - command:
               - httpd
               - -vv
               - -f
               - -p
               - "8080"
               - -h
               - /www
             image: ...
             name: backend
             volumeMounts:
               - mountPath: /www
                 name: local-backend
                 readOnly: true
       volumes:
           - hostPath:
               path: ./www
               type: Directory
             name: local-backend
           - hostPath:
               path: ./nginx
               type: Directory
             name: local-frontend
   ```
   {: codeblock}

- Pod to Pod

   To reach from one Pod to another, expose a `hostPort` on the target Pod. The source Pod can then make a request to the host on the exposed port to get to the target Pod.

   The source Pod can find the IP address of the host through the following command:

   ```bash
   ip route | awk '/default/ { print $3 }'
   ```
   {: codeblock}

#### Volumes
{: #volumes}

For Hyper Protect Container Runtime, volumes are managed by the [volumes](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_volumes) section in the contract. Based on this information, HPCR encrypts and mounts external block devices on the host. To mount these volumes into the pod, use the [hostPath](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath){: external} mount option on the volume.

Example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  containers:
  - name: main
    image: ...
    volumeMounts:
    - name: test-volume
      readOnly: true
      mountPath: /fromHost
  volumes:
  - name: test-volume
    hostPath:
      path: /var/hyperprotect
      type: Directory
  restartPolicy: Never
```
{: codeblock}

The `volumes` field here defines the data on the host to be mounted into the pod. It's different from `volumes` in the HPCR contract.
{: note}

### The `images` subsection
{: #hpcr_contract_images}

The `images` subsection is meant only for an image that is signed.

#### Images described by docker compose
{: #container-image-by-docker-compose}

The container image that is listed in the docker-compose file can be signed or not signed by using Docker Content Trust (DCT).

The following example shows an image URL:
```buildoutcfg
<container registry>/<username or namespace>/<image name>
eg- us.icr.io/mynamespace/my-haproxy:
```

The following shows an example of a notary URL:
```buildoutcfg
notary: "https://notary.us.icr.io"
```

The `publicKey` is the corresponding public key by which the image is signed by using DCT. Use the following command to get the public key:
```sh
cat ~/.docker/trust/tuf/us.icr.io/<username>/<imagename>/metadata/root.json
```
{: pre}

The following snippet is an example:
```yaml
images:
  dct:
    us.icr.io/mynamespace/my-haproxy:
      notary: "https://notary.us.icr.io"
      publicKey: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJpRENDQVM2Z0F3SUJBZ0lSQUxCMXBPYlpEQlRRc09GSFlxazMzaWd3Q2dZSUtvWkl6ajBFQXdJd0tqRW8KTUNZR0ExVUVBeE1mZFhNdWFXTnlMbWx2TDNCeVlXSm9ZWFF4TWpNdmJYa3RhR0Z3Y205NGVUQWVGdzB5TWpBMApNVE14TURFd01ETmFGdzB6TWpBME1UQXhNREV3TUROYU1Db3hLREFtQmdOVkJBTVRIM1Z6TG1samNpNXBieTl3CmNtRmlhR0YwTVRJekwyMTVMV2hoY0hKdmVIa3dXVEFUQmdjcWhrak9QUUlCQmdncWhrak9QUU1CQndOQ0FBU1AKWGsrelE2MlFZNjI3MWQ1cTBMZHY3SGc3QzZkMGZOUlRsQmJXekhOWWFDZzlpU0piYnVNdjVBY0JmMjlqQi83eApqYzhzVitxMksyemtkTHV4QWxGWm96VXdNekFPQmdOVkhROEJBZjhFQkFNQ0JhQXdFd1lEVlIwbEJBd3dDZ1lJCkt3WUJCUVVIQXdNd0RBWURWUjBUQVFIL0JBSXdBREFLQmdncWhrak9QUVFEQWdOSUFEQkZBaUIzd0JTa0IxaXAKZHZZYlBMbFBmS3RZT0hsYnZzUllKa0FZM2hnY0xuNWhwQUloQUt6cmhsU3p4K1I5bmdtMTBlZVkyaFNCRmgrawpMWHp6SFkwaktTVzhyM1FhCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
```
{: codeblock}

For an image that is not signed, no entry is required in the images subsection. However, for unsigned images, a digest is required. Complete the following steps to get the digest:
1. Log in to the Container Registry dashboard.
2. Open the image.
3. Click **Tag**, and then click **Digest**.

After you get the digest, add this digest in the `docker-compose.yaml` file. See the following example:
```yaml
services:
  <imagename>:
    image: s390x/redis@sha256:db467ab5c53bdeef65762a7534e26fecb94a0f218bd38afd2eaba1a670c472b1
```
{: codeblock}

#### Images described by Pod descriptors
{: #container-image-by-pod-descriptors}

Container images that are described by Pod descriptors can be validated by Red Hat Simple Signing.

If the image is referenced by a digest, the service allows its usage without additional checks.

Images without a digest need a GPG key to be validated. The key is transferred in a Base64 encoded binary format that can be created. See the following example:

```bash
gpg -a --export ${KEY_ID}|base64 -w0
```
{: codeblock}

This key is conveyed through the `rhs` subsection of the `images` section. This section is a map with the image identifier as the key and the GPG key in the `publicKey` field:

Example:

```yaml
images:
  rhs:
      OCI-image-identifier:
        publicKey: abcdef
```
{: codeblock}

### The `workload` - `volumes` subsection
{: #hpcr_contract_volumes}

The `volumes` section needs to be provided in the contract only if a data volume is attached to the instance at the time of creation. The information that is provided in this section is used to mount the attached data volume (provided by the user) and is later encrypted using the "seeds" provided in the `workload` and `env` sections. You can provide any path of your choice for the "mount" field. The path that is provided by the user is used internally to mount the data volume. The mount path that is provided in the contract must match the path that is provided under the volumes section of the `docker-compose.yaml` file so that all the data that is associated with the container workload is stored in this data volume.

The `volumes` subsection has support for auto encryption of the data volume with user-provided seeds. If a data volume is attached to the {{site.data.keyword.hpvs}} instance, it is encrypted automatically with the seeds that are provided through the "seed" field in the `volumes` subsections of the contract. Thus two seeds must be provided, one through the `workload` section (by the workload persona) and the other through the `env` section (by the deployer persona). These two seeds are internally converted to UTF8 sequences and then concatenated. Later, the hash (SHA256) of the concatenated sequence is computed as a hexdigest, which is used as the LUKS passphrase to encrypt the data volume.

Currently, the `env` and `workload` seeds require a **minimum length of 3 characters**. Starting from **March 2026**, this minimum will be **increased to 15 characters**. Any seed values shorter than 15 characters **after March 2026** may result in errors or failed deployments. To avoid disruptions, **update all seed values** to a length of at least **15 characters** as soon as possible. The rules are defined in the following section.
{: important}

Following are the rules for creating seeds. Starting from **March 2026**, you must follow these rules to create seeds:

- No spaces allowed.
- Must have at least `15` characters. Following are the allowed characters:
   - Lowercase letters `(a-z)`
   - Uppercase letters `(A-Z)`
   - Numbers `(0-9)`
   - Special characters `!@#$%^&*(),.?":{}|<>_-`

You can use the following command to validate the hexdigest:

```buildoutcfg
echo -n "seed1seed2" | sha256sum
```
{: pre}

Here you can learn how the "seed" can be provided in the workload section of the contract. For more information about how the "seed" input can be provided through the `env` section, see [The `env` section](#hpcr_contract_env). It is mandatory to provide both the seeds for encryption. Encryption fails if only one of the seeds is provided instance shuts down.

You can add a higher level of encryption protection and control to your data at rest by integrating with Hyper Protect Crypto Services. Starting from `ibm-hyper-protect-container-runtime-1-0-s390x-11`, you can use Hyper Protect Crypto Services to generate a random value as the third seed and wrap it with your root key. The LUKS passphrase is generated by using **three** seeds - the seed in the metadata partition and the two seeds from the contract. For more information, See [Securing your data](/docs/vpc?topic=vpc-hyper-protect-virtual-server-mng-data).
{: note}

The following snippet is an example for the volumes section:
```yaml
volumes:
  test:
    filesystem: ext4
    mount: /mnt/data
    seed: "workload_phrase"
```
{: codeblock}

Starting with the HPCR image version `ibm-hyper-protect-container-runtime-1-0-s390x-9`, for new {{site.data.keyword.hpvs}} for VPC instances, the data volume is partitioned into two parts. The first partition (100Mib) is reserved for internal metadata; the second partition remains as the data volume for workload. Only new volumes are partitioned, and you can't use the partitioned volume with an older version of the HPCR image. Provisioning with an existing encrypted volume also works. The difference is that the existing volume does not get partitioned, and you can also go back to an older image with this volume.
{: note}

Starting from `ibm-hyper-protect-container-runtime-1-0-s390x-12`, the deployer and provider can use the "rolling of seeds" feature. An option is provided to roll or rotate the seeds to increase the security posture, or if the seed is compromised. When the deployer or the provider or both of them want to roll the seeds, the current seed information must be specified in the `previousSeed` parameter, and the new seed information must be specified in the `seed` parameter.

The following snippet is an example for the volumes section:
```yaml
volumes:
  test:
    filesystem: ext4
    mount: /mnt/data
    seed: "workload_phrase1"
    previousSeed: "workload phrase"
```
{: codeblock}

Starting from `ibm-hyper-protect-container-runtime-1-0-s390x-13`, you can attach multiple volumes when you bring up the virtual server instance. Volumes that are attached when the instance is running are ignored.

The following snippet is an example for the volumes section:
```yaml
volumes:
  test1:
    filesystem: "ext4"
    mount: "/mnt/data"
    seed: "seed_value_with_minimum_15_characters"
  test2:
    filesystem: "ext4"
    mount: "/mnt/test2"
    seed: "seed_value_with_minimum_15_characters"
  test3:
    filesystem: "ext4"
    mount: "/mnt/test3"
    seed: "seed_value_with_minimum_15_characters"
```
{: codeblock}

## The `env` section
{: #hpcr_contract_env}

The `env` section is also one of the most important sections in a contract. The `env` section of a contract deals with information that is specific to the cloud environment and is not known to the workload persona. This section is created by the deployer persona.

The subsections for the `env` section are:
* `type`: env. This subsection is mandatory.
* [`logging`](#hpcr_contract_env_log). This subsection is mandatory.
* [`volumes`](#hpcr_contract_env_vol). This subsection must be used only when a data volume is attached.
* [`signingKey`](#hpcr_contract_env_signkey). This subsection must be used only when you want to use a contract signature.
* [`env`](#hpcr_contract_env_env). This subsection is used to specify values for `env` variables if they are defined by the workload provider.


### The `logging` subsection
{: #hpcr_contract_env_log}

#### ICL
{: #hpcr_contract_icl}

The minimum subsection that is required for this section is logRouter. For more information, see [Logging for {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-logging-for-hyper-protect-virtual-servers-for-vpc#cloud-logs).

The following snippet is an example of the `ICL` subsection:

```yaml
 env:
   logging:
     logRouter:
       hostname: <host name of the service instance> /
       iamApiKey: <iamApiKey of the service instance> / xxxx
       port: <port of the service instance(443)
```
{: codeblock}

### The `env` - `volumes` subsection
{: #hpcr_contract_env_vol}

Read the [workload - volumes](#hpcr_contract_volumes) subsection of the workload section before you continue with this section. As already mentioned, for auto disk encryption of the attached data volume, you must provide two customer seeds, one in the `workload` - `volumes` subsection, and the other in the `env`- `volumes` subsection. The seeds can be any random text of your choice.

See the following example of the `env` - `volumes` subsection:
```yaml
volumes:
  test:
    seed: "seed_value_with_minimum_15_characters"
```
{: codeblock}

When you are using the "rolling of seeds" feature, you can use the following snippet as an example:

```yaml
volumes:
  test:
    seed: "seed_value_with_minimum_15_characters"
    previousSeed: "env phrase"
```
{: codeblock}

When you use multiple volumes, the deployer must ensure that the volumes are created in advance, and also specify the volume ID in the contract file. Otherwise, the volume name must be specified and the volume must be created with the same name. If both are not specified, then the volume key is considered as the volume name and the volume must be created with the same name while or before the virtual server instance is created. The following snippet is an example:

```yaml
 env: |
   logging:
     logRouter:
       hostname: 34be57c7-6ff2-4685-8839-903921e90ab9.ingress.jp-tok.logs.cloud.ibm.com
       iamApiKey: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       port: 443
   volumes:
     test1:
       apiKey: "L4SsSE32xxxxxjAgfHCVkdW8xl_CiqMn4Lpc1dzTD"
       volumeID: "r006-f7b44467-01af-xxx-xxxx-xxxxxxx"
       seed: "seed_value_with_minimum_15_characters"
     test2:
       apiKey: "L4SsSE32xxxxxjAgfHCVkdW8xl_CiqMn4Lpc1dzTD"
       volumeName: "volume2"
       seed: "seed_value_with_minimum_15_characters"
     test3:
       apiKey: "L4SsSE32xxxxxjAgfHCVkdW8xl_CiqMn4Lpc1dzTD"
       seed: "seed_value_with_minimum_15_characters"
```
{: codeblock}

Where:
- Volume Name: is the name that you specified when you created the volume on the VPC.
- Volume ID: is the system-generated volume ID of the created volume.
- Volume Key: is the unique volume name for each volume.


The volume key must be the same as the key in the [workload - volumes](#hpcr_contract_volumes) subsection.
{: note}

As mentioned, you can integrate with Hyper Protect Crypto Services to generate a third seed and wrap it with your root key. See the following example. For more information, See [Securing your data](/docs/vpc?topic=vpc-hyper-protect-virtual-server-mng-data).

```yaml
volumes:
 test:
   kms:
     - apiKey: "L4SsSE32xxxxxjAgfHCVkdW8xl_CiqMn4Lpc1dzTD"
       crn: "crn:v1:bluemix:public:hs-crypto:us-south:a/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:key:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
       type: "public"
     - apiKey: "L4SsSE32xxxxxjAgfHCVkdW8xl_CiqMn4Lpc1dzTD"
       crn: "crn:v1:bluemix:public:hs-crypto:us-south:a/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:key:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
       type: "private"
   seed: "seed_value_with_minimum_15_characters"
   apiKey: "**********************"
   kmsTimeout: 10
```
{: codeblock}

### `signingKey` subsection
{: #hpcr_contract_env_signkey}

For more information about how to use the `signingKey`, see [Contract signature](#hpcr_contract_sign).

### `env` subsection
{: #hpcr_contract_env_env}

- If Pod descriptors are used in the `workload` section:

   See the example for the template format in [the `play` subsection](#hpcr_contract_play).

- If a docker compose file is used in the `workload` section:

   If the docker compose file has an environment section, you can use the following snippet as an example:
   ```sh
   environment:
     KEY1: "${Value1}"
     KEY2: "${Value2}"
   ```
   {: codeblock}

   When the docker compose file has an environment section, as shown in the previous example, you can pass the values in the `env` section of the deployer. The following example shows how to specify the values for the `env` variables:
   ```buildoutcfg
   env:
    value1: "abc"
    value2: "xyz"
   ```
   {: codeblock}

## Contract encryption
{: #hpcr_contract_encrypt}

You can encrypt the contents of a contract. Although you can also pass in the contract through the **User Data** without encryption, it is recommended that you encrypt the contract. It is also recommended that you initially try to use a nonencrypted contract for testing purposes, and after it works as expected, you can use an encrypted contract for your production environment.

You can decide which sections of the contract need encryption. For example, you can choose to [encrypt only the `workload` section](#hpcr_contract_encrypt_workload), or [encrypt only the `env` section](#hpcr_contract_encrypt_env).

When the {{site.data.keyword.hpvs}} for VPC instance boots, the bootloader decrypts the contract. It takes the value of each of the sections in the contract and decrypts it if it is encrypted. If it finds that a section is not encrypted, it considers it as it is without any decryption. You must use the public key to encrypt the contract before you pass it as an input through the **User Data** section.

The encryption and attestation certificates are signed by the IBM intermediate certificate. The IBM intermediate certificate is signed by the IBM Digicert intermediate cert, which in turn is signed by DigiCert Trusted Root G4. For more information about the certificates, see [DigiCert Trusted Root Authority Certificates](https://www.digicert.com/kb/digicert-root-certificates.htm).


### Downloading the encryption certificate and extracting the public key
{: #encrypt_downloadcert}

1. Download the certificate. The following table lists the encryption certificate expiry dates and image deprecation/obsolete date based on the version of the image.

   From 25 March 2025, the certificate links are changed.
   {: note}

   | Image version| Certificate link | Encryption cert expiry date | Deprecation date |
   | -------- | ----------- | ----------- | ----------- |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-25` | [Certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-25/ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt){: external} | 06 August 2026  | 31 March 2026 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-24` | [Certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-24/ibm-hyper-protect-container-runtime-1-0-s390x-24-encrypt.crt){: external} | 06 August 2026  | 31 March 2026 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-23` | [Certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-23/ibm-hyper-protect-container-runtime-1-0-s390x-23-encrypt.crt){: external} | 26 February 2026 | 15 December 2025 |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-22` | [Certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-22/ibm-hyper-protect-container-runtime-1-0-s390x-22-encrypt.crt){: external} | 26 February 2026 | 15 December 2025   |
   | `ibm-hyper-protect-container-runtime-1-0-s390x-21` | [Certificate](https://hpvsvpcubuntu.s3.us.cloud-object-storage.appdomain.cloud/s390x-21/ibm-hyper-protect-container-runtime-1-0-s390x-21-encrypt.crt){: external} | 26 February 2026 |  10 October 2025  | 
   {: caption="Encryption certificate expiry dates and image deprecation/ obsolete dates" caption-side="bottom"}

   **Note:**
   * **Deprecated**: You can use the image to create an instance from the IBM Cloud CLI. Using the deprecated status can discourage the use of the image before its status is changed to obsolete. The image catalog always maintains the two most recent image versions: n and n-1. When a new version (n+1) becomes available, the system deprecates the oldest version (n-1).
   * **Obsolete**: After the certificate associated with the image is expired, the image will no longer be available to provision new instance.
   * Always download the encryption certs corresponding to the image and encrypt the contracts.

   To check the image deprecation or obsolete status, you can also use the IBM Cloud image list command.

   You can refer to the [documentation](/docs/vpc?topic=vpc-vpc-reference&interface=ui#images-list) for the image list command to get the status of the image.

2. Validate the encryption certificate by following the instructions in the [Validating the contract encryption certificate](/docs/vpc?topic=vpc-cert_validate#validate_encrypt_cert) topic.

### Creating the encrypted `workload` section of a contract
{: #hpcr_contract_encrypt_workload}

The value of any section in a contract can be plain text or encrypted.
Complete the following steps on an Ubuntu system to encrypt the workload section used in a contract:

1. Create the `docker-compose.yaml` file based on your workload requirements. For example,
   ```yaml
   services:
     redisnode01:
       image: s390x/redis@sha256:db467ab5c53bdeef65762a7534e26fecb94a0f218bd38afd2eaba1a670c472b1
       ports:
         - "6379:6379"
   ```
   {: codeblock}

   For more information, see [Overview of Docker Compose](https://docs.docker.com/compose/).

2. Create the [workload section](#hpcr_contract_workload) of the contract and add the contents in the `workload.yaml` file.


3. Export the complete path of the `workload.yaml` file and `ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt`:

   ```yaml
   WORKLOAD="<PATH to workload.yaml>"
   CONTRACT_KEY="<PATH to ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt>"
   ```
   {: pre}

4. Use the following command to create a random password (the contract is encrypted through symmetric AES with a random PASSWORD):
   ```yaml
   PASSWORD="$(openssl rand 32 | base64 -w0)"
   ```
   {: pre}

5. Use the following command to encrypt password with `ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt`:

   ```yaml
   ENCRYPTED_PASSWORD="$(echo -n "$PASSWORD" | base64 -d | openssl rsautl -encrypt -inkey $CONTRACT_KEY -certin | base64 -w0 )"
   ```
   {: pre}

6. Use the following command to encrypt the `workload.yaml` file with a random password:
   ```sh
   ENCRYPTED_WORKLOAD="$(echo -n "$PASSWORD" | base64 -d | openssl enc -aes-256-cbc -pbkdf2 -pass stdin -in "$WORKLOAD" | base64 -w0)"
   ```
   {: pre}

7. Use the following command to get the encrypted section of the contract:
   ```yaml
   echo "hyper-protect-basic.${ENCRYPTED_PASSWORD}.${ENCRYPTED_WORKLOAD}"
   ```
   {: pre}

8. Get the output from step 7 and add it to the `user-data.yaml` file.
   ```yaml
   workload: hyper-protect-basic.js7TGt77EQ5bgTIKk5C0pViFTRHqWtn..............
   ```
   {: pre}

   The prefix `hyper-protect-basic` is mandatory.
   {: note}

### Creating the encrypted `env` section of a contract
{: #hpcr_contract_encrypt_env}

Complete the following steps on an Ubuntu system to encrypt the `env` section used in a contract:

1. Create the [`env` section](#hpcr_contract_env) of the contract and add the contents in the `env.yaml` file.
   
   Following is an example of `env.yaml`:

   ```yaml
   type: env
     logging:
     syslog:
       hostname: ${RSYSLOG_SERVER_IP}
       port: 6514
       server: "${RSYSLOG_SERVER_ROOT_CA}"
       cert: "${RSYSLOG_CLIENT_CA}"
       key: "${RSYSLOG_CLIENT_KEY}"
     volumes:
       test0:
         seed: "stsolutiontest1"
   ```
   {: codeblock}

2. Export the complete path of the `env.yaml` file and `ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt`:

   ```yaml
   ENV="<PATH to env.yaml>"
   CONTRACT_KEY="<PATH to ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt>"
   ```
   {: pre}

3. Use the following command to create a random password:

   ```yaml
   PASSWORD="$(openssl rand 32 | base64 -w0)"
   ```
   {: pre}

4. Use the following command to encrypt password with `ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt`:

   ```yaml
   ENCRYPTED_PASSWORD="$(echo -n "$PASSWORD" | base64 -d | openssl rsautl -encrypt -inkey $CONTRACT_KEY  -certin | base64 -w0)"
   ```
   {: pre}

5. Use the following command to encrypt `env.yaml` with a random password:
   ```yaml
   ENCRYPTED_ENV="$(echo -n "$PASSWORD" | base64 -d | openssl enc -aes-256-cbc -pbkdf2 -pass stdin -in "$ENV" | base64 -w0)"
   ```
   {: pre}

6. Use the following command to get the encrypted section of the contract:
   ```yaml
   echo "hyper-protect-basic.${ENCRYPTED_PASSWORD}.${ENCRYPTED_ENV}"
   ```
   {: pre}

7. To encrypt the workload section, see [Creating the encrypted `workload` section of a contract](#hpcr_contract_encrypt_workload).

8. Get the output from step 6 and add it to the `user-data.yaml` file.
   ```yaml
   env: hyper-protect-basic.VWg/5/SWE+9jLfhr8q4i.........
   ```
   {: pre}


## Contract signature
{: #hpcr_contract_sign}


Contract signature is an optional feature that can be used with the contract. You can choose to sign a contract before it is passed as input. You can also set the expiry of the contract during signature. Contracts that are in plain text or encrypted can be signed. Validation of the contract signature is done by the {{site.data.keyword.hpvs}} for VPC image.
The purpose of this signature feature is to ensure that the `workload` and `env` sections are always used together and are not tampered with by a third party. This feature also supports setting expiry for contract. That is, if the instance is booted after the signature expired, the boot process fails. The signature of the `workload` and the `env` sections are added as the value to the `envWorkloadSignature` section.
The following are two sections in a contract that are relevant while creating and adding a contract signature:
* `envWorkloadSignature`: This section is where the signature of the other sections of the contract is added. This section is not required for a contract that is not signed.
* `signingKey`: This subsection must be added to the `env` section of the contract. This subsection holds the value to the user-generated public key, whose corresponding private key was used to create the contract signature. A public key or certificate can also be parsed as a Base64 string.

Complete the following steps on an Ubuntu system to create the contract signature:
1. Use the following command to generate key pair to sign the contract (note that "test1234" is the passphrase to generate keys, you can use your own):
   ```sh
   openssl genrsa -aes128 -passout pass:test1234 -out private.pem 4096
   openssl rsa -in private.pem -passin pass:test1234 -pubout -out public.pem
   ```
   {: pre}

2. The following command is an example of how you can get the signing key:
   ```sh
   key=$(awk -vRS="\n" -vORS="\\\n" '1' public.pem)
   echo ${key%\\n}
   ```

3. Optionally, if you want to pass the signing key as Base64:
    ```sh
    key=$(cat public.pem | base64 -w 0)
    echo $key
    ```

4. Optionally, if you want to enable contract expiry, follow these steps:
   1. Use the following command to generate a certificate request:
      ```sh
      openssl req -new -key private.pem -passin pass:test1234 -out csr.pem
      ```
      {: pre}

   2. The command generates the certificate.
       - Use the following command to generate a private key for CA:
        ```sh
        openssl genrsa -out personal_ca.key 2048
        ```
        {: pre}

       - Generate a self-signed CA certificate by using the following command:
        ```sh
        openssl req -new -x509 -key personal_ca.key -out personal_ca.crt
        ```
        {: pre}

       - The following command signs CSR with self-signed CA certificate along with number of days for the certificate to be valid. The end date of the generated certificate is the contract expiry date:
        ```sh
        openssl x509 -req -in csr.pem -CA personal_ca.crt -CAkey personal_ca.key -CAcreateserial -out certificate.pem -days 365
        ```
        {: pre}

   3. The following command is an example to get the certificate:
        ```sh
        certificate=$(awk -vRS="\n" -vORS="\\\n" '1' certificate.pem)
        echo ${certificate%\\n}
        ```
        {: pre}

   4. Optionally, use the following command as an example to get the certificate in Base64 format:
      ```sh
      certificate=$(cat certificate.pem | base64 -w 0)
      echo $certificate
      ```

5. Create the `env.yaml` file. See the following example:
   1. If `signingkey` is a public key:

      ```yaml
      env: |
        type: env
        logging:
          logRouter:
            hostname: 34be57c7-6ff2-4685-8839-903921e90ab9.ingress.jp-tok.logs.cloud.ibm.com
            iamApiKey: <iamApiKey of the service instance> / xxxx
            port: 443
        volumes:
          test:
          seed: "seed_value_with_minimum_15_characters"
        signingKey: "-----BEGIN PUBLIC KEY-----\nMIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKCAgEAvLaeSA8Nc3p99HNUMwon\n5lMMALAsIxRRpWUaEZ5IcUky2sgCi/rSmxU2sm6FK/BmCftk33f5W2BsYHdY9R/0\nELZ9A4POQcJsPF3ronU2QHwnRjcqYuUFXmf1VqfPPLpELriFNoCb2FN2zCa+VUmu\n+fGhroZ3Fr9kBPwJhGr917E5jeCQ+MzsGkulcTvr0SfvThiZQQ/KlU0R35ThamF3\n8C0F5IQBpqDUwDFmWvD5lF2SmprpluDBFEj8LLfLxvW9M2Qwku6nGUnnFReg3vNH\n7IF0SRr1K1AdO5hEmevCdyG9hgTdUY6dXcjntiN/kbqXErILknvzDnb4jyPZZRdK\ndrOzVt8hjbdmkS396SrMFtA++QrV3GNZl5zCscpn6d8S7BEA8mDzroo2UAbrypVP\n9l9AmzUnmnPCpZQySUUHoY0xG2vgMSA50CWH7Uwjmpixr02Td4/LU8fE7NWCO6ci\nx4++ANSaxu+uuZ2Pe1OjjgV98r06ZUs38eaxptLZqLpn3N6w8WAJxGwSLapZwNtP\ng2spUXu2Eh/TN5t4/ly5iXOsyIy8IPtTrUPX7rpaaqFZ72P6BJLj3WLEvOG/eF/8\nBTjrsZAjb8YjkO1uGk10IPa63sniZWe5vlm9w9UKy2uGuy6RhWxwoVHRRbfhboQF\nsO20dsVwgTZn8c46HMD2PoMCAwEAAQ==\n-----END PUBLIC KEY----"
      ```
      {: codeblock}

   2. If `signingkey` is a certificate:

      ```yaml
        env: |
          type: env
          logging:
            logRouter:
              hostname: 34be57c7-6ff2-4685-8839-903921e90ab9.ingress.jp-tok.logs.cloud.ibm.com
              iamApiKey: <iamApiKey of the service instance> / xxxx
              port: 443
          volumes:
            test:
            seed: "seed_value_with_minimum_15_characters"
          signingKey: "-----BEGIN CERTIFICATE-----\nMIIFETCCAvkCFBAMxyO6Cl7BNKBGxtlAzHpI2oiNMA0GCSqGSIb3DQEBCwUAMEUx\nCzAJBgNVBAYTAkFVMRMwEQYDVQQIDApTb21lLVN0YXRlMSEwHwYDVQQKDBhJbnRl\ncm5ldCBXaWRnaXRzIFB0eSBMdGQwHhcNMjQwMTMwMDM1ODMzWhcNMjQwNTA5MDM1\nODMzWjBFMQswCQYDVQQGEwJBVTETMBEGA1UECAwKU29tZS1TdGF0ZTEhMB8GA1UE\nCgwYSW50ZXJuZXQgV2lkZ2l0cyBQdHkgTHRkMIICIjANBgkqhkiG9w0BAQEFAAOC\nAg8AMIICCgKCAgEAv5h6i7Fn1DMUM+3AnPPZUNMe1ss3KL/AmUmptwlAPErVoH1k\naiqTUsSNjXctj+nk95I+e2nugw/HlaVT1eRgEtvjssheXKboFn+zW/i31Nq9USgQ\nZA325VtchYlgJLXMPaH/ukBUr0UI4LnjC/dNdAQzKwWPNF2Jlv5wKX8OBVOQO9Df\nExVmcEkKDoh0nZk5eOA8vzJGhfr8TvQx9FQFsP4OXTwQgcdZV26mLm0bMkqEt3o5\n8OSpisqNGY1XnMHjOWNqSbErkpbIKEFAQSnWmzEvJdHsQX+7eTF7CisHJREseT4s\nUSuIFBZKXbS3qq6EL/EYviu0EGnY/rkJJcIRb8hycqHRgoITT2bWT7PSMUyXoX3G\nVKfp/xKFhkYzoRDSb5S0lh8sugmoRkioAkw6G56CP2hablPZRUMmUKceFfOG/k4L\nei8qJtbfQJ9BlCNRPpjqY3sGSdeXI4zefyQ8xxcus9Sl5wXZV86lz2lO/fz3Cvpd\n0eKvfv5uXyvF3O36lrlEERmSukaZYaEJECjxOUeafc7E1DVyIaMpc2SOum1crwMG\nRKhnU1JShDON0yClnKOlACfjFIpdpEMpE4lLps1x+PXV+x21zGBMUvXYa4xpbyWR\nK1gfMWmuvGOivl9y0mPSIeyJ9R/7bSRAbcYJR4N99TrtWxZU1yQi7HSRV5cCAwEA\nATANBgkqhkiG9w0BAQsFAAOCAgEAg006zJ4ZKwT8moOOl3PdThFUcf8rrIlec9Iy\nqPcWSqt5UTeYLIe58oGhhQmcaIRUOQaib5JqH2ukzqpo+gsJ3zZb3eIn4FB7cKef\nLqaiemOveEe1/qSwAGqMZyZELssiOflhnJdzuYSRWO8DO6Q6JMqQthDcw20budjO\nzP4nhXQqT+s8ljzqSJW77hDbrNAezTz/0SJFDtaMBs5UweX//7/4sXtJ8kBIBSxd\n7y4w8tuuxUaXOtYMjNrJAYLwFVeeO8CFURpbEuv7ABT0k8U4E8C6j4U4Jysx4XVP\nZj36rIAtvctchh0yAhHz8whXe1tvaFw9wzRDATnThFAuJG4Z07K2/rlDP9kO9wmn\ng8hHxKeqQMJDp29e0sGkz8oDi6Mz24k9CqFJJ0CUz1ntz7rrDkA3QwQbFRzk938y\n3rSfePO5qXlUQ9mm05hYr1EKKceTLEowc4XOouNLlUWGiRshRR1szMw5C29prFJ2\nyYuV9tBaFYkq7dnh8JnmrreEvAnsKyyECxMmtV/W701OSUYBcThwgAo+hkEeOJ+/\nwrOS7yoJqDF1y+5LLQJmUlrLCPXem3ZTa4UMe1p2g7ge7Dg6Zud9NDBcMigdHByt\nJP/i9PcJSEWrccWJ1ajToUCZ0wqfJ3Z4KqoEd0fadQhb32AuDUbu7E12EUFNPGIH\n8rQKbDU=\n-----END CERTIFICATE-----"
      ```
      {: codeblock}

   3. If `signingkey` is a Base64 encoded signing key or certificate:

      ```yaml
        env: |
          type: env
          logging:
            logRouter:
              hostname: 34be57c7-6ff2-4685-8839-903921e90ab9.ingress.jp-tok.logs.cloud.ibm.com
              iamApiKey: <iamApiKey of the service instance> / xxxx
              port: 443
          volumes:
            test:
            seed: "seed_value_with_minimum_15_characters"
          signingKey: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tXG5NSUlGRVRDQ0F2a0NGQkFNeHlPNkNsN0JOS0JHeHRsQXpIcEkyb2lOTUEwR0NTcUdTSWIzRFFFQkN3VUFNRVV4XG5DekFKQmdOVkJBWVRBa0ZWTVJNd0VRWURWUVFJREFwVGIyMWxMVk4wWVhSbE1TRXdId1lEVlFRS0RCaEpiblJsXG5jbTVsZENCWGFXUm5hWFJ6SUZCMGVTQk1kR1F3SGhjTk1qUXdNVE13TURNMU9ETXpXaGNOTWpRd05UQTVNRE0xXG5PRE16V2pCRk1Rc3dDUVlEVlFRR0V3SkJWVEVUTUJFR0ExVUVDQXdLVTI5dFpTMVRkR0YwWlRFaE1COEdBMVVFXG5DZ3dZU1c1MFpYSnVaWFFnVjJsa1oybDBjeUJRZEhrZ1RIUmtNSUlDSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DXG5BZzhBTUlJQ0NnS0NBZ0VBdjVoNmk3Rm4xRE1VTSszQW5QUFpVTk1lMXNzM0tML0FtVW1wdHdsQVBFclZvSDFrXG5haXFUVXNTTmpYY3RqK25rOTVJK2UybnVndy9IbGFWVDFlUmdFdHZqc3NoZVhLYm9Gbit6Vy9pMzFOcTlVU2dRXG5aQTMyNVZ0Y2hZbGdKTFhNUGFIL3VrQlVyMFVJNExuakMvZE5kQVF6S3dXUE5GMkpsdjV3S1g4T0JWT1FPOURmXG5FeFZtY0VrS0RvaDBuWms1ZU9BOHZ6SkdoZnI4VHZReDlGUUZzUDRPWFR3UWdjZFpWMjZtTG0wYk1rcUV0M281XG44T1NwaXNxTkdZMVhuTUhqT1dOcVNiRXJrcGJJS0VGQVFTbldtekV2SmRIc1FYKzdlVEY3Q2lzSEpSRXNlVDRzXG5VU3VJRkJaS1hiUzNxcTZFTC9FWXZpdTBFR25ZL3JrSkpjSVJiOGh5Y3FIUmdvSVRUMmJXVDdQU01VeVhvWDNHXG5WS2ZwL3hLRmhrWXpvUkRTYjVTMGxoOHN1Z21vUmtpb0FrdzZHNTZDUDJoYWJsUFpSVU1tVUtjZUZmT0cvazRMXG5laThxSnRiZlFKOUJsQ05SUHBqcVkzc0dTZGVYSTR6ZWZ5UTh4eGN1czlTbDV3WFpWODZsejJsTy9mejNDdnBkXG4wZUt2ZnY1dVh5dkYzTzM2bHJsRUVSbVN1a2FaWWFFSkVDanhPVWVhZmM3RTFEVnlJYU1wYzJTT3VtMWNyd01HXG5SS2huVTFKU2hET04weUNsbktPbEFDZmpGSXBkcEVNcEU0bExwczF4K1BYVit4MjF6R0JNVXZYWWE0eHBieVdSXG5LMWdmTVdtdXZHT2l2bDl5MG1QU0lleUo5Ui83YlNSQWJjWUpSNE45OVRydFd4WlUxeVFpN0hTUlY1Y0NBd0VBXG5BVEFOQmdrcWhraUc5dzBCQVFzRkFBT0NBZ0VBZzAwNnpKNFpLd1Q4bW9PT2wzUGRUaEZVY2Y4cnJJbGVjOUl5XG5xUGNXU3F0NVVUZVlMSWU1OG9HaGhRbWNhSVJVT1FhaWI1SnFIMnVrenFwbytnc0ozelpiM2VJbjRGQjdjS2VmXG5McWFpZW1PdmVFZTEvcVN3QUdxTVp5WkVMc3NpT2ZsaG5KZHp1WVNSV084RE82UTZKTXFRdGhEY3cyMGJ1ZGpPXG56UDRuaFhRcVQrczhsanpxU0pXNzdoRGJyTkFlelR6LzBTSkZEdGFNQnM1VXdlWC8vNy80c1h0SjhrQklCU3hkXG43eTR3OHR1dXhVYVhPdFlNak5ySkFZTHdGVmVlTzhDRlVScGJFdXY3QUJUMGs4VTRFOEM2ajRVNEp5c3g0WFZQXG5aajM2cklBdHZjdGNoaDB5QWhIejh3aFhlMXR2YUZ3OXd6UkRBVG5UaEZBdUpHNFowN0syL3JsRFA5a085d21uXG5nOGhIeEtlcVFNSkRwMjllMHNHa3o4b0RpNk16MjRrOUNxRkpKMENVejFudHo3cnJEa0EzUXdRYkZSems5Mzh5XG4zclNmZVBPNXFYbFVROW1tMDVoWXIxRUtLY2VUTEVvd2M0WE9vdU5MbFVXR2lSc2hSUjFzek13NUMyOXByRkoyXG55WXVWOXRCYUZZa3E3ZG5oOEpubXJyZUV2QW5zS3l5RUN4TW10Vi9XNzAxT1NVWUJjVGh3Z0FvK2hrRWVPSisvXG53ck9TN3lvSnFERjF5KzVMTFFKbVVsckxDUFhlbTNaVGE0VU1lMXAyZzdnZTdEZzZadWQ5TkRCY01pZ2RIQnl0XG5KUC9pOVBjSlNFV3JjY1dKMWFqVG9VQ1owd3FmSjNaNEtxb0VkMGZhZFFoYjMyQXVEVWJ1N0UxMkVVRk5QR0lIXG44clFLYkRVPVxuLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLVxu
      ```
      {: codeblock}

6. Create the `env.yaml` file. See the following example:

   ```yaml
    env: |
      type: env
      logging:
        logRouter:
          hostname: 34be57c7-6ff2-4685-8839-903921e90ab9.ingress.jp-tok.logs.cloud.ibm.com
          iamApiKey: <iamApiKey of the service instance> / xxxx
          port: 443
      volumes:
        test:
        seed: "seed_value_with_minimum_15_characters"
      signingKey: "-----BEGIN PUBLIC KEY-----\nMIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKCAgEAvLaeSA8Nc3p99HNUMwon\n5lMMALAsIxRRpWUaEZ5IcUky2sgCi/rSmxU2sm6FK/BmCftk33f5W2BsYHdY9R/0\nELZ9A4POQcJsPF3ronU2QHwnRjcqYuUFXmf1VqfPPLpELriFNoCb2FN2zCa+VUmu\n+fGhroZ3Fr9kBPwJhGr917E5jeCQ+MzsGkulcTvr0SfvThiZQQ/KlU0R35ThamF3\n8C0F5IQBpqDUwDFmWvD5lF2SmprpluDBFEj8LLfLxvW9M2Qwku6nGUnnFReg3vNH\n7IF0SRr1K1AdO5hEmevCdyG9hgTdUY6dXcjntiN/kbqXErILknvzDnb4jyPZZRdK\ndrOzVt8hjbdmkS396SrMFtA++QrV3GNZl5zCscpn6d8S7BEA8mDzroo2UAbrypVP\n9l9AmzUnmnPCpZQySUUHoY0xG2vgMSA50CWH7Uwjmpixr02Td4/LU8fE7NWCO6ci\nx4++ANSaxu+uuZ2Pe1OjjgV98r06ZUs38eaxptLZqLpn3N6w8WAJxGwSLapZwNtP\ng2spUXu2Eh/TN5t4/ly5iXOsyIy8IPtTrUPX7rpaaqFZ72P6BJLj3WLEvOG/eF/8\nBTjrsZAjb8YjkO1uGk10IPa63sniZWe5vlm9w9UKy2uGuy6RhWxwoVHRRbfhboQF\nsO20dsVwgTZn8c46HMD2PoMCAwEAAQ==\n-----END PUBLIC KEY----"
   ```
   {: codeblock}

7. Use the following command to export complete path of `env.yaml` and `ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt`:

   ```sh
   ENV="<PATH to env.yaml>"
   CONTRACT_KEY="<PATH to ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt>"
   ```
   {: pre}

8. Use the following command to create a random password:

   ```sh
   PASSWORD="$(openssl rand 32 | base64 -w0)"
   ```
   {: pre}

9. Use the following command to encrypt password with `ibm-hyper-protect-container-runtime-1-0-s390x-25-encrypt.crt`:

   ```yaml
   ENCRYPTED_PASSWORD="$(echo -n "$PASSWORD" | base64 -d | openssl rsautl -encrypt -inkey $CONTRACT_KEY  -certin | base64 -w0)"
   ```
   {: pre}

10. Use the following command to encrypt `env.yaml` with a random password:

    ```yaml
    ENCRYPTED_ENV="$(echo -n "$PASSWORD" | base64 -d | openssl enc -aes-256-cbc -pbkdf2 -pass stdin -in "$ENV" | base64 -w0)"
    ```
    {: pre}

11. Use the following command to extract the encrypted `env` section:

    ```yaml
    echo "hyper-protect-basic.${ENCRYPTED_PASSWORD}.${ENCRYPTED_ENV}"
    ```
    {: pre}

Steps 7 - 11 are used to encrypt the `env` section. If you choose to not encrypt this section, skip these steps.
{: note}

A notification about the expiry of the contract is sent to your logging service.
Timelines for the notification are as follows:
- On the first of every month
- Everyday for 30 days before the expiry
- Once in every 4 hours if the contract is about to expire in 7 days
- Every hour if the contract has already expired or is about to expire in a day


## Preparing the Attestation section
{: #hpcr_attestation_prepare}

Attestation is the optional feature that can be used with contract. The `attestationPublicKey` is the user provided public key that is used to encrypt the attestation document. This can be provided as a public RSA key or Base64 encoded of the public RSA key as a part of the contract.

*  If you use the plain text public RSA key for the `attestationPublicKey` in the yaml file, use the following example:
   ```text
   attestationPublicKey: "-----BEGIN PUBLIC KEY-----\nMIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKCAgEAvLaeSA8Nc3p99HNUMwon\n5lMMALAsIxRRpWUaEZ5IcUky2sgCi/rSmxU2sm6FK/BmCftk33f5W2BsYHdY9R/0\nELZ9A4POQcJsPF3ronU2QHwnRjcqYuUFXmf1VqfPPLpELriFNoCb2FN2zCa+VUmu\n+fGhroZ3Fr9kBPwJhGr917E5jeCQ+MzsGkulcTvr0SfvThiZQQ/KlU0R35ThamF3\n8C0F5IQBpqDUwDFmWvD5lF2SmprpluDBFEj8LLfLxvW9M2Qwku6nGUnnFReg3vNH\n7IF0SRr1K1AdO5hEmevCdyG9hgTdUY6dXcjntiN/kbqXErILknvzDnb4jyPZZRdK\ndrOzVt8hjbdmkS396SrMFtA++QrV3GNZl5zCscpn6d8S7BEA8mDzroo2UAbrypVP\n9l9AmzUnmnPCpZQySUUHoY0xG2vgMSA50CWH7Uwjmpixr02Td4/LU8fE7NWCO6ci\nx4++ANSaxu+uuZ2Pe1OjjgV98r06ZUs38eaxptLZqLpn3N6w8WAJxGwSLapZwNtP\ng2spUXu2Eh/TN5t4/ly5iXOsyIy8IPtTrUPX7rpaaqFZ72P6BJLj3WLEvOG/eF/8\nBTjrsZAjb8YjkO1uGk10IPa63sniZWe5vlm9w9UKy2uGuy6RhWxwoVHRRbfhboQF\nsO20dsVwgTZn8c46HMD2PoMCAwEAAQ==\n-----END PUBLIC KEY----"
   ```

*  If you use the Base64 encoded signing key for the `attestationPublicKey` in the yaml file, use the following command and example.

   ```sh
   base64 -w0 <public RSA key file>
   ```
   You need to replace `<public RSA key file>` with the path to your actual public RSA key file.
   {: note}

   ```text
   attestationPublicKey: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJpRENDQVM2Z0F3SUJBZ0lSQUxCMXBPYlpEQlRRc09GSFlxazMzaWd3Q2dZSUtvWkl6ajBFQXdJd0tqRW8KTUNZR0ExVUVBeE1mZFhNdWFXTnlMbWx2TDNCeVlXSm9ZWFF4TWpNdmJYa3RhR0Z3Y205NGVUQWVGdzB5TWpBMApNVE14TURFd01ETmFGdzB6TWpBME1UQXhNREV3TUROYU1Db3hLREFtQmdOVkJBTVRIM1Z6TG1samNpNXBieTl3CmNtRmlhR0YwTVRJekwyMTVMV2hoY0hKdmVIa3dXVEFUQmdjcWhrak9QUUlCQmdncWhrak9QUU1CQndOQ0FBU1AKWGsrelE2MlFZNjI3MWQ1cTBMZHY3SGc3QzZkMGZOUlRsQmJXekhOWWFDZzlpU0piYnVNdjVBY0JmMjlqQi83eApqYzhzVitxMksyemtkTHV4QWxGWm96VXdNekFPQmdOVkhROEJBZjhFQkFNQ0JhQXdFd1lEVlIwbEJBd3dDZ1lJCkt3WUJCUVVIQXdNd0RBWURWUjBUQVFIL0JBSXdBREFLQmdncWhrak9QUVFEQWdOSUFEQkZBaUIzd0JTa0IxaXAKZHZZYlBMbFBmS3RZT0hsYnZzUllKa0FZM2hnY0xuNWhwQUloQUt6cmhsU3p4K1I5bmdtMTBlZVkyaFNCRmgrawpMWHp6SFkwaktTVzhyM1FhCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
   ```

* If you encrypted the `attestationPublicKey` in the yaml file, you must follow the same steps as mentioned in the [Contract encryption](#hpcr_contract_encrypt).

   To decrypt the attestation document, follow the instructions in the [Decrypting the attestation document](/docs/vpc?topic=vpc-about-attestation#decrypt_attest_record).

## Preparing the signature
{: #hpcr_signature_prepare}

Complete the following steps on an Ubuntu system to prepare the signature:

If you have the encrypted `user-data.yaml` from the [Creating the encrypted `workload` section of a contract](#hpcr_contract_encrypt_workload) and [Creating the encrypted `env` section of a contract](#hpcr_contract_encrypt_env) sections, skip to step 3.

1. Get the encrypted `workload.yaml` and encrypted `env.yaml` files.

2. Add them into the `user-data.yaml` file.
   ```yaml
   workload: hyper-protect-basic.js7TGt77EQ5bgTIKk5C0pViFTRHqWtn..............
   env: hyper-protect-basic.VWg/5/SWE+9jLfhr8q4i.........
   ```
   {: pre}

3. Create the `contract.txt` file. Add the value of `workload` first, then add the value of `env` from the `user-data.yaml` file. Make sure that no space or new line is present after `workload` and before `env`. Also, make sure that no new line or space is added at the end of the file. It is recommended to cross-check the binary content of the `contract.txt` file with tools such as `hexdump`. In the binary file dump, make sure that you do not see the `0a` ASCII value as the last entry.
   ```yaml
   hyper-protect-basic.js7TGt77EQ5bgTIKk5C0pViFTRHqWtn..............hyper-protect-basic.VWg/5/SWE+9jLfhr8q4i.........
   ```
   {: pre}

4. Use the following command to generate the signature:
   ```sh
   echo $(cat contract.txt | tr -d "\n\r" | openssl dgst -sha256 -sign private.pem | openssl enc -base64) | tr -d ' '
   ```
   {: pre}

5. Add the signature into the `user-data.yaml` file:
   ```yaml
   workload: hyper-protect-basic.js7TGt77EQ5bgTIKk5C0pViFTRHqWtn..............
   env: hyper-protect-basic.VWg/5/SWE+9jLfhr8q4i.........
   envWorkloadSignature: Icbm1D/CVpLNYkWRC9e .....
   ```
   {: codeblock}


## Getting started with a simple {{site.data.keyword.hpvs}} for VPC contract
{: #getting_started_simplehpcr_contract}

The following command examples are run on an Ubuntu system.

### 1. Get the details of your logging instance
{: #step1}

You can configure logging with IBM Log Analysis or a generic syslog backend. This example uses IBM Log Analysis. You can choose from different plans. To understand how you get the required details such as the hostname and the ingestion key, see [Logging for {{site.data.keyword.hpvs}} for VPC](/docs/vpc?topic=vpc-logging-for-hyper-protect-virtual-servers-for-vpc).

### 2. Create the `env` section
{: #step2}

Make sure that you do not miss the pipe symbol '|' if you are using a plain text contract. It is not required if you are planning to encrypt the section.

```yaml
env: |
  type: env
  logging:
    logRouter:
      hostname: 34be57c7-6ff2-4685-8839-903921e90ab9.ingress.jp-tok.logs.cloud.ibm.com
      iamApiKey: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
      port: 443
```
{: codeblock}

### 3. Prepare the docker-compose file
{: #step3}

Assuming that you have the logging details, find a simple docker compose file. The following example has a public NGINX container. Create a docker-compose.yaml by using the example.

```yaml
services:
  nginx:
    image: docker.io/library/nginx@sha256:b1306efee704017b0e02efadc011d374063a4e9c47b86bdc57744fc3f0666383
    ports:
    - 80:80
    - 443:443
```
{: codeblock}

### 4. Get the base64-encoded version of the docker-compose file
{: #step4}

```sh
tar -czvf compose.tgz docker-compose.yaml
```
{: pre}

```sh
base64 -i compose.tgz > compose.b64
```
{: pre}

### 5. Create the compose section with it
{: #step5}

```yaml
  compose:
    archive: H4sIADXNg2IAA+3W326CMBQGcK59it555XbanraMq70KlOLIJhjqzPb2q6g3S9xiIi7T75eQlj+hDYcP6vvVuo/hMZsQJc6YsU2+t2NfsmKyyhHLjKRUSmbCTDmpo/e4KQchsqHvNz9d99v5f8of6l/3/jUMi8Puw+fq7XJj7ApsmU/WX2m7r7/j9Abs6s/W2kzQ5aZw2p3XfxuG2PZdIeZ6Poth2LY+xGImRLdsu49dR4h2VS5DsT/yHF9KZWxRSU02NCGkxJJ0FQVSoSlrn8Jba5eyrEsOT55dlduq9sY55sbrhlJpda7HG6/7YRP3YyxETkVOhz6zLtI2++unc/tO5b+84AfgrPyn4JM0pDXyfw3I/32bMvdH5+SfOK0TpdZWIv/XgPzftynX/Udn/f/NmH+l8P+/CuQfAAAAAAAAAAAAAOD2fAEPQbuiACgAAA==
```
{: codeblock}

### 6. Populate the workload section
{: #step6}

Make sure that you do not miss the pipe symbol (|) if you are using a plain text contract.

```yaml
workload: |
  type: workload
  compose:
    archive: H4sIADXNg2IAA+3W326CMBQGcK59it555XbanraMq70KlOLIJhjqzPb2q6g3S9xiIi7T75eQlj+hDYcP6vvVuo/hMZsQJc6YsU2+t2NfsmKyyhHLjKRUSmbCTDmpo/e4KQchsqHvNz9d99v5f8of6l/3/jUMi8Puw+fq7XJj7ApsmU/WX2m7r7/j9Abs6s/W2kzQ5aZw2p3XfxuG2PZdIeZ6Poth2LY+xGImRLdsu49dR4h2VS5DsT/yHF9KZWxRSU02NCGkxJJ0FQVSoSlrn8Jba5eyrEsOT55dlduq9sY55sbrhlJpda7HG6/7YRP3YyxETkVOhz6zLtI2++unc/tO5b+84AfgrPyn4JM0pDXyfw3I/32bMvdH5+SfOK0TpdZWIv/XgPzftynX/Udn/f/NmH+l8P+/CuQfAAAAAAAAAAAAAOD2fAEPQbuiACgAAA==
```
{: codeblock}

### 7. Your simple contract is ready
{: #step7}

```yaml
env: |
  type: env
  logging:
    logRouter:
      hostname: 34be57c7-6ff2-4685-8839-903921e90ab9.ingress.jp-tok.logs.cloud.ibm.com
      iamApiKey: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
      port: 443
workload: |
  type: workload
  compose:
    archive: H4sIADXNg2IAA+3W326CMBQGcK59it555XbanraMq70KlOLIJhjqzPb2q6g3S9xiIi7T75eQlj+hDYcP6vvVuo/hMZsQJc6YsU2+t2NfsmKyyhHLjKRUSmbCTDmpo/e4KQchsqHvNz9d99v5f8of6l/3/jUMi8Puw+fq7XJj7ApsmU/WX2m7r7/j9Abs6s/W2kzQ5aZw2p3XfxuG2PZdIeZ6Poth2LY+xGImRLdsu49dR4h2VS5DsT/yHF9KZWxRSU02NCGkxJJ0FQVSoSlrn8Jba5eyrEsOT55dlduq9sY55sbrhlJpda7HG6/7YRP3YyxETkVOhz6zLtI2++unc/tO5b+84AfgrPyn4JM0pDXyfw3I/32bMvdH5+SfOK0TpdZWIv/XgPzftynX/Udn/f/NmH+l8P+/CuQfAAAAAAAAAAAAAOD2fAEPQbuiACgAAA==
```
{: codeblock}

## Next steps
{: #next-steps-contract}

* [Creating an instance by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers)
* [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli)
