# Querying DNS data

## Catch the Records

```ssh
root@sdi08b:~# dig www.natan-cafeandbar.com CNAME +short

natan-cafeandbar.com.
```

```ssh
root@sdi08b:~# dig www.natan-cafeandbar.com A +short

natan-cafeandbar.com.
81.169.145.70
```

```ssh
root@sdi08b:~# dig www.natan-cafeandbar.com MX +short

natan-cafeandbar.com.
100 alt3.aspmx.l.google.com.
200 alt2.aspmx.l.google.com.
20 alt1.aspmx.l.google.com.
10 aspmx.l.google.com.
```

```ssh
root@sdi08b:~# dig www.natan-cafeandbar.com NS +short

natan-cafeandbar.com.
shades20.rzone.de.
docks02.rzone.de.
```

```ssh
root@sdi08b:~# dig -x 81.169.145.70 +short

w06.rzone.de.
```
