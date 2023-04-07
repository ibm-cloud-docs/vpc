
You can remove scheduled status changes by updating the `deprecated_at` or `obsoleted_at` properties. Run the following command and change the date and time value for the variables to `null`. The image updates to remove the date and time from the image and the image is no longer scheduled for that status change.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/images/$image_id?version=2022-11-21&generation=2" -H "Authorization: Bearer $iam_token" -d '{ "deprecated_at": null, “obsoleted_at": null
}'
```
{: pre}

If you remove both the `deprecated_at` and `obsoleted_at` properties, the image status returns to `available`.
