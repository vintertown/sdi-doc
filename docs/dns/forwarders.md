# Forwarders

## What are DNS forwarders?

If you do not want to send [DNS](/acronyms) queries to the root server, you can configure [Bind](<(/acronyms)>) in forwarder mode. It then forwards queries to other [DNS](/acronyms) servers (e.g. from the [ISP](/acronyms)). A new forwarders block is created in the options block for this purpose.

## For Example

If we want to [dig](/acronyms) a [DNS](/acronyms) query from outside of our server the following error message is shown:

```ssh
dig @141.62.75.108 w3.org A +short
;; communications error to 141.62.75.108#53: timed out
```

When the [DNS](/acronyms) server is not configured with a forwarder, it might be trying to perform a recursive query itself, which can lead to timeouts if it's unable to resolve the query within a reasonable time frame. This is why we need to configure a forwarder.

## How to configure a forwarder?

In order to configure our [DNS](/acronyms) forwarder we can add the following parameters to the named.config.options (/etc/bind/named.config.options) file.

```ssh
options {
	...
	forwarders {
		8.8.8.8;
	};
	forward only;
	...
};
```

```ssh
...
forwarders {
	8.8.8.8;
};
...
```

A list of IP addresses that the system is forwarding to. `8.8.8.8` is the Google server.

```ssh
...
forwarders {
	8.8.8.8;
};
...
```

Forward only means that all external queries by default are being forwarded.

```ssh
service bind9 restart;
```

Make sure to restart or reload your DNS server after making changes to the configuration to apply the new settings.

```ssh
dig @141.62.75.108 w3.org A +short
104.18.23.19
```

When digging the same configuration again we can see the forwarded resolution:

## References

1. [digitalocean.com/community/tutorials/how-to-configure-bind-as-a-caching-or-forwarding-dns-server-on-ubuntu-14-04#configure-as-a-forwarding-dns-server](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-caching-or-forwarding-dns-server-on-ubuntu-14-04#configure-as-a-forwarding-dns-server)
2. [powerdmarc.com/what-is-dns-forwarding/](https://powerdmarc.com/what-is-dns-forwarding/)
3. [wiki.ubuntuusers.de/DNS-Server_Bind/](https://wiki.ubuntuusers.de/DNS-Server_Bind/)
