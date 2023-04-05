
You can remove previously scheduled status changes using the `--reset-deprecate-at` or `--reset-obsolete-at` options. Using either of these with the **ibmcloud is image-update** command removes the date and time from the image. The image is no longer scheduled for that status change.

```sh
ibmcloud is image-update IMAGE [--reset-deprecate-at] [--reset-obsolete-at]
```
{: pre}

If you use both the `--reset-deprecate-at` and `--reset-obsolete-at` options, the image status returns to `available`.
