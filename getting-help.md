---

copyright:
  years: 2018, 2024
lastupdated: "2024-03-07"

keywords: help, support

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting help and support for VPC
{: #vpc-getting-help-and-support-for-vpc}

If you experience an issue or have questions when using VPC infrastructure services, you can use the following resources before you open a support case.
{: shortdesc}

* Review the [FAQs](/docs/vpc?topic=vpc-faqs-for-VPC) in the product documentation.
* Review the [troubleshooting documentation](/docs/vpc?topic=vpc-troubleshooting-vpc&interface=ui) to troubleshoot and resolve common issues.
* Check the status of the {{site.data.keyword.Bluemix_notm}} platform and resources by going to the [Status page](https://cloud.ibm.com/status){: external}.
* Review [Stack Overflow](https://stackoverflow.com/questions/tagged/ibm-cloud){: external} to see whether other users experienced the same problem. When you ask a question, tag the question with `ibm-cloud` and `vpc`, so that it's seen by the {{site.data.keyword.Bluemix_notm}} development teams.

If you still can't resolve the problem, you can open a support case. For more information, see [Creating support cases](/docs/get-support?topic=get-support-open-case). And, if you're looking to provide feedback, see [Submitting feedback](/docs/overview?topic=overview-feedback).

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
