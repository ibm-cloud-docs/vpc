---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-19"

keywords: host failure recovery, recovery

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Host failure recovery policies
{: #host-failure-recovery-policies}

If a host fails unexpectedly and cannot be recovered, virtual server instances on the failed host are automatically restarted on a healthy host. The restart policy can be configured to not restart the virtual server instance when you create an instance or on an existing instance.
{: shortdesc}

{{site.data.keyword.cloud}} uses continual monitoring procedures and operations that help prevent disruption to your workloads during unplanned host failures. {{site.data.keyword.cloud}} continuously monitors your infrastructure to make sure that all hosts are healthy and responsive. Detection of a host issue occurs within 30 seconds of occurrence. When a host failure is detected and the host cannot be immediately recovered, {{site.data.keyword.cloud}} initiates the selected failure recovery policy within 5 minutes for all virtual servers on the affected host.

## Available recovery policies
{: #available-policies}

The default setting for the host failure policy of an instance is 'restart'. The policy can be changed at any time without disrupting the instance and is only used when during recovery from a host failure event.

| Host failure policy | Policy description |
|---------|---------|
| restart | The instance is automatically restarted on another compute host |
| stop | The instance is not to be restarted on another compute host |
{: caption="Host failure recovery policies" caption-side="bottom"}

### Restart
{: #restart}

When the host failure policy is set to restart and the host fails, the instance is relocated to another available host and restarted.
The restarted instance uses the same boot volume and the same data volumes as the original instance. The restarted instance is assigned the same floating IP, static and dynamic IP addresses on the new host.

### Stop
{: #stop}

When the host failure policy is set to stop when the host fails, the instance is not restarted. The status of the instance is changed to stopped. The user can choose to restart the stopped instance by issuing an instance start action. When restarted, the instance is placed on an available host.

## Setting the recovery policy in the console
{: #set-policy-ui}
{: ui}

-  To set the failure recovery policy in the console during instance provisioning, find the Advanced options on the provisioning page.
   1. In the Advanced options, find 'Host failure auto restart'. This option can be toggled or off.

- To set the failure recovery policy for an existing instance, complete the following steps.
   1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com){: external}, click the **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
   2. On the **Virtual server instances** page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the instance that you want to manage.
   3. From the instance details page, locate 'Host failure auto restart'. Click the pencil icon and choose Enabled or Disabled to toggle the status of the host recovery policy on or off.

For more information, see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers) and [Managing virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui).

## Setting the recovery policy from the CLI
{: #set-policy-cli}
{: cli}

During instance create or patch, the following attributes can be used to set the host failure policy.

For more information, see [Creating virtual server instances by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli) and [Managing virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=cli).

| Host failure policy | Policy attribute |
|---------|---------|
| restart | `--host-failure-policy restart` |
| stop | `--host-failure-policy stop` |
{: caption="Recovery policy attributes" caption-side="bottom"}

### Creating an instance with host failure policy
{: #create-instance-policy}

You can create an instance in your IBM Cloud VPC and change the availability policy on host failure by using the command-line interface (CLI). Run the ibmcloud is instance-create command and set the `--host-failure-policy` property to `restart` or `stop`. The host failure policy service is set to `restart` by default.

```sh
ibmcloud is inc test r006-a0162c41-6a75-4a04-aabb-da1c78539531 us-south-2  bx2-2x8  7284-47efd8c6-0efc-462e-89c0-e0457119f90b --image r006-63363662-a4ee-4ba4-a6c4-92e6c78c6b58 --host-failure-policy stop
Creating instance test under account VPC1 as user myuser@mycompany.com...

ID                                    7284_683902df-85ce-4546-808c-3675247074d8
Name                                  test
CRN                                   crn:v1:bluemix:public:is:us-south-2:a/a1234567::instance:7284_683902df-85ce-4546-808c-3675247074d8
Status                                pending
Availability policy on host failure   stop
Startable                             true
Profile                               bx2-2x8
Architecture                          amd64
vCPU Manufacturer                     Intel
vCPUs                                 2
Memory(GiB)                           8
Bandwidth(Mbps)                       4000
Image                                 ID                                          Name
                                      r006-63363662-a4ee-4ba4-a6c4-92e6c78c6b58   ibm-centos-7-9-minimal-amd64-3

VPC                                   ID                                          Name
                                      r006-a0162c41-6a75-4a04-aabb-da1c78539531   cli-vpc-1

Zone                                  us-south-2
Resource group                        ID                                 Name
                                      11caaa983d9c4beb82690daab08717e9   Default

Created                               2021-10-25T16:39:30+05:30
Boot volume                           ID   Name           Attachment ID                               Attachment name
                                      -    PROVISIONING   7284-69923add-65e2-4b93-bee4-a4bca3836696   collector-reverb-exiting-swinging
```
{: screen}

### Updating an instance with host failure policy
{: #update-instance-policy}

You can update an instance in your IBM Cloud VPC with and change the availability policy on host failure by using the command-line interface (CLI). Run the ibmcloud is instance-update command and set the `--host-failure-policy` property to `start` or `stop`. The host failure policy service is set to `restart` by default.

```sh
ibmcloud is inu 7284_683902df-85ce-4546-808c-3675247074d8 --host-failure-policy restart
Updating instance 7284_683902df-85ce-4546-808c-3675247074d8 under account VPC1 as user myuser@mycompany.com...

ID                                    7284_683902df-85ce-4546-808c-3675247074d8
Name                                  test
CRN                                   crn:v1:bluemix:public:is:us-south-2:a/a1234567::instance:7284_683902df-85ce-4546-808c-3675247074d8
Status                                running
Availability policy on host failure   restart
Startable                             true
Profile                               bx2-2x8
Architecture                          amd64
vCPU Manufacturer                     Intel
vCPUs                                 2
Memory(GiB)                           8
Bandwidth(Mbps)                       4000
Image                                 ID                                          Name
                                      r006-63363662-a4ee-4ba4-a6c4-92e6c78c6b58   ibm-centos-7-9-minimal-amd64-3

VPC                                   ID                                          Name
                                      r006-a0162c41-6a75-4a04-aabb-da1c78539531   cli-vpc-1

Zone                                  us-south-2
Resource group                        ID                                 Name
                                      11caaa983d9c4beb82690daab08717e9   Default

Created                               2021-10-25T16:39:30+05:30
Boot volume                           ID                                          Name                            Attachment ID                               Attachment name
                                      r006-780e6d41-b8c0-4023-b81f-2dcabf0b834f   aardvark-matrix-tidy-fragment   7284-69923add-65e2-4b93-bee4-a4bca3836696   collector-reverb-exiting-swinging
```
{: screen}

## Setting the recovery policy with the API
{: #set-recovery-policy-api}
{: api}

During the instance [create](/apidocs/vpc/latest#create-instance) or [update](/apidocs/vpc/latest#update-instance) operations, the `host_failure` subproperty can be used to set the host failure `availability_policy` of the virtual server instance. If the compute host experiences a failure, specify `restart` or `stop` to set the policy.

| Host failure policy | Attribute  |
|---------|---------|
| restart | 'restart' |
| stop | 'stop' |
{: caption="Recovery policy API " caption-side="bottom"}

For more information, see [Create an instance](/apidocs/vpc/latest#create-instance) and [Managing virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=api).

## Next steps
{: #host-fail-next-steps}

For more information about planned and unplanned host outages, see the FAQ [In what cases is my virtual server migrated to a different host?](/docs/vpc?topic=vpc-faqs-for-vsis#faq-vsi-13)
