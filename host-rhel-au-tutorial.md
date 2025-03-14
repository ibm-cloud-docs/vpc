---

copyright:
  years: 2025
lastupdated: "2025-03-14"

keywords:

subcollection: vpc

content-type: tutorial
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Running a model in RHEL AI on IBM Cloud 
{: #how-to-rhel-ai-tutorial}

Red Hat Enterprise Linux AI (RHEL AI) is a foundation model platform that is designed to develop, test, and run large language models (LLMs) for enterprise applications. It is available as a bootable Red Hat Enterprise Linux image optimized for AI workloads. These images include PyTorch, hardware acceleration libraries for NVIDIA, Intel, and AMD GPUs, and essential runtime libraries, which help streamline the setup of your AI development platform. For more information about RHEL AI, see [Introducing an accessible open source AI platform](https://developers.redhat.com/products/rhel-ai/overview){: external}. {{site.data.keyword.cloud_notm}} provides different GPU-based virtual server instances (VSIs) where you install RHEL AI and run the models. The following guide provides step-by-step instructions to deploy RHEL AI on {{site.data.keyword.cloud_notm}}.
{: shortdesc}

{{site.data.keyword.cloud_notm}} provides GPU-based virtual server instances (VSIs) for different AI model requirements. Running a model that uses InstructLab (ilab) requires a GPU-based VSI on {{site.data.keyword.cloud_notm}} before you download a model from Hugging Face and use it to chat with the LLM. 

In this tutorial, you learn how to complete the following tasks:

1. Host RHEL AI on {{site.data.keyword.cloud_notm}}.
2. Run and test foundation models by using InstructLab on hosted VSI.

## Before you begin
{: #how-to-pre-req}

- [Create an {{site.data.keyword.cloud_notm}} account](/registration) if you don't already have one
- [Download the RHEL AI image](https://developers.redhat.com/products/rhel-ai/download){: external}
- [Create user access tokens in Hugging Face](https://huggingface.co/docs/hub/en/security-tokens){: external}
- (Optional) Create a Red Hat account with the activation key. For more information, see [Creating and managing activation keys](https://docs.redhat.com/en/documentation/subscription_central/1-latest/html/getting_started_with_activation_keys_on_the_hybrid_cloud_console/assembly-creating-managing-activation-keys){: external}

## Setting up RHEL AI on {{site.data.keyword.cloud_notm}}
{: #how-to-host}

Complete the following steps to host an RHEL AI instance on {{site.data.keyword.cloud_notm}}.

### Step 1: Create a Cloud Object Storage instance and upload the RHEL AI image to a bucket
{: #how-to-step-1}

Store the RHEL AI image that you downloaded in a Cloud Object Storage bucket. The stored image is later used to create a custom image in {{site.data.keyword.cloud_notm}}.

1. Log in to the [{{site.data.keyword.cloud_notm}} console](/login){: external}.
1. [Create a Cloud Object Storage instance](/docs/cloud-object-storage?topic=cloud-object-storage-secure-content-store).
1. Upload the RHEL AI qcow image, which you downloaded, to the Cloud Object Storage bucket.

Uploading the large qcow image directly into Cloud Object Storage might fail. Use [IBM Aspera](https://www.ibm.com/products/aspera){: external} for large file transfers to your Cloud Object Storage bucket.
{: note}

### Step 2: Grant VPC access to Cloud Object Storage to create a custom image
{: #how-to-step-2}

Before you can create a custom image, grant the {{site.data.keyword.cloud_notm}} VPC service access to Cloud Object Storage. This service-to-service authorization gives VPC access to create a custom image based on the one stored in Cloud Object Storage.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Manage > Access (IAM) > Authorizations**. Then, click **Create**.
1. Specify a policy that gives the source service, **VPC Infrastructure Service**, reader access to the target service, **Cloud Object Storage**.
   1. Select **This account** as the source account and click **Next**.
   1. Select **VPC Infrastructure Service** as the source service and click **Next**.
   1. Select **Specific resources**, select **Resource type**, and choose **Image Service for VPC**. Then, click **Next**.
   1. Select **Cloud Object Storage** as the target service and click **Next**.
   1. Select **All resources** and click **Next**.
   1. Select **Reader** and click **Review > Authorize**.

You can also use the {{site.data.keyword.cloud_notm}} CLI to create a service-to-service authorization:

```sh
ibmcloud iam authorization-policy-create Reader --source-service-name is.vpc --target-service-name cloud-object-storage --source-service-account <ACCOUNT_ID> --source-resource-type image
```
{: codeblock}

### Step 3: Create a custom image
{: #how-to-step-3}

To host new RHEL AI instances, create a custom image that uses the RHEL AI image that you uploaded to the Cloud Object Storage bucket.

1. Click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) > **Infrastructure > Compute > Images**. Then, click **Create**.
1. Select the location where you want to create the custom image.
1. Enter the name for the image and select a resource group.
1. Select **Cloud Object Storage** as the image source.
1. Select the Cloud Object Storage instance, region, and bucket where you saved the RHEL AI image.
1. Select **Red Hat Enterprise Linux AI** as the operating system.
1. Click **Create**.

### Step 4: Check the status of your custom image
{: #how-to-step-4}

Check the status of the custom image to make sure that it was successfully created and is ready for use.

1. Click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) > **Infrastructure > Compute > Images > Custom images**.
1. Click the image name to see the details.
1. When the status is **Available**, it is ready to use.

### Step 5: Create a VPC to host the RHEL AI instance
{: #how-to-step-5}

Before you create a VSI instance with the RHEL AI custom image, you must set up a Virtual Private Cloud (VPC). The VPC provides the necessary networking environment for your instance.

To create a VPC, complete the following steps:
1. Click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) > **Infrastructure > Network > VPCs**.
1. Click **Create** and give the VPC instance a name. You can keep the remaining default selections.
1. Click **Create virtual private cloud**.

### Step 6: Host the RHEL AI instance
{: #how-to-step-6}

After you create a VPC, you can now provision a VSI with the RHEL AI custom image.

1. Click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) > **Infrastructure > Compute > Virtual server instances**.
1. Click **Create**.
1. Select a location and provide details, such as the name and resource group.
1. Go to the section **Server configuration**.
   1. Choose the custom image for RHEL AI by going to **Image** and clicking **Change image**.
   1. Click **Custom images** and select the custom image that you previously created.
   1. Choose the Instance Profile (GPU) in the **Profile** section by clicking **Change profile**.
   1. Select the **GPU** category.
   1. Select profile **gx3-48x240x2l40s**.
   1. Select or create an SSH key and store the private key. You use the private key to access the VSI after creation.
1.  Go to the Storage section and increase the size of the boot volume to 250 GB by clicking the **Edit** icon ![Edit icon](../icons/edit-tagging.svg "Edit").
1. Click **Create virtual server**.

### Step 7: Verify that the VSI is running
{: #how-to-step-7}

Make sure to verify that the VSI instance is up and running.

1. Click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) > **Infrastructure > Compute > Virtual server instances**.
1. Make sure that the VSI instance has the status **Running**.

   If the state of the VSI is **Pending**, wait to continue.
   {: note}

### Step 8: Set up a floating IP to access the VSI with SSH
{: #how-to-step-8}

To access the VSI instance after it finishes provisioning, you must have a floating IP, also known as a public IP address. This way, you can access the VSI over the internet. To attach a floating IP, complete the following steps:

1. Click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) > **Infrastructure > Compute > Virtual server instances** and click on your VSI.
1. On the **Networking** tab, click the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"). and select **Edit floating IPs**.
1. Click **Attach**.
1. Reserve a new floating IP or select an existing one and click **Attach**.
1. The VSI now shows the floating IP attached to it.

   Make note of this floating IP. It’s the public address that you use to connect to your VSI over SSH.
   {: tip}

1. Exit to go back to the VSI **Networking** tab.

### Step 9: Enable SSH port in security groups and network access control lists
{: #how-to-step-9}

After you provision the floating IP, you will require network access to the VSI instance. Security groups and network access control lists (NACLs) attached to the VSI instance have the access controls, also known as inbound and outbound rules. Create inbound rules and outbound rules that allow users to SSH into the VSI instance to run and chat with models. For a higher level of security, allow only your IP address or CIDR address range to access the VSI instance.

1. On the VSI **Networking** tab, click the number that is listed in the Security groups column.
1. Click the name of the security group and go to **Rules**.
1. Click **Create** to add an inbound rule to allow SSH port 22.
1. Select **TPC** and **Port range**. Then, enter `22` for the Port min and Port max.
1. (Optional) You can select Source type **IP address** or **CIDR block** to allow only your IP address or CIDR address range to access the VSI instance.
1. Click **Create**.
1. Click **Infrastructure > Network > Access control lists**.
1. Click the name of the access control list. 
1. Add inbound and outbound rules to allow SSH port 22:
   1. In Inbound rules, click **Create**.
   1. Name the inbound rule `ssh`.
   1. Select **Allow** and **TCP**.
   1. In the Source section, select **Any** as the **Type**. Then, enter `22` for the Port min and Port max.
   1. In the Destination section, select **Any** as the **Type**. Then, enter `22` for the Port min and Port max.
   1. Click **Create**.
   1. In Outbound rules, click **Create**.
   1. Name the outbound rule `ssh`.
   1. Select **Allow** and **TCP**.
   1. In the Source section, select **Any** as the **Type**. Then, enter `22` for the Port min and Port max.
   1. In the Destination section, select **Any** as the **Type**. Then, enter `22` for the Port min and Port max.
   1. Click **Create**.

### Step 10: Log in to the RHEL AI instance
{: #how-to-step-10}

You are ready to log in to your VSI instance with the floating IP and your private key.

1. Make sure that limited access is set up on your local private key. Run this command before you attempt to SSH into your VSI:

   ```sh
   chmod 640 ~/.ssh/mysshkey.prv
   ```
   {: codeblock}

1. To log in to your VSI instance with the floating IP and private key, run the following command:

   ```sh
   ssh -i ~/.ssh/mysshkey.prv root@instanceIP
   ```
   {: codeblock}

## Running LLM foundation models on your VSI
{: #how-to-run-model}

Now that you have access to an RHEL AI instance on {{site.data.keyword.cloud_notm}}, you have an environment where you can run, train, and run inference on your models by using InstructLab. InstructLab is an open source project for enhancing large language models (LLMs) used in generative artificial intelligence (gen AI) applications. For more information, see [What is InstructLab?](https://www.redhat.com/en/topics/ai/what-is-instructlab){: external}.

The following steps describe how to configure InstructLab, download models, and run them on your RHEL AI instance.

### Step 1: Initialize ilab on your VSI
{: #how-to-run-step1}

First you must initialize ilab, the environment for running and fine-tuning LLMs on your VSI.

1. Initialize ilab using the `ilab config init` command. For more information, see the [Red Hat Enterprise Linux AI CLI reference](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_ai/1.3/html/cli_reference/cli_reference#rhelai_cli_ref){: external}.
1. Create the `insights-opt-out` file in the `/etc/ilab` directory:

    ```sh
    # cd /etc
    # mkdir ilab
    # cd ilab
    # echo >insights-opt-out

    # ls -l /etc/ilab
    total 4
    -rw-r--r--. 1 root root 1 Oct 14 15:16 insights-opt-out
    ```

    ```sh
    # cd /root
    ```

    ```sh
    # ls -asl
    total 8
    4 drwxr-xr-x. 2 root root 4096 Sep 19 20:05 .
    4 drwxr-xr-x. 8 root root 4096 Sep 19 20:04 ..

    ```

1. Check `ilab` CLI installation:

   ` ilab `

   If you see the following message, you must have the activation from your Red Hat account. For more information, see. [Creating and managing activation keys](https://docs.redhat.com/en/documentation/subscription_central/1-latest/html/getting_started_with_activation_keys_on_the_hybrid_cloud_console/assembly-creating-managing-activation-keys){: external}.

    ```sh
    [root@instructlab-rhel-ai ~]# ilab
    This host is not connected to Red Hat Insights.

    To connect this host to Red Hat Insights run the following command:

    `sudo rhc connect --organization <org_id> --activation-key <your_activation_key>`

    To generate an Activation Key:
    https://console.redhat.com/insights/connector/activation-keys (this page will also display your Organization ID).

    For more information on Red Hat Insights, visit:
    https://docs.redhat.com/en/documentation/subscription_central/1-latest/html/getting_started_with_activation_keys_on_the_hybrid_cloud_console/assembly-creating-managing-activation-keys

    You must have the activation from your Red Hat account. Please read the [documentation](https://docs.redhat.com/en/documentation/subscription_central/1-latest/html/getting_started_with_activation_keys_on_the_hybrid_cloud_console/assembly-creating-managing-activation-keys)

    From your RedHat account, gather the `organization-id` and the `activation-key`. For more information on the organization-id and activation-key, read the note below

    Connect the host to Red Hat Insights:

    `sudo rhc connect --organization <organization-id> --activation-key <activation-key>`

    ```
    {: codeblock}

1. Configure `ilab` as shown in the following example:


   ```sh
   # ilab config init
   ```

    ```sh
    Welcome to InstructLab CLI. This guide will help you to setup your environment.
    Please provide the following values to initiate the environment [press Enter for defaults]:
    Cloning https://github.com/instructlab/taxonomy.git...
    Generating `/mnt/djblock/demo/.config/instructlab/config.yaml`...
    Please choose a train profile to use:
    [0] No profile (CPU-only)
    [1] A100_H100_x2.yaml
    [2] A100_H100_x4.yaml
    [3] A100_H100_x8.yaml
    [4] L40_x4.yaml
    [5] L40_x8.yaml
    [6] L4_x8.yaml
    Enter the number of your choice [hit enter for the default CPU-only profile] [0]: 0
    Using default CPU-only train profile.
    Initialization completed successfully, you're ready to start using `ilab`. Enjoy!
    ```

1. View the `.cache`, `.config`, and `.local` directories to see the contents.

    ```sh
    # ls -asl
    total 20
    4 drwxr-xr-x. 5 root root 4096 Sep 19 20:24 .
    4 drwxr-xr-x. 8 root root 4096 Sep 19 20:04 ..
    4 drwxr-xr-x. 3 root root 4096 Sep 19 20:24 .cache
    4 drwxr-xr-x. 3 root root 4096 Sep 19 20:24 .config
    4 drwxr-xr-x. 3 root root 4096 Sep 19 20:24 .local
    ```

### Step 2: Download the model
{: #how-to-run-step2}

Most open source models reside in the `huggingface` repository. You can download the models from the `huggingface` repository by using InstructLab.

1. View the contents of the `.cache` directory:

    ```sh
    # ls -LR .cache/instructlab
    .cache/instructlab:
    models  oci

    .cache/instructlab/models:

    .cache/instructlab/oci:
    ```

1. Download the model:

    ```sh
    # ilab model download --repository instructlab/granite-7b-lab --hf-token <your-huggingface-token>
    ```

    You might need to provide a `huggingface` token. Create an account in `huggingface` and create a token. For more information on how to create user access tokens, see [User access tokens](https://huggingface.co/docs/hub/en/security-tokens){: external}.
    {: note}

    ```sh
    Downloading model from Hugging Face: instructlab/granite-7b-lab@main to /mnt/djblock/demo/.cache/instructlab/models...
    Fetching 19 files:   0%|                                                                                                                  | 0/19 [00:00<?, ?it/s]
    Downloading 'added_tokens.json' to '/mnt/djblock/demo/.cache/instructlab/models/instructlab/granite-7b-lab/.cache/huggingface/download/added_tokens.json.ab9131bbe927da43320e49abbd99b99ef56a9c9b.incomplete'

    ...
    ```

1. List the model:

    ```sh
    # ilab model list
    +----------------------------+---------------------+---------+
    | Model Name                 | Last Modified       | Size    |
    +----------------------------+---------------------+---------+
    | instructlab/granite-7b-lab | 2024-09-19 20:39:03 | 12.6 GB |
    +----------------------------+---------------------+---------+
    ```

1. View the contents of the `.cache` directory:

    ```sh
    ls -LR .cache/instructlab
    .cache/instructlab:
    models  oci

    .cache/instructlab/models:
    instructlab

    .cache/instructlab/models/instructlab:
    granite-7b-lab

    .cache/instructlab/models/instructlab/granite-7b-lab:
    README.md          generation_config.json            model-00003-of-00003.safetensors  paper.pdf                tokenizer.model
    added_tokens.json  model-00001-of-00003.safetensors  model-card                        special_tokens_map.json  tokenizer_config.json
    config.json        model-00002-of-00003.safetensors  model.safetensors.index.json      tokenizer.json

    .cache/instructlab/models/instructlab/granite-7b-lab/model-card:
    'Model Card for Merlinite 7b 28cc0b72cf574a4a828140d3539ede4a_Screenshot_2024-02-22_at_11.26.13_AM.png'  'Model Card for Merlinite 7b 28cc0b72cf574a4a828140d3539ede4a_Untitled.png'
    'Model Card for Merlinite 7b 28cc0b72cf574a4a828140d3539ede4a_Untitled 1.png'                            'Model Card for Merlinite 7b 28cc0b72cf574a4a828140d3539ede4a_intuition.png'
    'Model Card for Merlinite 7b 28cc0b72cf574a4a828140d3539ede4a_Untitled 2.png'

    .cache/instructlab/oci:


    ```

### Step 3: Serve the model
{: #how-to-run-step3}

After you download the model, you can serve the model by using InstructLab. InstructLab loads the model and prepares it for use.

```sh
ilab -v model serve --model-path /root/.cache/instructlab/models/instructlab/granite-7b-lab  --backend vllm --gpus 2 -- --served-model-name hello1
```

```sh
...

INFO 09-19 20:46:19 api_server.py:257] Available routes are:
INFO 09-19 20:46:19 api_server.py:262] Route: /openapi.json, Methods: HEAD, GET
INFO 09-19 20:46:19 api_server.py:262] Route: /docs, Methods: HEAD, GET
INFO 09-19 20:46:19 api_server.py:262] Route: /docs/oauth2-redirect, Methods: HEAD, GET
INFO 09-19 20:46:19 api_server.py:262] Route: /redoc, Methods: HEAD, GET
INFO 09-19 20:46:19 api_server.py:262] Route: /health, Methods: GET
INFO 09-19 20:46:19 api_server.py:262] Route: /tokenize, Methods: POST
INFO 09-19 20:46:19 api_server.py:262] Route: /detokenize, Methods: POST
INFO 09-19 20:46:19 api_server.py:262] Route: /v1/models, Methods: GET
INFO 09-19 20:46:19 api_server.py:262] Route: /version, Methods: GET
INFO 09-19 20:46:19 api_server.py:262] Route: /v1/chat/completions, Methods: POST
INFO 09-19 20:46:19 api_server.py:262] Route: /v1/completions, Methods: POST
INFO 09-19 20:46:19 api_server.py:262] Route: /v1/embeddings, Methods: POST
INFO:     Started server process [34]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)

...

```

### Step 4: Test the model
{: #how-to-run-step4}

Use the following commands to verify whether the model is running:

```sh
# curl http://localhost:8000/version | jq .

{
  "version": "0.5.2.2"
}
```

```sh
# curl http://localhost:8000/v1/models | jq .

{
  "object": "list",
  "data": [
    {
      "id": "hello1",
      "object": "model",
      "created": 1726778941,
      "owned_by": "vllm",
      "root": "hello1",
      "parent": null,
      "max_model_len": 4096,
      "permission": [
        {
          "id": "modelperm-02142f12f187422c8b823a61312ed686",
          "object": "model_permission",
          "created": 1726778941,
          "allow_create_engine": false,
          "allow_sampling": true,
          "allow_logprobs": true,
          "allow_search_indices": false,
          "allow_view": true,
          "allow_fine_tuning": false,
          "organization": "*",
          "group": null,
          "is_blocking": false
        }
      ]
    }
  ]
}

```

### Step 5: Chat with the model
{: #how-to-run-step5}

InstructLab served the model and you are ready to chat with the models. Run the following command to get started:

```sh
# ilab model chat --endpoint-url http://127.0.0.1:8000/v1 --model hello1
```

```sh
╭─────────────────────────────────────────────────────────────────────────────────── system ────────────────────────────────────────────────────────────────────────────────────╮
│ Welcome to InstructLab Chat w/ HELLO1 (type /h for help)                                                                                                                      │
╰───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
>>> When did the Olympics began                                                                                                                                      [S][default]
╭─────────────────────────────────────────────────────────────────────────────────── hello1 ────────────────────────────────────────────────────────────────────────────────────╮
│ The Olympics have a rich history, dating back to ancient times. The first Olympic Games are believed to have taken place in 776 BC in Olympia, a sanctuary site in Greece.    │
│ These early games were part of the Panhellenic Games, a series of athletic festivals held in honor of the Greek god Zeus. The Olympics were initially an integral part of the │
│ religious cult, serving as a celebration of human strength, skill, and athletic prowess.                                                                                      │
│                                                                                                                                                                               │
│ The modern Olympics, as we know them today, were first held in 1896 in Athens, Greece. The founder of the modern Games was Pierre de Coubertin, a French educator and         │
│ historian. He aimed to promote peace, unity, and friendship among nations through the celebration of sports. The games were initially held every four years, but in 1932,     │
│ they were moved to an annual basis.                                                                                                                                           │
│                                                                                                                                                                               │
│ The Olympics have since evolved into a significant global event, featuring athletes from more than 200 countries. The games include a wide variety of sports and disciplines, │
│ from track and field and swimming to skiing and snowboarding. The Olympics have also become a platform for social change, with initiatives aimed at promoting gender          │
│ equality, environmental sustainability, and cultural exchange.                                                                                                                │
│                                                                                                                                                                               │
│ In conclusion, the Olympics have a history spanning over three millennia, and they continue to be a significant cultural and sporting event today. The games have evolved     │
│ over time, reflecting the changing values and priorities of society, but their core mission remains the same: to celebrate human achievement, promote peace, and unite people │
│ from all corners of the world.                                                                                                                                                │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── elapsed 4.443 seconds ─╯
>>>

```

## Conclusion
{: #conclusion}

You successfully deployed an RHEL AI instance on {{site.data.keyword.cloud_notm}} VPC and can chat with the model of your choice.
