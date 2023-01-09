---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-22"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Migrating from the Beta release
{: #vpn-client-to-site-migration}

The Client VPN for VPC service is now Generally Available (GA). Beta participants must migrate from Beta to the GA version before 30 September 2022.
{: shortdesc}

After 30 September 2022, any remaining VPN server instances in the Beta plan will be removed.
{: important}

* For information about Client VPN for VPC GA pricing, see [IBM Cloud VPC Pricing](https://www.ibm.com/cloud/vpc/pricing).
* For instructions about using the command line interface, see [Getting started with the IBM Cloud CLI](/docs?tab=develop).

To migrate existing VPN servers, follow these steps:

1. Review the current pricing plan. To do so, enter the following command:

   ```sh
   ibmcloud resource service-instance <your-vpnserver-name>
   ```

   Where `your-vpnserver-name` is the name of your VPN server.

1. Migrate to the official pricing plan:

   ```sh
   ibmcloud resource service-instance-update <your-vpnserver-name> --service-plan-id  2e806cd9-533f-4713-9d32-7ecd97d9274d
   ```

   Where `your-vpnserver-name`  is the name of your VPN server and  `2e806cd9-533f-4713-9d32-7ecd97d9274d` is the plan ID.

1. After migration, re-check your current pricing plan:

   ```sh
   ibmcloud resource service-instance <your-vpnserver-name>
   ```
   Where `your-vpnserver-name`  is the name of your VPN server.

## Example output
{: #vpn-example-output}

```json
test@abc:/root$  ibmcloud resource service-instance demo-plan-migration
Retrieving service instance demo-plan-migration in all resource groups under account CNS Development Account - netsvs as abc@ibm.com...
OK

Name:                  demo-plan-migration
ID:                    crn:v1:bluemix:public:is:us-south:a/be636a7a6e4d4b6296bedf669ce8f757::vpn-server:r006-b271dab4-51a7-4378-be7d-e6ac1b44ddd4
GUID:                  crn:v1:bluemix:public:is:us-south:a/be636a7a6e4d4b6296bedf669ce8f757::vpn-server:r006-b271dab4-51a7-4378-be7d-e6ac1b44ddd4
Location:              us-south
Service Name:          is.vpn-server
Service Plan Name:     gen2-vpn-server-beta
Resource Group Name:   Default
State:                 active
Type:                  service_instance
Sub Type:
Locked:                false
Created at:            2022-07-14T09:43:47Z
Created by:            abc@ibm.com
Updated at:            2022-07-14T09:45:46Z
Last Operation:
                       Status    create succeeded
                       Message   Instance provisioning is completed.

test@abc:/root$ ibmcloud resource service-instance-update demo-plan-migration --service-plan-id  2e806cd9-533f-4713-9d32-7ecd97d9274d
Updating service instance demo-plan-migration in all resource groups under account CNS Development Account - netsvs as abc@ibm.com...
OK
Service instance demo-plan-migration with ID crn:v1:bluemix:public:is:us-south:a/be636a7a6e4d4b6296bedf669ce8f757::vpn-server:r006-b271dab4-51a7-4378-be7d-e6ac1b44ddd4 is updated successfully


Name:             demo-plan-migration
ID:               crn:v1:bluemix:public:is:us-south:a/be636a7a6e4d4b6296bedf669ce8f757::vpn-server:r006-b271dab4-51a7-4378-be7d-e6ac1b44ddd4
GUID:             crn:v1:bluemix:public:is:us-south:a/be636a7a6e4d4b6296bedf669ce8f757::vpn-server:r006-b271dab4-51a7-4378-be7d-e6ac1b44ddd4
Location:         us-south
State:            active
Type:             service_instance
Sub Type:
Allow Cleanup:    false
Locked:           false
Created at:       2022-07-14T09:43:47Z
Updated at:       2022-07-15T02:10:12Z
Last Operation:
                  Status    update succeeded
                  Message   Completed update instance operation

test@abc:/root$ ibmcloud resource service-instance demo-plan-migration
Retrieving service instance demo-plan-migration in all resource groups under account CNS Development Account - netsvs as abc@ibm.com...
OK

Name:                  demo-plan-migration
ID:                    crn:v1:bluemix:public:is:us-south:a/be636a7a6e4d4b6296bedf669ce8f757::vpn-server:r006-b271dab4-51a7-4378-be7d-e6ac1b44ddd4
GUID:                  crn:v1:bluemix:public:is:us-south:a/be636a7a6e4d4b6296bedf669ce8f757::vpn-server:r006-b271dab4-51a7-4378-be7d-e6ac1b44ddd4
Location:              us-south
Service Name:          is.vpn-server
Service Plan Name:     gen2-vpn-server
Resource Group Name:   Default
State:                 active
Type:                  service_instance
Sub Type:
Locked:                false
Created at:            2022-07-14T09:43:47Z
Created by:            abc@ibm.com
Updated at:            2022-07-15T02:10:12Z
Last Operation:
                       Status    update succeeded
                       Message   Completed update instance operation
```
{: screen}
