---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-29"

keywords: vpc, help, support

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

For information about opening an IBM support ticket, or about support levels and ticket severities, see [Contacting support](/docs/get-support?topic=get-support-getting-customer-support).
{:shortdesc} 

If you need to open a support ticket, collect as much information as possible to help Support analyze and diagnose your problem. 

For UI issues:

* Provide error codes and reference IDs.
* Save the full URL of the console when the problem occurred, for example: `https://cloud.ibm.com/vpc-ext/provision/vs`
* Include steps to reproduce the issue, along with your inputs and expected outputs.
* Note the approximate time the error occurred.
* Provide the code version and error details: 
  1. Right-click the console page and select the **Inspect** or **Inspect Element** option.
  2. Click the **Console** tab and copy the version information at the top of the output (Project, Version, Build Time, etc.).
  3. Scroll to the bottom of the output and copy any errors or stack traces.

* Provide the network response: 
  1. While inspecting the page, click the **Network** tab.
  2. Refresh the page and reproduce the problem.
  3. Filter all requests by the word "graph".
  4. Starting at the bottom of the list, click each request and view the **Preview** tab. If the request has an "errors" node, expand that node to show the full error.
  ![Network tab](images/network-response.png)
  5. Click the **Response** tab and include the full response string and the URL that generated the response.




