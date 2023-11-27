# Mail Exchange

## What is Mail Exchange (MX Record)?

A Mail Exchange (MX) record is a type of DNS (Domain Name System) record that specifies the mail servers responsible for receiving email on behalf of a domain.

## Exercise

Provide a mail exchange record pointing to mx1.hdm-stuttgart.de. Test this configuration using dig accordingly.

Caveat: Configuring a client machine using your name server and sending a mail to xy123@g7.sdi.mi.hdm-stuttgart.de won't actually work since mail.hdm-stuttgart.de will reject mails being sent to any domain other than certain subdomain of hdm-stuttgart.de.

```ssh
...

; MX Record
@       IN    MX    10 mx1.hdm-stuttgart.de.
```

Test configurations with

```ssh
dig vm1.g8.sdi.mi.hdm-stuttgart.de
```

Answer

```ssh
; <<>> DiG 9.18.19-1~deb12u1-Debian <<>> vm1.g8.sdi.mi.hdm-stuttgart.de MX
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 16537
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;vm1.g8.sdi.mi.hdm-stuttgart.de.	IN	MX

;; AUTHORITY SECTION:
g8.sdi.mi.hdm-stuttgart.de. 10265 IN	SOA	nssdi.mi.hdm-stuttgart.de. goik.hdm-stuttgart.de. 2022110842 14400 7200 1209600 43200

;; Query time: 0 msec
;; SERVER: 141.62.64.21#53(141.62.64.21) (UDP)
;; WHEN: Mon Nov 27 17:06:28 CET 2023
;; MSG SIZE  rcvd: 106
```

## References
