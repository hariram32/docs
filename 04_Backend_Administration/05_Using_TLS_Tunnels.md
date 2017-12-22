<!-- @@@title:Using TLS Tunnels@@@ -->

>This guide is for Ubuntu.  For other OS's, your file locations may vary.

LogZilla Server Configuration
---
>Note:  In this example, we've used port 1999, you can use any port you'd like.

### LogZilla Server SSL Key Creation
In this example, we've stored the keys in `/etc/syslog-ng/ssl`.  
You'll be prompted for a passphrase during this process, but it will only be
used to create the keys. Once the keys are created, the passphrase will be removed.
You'll also be asked questions about the server name, location, and contact information.

The server name *must* match the entry in your `/etc/hostname` file.

    cd /etc/syslog-ng
    mkdir ssl
    cd ssl
    openssl genrsa -des3 -out logserver.key 2048
    openssl req -new -key logserver.key -out logserver.csr

Remove the passphrase from the key:

    cp logserver.key logserver.key.org
    openssl rsa -in logserver key.org -out logserver.key

Next, generate a self-signed certificate:

    openssl x509 -req -days 365 -in logserver.csr -signkey logserver.key -out logserver.crt

### Configure syslog-ng
Create a file named `tls.conf` in the `/etc/syslog-ng/conf.d` directory with the following:

    source s_tls {
      tcp(port(1999)
      tls( key_file("/etc/syslog-ng/ssl/logserver.key")
        cert_file("/etc/syslog-ng/ssl/logserver.crt")
      peer_verify(optional-untrusted))
      flags(no-multi-line)
      );
    };
Next, change the `source` statement in the LogZilla config file for syslog-ng located at `/etc/syslog-ng/conf.d/`:

    sed -ri 's/s_logzilla/s_tls/' /etc/syslog-ng/conf.d/006.logzilla.log-outputs.conf

> Warning: When upgrading the LogZilla product, this file may be changed. After upgrading, you may need to run the `sed` command above.

Restart syslog-ng by typing `service syslog-ng restart`.

### Configure Client System

Connect to the Client and `mkdir -p /etc/syslog-ng/ssl`.
Download/Upload the `/etc/syslog-ng/ssl/logserver.crt` which was created earlier on the *LogZilla Server* to the *Client* system and put the file in `/etc/syslog-ng/ssl` on the Client.

Find the hash for your key by running `openssl x509 -noout -hash -in /etc/syslog-ng/ssl/logserver.crt`

The result (for example `84d92a45`) is a series of alphanumeric characters based on the Distinguished Name of the certificate.

Next, create a symbolic link to the certificate that uses the hash returned by the previous command, with an added `.0` suffix.

    ln -s /etc/syslog-ng/ssl/logserver.crt /etc/syslog-ng/ssl/84d92a45.0                

### Configure syslog-ng on the Client

Create a new file named `/etc/syslog-ng/conf.d/tls_to_LogZilla.conf` and add the following,
> Replace `LZ_SERVER` with the DNS Name or IP Address of your LogZilla Server.
> You may also need to replace `s_src` with your locally configured source name which is defined in the main `/etc/syslog-ng/syslog-ng.conf` file.

    destination d_tls {
      tcp("LZ_SERVER" port(1999)
      tls( ca_dir("/etc/syslog-ng/ssl/")) );
    };

    log {
      source(s_src);
      destination(d_tls);
    };

Restart syslog-ng on the Client system by typing `service syslog-ng restart`

Check your LogZilla server to verify that events are now being received by this Client.

If you encounter any issues, refer to the <a href="/help/receiving_events_from_other_systems/debugging_event_reception">Debugging Event Reception</a> section of this guide.
