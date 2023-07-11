
Make a `PATCH /images` request to remove any scheduled status change by updating `deprecation_at` or `obsolescence_at` to `null`. This property change removes the date and time from the image and the image is no longer scheduled for that status change. 

* To change an image from `deprecated` to `available`, change the `deprecation_at` property to `null`.

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/images/$image_id?version=2022-11-21&generation=2" -H "Authorization: Bearer $iam_token" -d '{ "deprecation_at": null }'
   ```
   {: pre}

* To change an image from `obsolete` to its previous state, change `obsolescence_at` to `null`. If the image previously contained a value other than `null` for `deprecation_at`, then this property change removes the `obsolete` state and changes it back to `deprecated`. If the previous state was `available`, meaning the image was moved from `available` directly to `obsolete`, then this property change moves from `obsolete` back to `available`.

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/images/$image_id?version=2022-11-21&generation=2" -H "Authorization: Bearer $iam_token" -d '{ “obsolescence_at": null }'
   ```
   {: pre}

* To change an image that has both the `deprecation_at` or `obsolescence_at` properties set back to `available`, you must update both the `deprecation_at` or `obsolescence_at` properties.

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/images/$image_id?version=2022-11-21&generation=2" -H "Authorization: Bearer $iam_token" -d '{ "deprecation_at": null, “obsolescence_at": null }'
   ```
   {: pre}
