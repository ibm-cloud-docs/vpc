---

copyright:
  years: 2022
lastupdated: "2022-11-04"

keywords: question about Red Hat Enterprise Linux virtual server instance in failed state created from classic infrastructure custom image

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my RHEL virtual server created from a Classic Infrastructure custom image in failed state?
{: #troubleshoot-rhel-image-from-classic-infra}
{: troubleshoot}
{: support}

After importing a Red Hat Enterprise Linux custom image from {{site.data.keyword.cloud}} Classic Infrastructure to {{site.data.keyword.vpc_short}}, you use it to create a new virtual server instance in VPC. However, the new virtual server instance remains in failed state.
{: shortdesc}

You use a Red Hat Enterprise Linux custom image from {{site.data.keyword.cloud_notm}} Classic Infrastructure that was imported to {{site.data.keyword.vpc_short}} to provision a new virtual server instance in VPC. However, the new virtual server instance does not start successfully; it is in a failed state.
{: tsSymptoms}

When the new VPC virtual server that was created from a Classic Infrastructure custom image attempts to start, the Red Hat Enterprise Linux license activation fails.
{: tsCauses}

To resolve the licensing issue for the Red Hat Enterprise Linux image in the VPC environment, you must log in to the failed virtual server, rearrange the network routes, and re-register the operating system. When the licensing issue is fixed, the virtual server instance should start successfully.
{: tsResolve}

To complete these steps, you need to use the default user account. The default user account for Linux depends on the operating system. To see the list of operating systems and their corresponding default user accounts, see [Connecting to Linux instances: Determining the default user account](/docs/vpc?topic=vpc-vsi_is_connecting_linux&interface=ui#determining-default-user-account).

1. Log in to the failed virtual server instance by using SSH. Locate the fabric IP address for the virtual server instance in the {{site.data.keyword.cloud_notm}} console on the Virtual server instance page. Log in to the failed virtual server instance by using SSH in a terminal. The following example shows how to log in to the virtual server instance from a terminal by using SSH.

   ```text
   ~ % ssh -i .ssh/id_rsa_vsikey1 <default-user-account>@52.116.206.42
   Warning: Permanently added '52.116.206.42' (ECDSA) to the list of known hosts.
   [root@skv-vsirh2x7 ~]#
   ```
   {: screen}

2. Rearrange the network routes for the Red Hat Enterprise Linux operating system that is in the failed state.

    1. Determine the right gateway from the ip/route table. Use the ip show/ route command to get the entire list of routes that are present. Use the ping command to determine the right gateway. See the following example:

       ```text
       [root@skv-vsic1x7im ~]# route -n
       Kernel IP routing table
       Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
       0.0.0.0         10.240.66.1     0.0.0.0         UG    0      0        0 eth0
       0.0.0.0         10.240.66.1     0.0.0.0         UG    100    0        0 eth0
       10.0.0.0        10.187.149.65   255.0.0.0       UG    100    0        0 eth0
       10.187.149.65   0.0.0.0         255.255.255.255 UH    100    0        0 eth0
       10.240.66.0     0.0.0.0         255.255.255.0   U     100    0        0 eth0
       161.26.0.0      10.187.149.65   255.255.0.0     UG    100    0        0 eth0
       166.8.0.0       10.187.149.65   255.252.0.0     UG    100    0        0 eth0
       ```
       {: screen}

       ```text
       [root@skv-vsic1x7im ~]# ping 10.240.66.1
       PING 10.240.66.1 (10.240.66.1) 56(84) bytes of data.
       64 bytes from 10.240.66.1: icmp_seq=1 ttl=64 time=0.183 ms
       64 bytes from 10.240.66.1: icmp_seq=2 ttl=64 time=0.176 ms
       ^C
       --- 10.240.66.1 ping statistics ---
       2 packets transmitted, 2 received, 0% packet loss, time 1000ms
       rtt min/avg/max/mdev = 0.176/0.179/0.183/0.013 ms
       ```
       {: screen}

    2. Delete the unnecessary static routes by using the IP or route command.
       By using the information displayed in the route table, delete the unnecessary routes. See the following example:

       ```text
       [root@skv-vsic1x7im ~]# route del -net 166.8.0.0 gw 10.187.149.65 netmask 255.252.0.0 dev eth0
       [root@skv-vsic1x7im ~]# route del -net 161.26.0.0 gw 10.187.149.65 netmask 255.255.0.0 dev eth0
       [root@skv-vsic1x7im ~]# route del -net 10.187.149.65 gw 0.0.0.0 netmask 255.255.255.255 dev eth0
       [root@skv-vsic1x7im ~]# route del -net 10.0.0.0 gw 10.187.149.65 netmask 255.0.0.0 dev eth0
       [root@skv-vsic1x7im ~]# route -n
       Kernel IP routing table
       Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
       0.0.0.0         10.240.66.1     0.0.0.0         UG    0      0        0 eth0
       0.0.0.0         10.240.66.1     0.0.0.0         UG    100    0        0 eth0
       10.240.66.0     0.0.0.0         255.255.255.0   U     100    0        0 eth0
       ```
       {: screen}

    3. Delete any unnecessary gateways permanently from the system files. Old gateway route details can be found in the file `/etc/sysconfig/network-scripts/route-eth0`. See the following example.

       ```text
       # Created by cloud-init on instance boot automatically, do not edit.
       #
       ADDRESS0=10.0.0.0
       GATEWAY0=10.187.149.65
       NETMASK0=255.0.0.0
       ADDRESS1=161.26.0.0
       GATEWAY1=10.187.149.65
       NETMASK1=255.255.0.0
       ADDRESS2=166.8.0.0
       GATEWAY2=10.187.149.65
       NETMASK2=255.252.0.0
       ```
       {: screen}

       Delete the unnecessary gateway information so that the file looks similar to the following example.

       ```text
       # Created by cloud-init on instance boot automatically, do not edit.
       #
       ```
       {: screen}

3. Re-register the Red Hat Enterprise Linux operating system on your virtual server instance. At this time the Red Hat Enterprise Linux operating system is still in an unsubscribed state.

    1. You can verify the unsubscribed state by running the following command. See the following example:

       ```text
       [root@skv-vsic1x7im ~]# subscription-manager list
       You are attempting to use a locale that is not installed.
       +-------------------------------------------+
             Installed Product Status
       +-------------------------------------------+
       Product Name:   Red Hat Enterprise Linux Server
       Product ID:     69
       Version:        7.9
       Arch:           x86_64
       Status:         Unknown
       Status Details:
       Starts:
       Ends:

       [root@skv-vsic1x7im ~]#
       ```
       {: screen}

    2. Run the subscription script `/var/lib/cloud/instances/(VSI/Instance ID)/scripts/vendor/part-00*` which contains the register subscription() function.  When the script is complete the operating system is subscribed. See the following example:

       ```text
       [root@skv-vsic1x7im ~]#./var/lib/cloud/instances/0727_60b2078d-8e30-4c66-a894-b1fbd0ed7acf/scripts/vendor/part-004
       ```
       {: screen}

       After the script completes, verify the details. See the following example:

       ```text
       [root@skv-vsic1x7im ~]# subscription-manager list
       You are attempting to use a locale that is not installed.
       +-------------------------------------------+
           Installed Product Status
       +-------------------------------------------+
       Product Name:   Red Hat Enterprise Linux Server
       Product ID:     69
       Version:        7.9
       Arch:           x86_64
       Status:         Subscribed
       Status Details:
       Starts:         12/31/20
       Ends:           01/16/22

       [root@skv-vsic1x7im ~]#
       ```
       {: screen}

4. Restart the virtual server instance from the {{site.data.keyword.cloud_notm}} console. The *Failed* state changes to *Running* state.
