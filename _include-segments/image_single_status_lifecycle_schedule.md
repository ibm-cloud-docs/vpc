
{{site.data.keyword.attribute-definition-list}}

You can schedule a single status change for an image.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images**.
1. On the **Custom images** tab,  click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for a specific image and select from the available options.
1. Select **Schedule lifecycle**.
1. In **Image status**, select the status change for the image.
1. In **Deprecation details**, select whether to change status **Immediately** or to **Schedule future date**.
1. If you selected **Schedule future date**, you need to fill out the following:
   - Select either **By calendar date** or **By number of days**.
      - If you selected **By calendar date**, enter the date and time information. This is the date and time when the status change will occur.
      - If you selected **By number of days**, put in the number of days that will pass before the status is changed.
1. Click **Save**.
