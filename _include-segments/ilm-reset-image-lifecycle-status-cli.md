
You can remove a scheduled status change by using the `--reset-deprecate-at` or `--reset-obsolete-at` options. If you use either of these options with the **ibmcloud is image-update** command, the date and time are removed from the image. The image is no longer scheduled for that status change.

```sh
ibmcloud is image-update IMAGE [--reset-deprecate-at] [--reset-obsolete-at]
```
{: pre}

If you use both the `--reset-deprecate-at` and `--reset-obsolete-at` options, the image status returns to `available`.
