# GraySquid
A Graylog squid content pack.

This content pack will launch a SYSLOG_TCP input on port 19302 and will parse your squid logs to be ingested and processed into greylog.

example log


**Required Graylog version:** 2.0.0 and later

## Installation

Open GrayLog interface System / Inputs -> Content Packs -> Import Content Pack.

Activate the content pack.

## Useage

On linux with rsyslogd v7.6 and newer 

~~~~
rsyslogd -v
rsyslogd 8.16.0
~~~~

~~~~
nano /etc/rsyslog.d/60-squid.conf
~~~~
change 10.x.x.x in the below config to your GrayLog server ip.
~~~~
# Load Modules
module(load="imfile")
module(load="omfwd")

# rsyslog Input Modules
input(type="imfile"
         File="/var/log/squid/access.log"
         Tag="squid-access"
         Severity="info"
         Facility="local7"
         ruleset="SquidForward")

# rsyslog RuleSets
ruleset(name="SquidForward") {
        action(type="omfwd"
        Target="10.x.x.x"
        Port="19302"
        Protocol="tcp"
        template="RSYSLOG_SyslogProtocol23Format")
}
~~~~
Add graylog to have permissions to the proxy logs. (probbly not how to do it)
~~~~
usermod -a -G proxy syslog
~~~~
Restart rsyslog
~~~~
/etc/init.d/rsyslog restart
~~~~

