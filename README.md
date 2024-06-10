<staging># VPC documentation repo README file

You can submit suggested changes to any existing documentation page by using the "Edit topic" button at the end of each page. By following that link and editing the page, you can submit updates in a pull request (PR). See [Submitting pull requests](https://test.cloud.ibm.com/docs/writing?topic=writing-pr) for more information. Make sure any PRs are created in the Review branch.

If you require brand new topics or updates for a new feature, follow the [Client Services Feature Intake Process](https://confluence.swg.usma.ibm.com:8445/display/UI/Client+Services+Feature+Intake+Process)

When you open a pull request, make sure that you tag the request with a severity label. The severity levels are as follows. The timeline indicates only when a JIRA work item is created and the GitHub issue is updated with the JIRA information. Timeline for completion of work depends on what is needed for a solution.

| Severity | Label | Description |
| --- | --- | --- |
| Sev 1 | `vpc-sev1` | Content error causes data loss or broken environment. These issues will be addressed as soon as possible. A Jira work item and the GitHub issue is updated within 24 hours. |
| Sev 2 | `vpc-sev2` | Incorrect instructions; user can’t proceed to successfully complete a task. These issues will be addressed in the next two-week sprint or earlier if the task is high priority. A Jira work item and the GitHub issue is updated within 48 hours. |
| Sev 3 | `vpc-sev3` | Content enhancement would be helpful to user. This issue is added to the documentation backlog and will be addressed as prioritization allows. A Jira work item and the GitHub issue is updated within 1 week. |
| Sev 4 | `vpc-sev4` | Typos, misspellings, formatting, and other style issues. This issue is added to the documentation backlog and will be addressed as prioritization allows. A Jira work item and the GitHub issue is updated within 1 week. |
{: caption="Table 1. Severities for documentation pull requests" caption-side="bottom"}

If you have any questions about self-service documentation requests, you can ask those questions in the [#docs-iaas-self-service](https://ibm-cloudplatform.slack.com/archives/C06208Q8B8F).


## VPC documentation areas and contacts

* Compute: Gudrun Wolfgram and Anna Matetic
* Storage: Viktoria Muirhead
* Networking: Dana Liburdi
* Bare Metal: Keith Merchant
* Classic: Sterling Milstead
* API: Kelly Parr
* Release Notes: Anna Matetic

## Release notes process

Release notes focals:

Anna Matetic, VPC

Sterling Milstead, Classic

1. Create a PR on the [Review branch file](https://github.ibm.com/cloud-docs/vpc/blob/review/release-notes.md). This PR should contain only the release note and not any other content.
2. Write the release notes according to the guidelines that are in the Content design and development in IBM Cloud creating release notes content.
   - Make sure the release note title is descriptive.
   - Title the PR with the feature name and the estimated date of publication.
   - Add the release notes label.
   - Assign the PR to the release notes focal.
3. The Review branch release note PR is reviewed to ensure proper release note tagging and any minor edits (such as typos and IBM Style).
4. The Review branch release note PR will be committed a week before GA. Writers can continue to edit the Review branch content by using the release note Review branch PR. Note: The Review branch release note PR is not merged if the Publish branch release note PR is not created.
5. When the Review branch release note is finalized/completed (try for at least a week before GA), create a PR on the [Publish branch file](https://github.ibm.com/cloud-docs/vpc/blob/publish/release-notes.md).
   - Title the PR with the feature name and the estimated date of publication.
   - Add the release notes label.
   - Assign the PR to the release notes focal.
   - The Review branch release note PR will not be merged until the Publish branch release note PR is created.
6. When it's time to publish, let the release note focal know via Slack.
7. Only the Release note focal or back up publish the release note to the publication branch.

## Troubleshooting

There are two types of troubleshooting content. These are the Getting help and support page and individual troubleshooting topics.

[Getting help and support for VPC](https://test.cloud.ibm.com/docs/vpc?topic=vpc-getting-help-and-support-for-vpc) is a starting point where clients can find links to the following:
* FAQs and Troubleshooting content
* IBM Cloud platform Status page
* Stack Overflow for Cloud Platform
* IBM Support Forum
* Creating support cases
* Submitting feedback

This page also provides information on gathering and providing details for clients who need to open support cases. For any troubleshooting information that is information gathering for a support case, that information is provided here and not in individual troubleshooting topics.

Individual troubleshooting topics are only created for issues that help users diagnose and resolve problems without contacting IBM support. Any new troubleshooting content must follow the format in the [troubleshooting template](https://github.ibm.com/cloud-docs/vpc/blob/review/troubleshooting_vpc_template.md). Each entry must explain the symptoms of the problem, the cause, and the solution. For additional information on what to include in troubleshooting content, see [Troubleshooting topics](https://test.cloud.ibm.com/docs/writing?topic=writing-troubleshooting-topics).

## API change log and spec update
To create a change log pull request and understand the requirements to promote your feature spec to production, review these topics on the [VPC wiki](https://github.ibm.com/cloud-api-docs/vpc/wiki):
- [API spec and change log checklist](https://github.ibm.com/cloud-api-docs/vpc/wiki/API-spec-and-change-log-checklist) (target audience PM and software developers)
- [API feature and change log requirements](https://github.ibm.com/cloud-api-docs/vpc/wiki/API-feature-and-change-log-requirements) (target audience software and content developers)
- [Change log entry workflow](https://github.ibm.com/cloud-api-docs/vpc/wiki/Change-log-entry-workflow) (target audience software and content developers)
</staging>

