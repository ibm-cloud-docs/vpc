
{{site.data.keyword.attribute-definition-list}}

You can schedule the complete lifecycle for the image. The image is first deprecated and then changes to obsolete according to the schedule that you define. Use the following steps to schedule a lifecycle change.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation menu** icon![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images**.
1. On the **Custom images** tab, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for a specific image and select from the available options.
1. Select **Schedule lifecycle**.
1. Select **Schedule complete lifecycle**.
   - If you selected **By calendar date**, enter the date and time for when you want the status change occur.
   - If you selected **By number of days**, put in the number of days that you want to pass before the status is changed.

   The date for obsolescence must be after the deprecation date.
   {: note}

1. Click **Schedule**.
