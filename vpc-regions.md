---

copyright:
  years: 2020, 2025
lastupdated: "2025-12-21"

keywords: region, zone, deploy, datacenter, data, center, federated, CLI, API, account, failover, disaster, recovery, DR, data center

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a VPC in a different region
{: #creating-a-vpc-in-a-different-region}

A region is a specific geographical location where you can deploy apps, services, and other {{site.data.keyword.cloud}} resources. Regions consist of one or more zones, which are physical data centers that house the compute, network, and storage resources, with related cooling and power, for host services and applications. Zones are isolated from each other, which makes sure that no shared single point of failure within a region occurs.

The {{site.data.keyword.cloud}} VPC service is regional. To allow for disaster recovery (DR), you must provide the ability for recovery by using failover to an alternative region, by establishing a VPC in one of our other regions.
{: note}

Virtual Private Cloud is available in the following {{site.data.keyword.cloud}} regions.

|   Location              | Region     | API Endpoint |
|-------------------------|:----------:|:------------:|
| US South (Dallas)       | `us-south` | `us-south.iaas.cloud.ibm.com`|
| US East (Washington DC) | `us-east`  | `us-east.iaas.cloud.ibm.com`|
| Brazil (São Paulo)      | `br-sao`   | `br-sao.iaas.cloud.ibm.com` |
| Canada (Toronto)        | `ca-tor`   | `ca-tor.iaas.cloud.ibm.com` |
| Canada (Montreal)       | `ca-mon`   | `ca-mon.iaas.cloud.ibm.com` |
{: class="simple-tab-table"}
{: tab-title="Americas"}
{: caption="IBM Cloud regions for North and South America" caption-side="bottom"}
{: summary="This table displays the IBM Cloud regions and is organized by geographical location. Click on the tab for the geographical location you are interested in. You then see all the IBM Cloud regions for that geographical location."}
{: tab-group="vpc-regions-api-endpoint"}
{: #vpc-north-america-regions}

|   Location              | Region  | API Endpoint |
|-------------------------|:-------:|:------------:|
| United Kingdom (London) | `eu-gb` | `eu-gb.iaas.cloud.ibm.com`|
| Germany (Frankfurt)     | `eu-de` | `eu-de.iaas.cloud.ibm.com`|
| Spain (Madrid)          | `eu-es` | `eu-es.iaas.cloud.ibm.com` |
{: class="simple-tab-table"}
{: tab-title="Europe"}
{: caption="IBM Cloud regions for Europe" caption-side="bottom"}
{: summary="This table displays the IBM Cloud regions and is organized by geographical location. Click on the tab for the geographical location you are interested in. You then see all the IBM Cloud regions for that geographical location."}
{: tab-group="vpc-regions-api-endpoint"}
{: #vpc-europe-regions}

For x86-64 dedicated host profiles, the Madrid region supports only dedicated host profiles with instance storage. For more information, see [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui).
{: important}

|   Location         | Region   | API Endpoint |
|--------------------|:--------:|:------------:|
| India (Chennai)    | `in-che` | `in-che.iaas.cloud.ibm.com` |
| Japan (Tokyo)      | `jp-tok` | `jp-tok.iaas.cloud.ibm.com` |
| Japan (Osaka)      | `jp-osa` | `jp-osa.iaas.cloud.ibm.com` |
| Australia (Sydney) | `au-syd` | `au-syd.iaas.cloud.ibm.com` |
{: class="simple-tab-table"}
{: tab-title="Asia Pacific"}
{: caption="IBM Cloud regions for Asia Pacific" caption-side="bottom"}
{: summary="This table displays the IBM Cloud regions and is organized by geographical location. Click on the tab for the geographical location you are interested in. You then see all the IBM Cloud regions for that geographical location."}
{: tab-group="vpc-regions-api-endpoint"}
{: #vpc-asia-pacific-regions}

The Regional API (VPC) endpoint is automatically set by the {{site.data.keyword.cloud}} CLI when you log in to a specific region.
{: note}

## Logging in to a specific region by using the CLI
{: #log-in-to-a-specific-region-using-the-cli}
{: cli}

When you log in to {{site.data.keyword.cloud}}, you can specify a region or choose it later. For example, to log in to the global API endpoint in the Dallas (`us-south`) region directly, run the following commands. These commands vary according to whether you have a federated account (SSO) or a nonfederated account.

For a federated account,

```sh
ibmcloud login -a https://cloud.ibm.com -r us-south --sso
```
{: pre}

For a nonfederated account,

```sh
ibmcloud login -a https://cloud.ibm.com -r us-south
```
{: pre}

If you do not specify the `-r <region>` parameter, the CLI prompts you to choose a region.

Example output:

```json
API endpoint: cloud.ibm.com

Get One Time Code from https://identity-2.eu-central.iam.cloud.ibm.com/identity/passcode to proceed.
Open the URL in the default browser? [Y/n]> y
One Time Code >
Authenticating...
OK

Select an account:
1. MyAccount (00a11aa1a11aa11a1111a1111aaa11aa) <-> 1234567
2. TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321
Enter a number> 2
Targeted account TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321


Targeted resource group Default

Select a region (or press enter to skip):
1. au-syd
2. in-che
3. jp-osa
4. jp-tok
5. kr-seo
6. eu-de
7. eu-es
8. eu-gb
9. ca-tor
10. us-south
11. us-east
12. br-sao

Enter a number> 12

Targeted region us-south


API endpoint:      https://cloud.ibm.com
Region:            us-south
User:              first.last@email.com
Account:           TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321
Resource group:    Default
CF API endpoint:
Org:
Space:

...
```
{: screen}

## Switch regions by using the CLI
{: #switch-regions-using-the-cli}
{: cli}

To get the most recent status of the VPC regions, run the following command:

```sh
ibmcloud is regions
```
{: pre}



To switch to a different region, run the `ibmcloud target -r <region>` command. For example, to switch to the Washington DC region, run the following command:

```sh
ibmcloud target -r us-east
```
{: pre}

To check your current location, run the following command:

```sh
ibmcloud target
```
{: pre}

## Switch regions by using the API
{: #switch-regions-using-the-api}
{: api}

To interact with the Regional VPC API by using REST, direct your request to the API endpoint that is associated with the region in which you want to create resources. The region's API endpoint is shown in the previous table. You can also find the endpoints that are associated with the regions by running the following command:

```sh
ibmcloud is regions
```
{: pre}



For example, to get the list of VPCs in the `us-south` region, run the following command:

```sh
curl "https://us-south.iaas.cloud.ibm.com/v1/vpcs?version=$api_version&generation=2" -H "Authorization: $iam_token"
```
{: pre}


## Get zones by using the CLI
{: #get-zones-using-the-cli}
{: cli}

To get the list of zones available for each region, run the command `ibmcloud is zones` in the target region. In the following example, the first command switches the target to `us-south` region. The second command lists the available zones.

```sh
ibmcloud target -r us-east; ibmcloud is zones
Switched to region us-east

API endpoint:      https://cloud.ibm.com
Region:            us-east
User:              test.user@ibm.com
Account:           Test Account (a1234567) <-> 1414935
Resource group:    No resource group targeted, use 'ibmcloud target -g RESOURCE_GROUP'
CF API endpoint:
Org:
Space:
Listing zones in target region us-east under account Test Account as user test.user@ibm.com...
Name         Universal name      Data center    Region      Status
us-south-1   us-south-dal13-a    DAL13          us-south    available
us-south-2   us-south-dal14-a    DAL14          us-south    available
us-south-3   us-south-dal10-a    DAL10          us-south    available
```
{: pre}



For more information about available command options, see [`ibmcloud is zones`](/docs/vpc?topic=vpc-vpc-reference&interface=cli#zones-list){: external}.
