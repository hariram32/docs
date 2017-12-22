<!-- @@@title:Backend Configuration Options@@@ -->


lz5configmanager
---

In order to protect users from damaging the system, some of the settings for LogZilla are not configurable in the UI. These settings are documented below.
>Note: Changing any of these settings may cause irreparable damage to your server. Please use extreme caution.

All of the following settings require a restart of LogZilla before they will take effect.

| Setting                         | Default Value                     | Type   |
|---------------------------------|-----------------------------------|--------|
| DEBUG                           | FALSE                             | Bool   |
| DEDUP_WINDOW                    | 600                               | Int    |
| TIME_TOLERANCE                  | 90                                | Int    |
| LOG_FILE                        | /var/log/logzilla/logzilla.log    | String |
| LOG_MAX_LEVEL                   | INFO                              | String |
| INTERNAL_EVENTS_MAX_LEVEL       | WARNING                           | String |
| LOG_INTERNAL_COUNTERS           | TRUE                              | Bool   |
| RBAC_ENABLED                    | FALSE                             | Bool   |
| GLOBAL_TZ                       | GMT                               | String |
| EULA_ACCEPTED                   | FALSE                             | Bool   |
| SPHINX_DIR                      | /var/lib/logzilla/search          | String |
| SPHINX_MAX_DOCUMENTS_PER_INDEX  | 1000000                           | Int    |
| SPHINX_MAX_INDEXING_TIME        | 5                                 | Int    |
| SPHINX_MIN_INDEX_LEN            | 30                                | Int    |
| SPHINX_REINDEX_PROC_MAX         | 1                                 | Int    |
| SPHINX_REINDEX_DELAY            | 5                                 | Int    |
| SPHINX_MERGE_PROC_MAX           | 1                                 | Int    |
| SPHINX_API_PORT                 | 11350                             | Int    |
| SPHINX_MYSQL_PORT               | 11351                             | Int    |
| SPHINX_DISABLE_MERGING          | FALSE                             | Bool   |
| SPHINX_DISABLE_AUTO_SPLITTING   | FALSE                             | Bool   |
| SPHINX_MAX_MATCHES              | 1000000                           | Int    |
| SEARCHD_RESTART_DELAY           | 10                                | Int    |
| SEARCHD_KILL_DELAY              | 10                                | Int    |
| SEARCHD_START_TIMEOUT           | 120                               | Int    |
| SPHINX_SAVE_POSTMORTEM          | FALSE                             | Bool   |
| SPHINX_DISABLE_PROCESSING       | FALSE                             | Bool   |
| SPHINX_MIN_WORD_LENGTH          | 4                                 | Int    |
| SPHINX_MIN_PREFIX_LENGTH        | 4                                 | Int    |
| SPHINX_MIN_INFIX_LENGTH         | 0                                 | Int    |
| MAX_QUERY_WORKERS               | 6                                 | Int    |
| STORAGE_DIR                     | /var/lib/logzilla/storage         | String |
| STORAGE_SYNC_MODE               | TRUE                              | Bool   |
| REPORTS_DIR                     | /var/lib/logzilla/reports         | String |
| CELERYBEAT_DIR                  | /var/lib/logzilla/celerybeat      | String |
| MAINTENANCE_DIR                 | /var/lib/logzilla/maintenance     | String |
| SUPERVISOR_INET_HTTP_SERVER     | localhost:11389                   | String |
| ZMQ_SOCK_CONTROL                | tcp://127.0.0.1:11300             | String |
| ZMQ_SOCK_LOGGING                | tcp://127.0.0.1:11301             | String |
| ZMQ_SOCK_RAW_PULL               | tcp://127.0.0.1:11311             | String |
| ZMQ_SOCK_RAW_ROUTER             | tcp://127.0.0.1:11312             | String |
| ZMQ_SOCK_STORAGE_PULL           | tcp://127.0.0.1:11313             | String |
| ZMQ_SOCK_STORAGE_PUB            | tcp://127.0.0.1:11314             | String |
| ZMQ_SOCK_STORAGE_REMOTE         | tcp://127.0.0.1:11315             | String |
| ZMQ_SOCK_STORAGE_INTERNAL_PULL  | tcp://127.0.0.1:11316             | String |
| TRIGGER_SCRIPTS                 | TRUE                              | Bool   |
| TRIGGER_EMAIL                   | TRUE                              | Bool   |
| TRIGGER_WEBHOOKS                | TRUE                              | Bool   |
| SCRIPTS_DIR                     | /var/lib/logzilla/scripts         | String |
| SEND_MAIL_PERIOD                | 60                                | Int    |
| SEND_WEBHOOK_PERIOD             | 10                                | Int    |
| EXEC_SCRIPT_PERIOD              | 1                                 | Int    |
| MAIL_SENDER                     | lz5@unconfigured.lzil.la          | String |
| SMTP_SERVER                     | 127.0.0.1                         | String |
| SMTP_PORT                       | 25                                | Int    |
| SMTP_AUTH_REQUIRED              | FALSE                             | Bool   |
| SMTP_USER                       | ,StringSMTP_PASS ,                | String |
| SMTP_CRYPT                      | NONE                              | String |
| REPORTS_BASE_URL                | http://beefcake.lzil.la           | String |
| INFLUXDB_EXPORT_USAGE           | FALSE                             | Bool   |
| INFLUXDB_EXPORT_QUERY_RESULTS   | FALSE                             | Bool   |
| INFLUXDB_HOST                   | 127.0.0.1                         | Bool   |
| INFLUXDB_DIR                    | /var/lib/influxdb/                | Bool   |
| INFLUXDB_PORT                   | 8086                              | Int    |
| INFLUXDB_ADMIN_PORT             | 8083                              | Int    |
| INFLUXDB_USER                   | lz5                               | String |
| INFLUXDB_PASS                   | lz5influxdbpass                   | String |
| INFLUXDB_USAGE_DATABASE         | lz5rusage                         | String |
| INFLUXDB_QUERY_RESULTS_DATABASE | lz5results                        | String |
| INFLUXDB_AGGREGATES_DATABASE    | lz5aggregates                     | String |
| INFLUXDB_TELEGRAF_DATABASE      | lz5localhost                      | String |
| LICENSE_SERVER_URL              | http://license.logzilla.net/keys/ | String |
| LICENSE_PATH                    | /etc/logzilla_license.json        | String |


    usage: lz5configmanager [-h] [-l] [-g NAME] [-ss NAME VALUE] [-sb NAME VALUE]
                            [-si NAME VALUE] [-sl NAME VALUE] [-sf NAME VALUE]
                            [-sj NAME VALUE] [-c PATH]
      
    optional arguments:
      -ss NAME VALUE, --set-string NAME VALUE, --set NAME VALUE
                            Set new value of type string
      -sb NAME VALUE, --set-bool NAME VALUE
                            Set new value of type bool (you can use 0 for False, 1
                            for True)
      -si NAME VALUE, --set-int NAME VALUE
                            Set new value of type int
      -sl NAME VALUE, --set-long NAME VALUE
                            Set new value of type long int
      -sf NAME VALUE, --set-float NAME VALUE
                            Set new value of type float
      -sj NAME VALUE, --set-json NAME VALUE
                            Set new value of compound time, given as json
