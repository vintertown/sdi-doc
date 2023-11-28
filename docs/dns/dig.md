# Querying DNS data

## What is dig?

[dig](/acronyms) is a programm/command that is mainly used to analyse and check [DNS](/acronyms) servers and query information from it. It has the following syntax:

```ssh
dig [@Server] [Domain] [Type] [-x IP-Address]
```

The server is not necessary, when you only want general [DNS](/acronyms) information for an address (see examples above).

## Example

[CNAME](/acronyms) Record

```ssh
root@sdi08b:~# dig www.natan-cafeandbar.com CNAME +short

natan-cafeandbar.com.
```

[A](/acronyms) Record

```ssh
root@sdi08b:~# dig www.natan-cafeandbar.com A +short

natan-cafeandbar.com.
81.169.145.70
```

[MX](/acronyms) Record

```ssh
root@sdi08b:~# dig www.natan-cafeandbar.com MX +short

natan-cafeandbar.com.
100 alt3.aspmx.l.google.com.
200 alt2.aspmx.l.google.com.
20 alt1.aspmx.l.google.com.
10 aspmx.l.google.com.
```

[NS](/acronyms) Record

```ssh
root@sdi08b:~# dig www.natan-cafeandbar.com NS +short

natan-cafeandbar.com.
shades20.rzone.de.
docks02.rzone.de.
```

Reverse Lookup

```ssh
root@sdi08b:~# dig -x 81.169.145.70 +short

w06.rzone.de.
```

## References

1. [wiki.ubuntuusers.de/dig/](https://wiki.ubuntuusers.de/dig/)
