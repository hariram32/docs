<!-- @@@title:PCI Compliance@@@ -->


# PCI Logs
LogZilla already stores its data in a binary format, making it very difficult for someone to alter the logs. However, some customers may wish to create a secondary store using MD5 hashes to ensure that logs have not been tampered with. Fortunately, this is a very simple process.

* The following command will automatically create the necessary config files for both `syslog-ng` and `cron`. 
* As the **root** user, copy and paste the code below into your LogZilla server's ssh console.

```bash
cat << 'EOF' > /etc/syslog-ng/conf.d/pci.conf
options {
    create_dirs(yes);
};

########################
# Sources
# Added for PCI Compliance
template t_tsv { template("$R_UNIXTIME\t$HOST\t$PRI\t$PROGRAM\t$MSG\n"); };
destination df_PCI { file("/var/log/PCI/$R_MONTH/$R_YEAR-$R_MONTH-$R_DAY.log" template(t_tsv)); };
log { source(s_logzilla); destination(df_PCI); };
EOF

```

Next, create a cron entry which will compress the logs at the end of each day and create an MD5 Checksum file.

* As the **root** user, copy and paste the code below into your LogZilla server's ssh console.

```
cat << EOF > /etc/cron.d/lz5pci
# Cron entry to forward syslog-ng to text logs and compress with a checksum
0 0 * * * root (find /var/log/PCI/*/*.log -daystart -mtime +0 -type f -exec echo "compressing '{}'" ';' -exec gzip '{}' ';' -exec md5sum '{}'.gz ';' >> /var/log/PCI/checksums) 2>&1
EOF

```

The result will be a file named `/var/log/PCI/checksums` containing checksums for all compressed daily archives. If security is a concern, the `checksums` file should be moved to a different server on a daily basis.

# Reload syslog-ng
Lastly, be sure to re-read the new PCI config file into syslog-ng using:

```
service syslog-ng reload
```






