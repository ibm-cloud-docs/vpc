---

copyright:
  years: 2018, 2023
lastupdated: "2023-01-23"

keywords: help, support

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting help and support
{: #getting-help} 

 Depending on the level of help you need, use the following information to sign up for Slack communication or open an IBM support case.  

## Slack channel
{: #getting-help-slack}

We actively monitor our Slack channel for questions about VPC infrastructure services. Join a community of customers and IBM employees where you can ask questions or discuss IBM Cloud VPC releases.

To sign up, complete the following steps.

1. Request to [join the public Slack channel](https://cloud.ibm.com/kubernetes/slack){: external}.
2. Next, [sign in to Slack](https://ibm-cloud-success.slack.com){: external}.
3. Finally, join our `#virtual-private-cloud` channel.

## Support cases
{: #getting-help-tickets}

For more information about opening an IBM support cases, or about support levels and case severities, see [Contacting support](/docs/get-support?topic=get-support-using-avatar).
{: shortdesc} 

If you need to open a support case, collect as much information as possible to help Support analyze and diagnose your problem. 

For UI issues:

* Provide error codes and reference IDs.
* Save the full URL of the console when the problem occurred, for example: `https://cloud.ibm.com/vpc-ext/provision/vs`
* Include steps to reproduce the issue, along with your inputs and expected outputs.
* Note the approximate time that the error occurred.
* Provide the code version and error details: 
    1. Right-click the console page and select the **Inspect** or **Inspect Element** option.
    2. Click the **Console** tab and copy the version information at the beginning of the output (Project, Version, Build Time, and so on).
    3. Scroll to the end of the output and copy any errors or stack traces.

* Provide the network response: 
    1. While you inspect the page, click the **Network** tab.
    2. Refresh the page and reproduce the problem.
    3. Filter all requests by the word "graph".
    4. Starting at the end of the list, click each request and view the **Preview** tab. If the request has an "errors" node, expand that node to show the full error.
    5. Click the **Response** tab and include the full response string and the URL that generated the response.





