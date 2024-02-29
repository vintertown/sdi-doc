# Transfer DNS configurations

## Querying DNS zone

In order to transfer our current DNS configuration to the MI nameserver of the HdM nssdi.mi.hdm-stuttgart.de, we have to export the [HMAC](/acronyms) secret key corresponding to our subdomain.

```ssh
export HMAC=hmac-sha256:mykey.g8:"your-key"
dig @nssdi.mi.hdm-stuttgart.de -y $HMAC -t AXFR g8.sdi.mi.hdm-stuttgart.de
...
```

Your answer should look something like:

```ssh
; <<>> DiG 9.18.19-1~deb12u1-Debian <<>> @nssdi.mi.hdm-stuttgart.de -y hmac-sha256 -t AXFR g8.sdi.mi.hdm-stuttgart.de
; (1 server found)
;; global options: +cmd
g8.sdi.mi.hdm-stuttgart.de. 86400 IN    SOA     nssdi.mi.hdm-stuttgart.de. goik.hdm-stuttgart.de. 2022110842 14400 7200 1209600 43200
g8.sdi.mi.hdm-stuttgart.de. 86400 IN    NS      nssdi.mi.hdm-stuttgart.de.
g8.sdi.mi.hdm-stuttgart.de. 86400 IN    A       141.62.75.229
g8.sdi.mi.hdm-stuttgart.de. 900 IN      TXT     "Hi nerds, how are you going? :-)"
goik45678.g8.sdi.mi.hdm-stuttgart.de. 86400 IN A 141.62.75.108
lg088.g8.sdi.mi.hdm-stuttgart.de. 86400 IN CNAME sdi1.g8.sdi.mi.hdm-stuttgart.de.
manual.g8.sdi.mi.hdm-stuttgart.de. 86400 IN CNAME sdi1.g8.sdi.mi.hdm-stuttgart.de.
sdi1.g8.sdi.mi.hdm-stuttgart.de. 86400 IN A     141.62.75.108
sdi2.g8.sdi.mi.hdm-stuttgart.de. 86400 IN A     141.62.75.122
www.g8.sdi.mi.hdm-stuttgart.de. 86400 IN A      141.62.75.108
g8.sdi.mi.hdm-stuttgart.de. 86400 IN    SOA     nssdi.mi.hdm-stuttgart.de. goik.hdm-stuttgart.de. 2022110842 14400 7200 1209600 43200
mykey.g8.               0       ANY     TSIG    hmac-sha256. 1702656342 300 32 1Tcfu3PiJPScTyaGQ1QMf6xnWCTnaP6fHp+kvDdywEI= 43084 NOERROR 0
;; Query time: 0 msec
;; SERVER: 141.62.75.229#53(nssdi.mi.hdm-stuttgart.de) (TCP)
;; WHEN: Fri Dec 15 17:05:42 CET 2023
;; XFR size: 11 records (messages 1, bytes 486)
```

## Creating an A record

To associate the domain name `vm1.g8.sdi.mi.hdm-stuttgart.de` with our server's [IP](/acronyms) address, we need to set an A record on the dedicated name server. For example, the A record might look like this:

```ssh
export HMAC=hmac-sha256:mykey.g8:Kk7bhUqdJNARTr4xpeNVxETuFcxRXKF7vqpZQ8yIE8k=
nsupdate -y $HMAC
	⁠server nssdi.mi.hdm-stuttgart.de
	⁠update add vm1.g8.sdi.mi.hdm-stuttgart.de 3600 A 141.62.75.108
	⁠send
	⁠quit
```

To test whether it worked you can use the following command:

```ssh
dig +noall +answer @nssdi.mi.hdm-stuttgart.de vm1.g8.sdi.mi.hdm-stuttgart.de
```

The result should look something like:

```ssh
vm1.g8.sdi.mi.hdm-stuttgart.de. 3600 IN A       141.62.75.108
```

now we can dig our name server globally if the [TTL](/acronyms) is set correctly

```ssh
dig vm1.g8.sdi.mi.hdm-stuttgart.de
```
