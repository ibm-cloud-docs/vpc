To make an immediate status change, use one of the following examples.

You can make an immediate status change only if a future status change is not scheduled. To remove a scheduled status change, see [Remove a scheduled custom image lifecycle status change by using the CLI](/docs/vpc?topic=vpc-managing-custom-images&interface=cli#schedule-reset-ilm-status-change-cli). After you remove the scheduled status change, you can make an immediate status change.
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

To schedule a status change, use the following example.

For the `deprecate-at` or `obsolete-at` option, specify a date in the ISO 8601 (`YYYY-MM-DDThh:mm:ss+hh:mm`) date and time format.

* `YYYY` is the four digit year
* `MM` is the two digit month
* `DD` is the two digit day
* `T` separates the date and time information
* `hh` is the two digit hours
* `mm` is the two digit minutes
* `+hh:mm` or `-hh:mm` is the UTC time zone

Thus, the date of 30 September 2023 at 8:00 PM in the North American Central Standard Time Zone (CST) would be `2023-09-30T20:00:00-06:00`

When scheduling the date and time, you can't use your current date and time. For example, if it is 8:00 AM on June 12, then the scheduled date and time must be after 8:00 AM on June 12. If you define both the `deprecate-at` and `obsolete-at` dates, the `obsolete-at` date must be after the `deprecate-at` date.

```sh
ibmcloud is image-update IMAGE [--deprecate-at YYYY-MM-DDThh:mm:ss+hh:mm] [--obsolete-at YYYY-MM-DDThh:mm:ss+hh:mm]
```
{: pre}

If you want to change the `deprecate-at` and `obsolete-at` date and time entries, you can run these commands again. The previous dates and times are then replaced with the new dates and times.
