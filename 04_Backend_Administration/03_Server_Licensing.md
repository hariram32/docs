<!-- @@@title:Server Licensing@@@ -->

## Server License Information
Your server license information can be found by browsing to `/api/license-info`

The actual license data file is stored at `/etc/logzilla_license.json`

## Generating a license key
To generate a license key for your server, log into the server's shell (console or ssh) and type:

    ~logzilla/src/bin/lz5license -k

This command will return your unique server key which may then be provided to your LogZilla account manager.

For example:

    73cde9bfce9a15a0ae3a97f0c5088f5712813fc6

After your account manager notifies you that he/she has updated your license on our licensing server, type the following command to update:

    ~logzilla/src/bin/lz5license -D
