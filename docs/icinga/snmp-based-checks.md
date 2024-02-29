# SNMP based checks

## Setup

Installing SNMP Daemon with the following command:

```ssh
sudo apt-get install snmpd
```

Configure the Daemon with:

```ssh
sudo nano /etc/snmp/snmpd.conf
```

by adding the following lines:

```ssh
rocommunity mi-snmp

# Include all disks for monitoring
includeAllDisks

```

Restart the SNMP Daemon

```ssh
sudo service snmpd restart
```

## Install SNMP to Icinga

#### Installing SNMP Plugin

With the following command you can firstly install the SNMP Plugin:

```ssh
apt-get install monitoring-plugins
```

#### Configure SNMP in Icinga Server

Configure the following file:

```ssh
nano /etc/icinga2/conf.d/commands.conf
```

with this code:

```ssh
object CheckCommand "snmp" {
  import "plugin-check-command"
  command = [ PluginDir + "/check_snmp" ]

  arguments += {
    "-H" = "$address$"
    "-C" = "$snmp_community$"
    "-o" = "$snmp_oid$"
  }
}
```

#### Define SNMP Service

Configure the following file:

```ssh
nano /etc/icinga2/conf.d/services.conf
```

with this code:

```ssh
apply Service "snmp-disk" {
  import "generic-service"

  check_command = "snmp"
  vars.address = "hostname_or_ip"
  vars.snmp_community = "your_snmp_community_string"
  vars.snmp_oid = ".1.3.6.1.2.1.25.2.3.1.3.1" # OID for total disk space

  assign where host.vars.os == "linux" # Adjust the condition as needed
}
```

#### Restart Icinga

```ssh
systemctl restart icinga2
```
