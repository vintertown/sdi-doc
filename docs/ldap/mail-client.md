# Accessing LDAP data by using a mail client

We use [Thunderbird](https://www.thunderbird.net/) as a mail client. 

## LDAP with Thunderbird
First you have to create a new [LDAP](/acronyms) address book in Thunderbird. To do this, proceed as follows:

`select the "Address book" button in the top menu bar`

`in the address book manager, click on "New --> LDAP address book"`

In the next dialog, you must enter the parameters for the address book connection. In our case:

    Name: LDAP-Test Connection
    Server address: vm1.g8.sdi.mi.hdm-stuttgart.de
    Base DN: dc=betrayer,dc=com
    Port number: 389
    Bind DN: uid=abean,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com

After saving, the address book should be available in the list. You can now perform a search, for example, by surname or e-mail address. You must also enter your password so that the search can be carried out on the server. This serves to protect the address book so that data cannot be accessed by unauthorized persons.
