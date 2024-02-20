# Providing WEB based user management to your LDAP Server

## Step 1:

First we install the ldap-account-manager with the following command:

```ssh
apt install ldap-account-manager
```

When the installation was successful we can enter the web platform via this url:
`http://g8.sdi.mi.hdm-stuttgart.de/lam`

## Step 2:

First we need to access the lam-configurations and edit the `server profiles`. The standard password is set to `lam`.

We update the server settings and the security settings within the general settings with our ldap configuration.
![ldap authentication management web platform](/media/lam_1.jpeg)
![ldap authentication management web platform](/media/lam_2.jpeg)
We set the right organizational under account types to access the user.
![ldap authentication management web platform](/media/lam_3.jpeg)
Now we can log in with the ldap admin credentials and get a list of all users in the organizational unit.
![ldap authentication management web platform](/media/lam_4.jpeg)
