<!-- @@@title:Migrating LogZilla To A New Server@@@ -->

# Process
The process for migrating to a new server requires the following steps:

> Step 1 *must* be done first. If not, you must restore to the exact same version of LogZilla on the new server.

1. Updating to the latest release of LogZilla.
2. Stopping LogZilla and all associated processes.
3. Stopping syslog-ng
4. Compressing relevant directories
5. Backing up the settings stored in postgresql. 
6. Restoring to the new server


> NOTE: Everything below should be run as root. Use `sudo su -` to log in as root.

## Old Server

    wget -qO- http://logzilla.sh | bash
Verify that the LogZilla version updated by running `dpkg -l logzilla` (Ubuntu) or `rpm -q logzilla` (RHEL/CentOS)

Once the old server is updated, run:

    ~logzilla/src/bin/lz5manage --stop
    service syslog-ng stop
    service influxdb stop

> *WARNING*: Be sure there is enough disk space available for the backup files.

### Extract and compress the pgsql data, use the following:

    su logzilla -c "pg_dump logzilla > logzilla.sql"

### Compress the relevant directories:

    tar czvf logzilla-backup.tgz /var/lib/logzilla /var/log/logzilla /var/lib/influxdb logzilla.sql


Once the backup file is ready, transfer both `logzilla.sql` and `logzilla-backup.tgz` to the new server.

## New Server

Install or update LogZilla on the new server using:

    wget -qO- http://logzilla.sh | bash

Verify that the LogZilla version matches the version on the Old Server by running `dpkg -l logzilla` (Ubuntu) or `rpm -q logzilla` (RHEL/CentOS)

Stop syslog-ng and Influx:

    service syslog-ng stop
    service influxdb stop

Reset LogZilla to a new install by clearing all the data (it will be replaced with the backups obtained from the Old Server):

    ~logzilla/src/bin/lz5setup --armageddon=kaboom --db-root-pass "LZ.$(hostid)"

Ensure that no LogZilla processes are running:

    ps -ef | egrep "logzilla|searchd|syslog-ng|influx"
If any processes are running, stop or kill them.

Restore the data from the Old Server:

    tar xzvf logzilla-backup.tgz --exclude="logzilla.sql" -C /
    tar xzvf logzilla-backup.tgz logzilla.sql -C /tmp
    cd /tmp; sudo -u logzilla bash -c 'psql logzilla < logzilla.sql'

After all files are restored, start the logzilla service and make sure syslog-ng is running:

    ~logzilla/src/bin/lz5manage --start
    service syslog-ng status

Depending on the amount of data you have, it will take some time for LogZilla to fully start and begin showing data in the user interface. You can check the status of the initialization by browsing to your server's `/api/monitor`. For example: `http://logzilla.mycompany.com/api/monitor`

Be sure to also check the logzilla.log:

    tail -f /var/log/logzilla/logzilla.log
