# Mail Exchange

## What is Mail Exchange (MX Record)?

A Mail Exchange ([MX](/acronyms)) record is a type of [DNS](/acronyms) record that specifies the mail servers responsible for receiving email on behalf of a domain. The [MX](/acronyms) record specifies how email messages should be forwarded in accordance with the Simple Mail Transfer Protocol ([SMTP](/acronyms), the standard protocol for all email). Like CNAME records, an MX record must always point to another domain.

## How to setup a MX Record

In order to configure our [MX](/acronyms) record that it points towards mx1.hdm-stuttgart.de, we can add the following parameters to the db.g8.sdi.mi.hdm-stuttgart.de (/etc/bind/Zones/db.g8.sdi.mi.hdm-stuttgart.de) file.

```ssh
nano /etc/bind/Zones/db.g8.sdi.mi.hdm-stuttgart.de
```

and add the following parameters:

```ssh
...

; MX Record
g8.sdi.mi.hdm-stuttgart.de.   IN   MX   10 mx1.hdm-stuttgart.de.
```

The "priority" numbers in front of the domains for these [MX](/acronyms) records indicate the preference; the lower "priority" value is preferred. The server always tries mailhost1 first, as 10 is lower than 20. If sending a message fails, the server chooses mailhost2 by default.

Test whether the set record points towards the mx1.hdm-stuttgart.de mail server with:

```ssh
dig @141.62.75.108 g8.sdi.mi.hdm-stuttgart.de MX +short
```

Answer

```ssh
10 mx1.hdm-stuttgart.de.
```

## References

1. [www.cloudflare.com/de-de/learning/dns/dns-records/dns-mx-record/](https://www.cloudflare.com/de-de/learning/dns/dns-records/dns-mx-record/)
