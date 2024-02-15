# Accessing LDAP by a Java™ application

We try to reach our LDAP server with a simple Java application.
To install all needed packages we configure the following artefactId with the following dependencies:

```ssh
  <parent>
    <groupId>at.stderr</groupId>
    <artifactId>maven-parent</artifactId>
    <version>2.3.0</version>
  </parent>

  <dependencies>
    <dependency>
      <groupId>org.ldaptive</groupId>
      <artifactId>ldaptive</artifactId>
      <version>2.3.0</version>
    </dependency>

    <dependency> <!-- required for ldaptive's internal logging -->
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>2.0.11</version>
    </dependency>

    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>2.0.11</version>
    </dependency>
  </dependencies>
```

For the main app we use the boilerplate code of Ldaptive to search for our user with the uid `bbein`.
The following output is achieved.

```ssh
package de.hdm_stuttgart.de.db_app;

import org.ldaptive.*;

public class Main
{
    public static void main( String[] args ) throws LdapException {
        SearchOperation search = new SearchOperation(
                DefaultConnectionFactory.builder()
                        .config(ConnectionConfig.builder()
                                .url("ldap://vm1.g8.sdi.mi.hdm-stuttgart.de")
                                .useStartTLS(false)
                                .build())
                        .build(),
                "dc=betrayer,dc=com");
        SearchResponse response = search.execute("(uid=*bbein)");

        for (LdapEntry entry : response.getEntries()) {
            System.out.println("DN: " + entry.getDn());
            for (LdapAttribute attribute : entry.getAttributes()) {
                System.out.println(attribute.getName() + ": " + attribute.getStringValue());
            }
        }

    }
}
```

Output

```ssh
DN: uid=bein,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
uid: bbein
cn: Bernd Bein
sn: Bein
objectClass: inetOrgPerson
homeDirectory: /home/bbein
uidNumber: 1001
gidNumber: 100
```
