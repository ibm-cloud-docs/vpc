To make an immediate status change, use one of the following examples. 

You can only make an immediate status change if a future status change is not scheduled. To remove a previously scheduled status change, see [Remove a previously scheduled custom image lifecycle status change using the CLI](#schedule-reset-ilm-status-change-cli). After removing the scheduled status change, you can then make an immediate status change.
{: note}

- Change the image lifecycle status to `deprecate`.

    ```sh
    ibmcloud is image-deprecate IMAGE
    ```
    {: pre}

- Change the image lifecycle status to `obsolete`.

    ```sh
    ibmcloud is image-obsolete IMAGE
    ```
    {: pre}

To schedule a status change, use the following example. For the `deprecate-at` or `obsolete-at` options, specify a date in the `YYYY-MM-DDThh:mm:ss+hh:mm` format. If you define both the `deprecate-at` and `obsolete-at` dates, the `obsolete-at` date must be after the `deprecate-at` date.

```sh
ibmcloud is image-update IMAGE [--deprecate-at YYYY-MM-DDThh:mm:ss+hh:mm] [--obsolete-at YYYY-MM-DDThh:mm:ss+hh:mm]
```
{: pre}

If you want to change the `deprecate-at` and `obsolete-at` date and time entries, you can run these commands again. The previous dates and times are then replaced with the newly specified dates and times.
