---

copyright:
  years: 2018, 2020
lastupdated: "2020-04-13"

keywords: help, support

subcollection: vpc


---

<!-- Common attributes used in the template are defined as follows: -->
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}


# Getting help and support
{: #getting-help} 

 Depending on the level of help you need, use the information below to sign up for Slack communication or open an IBM support ticket.  

## Slack channel
{: getting-help-slack}

We actively monitor our Slack channel for questions about VPC infrastructure services. Join a community of customers and IBM employees where you can ask questions or discuss IBM Cloud VPC releases.

To sign up, complete the following steps.

1. Request to [join the public Slack channel](https://cloud.ibm.com/kubernetes/slack).
2. Next, [sign in to Slack](https://ibm-cloud-success.slack.com).
3. Finally, join our `#virtual-private-cloud` channel.

## Support tickets
{: getting-help-tickets}

For information about opening an IBM support ticket, or about support levels and ticket severities, see [Contacting support](/docs/get-support?topic=get-support-getting-customer-support).
{:shortdesc} 

If you need to open a support ticket, collect as much information as possible to help Support analyze and diagnose your problem. 

For UI issues:

* Provide error codes and reference IDs.
* Save the full URL of the console when the problem occurred, for example: `https://cloud.ibm.com/vpc-ext/provision/vs`
* Include steps to reproduce the issue, along with your inputs and expected outputs.
* Note the approximate time that the error occurred.
* Provide the code version and error details: 
  1. Right-click the console page and select the **Inspect** or **Inspect Element** option.
  2. Click the **Console** tab and copy the version information at the beginning of the output (Project, Version, Build Time, etc.).
  3. Scroll to the end of the output and copy any errors or stack traces.

* Provide the network response: 
  1. While you inspect the page, click the **Network** tab.
  2. Refresh the page and reproduce the problem.
  3. Filter all requests by the word "graph".
  4. Starting at the end of the list, click each request and view the **Preview** tab. If the request has an "errors" node, expand that node to show the full error.
  5. Click the **Response** tab and include the full response string and the URL that generated the response.




