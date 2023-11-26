# DNS

## What is DNS?

[DNS](/glossary), or the Domain Name System, is a hierarchical and distributed naming system that is fundamental to the functioning of the Internet. It serves as a translator between human-readable domain names and numerical [IP](/glossary) addresses. It is indispencible for accessing services on the internet. Only with the [DNS](/glossary)
can an internet user access a website by entering a [URL](/glossary) or send an email with a domain ending (e.g. @hdm-stuttgart.de).

## How does DNS work?

[DNS](/glossary) works through a hirahierarchical, distributed order.

1. A user enters an [URL](/glossary) (e.g., www.beispieldomain.de) into a web browser or another application.
2. (When cache is not empty) The user's device checks its local [DNS](/glossary) resolver (typically provided by the Internet Service Provider or configured manually). This resolver may have a cache of recently resolved domain names.
3. When there is no cache available the root server is the first step in translating the given host names into [IP](/glossary) addresses. It will point to the top-level domain of the given domain name.
4. Top-level domain server (like denic) is responsible for translating the domain which the user bought (e.g. beispieldomain.de) to the given [IP](/glossary) address.
5. The top-level domain server provides informations about the authoritative [DNS](/glossary) server. The authoritative [DNS](/glossary) server for the second-level domain (e.g., "beispieldomain.de") holds information about the requested host name (e.g., "www"). It is this server that can provide the [IP](/glossary) address associated with the specific host name.

![DNS Namensbaum](/media/namensbaum.webp)

#### Good to know

DENIC operates a worldwide network of name servers to provide the name service for the German top-level domain .de and the German ENUM domain .9.4.e164.arpa. This currently comprises 19 locations in 17 cities on five continents.

## Types of DNS

- A (Address) Record: Maps a domain to an IPv4 address.
- AAAA (IPv6 Address) Record: Maps a domain to an IPv6 address.
- CNAME (Canonical Name) Record: Alias of one domain to another.
- MX (Mail Exchange) Record: Specifies mail servers responsible for receiving emails for the domain.
- NS (Name Server) Record: Indicates authoritative DNS servers for the domain.
- PTR (Pointer) Record: Used for reverse DNS lookups.
- SOA (Start of Authority) Record: Contains information about the domain and the zone.

## References

1. [www.denic.de/wissen/domain-name-system-dns](https://www.denic.de/wissen/domain-name-system-dns)
2. [www.cloudflare.com/en-gb/learning/dns/what-is-dns/](https://www.cloudflare.com/en-gb/learning/dns/what-is-dns/)
