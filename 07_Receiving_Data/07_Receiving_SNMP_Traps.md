<!-- @@@title:Receiving SNMP Traps@@@ -->

# Installing SNMPTRAPD

    apt-get -y install snmp snmpd snmptrapd snmp-mibs-downloader

## Configure for LogZilla

1. As the `root user`, paste the following into your LogZilla terminal.
        
```bash
snmp_setup() {
    apt-get -y install snmp snmpd snmptrapd snmp-mibs-downloader
    if [ -d "/var/log/logzilla" ]; then
        logfile="/var/log/logzilla/trapd.log"
    else
        logfile="/var/log/trapd.log"
    fi
    [[ -f '/etc/default/snmpd' ]] && file='/etc/default/snmpd'
    [[ -f '/etc/default/snmptrapd' ]] && file='/etc/default/snmptrapd'
    [[ -f "/etc/snmp/snmptrapd.conf" ]] && mv /etc/snmp/snmptrapd.conf /etc/snmp/snmptrapd.conf.$(date +%s)
cat << 'EOF' >  /etc/snmp/snmptrapd.conf
disableAuthorization yes
doNotRetainNotificationLogs yes
snmpTrapdAddr udp:162
authCommunity execute public
outputOption Q
format1 %A,Enterprise OID: %N Trap Type: %W  Trap Sub-Type: %q  Uptime: %T  Description: %W  PDU Attribute/Value Pair Array:%v\n
format2 %A,Enterprise OID: %N Trap Type: %W  Trap Sub-Type: %q  Uptime: %T  Description: %W  PDU Attribute/Value Pair Array:%v\n'
EOF
    if [[ -f "$file" ]]; then
        perl -i -pe 's/TRAPDRUN=no/TRAPDRUN=yes/g' $file
        perl -i -pe 's/^TRAPDOPTS/#TRAPDOPTS/g' $file
        echo "TRAPDOPTS='-p /var/run/snmptrapd.pid -Lf $logfile -Ls 0 -m ALL'" >> $file
    else
        echo "Missing $file, unable to continue!"
        exit 1
    fi
    perl -i -pe 's/^SNMPDOPTS/#SNMPDOPTS/g' /etc/default/snmpd
    echo "SNMPDOPTS='-Lsd -Lf /dev/null -u snmp -g snmp -I -smux,mteTrigger,mteTriggerConf -p /var/run/snmpd.pid'" >> /etc/default/snmpd
    echo "Restarting snmpd"
    service snmpd restart
    echo "Restarting snmptrapd"
    service snmptrapd restart
    echo ""
    echo "Process complete, please check $logfile for any problems"
}
```


2. Run the setup function created in step 1 by typing `snmp_setup`
3. Optional: Add MIBs using the `download-mibs` command.
> Note: the `download-mibs` command will add MIBs to your server so that incoming events get translated to text from OIDs. LogZilla provides this command as a courtesy but resolving any conflicting MIBs is up to the end user.
4. Restart snmpd by typing `service snmpd restart`
5. For Ubuntu 16+ servers, restart snmptrapd by typing `service snmptrapd restart` 
> Note: As of Ubuntu 16, `snmptrapd` is a *separate* program.

6. Check the trapd.log by typing `tail -f /var/log/logzilla/trapd.log`, which should contain entries such as:

		NET-SNMP version 5.7.2 AgentX subagent connected
		NET-SNMP version 5.7.2
7. Verify that snmptrapd is listening by typing `netstat -anop | grep snmptrapd`





    

