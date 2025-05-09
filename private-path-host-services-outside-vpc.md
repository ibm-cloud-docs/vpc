---

copyright:
  years: 2025
lastupdated: "2025-05-09"

keywords:

subcollection: vpc

content-type: tutorial
services: vpc
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Private Path: Enabling access to on-prem services from a consumer VPC
{: #how-to-expose-by-alb}
{: toc-content-type="tutorial"}
{: toc-services="vpc"}
{: toc-completion-time="30m"}

This tutorial guides you through setting up a basic infrastructure that connects an application load balancer (ALB) to a Private Path network load balancer (NLB). The setup uses layer-7 policies on the ALB and exposes the service through a Private Path NLB. This approach enables secure, private connectivity from a consumer's VPC to a provider’s on-premises service, allowing seamless integration without exposing the service to the public internet.

The following diagram shows the architecture you'll build. A service instance in the consumer VPC sends a request to the consumer’s endpoint gateway. The request is then routed to a Private Path service registered in the IBM Cloud catalog. This service connects to the provider’s Private Path NLB, which forwards the request to an ALB in its pool. In turn, the ALB uses an SNI policy on the listener to route traffic by server name to the appropriate backend service instance, configured as ALB pool members.

![Architecture for exposing a Private Path service backed by an ALB with layer-7 policy rules](images/bundle_5_tutorial.svg "Architecture for exposing a Private Path service backed by an ALB with layer-7 policy rules"){: caption="Architecture for exposing a Private Path service backed by an ALB with layer-7 policy rules" caption-side="bottom"}

1. Use Bash terminal variables to hold state as you run CLI and CURL commands in setting up this infrastructure. After some CLI commands, fetch certain IDs and addresses to store in variables for subsequent steps. For example, this tutorial uses the following notation:

   ```bash
   command_that_produces_output
   zone1="<PASTE THE VALUE OBSERVED FROM OUTPUT>"
   ```
   {: pre}

1. Provision the infrastructure on the Frankfurt region:

   ```bash
   env="https://eu-de.iaas.cloud.ibm.com"
   zone1="eu-de-1"
   ```
   {: pre}

1. Install the IBM Cloud CLI and required plugins:

   ```bash
   ibmcloud plugin install cloud-dns-services #  0.8.1 is used in this tutorial
   ibmcloud plugin install vpc-infrastructure # 12.6.0 is used in this tutorial
   ```
   {: pre}

1. Create an SSH key to use when connecting to instances on IBM Cloud:

   ```bash
   ssh-keygen -f ./test-bundle-key
   ibmcloud is key-create my-ssh-key @./test-bundle-key.pub
   ```
   {: pre}

1. Create the provider VPC and subnet in zone 1:

   ```bash
   ibmcloud is vpc-create provider-vpc
   vpcid="<VPC ID>"
   provider_vpc_crn="<VPC CRN>"
   provider_default_sg="<DEFAULT SECURITY GROUP ID FOR VPC>"

   ibmcloud is subnet-create provider-subnet-1 provider-vpc --ipv4-address-count \
     256 --zone "$zone1"
   subnetid="<SUBNET ID>"
   ```
   {: pre}

1. Create an instance in the provider VPC for **backend pool member** with a floating IP running a simple web server:

   ```bash
   ibmcloud is instance-create backend-instance-1 provider-vpc "$zone1" cx2-2x4 \
     provider-subnet-1 --image ibm-ubuntu-24-04-minimal-amd64-3 --keys my-ssh-key
   backend_instance_id="<INSTANCE ID>"
   backend_ip="<PRIMARY ATTACHMENT IP ADDRESS>"
   backend_instance_vni="<PRIMARY NETWORK ATTACHMENT VNI ID>"

   ibmcloud is floating-ip-reserve --vni "$backend_instance_vni"
   backend_instance_fip="<FLOATING IP ADDRESS>"
   ```
   {: pre}

1. Allow inbound SSH for your floating IP to access the backend instance from your device only:

    ```bash
    my_device_ip="$(curl -s ipinfo.io/ip)"
    echo "My IP is $my_device_ip"
    ibmcloud is security-group-rule-add "$provider_default_sg" inbound tcp
    --port-min 22 --port-max 22 --remote "$my_device_ip/32"
    ```
    {: pre}

1. Configure the backend instance to serve a simple HTTP response on port 80 containing the instance’s hostname:

    ```bash
    ssh -i ./test-bundle-5-key "root@$backend_instance_fip" -- '
    apt-get update
    echo y | apt-get install nginx
    hostname | cat > /var/www/html/index.html'
    ```
    {: pre}

1. Create a private DNS to expose a hostname for the back-end pool member instance:

    ```bash
    ibmcloud dns instance-create provider-dns standard-dns
    ibmcloud dns instance-target provider-dns
    ibmcloud dns zone-create private-service.home
    dnszoneid="<DNS ZONE ID>"
    ibmcloud dns resource-record-create "$dnszoneid" --type A --name \
      'private-service.home' --ipv4 "$backend_ip"
    ```
    {: pre}

    In this tutorial, the `private-service.home` domain name is used. The `.home` domain (along with five others) is exempt from ownership validation. For any other fully qualified domain names (FQDNs), you must register and verify ownership yourself. For more information, see [Registering and verifying ownership of service endpoints (FQDNs)](/docs/vpc?topic=vpc-private-path-service-about&interface=ui#pps-domain-register-verify).

1. Create an ALB with the instance hostname as the pool target and with SNI routing configured:

    ```bash
    ibmcloud is load-balancer-create provider-alb public \
      --subnet "$subnetid" \
      --pools '[
        {
          "algorithm": "round_robin",
          "protocol": "tcp",
          "health_monitor": {
            "delay": 2,
            "max_retries": 2,
            "type": "tcp",
            "timeout": 1
          }
        }
      ]'
    albid="<ALB ID>"
    albpoolid="<ALB Pool ID>"
    ```
    {: pre}

1. ALB appliances can only be configured when the provisioning status is **Active**. Use this **while** loop to poll the status of the ALB:

    ```bash
    while ibmcloud is load-balancer provider-alb --output json | \
      jq '.provisioning_status != "active"' --exit-status >/dev/null; do
      echo "Not active. Trying again..."
    done
    echo "ALB is Active"
    ```
    {: pre}

1. Add a member to the ALB targeting the backend instance 1:

    ```bash
    ibmcloud is load-balancer-pool-member-create provider-alb "$albpoolid" 80 \
      "$backend_instance_id"
    ```
    {: pre}

1. Add the listener to the ALB. This command leverages the new SNI hostname layer-7 policy routing capability. It must use a version date greater than or equal to 2025-04-08 for the syntax depicted here.

    This tutorial uses the IBM CLI to fetch the authorization header value.
    {: note}

    ```bash
    curl -sX POST "$env/v1/load_balancers/${albid}/listeners?version=2025-04-08&generation=2" \
    -H "Authorization: $(ibmcloud iam oauth-tokens --output json | jq -r '.iam_token')" \
    -d '{
      "default_pool": {
        "id": "'"$albpoolid"'"
      },
      "port_min": 80,
      "port_max": 80,
      "protocol": "tcp",
      "policies": [
        {
          "action": "forward_to_pool",
          "priority": 1,
          "target": {
            "id": "'"$albpoolid"'"
          },
          "rules": [{
            "type": "sni_hostname",
            "condition": "equals",
            "value": "private-service.home"
          }]
        }
      ]
    }' | jq
    ```
    {: pre}

1. After creating a policy, the ALB returns to **Updating** status. Use the **while** loop again to poll the status of the ALB:

    ```bash
    while ibmcloud is load-balancer provider-alb --output json | \
      jq '.provisioning_status != "active"' --exit-status >/dev/null; do
      echo "Not active. Trying again..."
    done
    echo "ALB is Active"
    ```
    {: pre}

1. Create a Private Path NLB with the ALB as a member target:

    ```bash
    ibmcloud is load-balancer-create provider-ppnlb private-path \
      --family network \
      --subnet "$subnetid" \
      --pools '[
        {
          "algorithm": "round_robin",
          "protocol": "tcp",
          "health_monitor": {
            "delay": 2,
            "max_retries": 2,
            "type": "tcp",
            "timeout": 1
          }
        }
      ]'
    ppnlbid="<PPNLB ID>"
    ppnlbpoolid="<PPNLB ID>"

    ibmcloud is load-balancer-listener-create provider-ppnlb --port-min 80 \
      --port-max 80 --protocol tcp --default-pool "$ppnlbpoolid"

    ibmcloud is load-balancer-pool-member-create provider-ppnlb \
      "$ppnlbpoolid" 80 "$albid"
    ```
    {: pre}

1. Create a Private Path service gateway in the provider VPC and publish as a Private Path service:

   ```bash
   ibmcloud is private-path-service-gateway-create \
     --name private-service-gateway \
     --load-balancer "$ppnlbid" \
     --service-endpoints "private-service.home" \
     --default-access-policy "permit"
   ibmcloud is private-path-service-gateway-publish private-service-gateway
   ppsg_crn="<PPSG CRN>"
   ```
   {: pre}

1. Create a consumer VPC and a subnet:

    ```bash
    ibmcloud is vpc-create consumer-vpc
    consumer_default_sg="<DEFAULT SECURITY GROUP ID FOR VPC>"
    ibmcloud is subnet-create consumer-subnet-1 consumer-vpc \
      --ipv4-address-count 256 --zone "$zone1"
    consumer_subnet_1="<SUBNET ID>"
    ```
    {: pre}

1. Create an endpoint gateway in a consumer VPC to consume the published Private Path service:

    ```bash
    ibmcloud is endpoint-gateway-create \
      --vpc consumer-vpc \
      --target "$ppsg_crn" \
      --name private-service-egw \
      --new-reserved-ip '{"subnet": {"name": "'"$consumer_subnet_1"'"}}'
    ```
    {: pre}

1. Create an instance in the consumer VPC to consume the endpoint gateway with a floating IP:

    ```bash
    ibmcloud is instance-create \
      consumer-instance-1 consumer-vpc "$zone1" cx2-2x4 consumer-subnet-1 \
      --image ibm-ubuntu-24-04-minimal-amd64-3 \
      --keys my-ssh-key
    consumer_instance_vni="<PRIMARY NETWORK ATTACHMENT VNI ID>"
    ```
    {: pre}

1. Allow inbound SSH for your floating IP to access the backend instance from your device only:

    ```bash
    ibmcloud is security-group-rule-add "$consumer_default_sg" inbound tcp \
      --port-min 22 --port-max 22 --remote "$my_device_ip/32"
    ```
    {: pre}

1. SSH to the consumer instance and fetch from the Private Path service to see the hostname of the backend (as we configured with Nginx):

    ```bash
    ssh -i ./test-bundle-5-key "root@$consumer_fip" -- \
      'curl -s private-service.home'
      backend-instance-1
    ```
    {: pre}

1. When you see this result, you have successfully completed your configuration:

    ```bash
    backend-instance-1
    ```
    {: pre}
