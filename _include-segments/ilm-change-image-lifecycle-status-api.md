

To make an immediate status change, use one of the following examples. For the`$image_id` variable, specify the ID of the custom image for the status change.

You can only make an immediate status change if a future status change is not scheduled. To remove a previously scheduled status change, see [Remove a previously scheduled custom image lifecycle status change using the API](#schedule-ilm-reset-status-change-API). After removing the scheduled status change, you can then make an immediate status change.
{: note}

- Change image lifecycle status to `deprecate`.

   ```sh
   curl -X POST "$vpc_api_endpoint/v1/images/$image_id/deprecate?version=2023-02-21&generation=2" -H “Authorization: Bearer $iam_token”
   ```
   {: pre}

- Change the image lifecycle status to `obsolete`.

   ```sh
   curl -X POST "$vpc_api_endpoint/v1/images/$image_id/obsolete?version=2023-12-21&generation=2" -H “Authorization: Bearer $iam_token”
   ```
   {: pre}

To schedule a status change, use one of the following examples. For the `deprecated_at` or `obsoleted_at` properties, specify a date in the `YYYY-MM-DDThh:mm:ss+hh:mm` format. Both of the dates and times must be for a future date and time. If you define both the `deprecated_at` and `obsoleted_at` date and times, the `obsoleted_at` date must be after the `deprecated_at` date and time.

- Schedule a status change to `deprecated`.

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/images/$image_id?version=2022-11-21&generation=2" -H "Authorization: Bearer $iam_token" -d '{
    "deprecated_at": "2023-03-01T06:11:28+05:30"
    }
   }'
   ```
   {: pre}

- Schedule a status change to `obsolete`.

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/images/$image_id?version=2022-11-21&generation=2" -H "Authorization: Bearer $iam_token" -d '{
    “obsoleted_at": "2023-12-31T06:11:28+05:30"
    }
   }'
   ```
   {: pre}

   If you want to change the `deprecated_at` and `obsoleted_at` date and time entries, you can run these commands again. The previous dates and times are then replaced with the newly specified dates and times.
