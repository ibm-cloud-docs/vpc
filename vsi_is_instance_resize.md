---

copyright:
  years: 2021
lastupdated: "2021-08-31"

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:beta: .beta}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:terraform: .ph data-hd-interface='terraform'}


# Resizing a virtual server instance
{: #resizing-an-instance}

You can resize your virtual server instance and vertically scale to any supported profile size in minutes. You can increase or decrease the amount of vCPU and RAM available for greater flexibility in workload management to address resource requirement changes, optimize cost or workload performance.
{:shortdesc}

Virtual servers are configured by using profiles, or a combination of instance attributes, such as the number of vCPUs, amount of RAM, network bandwidth, and more that define the size and capabilities of the virtual server instance.
When you upgrade or downgrade an existing server, you choose another profile that has the pre-defined specifications that you need. You cannot customize the configuration of a virtual server. The virtual server profile that you select determines the valid cores, RAM, bandwidth, and disk sizes on the resized instance. For more information about profiles, see [Instance Profiles](/docs/vpc?topic=vpc-profiles).

When you resize an instance, keep the following information in mind:
* You need to stop, update, and start the instance that you want to resize.
* Data isn't deleted from the primary volume or the data volume.
* RAM is wiped from the resized instance.
* All network configurations are maintained, such as private IPs, floating IPs, vNICs, and security groups.
* The instance name doesn't change.
* The data center location doesn't change.
* An instance with a profile that does not include instance storage cannot be resized to a profile that does include instance storage. 

After the instance is resized, you are billed the hourly rate of the new instance profile.

You can track the instance resize in Activity Tracker and {{site.data.keyword.la_full}} for troubleshooting and audit purposes.

## Resizing virtual servers on dedicated hosts
{: #resizing-dedicated-virtual-servers}

Virtual servers that run on dedicated hosts can be resized only to profiles that are supported by the dedicated host that the instance is hosted on. For example, a virtual server that is provisioned with a profile from the Memory family can resize to other profiles that belong to the Memory family.

Resizing virtual servers on dedicated hosts is not supported for LinuxONE (s390x processor architecture).  
{: note}

## Resizing virtual servers with instance storage
{: #resizing-with-instance-storage}

When you stop a virtual server instance with an instance storage profile, that storage is temporary storage and is available only while your virtual server is running. Data on the drive is unrecovered after the instance stops.

## Resizing virtual servers with data volumes
{: #resizing-with-data-volumes}

Attached data volumes remain intact and are attached in the resized instance.

## Resizing instances associated with instance templates and instance groups
{: #resizing-instance-templates-groups}  

When you resize an instance that's provisioned from an instance template or that's provisioned as part of an instance group, the following rules apply. 

* An instance that is provisioned from an instance template can be resized with a new instance profile. 
* Instance templates are not editable, except for the name. You cannot update an instance profile within an instance template. To choose a different profile for an instance template, you must create a new template.
* Resizing an instance that is part of an instance group removes it from the instance group. An instance must be stopped to resize it. When the instance is stopped, the instance group replaces it with a new instance with the same profile as the instance template describes. 

## Resizing a virtual server instance using the UI
{: #resizing-a-virtual-server-UI}
{: ui}

Complete the following steps to resize an existing virtual server instance.

1. From the **IBM Cloud Console** menu, select **Virtual server instances**.
2. From the **Virtual server instances for VPC** list, find the virtual server that you want to resize and verify that its status is Stopped or Stopping.
3. Select the vertical ellipsis, and select **Resize**.
4. From the list of available profiles, select the profile that you want to use.
    * If you are resizing a virtual server that is running on a dedicated host, you see only profiles that the dedicated host supports.
    * If you are resizing a profile that uses instance storage, you see only profiles that have instance profiles. You can't resize from a virtual server instance that has an instance storage profile to a profile that doesn't have instance storage.
5. Review and check the Terms and Conditions.
6. Select **Resize virtual server instance**.
7. Start the virtual server instance.

## Resizing a virtual server by using the CLI
{: #resizing-a-virtual-server-CLI}
{: cli}

Use the `instance-update` command to resize a virtual server.

```
ibmcloud is instance-update instance-id --profile profile-id  
```
{: pre}

Where:
* `instance-id` is the ID of the instance that you want to resize
* `profile-id` is the ID of the profile that you want to use

As an example, if you want to resize an instance to the _bx2-16x64_ profile, the command would look similar to the following command.

```
ibmcloud is instance-update 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --profile bx2-16x64
```
{: pre}

## Resizing a virtual server by using the API
{: #resizing-a-virtual-server-API}
{: api}

Use the `instance-update` command to resize a virtual server.

1. Run the following command to find the name of the profile you want to use:
   ```sh
   curl  -s -X GET "<api_endpoint>/v1/instance/profiles?generation=2&version=2021-02-01" -H "Authorization: Bearer <IAM token>"
   ```
   {:pre}
   
2. Select a compatible profile for your instance.
    * For a virtual server that is running on a dedicated host, choose a profile that the dedicated host supports.
    * If you use instance storage, choose a profile that has instance storage.
    * For data volumes, choose a profile that has data volumes.
3. Run the following command:
   ```sh
   curl -k -sS -X PATCH "<api_endpoint>/v1/instances/<instance id>?generation=2&version=2021-02-01" \
       -H "Authorization: Bearer <IAM token>" \
       -d '
   {
       "profile": {
          "name": "<new profile>"
       }
   } '
   ```
   {: pre}
   
   Where:
      * `instance-id` is the ID of the instance that you want to resize
      * `profile-id` is the ID of the profile that you want to use
