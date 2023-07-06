# apcupsd stats on Cacti (via SNMP)

## apcupsd Config

apcupsd is one of the standard ways of monitoring APC UPS' and has a huge variety of devices and connectivity options. It can also serve the status information to other hosts which avoids needing a network management card.

To get the status for Cacti, I use the network service (NIS as it is known in the config), so you need to ensure that this is enabled and available on the local interface. Since I have already got SNMP v3 configured with encryption on the server, I chose to use this to transmit the data rather than allow apcupsd to serve the data to the cacti monitoring server. The script I provide could easily be modified to allow remote monitoring without SNMP by passing the hostname argument through to apcaccess and adding labels to the parameters.

If your config is correct then you should be able to get a list of parameters from apcupsd by running:

```
$ /sbin/apcaccess status localhost
```

## Getting apcupsd satus over SNMP

I have created a simple Perl wrapper script for apcaccess to get it into SNMP.

I place script apcupsd-stats (make it executable first: chmod +x apcupsd-stats) in /etc/snmp

Add the lines from snmpd.conf.cacti-apcupsd to **/etc/snmp/snmpd.conf**.Once you have added all this in you can test apcupsd-stats by running it from the command line, and via SNMP by appending the appropriate SNMP OID to the "snmpwalk" commands shown previously.

## Cacti Templates

I have generated some basic Cacti Templates for apcupsd. Since different model UPS' give different information, not all the graphs may be appropriate on your UPS. On my one the Output Voltage always gives 230V. the Internal Temperature always 29.2°C, and although the Line Frequency does vary, it does this in steps of 1Hz which is way to course of be of interest.

Simply import the template cacti_host_template_apcupsd.xml, and add the graphs you want to the appropriate device graphs in Cacti. It should just work if your SNMP is working correctly for that device (ensure other SNMP parameters are working for that device).
