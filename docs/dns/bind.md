# Installing and configuring Bind

## What is bind?

[BIND](/acronyms) can function as both an authoritative [DNS](/acronyms) server for a zone and a [DNS](/acronyms) resolver simultaneously. It is open source and commonly used for hosting authoritative [DNS](/acronyms) servers.

## Setup

Update apt and install the bind service:

```ssh
apt update
apt install bind9 bind9utils bind9-doc
```

To check whether the installation was successful run the following command:

```ssh
named -v
```

Check whether the system is active and running:

```ssh
systemctl status bind9
```

## Adjust the bind configurations

Edit the file to disable recursion.

```ssh
nano /etc/bind/named.conf.options
```

Add the following lines to the named.conf.options file:

```ssh
 // hide version number from clients for security reasons.
 version "not currently available";

 // disable recursion on authoritative DNS server.
 recursion no;

 // enable the query log
 querylog yes;

 // disallow zone transfer
 allow-transfer { none; };
```

Restart [BIND](/acronyms) to confirm the set options:

```ssh
systemctl restart bind9
```

To configure zones edit the following file:

```ssh
nano /etc/bind/named.conf.local
```

Add the following parameters:

```ssh
zone "g8.sdia.sdi.mi.hdm-stuttgart.de" {
      type master;
      file "/etc/bind/Zones/db.g8.sdia.sdi.mi.hdm-stuttgart.de";
      allow-query { any; };
};
```

Edit the following Zone file which is located in the created `Zones` folder

```shh
$TTL 86400  ; Time-to-live for the zone (1 day)
$ORIGIN g8.sdi.mi.hdm-stuttgart.de.
@   IN  SOA vm1.g8.sdi.mi.hdm-stuttgart.de. admin.g8.sdi.mi.hdm-stuttgart.de. (
               2023110701  ; Serial number (YYYYMMDD##)
               86400        ; Refresh (1 day)
               7200         ; Retry (2 hours)
               604800       ; Expire (1 week)
               86400 )      ; Minimum TTL (1 day)

; Name Servers
                    IN  NS  vm1
; Hosts
vm1                 IN  A   141.62.75.108
vm2                 IN  A   141.62.75.122
www                 IN  CNAME   vm1
cloud                  IN  CNAME   vm2
```

To test the configuration, we can dig one of the local machines with the ip of the nameserver.

```ssh
dig @141.62.75.108 vm2.g8.sdi.mi.hdm-stuttgart.de A +short
```

The following output is showing:

```
141.62.75.122
```

This is the IP address of our second virtual machine. This displays the record is working.

## References

1. [isc.org/bind/](https://www.isc.org/bind/)
2. [linuxbabe.com/debian/authoritative-dns-server-debian-10-buster-bind9](https://www.linuxbabe.com/debian/authoritative-dns-server-debian-10-buster-bind9)
