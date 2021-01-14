---
title: SSL, Certificates and Keystores
cover: ./image.jpg
date: 2019-12-22
description: Walkaround of SSL, Certificates, Keystores and Configuring WebLogic Application Server to use Custom Keystores
tags: ['post', 'server']
draft: false
---

#### What is SSL?

SSL, an acronym for **Secure Sockets Layer**, is an Industry Standard for securing client-server communication.

SSL Capabilities  
- Encryption
- Integrity
- Authentication

When the client initiates the ssl session, server sends the digital certificates signed by the 3rd Party Certification Authority to the Client. And the Client verifies the trust or the root certificate in the certificate chain. The third party ca, ie certification authority guarantees the certificates are valid. If the trust certificate is not in the browser's trust store, or if the certificate is expired, then the ssl communication will not happen and the browser complains the certificate is not valid. 

#### Public and Private Keys

Private Key is a secret key, also called as Symmetric Key. In Private/Symmetric Key Cryptography, same key is used to encrypt and decrypt the messages. So client and server both should have the copy of private key for encryption and decryption at their respective ends. This type of cryptography is faster and more common when huge data transfers are involved.
 
Public Key on the other hand is public and is commonly wrapped into the digital cerificate and each public key will have an associated private key. Messages encrypted using public key, can only be decrypted using an associated private key and vice versa.
In public key cryptography, when the client initiates the ssl session, server sends the certificate to the client along with the public key. Before encrypting and sending the message, the client verifies the server's authenticity using ca's digital signature  on the certificate. Once authenticated, the message is encrypted using the embedded public key and sent to the server which then will be decrypted on the server using the associated private key which is kept secret and never revealed to anyone except the server or the owner.

You can have either one way or the two way SSL here. In one way SSL, only the server is authenticated to the client. And in two way SSL, both the client and server authenticates each other, where the client should also have the certificate installed. One way SSL is more common in internet based websites and web applications.

#### Keystores

A Keystore is a repository of security certificates and keys (private and symmetric keys).

There are two keystore providers:
- jks : java key-store
- oracle wallet : oracle keystore service

jks is used for Oracle Weblogic Server and all applications deployed on it including SOA Suite, Oracle WebCenter, Oracle Virtual Directory, Oracle Identity and Access Management and so on.

Oracle Wallet is used for Oracle Http Server, Oracle Internet Directory and Oracle WebCache.

#### Keystore Types

We have Identity Store and Trust Store.

1. Identity Store
Identity Store holds the private key and the digital certificates or commonly called as server certificates.


2. Trust Store
Trust Store holds Certification Authorities Trust Certificate. These can be Root or Intermediate Certificate in the certificate chain. 

We can store private key, server certificates and trust certificates in a single store, but its advisable to use different key stores for identity and trust in the production environments, because these have different security requirements. 

Identity Store should be more secure as it holds the sensitive information like the server's private key but Trust Store can be less secure as it holds publicly available trust or root certificates. So, generally its a good idea not to combine these two stores in production environment. 

#### How to manage Keystores in Fusion Middleware Applications?

Keystore can be managed in Fusion Middleware from a web-based interface called as Oracle EM Console, as well as also from the command-line interface using keystore utility.

Oracle provides an Enterprise Manager, popularly known as EM Cosole for managing its applications. Oracle Enterprise Manager is a web-based interface and serves as the primary tool for managing your Oracle database and application servers, and sets a new standard in ease-of-use.

After logging into the EM Console, keystore can be found under Security section in the WebLogic Domain. In the Keystore, some demo keystores are already provided under system stripe. A stripe is a unique reference that hold your keystores. To create a keystore, keystore name along with some other inputs are required. In the same manner, two keystores can be created, one for the Identity and the other for Trust.

![image-in-post1](./image-in-post.jpg)

After creating a keystore, we need to create a key-pair for that keystore. A key-pair is a public-private key combination, signed with a certification authority. 

![image-in-post3](./image-in-post.jpg)

![image-in-post4](./image-in-post.jpg)

