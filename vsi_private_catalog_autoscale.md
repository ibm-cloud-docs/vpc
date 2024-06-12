---

copyright:
  years: 2022, [{CURRENT_YEAR}]
lastupdated: "[{LAST_UPDATED_DATE}]"

keywords: 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Using a custom image in a private catalog with an instance group
{: #private-catalog-image-instance-group}

To use a custom image in a private catalog with an instance group, a service-to-service policy to `globalcatalog-collection.instance.retrieve` must be created first. This policy grants access to your custom image in a private catalog to be used when you provision instances for your instance groups. For more information about custom images that are shared to a private catalog, see [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images) and [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).

This service-to-service policy applies only to custom images in a private catalog.
{: note}

## Creating globalcatalog-collection.instance.retrieve service-to-service policy
{: #creating-globalcatalog-collection-policy}

Use the following steps to create the `globalcatalog-collection.instance.retrieve` service-to-service policy.

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, go to **Manage > Access (IAM) > Authorizations**
1. Click **Create**.
1. On the **Grant a service authorization** page, select **VPC Infrastructure Services** for **Source service** and then select **All resources** for **How do you want to scope the access?**
1. For **Target service**, select **Catalog Management** > **Resources based on selected attributes** for **How do you want to scope the access?**.
1. For **Add attributes**, click **Catalog** and then select **string equals** for the **Operator**.
1. For **Value**, enter the UUID of the private catalog.
1. For **Platform access**, click **Viewer**.
1. Click **Authorize**.

## Next steps
{: #next-steps-private-catalog-image-instance-group}
{: ui}

After the `globalcatalog-collection.instance.retrieve` service-to-service policy is created, you can set up your instance group. For more information, see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=ui).
