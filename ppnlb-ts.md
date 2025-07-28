---

copyright:
  years: 2025
lastupdated: "2025-07-28"

keywords: load balancer, network, faqs

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my Private Path network load balancer in Failed state?
{: #troubleshoot-ppnlb-failedstate}
{: troubleshoot}
{: support}

When troubleshooting the `Failed` health status of a Private Path network load balancer, it's important to understand the various issues that can lead to this state. Explore the potential causes, messages, and key areas to investigate for resolving the 'Failed' status.
{: shortdesc}

The health check poller was unable to connect to the specified health check server based on the defined parameters.
{: tsSymptoms}

A Private Path network load balancer regularly performs health checks on pool members based on the policy defined in the pool. A pool member can have one of the following health statuses: `Unknown`, `OK` or `Failed`. 

A customer-visible log is generated whenever a member’s health status changes to `Failed`. The log message provides details that can assist you in diagnosing the issue. The following sections outline the causes that can trigger a 'Failed' health status log, along with potential root causes and areas to investigate for each.

A `Failed` health status of a Private Path network load balancer indicates that the request can't be processed because of a conflict in the request. Reasons for a `Failed` status include:
{: tsCauses}

* Response `status_code`
* Health-check connection timeout
* `Connection refused` message

Follow these steps to resolve these possible causes:
{: tsResolve}

   Response `status_code`
   :   A load balancer pool configured to perform HTTP/HTTPS health checks expects a `200` status code to consider a pool member healthy. If a different status code is returned, this message indicates the issue. This message indicates that an HTTP status code (other than `200`) was received. 
   
       A good first step is to check if the URL, port, and health-check protocols are all configured correctly in the load balancer pool.

       You can refer to this [list of HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes){: external} for further insight into this issue.
       {: note}

       ```yaml
       "level":"err","msg_timestamp":"2025-01-20T17:30:14.831Z","message":"Health check failed for memberID UUID{cd9b2273-9fef-4036-8703-9b1dcd26ea52} poolID UUID{9c6d013d-9a0f-4683-b26c-4a0fdefa6e32}. Failure reason Received HTTP response status code 503    Service Unavailable, health check dest port 6, health check protocol HTTP","messageID":"is.load-balancer-member-health-check.00001E","generation":2,"logSourceCRN":"crn:v1:bluemix:public:is:us-south:a/86e90e9b18f44d90bf63fc261b6f12f5::load-   balancer:r006-03e8fb9b-60d5-4312-8e70-b311bfc0bc58","saveServiceCopy":true,"correlationId":"","requestId":"","documentsURL":["https://cloud.ibm.com/docs/vpc?topic=vpc-nlb-health-checks"],"resolution":"Please see VPC NLB health check troubleshooting    guide"
       ```
       {: codeblock}

   Health-check connection timeout
   :   A health-check connection timeout means there was a timeout when trying to reach the health-check server in the pool member. The load balancer health checker waits for a reply for the time specified in the load balancer pool before timing out.

       You receive different messages depending on whether or not SYN-ACK (Synchronize-Acknowledge) was received. 

       If SYN-ACK was received:

       ```yaml
       "level":"err","msg_timestamp":"2025-01-20T10:01:48.552Z","message":"Health check failed for memberID UUID{f5de109a-fccf-4cfb-897a-bced53f0e645} poolID UUID{8c11c175-fc56-4690-b01b-d3d45aa87932}. Failure reason Health-check connection timeout (note: SYN-ACK received), health check dest port 4, health check protocol HTTPS","messageID":"is.load-balancer-member-health-check.00001E","generation":2,"logSourceCRN":"crn:v1:bluemix:public:is:us-south:a/7a9f0042acab4050a4f3c310a29d9889::load-balancer:r006-b5b865ba-87b5-4c20-b903-beea9b300a1c","saveServiceCopy":true,"correlationId":"","requestId":"","documentsURL":["https://cloud.ibm.com/docs/vpc?topic=vpc-nlb-health-checks"],"resolution":"Please see VPC NLB health check troubleshooting guide"
       ```
       {: codeblock}

       If SYN-ACK wasn't received:

       ```yaml
       "level":"err","msg_timestamp":"2025-01-20T12:01:48.550Z","message":"Health check failed for memberID UUID{e139cf4b-a79a-45ca-ab4d-b3cb86fa75da} poolID UUID{23414694-b750-466b-aff2-707d42ad72c4}. Failure reason Health-check connection timeout (note: no SYN-ACK received), health check dest port 3, health check protocol HTTPS","messageID":"is.load-balancer-member-health-check.00001E","generation":2,"logSourceCRN":"crn:v1:bluemix:public:is:us-south:a/7a9f0042acab4050a4f3c310a29d9889::load-balancer:r006-b5b865ba-87b5-4c20-b903-beea9b300a1c","saveServiceCopy":true,"correlationId":"","requestId":"","documentsURL":["https://cloud.ibm.com/docs/vpc?topic=vpc-nlb-health-checks"],"resolution":"Please see VPC NLB health check troubleshooting guide"
       ```
       {: codeblock}
       
       Do the following:

       1. Verify that the system running the health check server is not blocking health check packets. First, check if a firewall in your virtual server instance (for example, IP tables) might be blocking the health-check connections. 
       1. If the system being checked is forwarding SYN packets to a different system, check if the destination is receiving the packets and responding as expected.

   `Connection refused` message
   :   This message indicates that the health-check connection was actively refused by the receiving member virtual server instance. The log indicates if a TCP RST packet was sent by the member in response to the health check.

       ```yaml
       "level":"err","msg_timestamp":"2025-01-20T03:39:52.905Z","message":"Health check failed for memberID UUID{0166e975-fc8d-4e26-be38-1060ab148576} poolID UUID{6ac6df06-b08b-4cb6-9c98-f6dc71e375f7}. Failure reason Connection refused (RST received), health check dest port 61, health check protocol TCP","messageID":"is.load-balancer-member-health-check.00001E","generation":2,"logSourceCRN":"crn:v1:bluemix:public:is:us-south:a/9ce2ff483f50488e91f86a7bbbf7e21c::load-balancer:r006-b0eef5eb-a4df-4ece-8853-d85344a6625f","saveServiceCopy":true,"correlationId":"","requestId":"","documentsURL":["https://cloud.ibm.com/docs/vpc?topic=vpc-nlb-health-checks"],"resolution":"Please see VPC NLB health check troubleshooting guide"
       ```
       {: codeblock}

       Do the following:

       1. Check if the port and health-check protocol are configured correctly in the load balancer pool.
       1. Verify the health check application server is up and running. 
       1. Check if a firewall in your virtual server instance (for example, IP tables) might be rejecting the health-check connections.

To continue debugging, you can run `tcpdump` on the member virtual server instance.
{: attention}
