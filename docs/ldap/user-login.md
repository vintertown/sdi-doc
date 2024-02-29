# LDAP based user login

## Prerequisites

We want to use our second virtual machine to authenticate against our previously configured [LDAP](/acronyms) server (as discussed in the previous chapter). To achieve this, we'll install an [LDAP](/acronyms) client. However, it's crucial to exercise caution when modifying the [PAM](/acronyms) configuration. Pluggable Authentication Modules ([PAM](/acronyms)) is an integrated UNIX login framework used by system components to authenticate users when logging into a UNIX system.

## Making a backup of the PAM config

To create a backup, we used the following commands:

```ssh
tar zcf /root/pam.tgz /etc/pam.conf /etc/pam.d
```

- `tar` - tape archiver: this command is being used to archive data on linux system
- `.tgz` - is an archive data format

We can check if the archive was built correctly by listing the contents of the compressed archive. It should show something like this:

```ssh
tar ztf /root/pam.tgz
```

```ssh
pam.conf
pam.d/
pam.d/common-auth
pam.d/cron
pam.d/newusers
pam.d/passwd
pam.d/login
pam.d/su-l
pam.d/sshd
pam.d/chpasswd
pam.d/common-password
pam.d/other
pam.d/chfn
pam.d/chsh
pam.d/common-session-noninteractive
pam.d/su
pam.d/common-account
pam.d/common-session
pam.d/runuser
pam.d/runuser-l
```

In case we failed to configure and therefore not being able to log in anymore we can always restore the working configuration like this:

```ssh
mv /etc/pam.d /etc/pam.d.orig
mv /etc/pam.conf /etc/pam.conf.orig

tar zxf /root/pam.tgz
```

<span style='color: #ff468e'>Important:</span>
Now it's time to open a second shell. In case of emergency this means that we accidentally log out of the server. In that case we unfortunately would not be able to log in again.

## Configuration of the LDAP Client

First we check whether the domain name is also resolved by our [LDAP](/acronyms) server. We can either ping the name directly to see if we get an answer or create an entry with the [IP](/acronyms) address under /etc/hosts.

for example like this:

```ssh
nano /etc/hosts
```

```ssh
141.62.75.108 vm1.g8.sdi.mi.hdm-stuttgart.de
```

For installing the [LDAP](/acronyms) client we use the exact instructions like in this one here [www.computingforgeeks.com/how-to-configure-ubuntu-as-ldap-client/](https://computingforgeeks.com/how-to-configure-ubuntu-as-ldap-client/)
## References

1. [www.ibm.com/docs/de/netcoolomnibus/8.1?topic=authentication-pam-unix-linux](https://www.ibm.com/docs/de/netcoolomnibus/8.1?topic=authentication-pam-unix-linux)

2. [wiki.ubuntuusers.de/tar/](https://wiki.ubuntuusers.de/tar/)

3. [computingforgeeks.com/how-to-configure-ubuntu-as-ldap-client/](https://computingforgeeks.com/how-to-configure-ubuntu-as-ldap-client/)
