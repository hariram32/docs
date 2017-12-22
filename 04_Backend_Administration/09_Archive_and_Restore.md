<!-- @@@title:Archive and Restore@@@ -->


LogZilla provides the ability to archive old data and later re-import that data should users need to access and search it later on. This helps users with smaller systems or low disk space to keep historical logs without the need to index all of them at all times.

Archival is particularly useful in environments where users need to be able to search and run reports on events within the last week or month, but may only periodically need to access events from a year ago.

## Live Data Retention
By default, LogZilla will keep 1 week of data "online" and up to 1 year of historical data. To make changes to your desired archive preferences, browse to the server's [settings page](/settings/system/generic).


## Restoring Archives

Restoring archived data may be accomplished by using the command line tool `/home/logzilla/src/bin/lz5archive`.

Restore options may be obtained by typing `-h` for the command. For example:

```
# /home/logzilla/src/bin/lz5archive restore -h
usage: lz5archive restore [-h] [-I ARCHIVE_DIR] [-i INPUT_FILE]
                          [--from-date FROM_DATE] [--to-date TO_DATE]

optional arguments:
  -h, --help            show this help message and exit
  -I ARCHIVE_DIR, --archive-dir ARCHIVE_DIR
                        Input directory (default=/var/lib/logzilla/archive)
  -i INPUT_FILE, --input-file INPUT_FILE
                        Input file
  --from-date FROM_DATE
                        Restore start date (ts or day/time, free format)
  --to-date TO_DATE     Restore end date (ts or day/time, free format)
```

### Restore By Date
To restore, for example, all data from August 11, 2017 to August 12, 2017, enter the following command:

```
/home/logzilla/src/bin/lz5archive restore --from-date 2017-08-11 --to-date 2017-08-12
```
>Note, users may also use Unix Timestamp in the restore command, for example:

```
/home/logzilla/src/bin/lz5archive restore --from-date @1503520500 --to-date @1503520560
```

Running the command above **will not return output** until the restore process completes. To see full output, use the `-d` (debug) command line option.

Once the data is restored, it will automatically be re-archived at midnight through the autoarchive process.

## Archive Logs
A full list of all archives activity is available via the web API web interface located at [`/api/archive-restore-logs`](/api/archive-restore-logs)

