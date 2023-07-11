

To make an immediate status change, use one of the following examples. For the`$image_id` variable, specify the ID of the custom image for the status change.

You can make an immediate status change only if a future status change is not scheduled. To remove a scheduled status change, see [Remove a scheduled custom image lifecycle status change by using the API](#schedule-ilm-reset-status-change-API). After you remove the scheduled status change, you can make an immediate status change.
{: note}

- Change image lifecycle status to `deprecated`.

   ```sh
   curl -X POST "$vpc_api_endpoint/v1/images/$image_id/deprecate?version=2023-02-21&generation=2" -H “Authorization: Bearer $iam_token”
   ```
   {: pre}

- Change the image lifecycle status to `obsolete`.

   ```sh
   curl -X POST "$vpc_api_endpoint/v1/images/$image_id/obsolete?version=2023-12-21&generation=2" -H “Authorization: Bearer $iam_token”
   ```
   {: pre}

To schedule a status change, use one of the following examples. 

For the `deprecation_at` or `obsolescence_at` properties, specify a date in the ISO 8601 (`YYYY-MM-DDThh:mm:ss+hh:mm`) date and time format.

* `YYYY` is the four digit year
* `MM` is the two digit month
* `DD` is the two digit day
* `T` separates the date and time information
* `hh` is the two digit hours
* `mm` is the two digit minutes
* `+hh:mm` or `-hh:mm` is the UTC time zone

Thus, the date of 30 September 2023 at 8:00 p.m. in the North American Central Standard Time Zone (CST) would be `2023-09-30T20:00:00-06:00`

When scheduling the date and time, you can't use your current date and time. For example, if it is 8 a.m. on June 12, then the scheduled date and time must be after 8 a.m. on June 12. If you define both the `deprecation_at` and `obsolescence_at` dates and times, the `obsolescence_at` date must be after the `deprecation_at` date and time.

- Schedule a status change to `deprecated`.

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/images/$image_id?version=2022-11-21&generation=2" -H "Authorization: Bearer $iam_token" -d '{
    "deprecation_at": "2023-03-01T06:11:28+05:30" }'
   ```
   {: pre}

- Schedule a status change to `obsolete`.

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/images/$image_id?version=2022-11-21&generation=2" -H "Authorization: Bearer $iam_token" -d '{
    “obsolescence_at": "2023-12-31T06:11:28+05:30" }'
   ```
   {: pre}

   If you want to change the `deprecation_at` and `obsolescence_at` date and time entries, you can run these commands again. The previous dates and times are replaced with the new dates and times.
