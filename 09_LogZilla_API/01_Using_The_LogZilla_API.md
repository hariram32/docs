<!-- @@@title:Using The LogZilla API@@@ -->

Try it out!
-------------
Users may try the API and get more documentation by visiting /api/docs on this server.

Command Line Access
-------------
The API is accessible via wget/curl or your favorite tool capable of sending GET/POST, etc. commands.

Authentication
--------------
All API functions require authentication via AUTHTOKEN

The AUTHTOKEN may be provided in two ways:
- Header
- Via the AUTHTOKEN parameter used in a request URI

Obtaining an Auth Token
==========
To create or revoke token, administrators may use the `~logzilla/src/bin/lz5authtoken` CLI tool:

    usage: lz5authtoken [-h] [-d] [-q] [-n] [-U USERNAME] [-C] [-R REVOKE_TOKEN]

    LogZilla AuthToken manipulation

    optional arguments:
      -h, --help            show this help message and exit
      -d, --debug           Debug mode
      -q, --quiet           Notify only on warnings and errors (be quiet).
      -n, --dry-run         Dry run
      -U USERNAME, --user USERNAME
                            Generate token for given user (default: admin)
      -C, --create          Create new token
      -R REVOKE_TOKEN, --revoke REVOKE_TOKEN
                            Revoke token


Using the Auth Token
-----------

Header based
============

Using an authtoken in Authorization HTTP header:

    Authorization: token 701a75372a019fc3b1572454a582a5705bc4e929d305694c

URL based
=========

Using an authtoken in request URL:

    POST /api/events??AUTHTOKEN=701a75372a019fc3b1572454a582a5705bc4e929d305694c

Example
========
After creating your token, users can connect to the API using any POST/GET/PATCH/PUT, etc. command.
As outlined in the Webhooks section (`/help/receiving_events_from_other_systems/webhooks`), an example of this would be to send a log message into LogZilla using CURL:

    curl -H 'Content-Type: application/json' -X POST -d '{"message": "Test Message"}' 'http://logzilla.mycompany.com/api/events?AUTHTOKEN=91289817dec1abefd728fab4f43aa58b5e6fa814f'
