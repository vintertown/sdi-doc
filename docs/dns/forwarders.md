# Forwarders

## What are DNS Forwarders?

DNS forwarders serve several purposes in a network infrastructure. Generally, all the servers meant to convert domain names into IP addresses are assigned a specific forwarder for forwarding all the requests they can’t resolve.

## Configuration

```ssh
acl goodclients {
	localhost;
	localnets;
};

options {
	...
```

```ssh
acl goodclients {
	localhost;
	localnets;
};

options {
	...
	recursion yes;
	allow-query { goodclients; };

	dnssec-validation auto;
	...
	 forwarders {
			8.8.8.8;
			8.8.4.4;
		};
		forward only;
	...
};
```

## References

1. [digitalocean.com/community/tutorials/how-to-configure-bind-as-a-caching-or-forwarding-dns-server-on-ubuntu-14-04#configure-as-a-forwarding-dns-server](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-caching-or-forwarding-dns-server-on-ubuntu-14-04#configure-as-a-forwarding-dns-server)
2. [powerdmarc.com/what-is-dns-forwarding/](https://powerdmarc.com/what-is-dns-forwarding/)
