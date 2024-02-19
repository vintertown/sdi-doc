# SSL/TSL Support

We used the linked manual as a base layer for this exercise: <https://dgu2000.medium.com/working-with-self-signed-certificates-in-chrome-walkthrough-edition-a238486e6858>

## Motivation

Our goal with this task is to create our own CA and store a certificate file in our browser so that we are able to authenticate via SSL and use a save connection with `https`. To achieve this we also need a certificate file, which is signed via the CA and store it on the remote server.

## Prerequisites

We have created a `/cert` folder under the `/home` directory to create all our data.

## Step 1: Becoming your own CA

First we want to become our own CA (Certificate Authority). For that we first have to generate a key and secondly create a certificate `.pem`-file

Generate a key with:

```ssh
openssl genrsa -des3 -out rootCA.key 2048
```

Generate a root certificate file that is valid for two years with:

```ssh
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 730 -out rootCA.pem
```

```ssh
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:DE
State or Province Name (full name) [Some-State]:Baden-Wuerttemberg
Locality Name (eg, city) []:Stuttgart
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Hochschule der Medien
Organizational Unit Name (eg, section) []:Medieninformatik
Common Name (e.g. server FQDN or YOUR name) []:g8.sdi.mi.hdm-stuttgart.de
Email Address []:nv023@hdm-stuttgart.de

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

To check the just created root certificate:

```ssh
openssl x509 -in rootCA.pem -text -noout
```

## Step 2: Creating a certificate request

We use the `openssl`-command-line tool to generate a private key.

```ssh
openssl genrsa -out tls.key 2048
```

Then the certificate signing request is created

```ssh
openssl req -new -key tls.key -out tls.csr
```

We can create a `openssl.cnf`-file with our domain name that is associated with the certificate.

```ssh
# GNU nano 7.2 openssl.cnf
# Extensions to add to a certificate request
basicConstraints = CA:FALSE
authorityKeyIdentifier = keyid:always, issuer:always
keyUsage = nonRepudiation, digitalSignature, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = *.g8.sdi.mi.hdm-stuttgart.de
```

## Step 3: Signing the certificate request to generate the certificate

```ssh
openssl x509 -req \
    -in tls.csr \
    -CA rootCA.pem \
    -CAkey rootCA.key \
    -CAcreateserial \
    -out tls.crt \
    -days 730 \
    -sha256 \
    -extfile openssl.cnf
```

To verify if the config works properly:

```ssh
openssl verify -CAfile rootCA.pem -verify_hostname .g8.sdi.mi.hdm-stuttgart.de tls.crt
```

We get the following output when testing the file:

```ssh
tls.crt:OK
```

## Step 4: Adding CA as trusted to your Browser

On our local machine

```ssh
sudo scp -i ~/.ssh/sdi_nv023 root@sdi08a.mi.hdm-stuttgart.de:/home/cert/rootCA.pem /home
```

Important: Use sudo for executing this command, otherwise you do not have the permission to copy the file to your machine.

We can import the CA in Google Chrome with the following steps:

1. Settings
2. Privacy & Security
3. Security
4. Manage Certificates
5. Authorities
6. Import

When visiting the website now we can see that the certificate is valid and `https` works.
The following image is shown, when clicking on the certificate:

![Certificate Viewer Config](/media/certificate_viewer_config.jpg)
