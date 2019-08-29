---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-29"

keywords: vpc, setup, environment, prerequisites, api, cli, command line interface, plugin, creating a vpc, iam, permissions, access, ssh key

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Setting up your API and CLI environment
{: #set-up-environment}

Before you can create a VPC using the API or CLI, set up your environment.
{:shortdesc}


## General prerequisites
{: #general-prerequisites}

1. Set up your account to access VPC (early access). Make sure your account is [upgraded to a paid account](/docs/account?topic=account-accountfaqs#changeacct){: new_window}. 
2. Make sure you have a public SSH key, which will be used to connect to the virtual server instance. For example, generate an SSH key on your Linux server by running the following command:

    ```
    ssh-keygen -t rsa
    ``` 
    {: pre}

   This command generates two files. The generated public key is in the `id_rsa.pub` file under an ``.ssh`` directory in your home directory, for example, ``.../.ssh/id_rsa.pub``.

## CLI prerequisites
{: #cli-prerequisites-setup}

Before you can use the CLI to create your VPC, you must install the IBM Cloud CLI and the VPC CLI plugin.

1. Install the [IBM Cloud CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/cli){: new_window}.
1. Install the VPC CLI plugin.

   ```
   ibmcloud plugin install vpc-infrastructure
   ```
  {: pre}

The VPC CLI actions use the extension `is`. To learn how to use the CLI commands, you can run:
    
```
ibmcloud is help
ibmcloud is help vpc-create
ibmcloud is help instance-create
```

To learn how to create resources using the CLI, see [Creating a VPC using the CLI](/docs/vpc?topic=vpc-creating-a-vpc-using-cli).

## API prerequisites
{: #api-prerequisites-setup}

Before you can use the API to create your VPC, you must get an IAM token, store the endpoint as a variable, and verify that you have access to the VPC API service. 

### Step 1: Store your API key as a variable
{: #store-api-key-variable}

Run the following command to store the API key for your account in an environment variable, If you don't have an API key, see [Creating an API key](/docs/iam?topic=iam-userapikey#create_user_key){: new_window}.

```bash
apikey="<YOUR_API_KEY>"
```
{: pre} 

### Step 2: Get an IBM Identity and Access Management (IAM) token
{: #get-iam-token}

Run the following command to get and parse an IAM token using the utility `jq`. You can modify the command to use another parsing tool, or you can remove the last part of the command if you prefer to manually parse the token.

```
iam_token=`curl -k -X POST \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --header "Accept: application/json" \
  --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
  --data-urlencode "apikey=$apikey" \
  "https://iam.bluemix.net/identity/token"  |jq -r '(.token_type + " " + .access_token)'`
```
{: pre}

To view the IAM token, run ``echo $iam_token``. The result should look like this:

```
Bearer <your token> 
```

The Authorization header expects the token to begin with "Bearer". If the result above doesn't include "Bearer", update the `iam_token` variable to include it. These examples assume that "Bearer" is included in the `iam_token`.

You must repeat the preceding step to refresh your IAM token every hour, because the token expires.
{: important}

### Step 3: Store the API endpoint as a variable
{: #store-api-endpoint-variable}

Run the following command to store the API endpoint in a variable so it can be reused later in your session. For `api_endpoint`, use the API endpoint given to you during on-boarding.

```
api_endpoint="<VPC_API_ENDPOINT>"
 ```
{: pre}

To verify that this variable was saved, run ``echo $api_endpoint`` and make sure the response is not empty.

### Step 4: Store the API version as a variable
{: #store-api-version-variable}


Every API request must include the `version` parameter, in the format `YYYY-MM-DD`. Run the following command to store the version date in a variable so it can be reused in your session. For more information about setting the `version` parameter, see **Versioning** in the [Virtual Private Cloud API (Early Access)](https://{DomainName}/apidocs/vpc#versioning) 

```
api_version="2019-05-31"
 ```
{: pre}

To verify that this variable was saved, run ``echo $api_version`` and make sure the response is not empty.

### Step 5: Verify you have API access
{: #verify-api-access}

If you run into unexpected results, add the `--verbose` (debug) flag after the `curl` command to obtain detailed logging information. You can also refer to   [Troubleshooting](/docs/vpc?topic=vpc-troubleshooting-vpc) for information about  commonly encountered errors.
{: tip}

 * Call the GET Regions API to see the regions available for VPC, in JSON format. At least one object should return.
  
  Send the `generation` parameter with every API request to specify which generation of infrastructure to use. For VPC (early access), specify `generation=2`. For more information, see **Generation** in the [Virtual Private Cloud API (Early Access)](https://{DomainName}/apidocs/vpc#api-generation-parameter) 
  {: tip}

    ```
    curl -X GET "$api_endpoint/v1/regions?version=$api_version&generation=2" \
      -H "Authorization: $iam_token"
    ```
{: pre}

 * Call the GET Zones API to see all zones available for VPC in a particular region, such as `us-south`, in JSON format.

    ```
    curl -X GET "$api_endpoint/v1/regions/us-south/zones?version=$api_version&generation=2" \
      -H "Authorization: $iam_token"
    ```
{: pre}

 * Call the GET Profiles API to see the profiles available for your virtual server instances, in JSON format. At least one object should return.

  Add ` | json_pp ` after the curl command to get a readable JSON string.
  {: tip}

    ```
    curl -X GET "$api_endpoint/v1/instance/profiles?version=$api_version&generation=2" \
      -H "Authorization: $iam_token"
    ```
{: pre}

 * Call the GET Images API to return the images available for your instances, in JSON format. At least one object should return.
 
   ```
   curl -X GET "$api_endpoint/v1/images?version=$api_version&generation=2" \
     -H "Authorization: $iam_token"
   ```
{: pre}

 * Call the GET VPCs API to see any VPCs that were already created under your account, in JSON format.

    ```
    curl -X GET "$api_endpoint/v1/vpcs?version=$api_version&generation=2" \
      -H "Authorization: $iam_token"
    ```
{: pre}

To learn how to create resources using the API, see [Creating a VPC using the REST APIs](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis).