After creating a key-pair, we need to generate a csr for the key-pair. A csr is a Certificate Signing Request which is sent to a third party Certification Authority. This csr can be copied or exported to a text file to be sent to a CA. 

![image-in-post5](./image-in-post.jpg)

Once the certificate is verified by the CA, it issues two things:
- a Root or Trust Certificate
- and, a server specific Digital Certificate 

The Root Certificate should be imported to the Truststore, and the Digital Server Certificate should be imported in the Identity Store.

Let's have a look into achieving the same thing from command-line using java keytool.

Keytool is an utility that comes along with your jdk and can be used to create and manage jks based keystore. We can use the following command to create a keystore and a key-pair for it in one go:

```bash
keytool -genkeypair -alias testAlias -keyalg RSA -keysize 2048 -dname "cn=itzsrv.com,c=gb" -keystore testKeystore.jks
```

Next, we need to create a Certificate Signing Request:

```bash
keytool -certreq -v -alias testAlias -file mycsr.csr -keystore testKeystore.jks
```

This mycsr.csr file can be sent to Certification Authority. Once its verified and you receive the certificates back, you can import them in following way:

```bash
keytool -importcert -file mycert.pem -alias testAlias -keystore testKeystore.jks
```

#### Configuring WebLogic Server to use Custom Keystores

From the WebLogic Admin Console's Homepage, navigate to the Server for which Custom Keystore has to be configured. Click on the tab **Keystores** under **Configuration**.

![image-in-post6](./image-in-post.jpg)

Here, the details of the Custom Keystore and Truststore has to be provided.


#### Java Keytool References

These commands allow you to generate a new Java Keytool keystore file, create a CSR, and import certificates. Any root or intermediate certificates will need to be imported before importing the primary certificate for your domain.

Generate a Java keystore and key pair
```bash
keytool -genkey -alias mydomain -keyalg RSA -keystore keystore.jks  -keysize 2048
```

Generate a certificate signing request (CSR) for an existing Java keystore

```bash
keytool -certreq -alias mydomain -keystore keystore.jks -file mydomain.csr
```

Import a root or intermediate CA certificate to an existing Java keystore
```bash
keytool -import -trustcacerts -alias root -file Thawte.crt -keystore keystore.jks
```

Import a signed primary certificate to an existing Java keystore
```bash
keytool -import -trustcacerts -alias mydomain -file mydomain.crt -keystore keystore.jks
```

Generate a keystore and self-signed certificate
```bash
keytool -genkey -keyalg RSA -alias selfsigned -keystore keystore.jks -storepass password -validity 360 -keysize 2048
```

If you need to check the information within a certificate, or Java keystore, use these commands.

Check a stand-alone certificate
```bash
keytool -printcert -v -file mydomain.crt
```

Check which certificates are in a Java keystore
```bash
keytool -list -v -keystore keystore.jks
```

Check a particular keystore entry using an alias
```bash
keytool -list -v -keystore keystore.jks -alias mydomain
```

Delete a certificate from a Java Keytool keystore
```bash
keytool -delete -alias mydomain -keystore keystore.jks
```

Change a Java keystore password
```bash
keytool -storepasswd -new new_storepass -keystore keystore.jks
```

Export a certificate from a keystore
```bash
keytool -export -alias mydomain -file mydomain.crt -keystore keystore.jks
```

List Trusted CA Certs
```bash
keytool -list -v -keystore $JAVA_HOME/jre/lib/security/cacerts
```

Import New CA into Trusted Certs
```bash
keytool -import -trustcacerts -file /path/to/ca/ca.pem -alias CA_ALIAS -keystore $JAVA_HOME/jre/lib/security/cacerts
```

> 
  Few More Important Resources that can be checked out:
  - [Securing Applications with Oracle Platform Security Services](https://docs.oracle.com/middleware/1213/idm/app-security/kssadm.htm#JISEC9596)
  - [Fusion Middleware Administrator's Guide](https://docs.oracle.com/cd/E23943_01/core.1111/e10105/wallets.htm#ASADM2021)
  - [Configuring the Keystore - BEA](https://docs.oracle.com/cd/E13214_01/wli/docs70/b2bsecur/keystore.htm)
