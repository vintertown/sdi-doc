# Reverse lookups

## What is a reverse lookup?

A reverse [DNS](/acronyms) lookup is a [DNS](/acronyms) query for the domain name that is assigned to a specific [IP](/acronyms) address. This is the opposite of the more commonly used forward [DNS](/acronyms) lookup, where an [IP](/acronyms) address is queried by the [DNS](/acronyms) system.

## How to perform a reverse lookup?

In order to perform a reverse lookup we have to configure the named.config.local (/etc/bind/named.config.local) file and add another zone to it with the following parameters:

```ssh
zone "75.62.141.in-addr.arpa" {
	type master;
	file "/etc/bind/Zones/rl.g8.sdi.mi.hdm-stuttgart.de";
	allow-query { any; };
};
```

```ssh
zone "75.62.141.in-addr.arpa" {
...
```

The zone specified here is "75.62.141.in-addr.[arpa](/acronyms)". IP addresses with inverted segments and the suffix ".in-addr.[arpa](/acronyms)" are stored in [PTR](/acronyms) records. It indicates that this reverse lookup zone is responsible for translating [IP](/acronyms) addresses in the range 141.62.75.x.

```ssh
...
type master;
...
```

The master option tells the server that it is the server responsible for the domain, i.e. the primary server, and the file only refers to the zone files.

```ssh
...
file "/etc/bind/Zones/rl.g8.sdi.mi.hdm-stuttgart.de";
...
```

This line indicates the path to the file where the zone data for "75.62.141.in-addr.[arpa](/acronyms)" is stored.

```ssh
...
	allow-query { any; };
};
```

This line specifies who is allowed to query the [DNS](/acronyms) server for information in this zone. In this case, "any" means that any client is allowed to query this [DNS](/acronyms) server for information in the specified reverse lookup zone.

## References

1. [www.cloudflare.com/de-de/learning/dns/glossary/reverse-dns/](https://www.cloudflare.com/de-de/learning/dns/glossary/reverse-dns/)
2. [developer.mozilla.org/en-US/docs/Glossary/ARPA?retiredLocale=de](https://developer.mozilla.org/en-US/docs/Glossary/ARPA?retiredLocale=de)
3. [wiki.ubuntuusers.de/DNS-Server_Bind/](https://wiki.ubuntuusers.de/DNS-Server_Bind/)
