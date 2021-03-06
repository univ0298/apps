---

copyright:
  years: 2015, 2018
lastupdated: "2018-12-04"

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}

# Creating certificate signing requests
{: #ssl_csr}

You can secure your applications by uploading SSL certificates and restricting access to the applications.
{:shortdesc}

Before you can upload the SSL certificates to which you’re entitled with {{site.data.keyword.cloud}}, you must create a certificate signing request (CSR) on your server. A CSR is a message that is sent to a certificate authority to request the signing of a public key and associated information. Most commonly, CSRs are in PKCS #10 format. The CSR includes a public key, and a common name, organization, city, state, country, and email. SSL certificate requests are accepted only with a CSR key length of 2048 bits.

## Required CSR contents

For the CSR to be valid, the following information must be entered when you generate the CSR:

### Country name

  A two-digit code for the country or region. For example, `US` is the country code for the United States. For other countries or regions, check the [list of ISO country codes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.iso.org/obp/ui/#search){:new_window} before you create the CSR.

### State or Province

  The full, unabbreviated name of the state or province.

### Locality

  The full name of the town or city.

### Organization

  The full name of the business or company, as legally registered in your locality, or personal name. For companies, be sure to include the registration suffix, such as Ltd., Inc., or NV.

### Organization unit

  The branch name of your company that is ordering the certificate, such as accounting or marketing.

### Common name

  The fully qualified domain name (FQDN) for which you’re requesting the SSL certificate.

The methods for creating a CSR vary depending on your operating system. The following example shows how to create a CSR by using [the OpenSSL command line tool ![External link icon](../icons/launch-glyph.svg "External link icon")](http://www.openssl.org/){:new_window}:

```
openssl req -out CSR.csr -new -newkey rsa:2048 -nodes -keyout privatekey.key
```
{: codeblock}

OpenSSL SHA-512 implementation depends on compiler support for 64-bit integer type. You can use the SHA-1 option for applications that have compatibility issues with the SHA-256 certificate.
{: tip}

A certificate is issued by a certificate authority and is digitally signed by that authority. After you create the CSR, you can generate your SSL certificate on a public certificate authority.

## Uploading SSL certificates
{: #ssl_certificate}

You can apply a security protocol to provide communication privacy for your application to prevent eavesdropping, tampering, and message forgery.

If your account owner has a free trial account, you must upgrade your account to upload a certificate.

Before you can upload certificates, you must create a certificate signing request.

When you use a custom domain to serve the SSL certificate, use the following region endpoints to provide the URL route for your organization in {{site.data.keyword.cloud_notm}}:

* US-South - `secure.us-south.cloud.ibm.com`
* US-East - `secure.us-east.cloud.ibm.com`
* EU-DE - `secure.eu-de.cloud.ibm.com`
* EU-GB - `secure.eu-gb.cloud.ibm.com`
* AU-SYD - `secure.au-syd.cloud.ibm.com`

To upload a certificate for your application, follow these steps.

1. Go to your resource list.

2. Select your app to open the app details view.

3. Click the **Routes** > **Manage domains**.

4. In the action column, click **Domains** from the additional actions menu and select your organization.

5. Click **Upload** in the SSL Certificate column and select your custom domain.

  #### Certificate

    A digital document that binds a public key to the identity of the certificate owner, which enables the certificate owner to be authenticated. A certificate is issued by a certificate authority and is digitally signed by that authority.

    A certificate is generally issued and signed by a certificate authority. However, for testing and development purposes you might use a self-signed certificate.

    The following types of certificates are supported in {{site.data.keyword.cloud_notm}}:

	* PEM (`pem`, `.crt`, `.cer`, and `.cert`)
	* DER (`.der` or `.cer`)
	* PKCS #7 (`p7b`, `p7r`, `spc`)

  #### Private key

    An algorithmic pattern that is used to encrypt messages that only the corresponding public key can decrypt. The private key is also used to decrypt messages that were encrypted by the corresponding public key. The private key is kept on the user system and is protected by a password.

    The following types of private keys are supported in {{site.data.keyword.cloud_notm}}:

    * PEM (`pem`, `.key`)
    * PKCS #8 (`p8`, `pk8`)

  #### Intermediate certificate

    A subordinate certificate that is issued by the trusted root certificate authority (CA) specifically to issue end-entity server certificates. The result is a certificate chain that begins at the trusted root CA, passes through the intermediate certificate, and ends with the SSL certificate issued to the organization.

    Use an intermediate certificate to verify the authenticity of the main certificate. Intermediate certificates are typically obtained from a trusted third party. You might not require an intermediate certificate when you test your application before you deploy it to production.

  #### Enable request of client certificate

    If you enable this option by uploading a client certificate truststore file, a user who tries to access an SSL protected domain is requested to provide a client-side certificate. For example, in a web browser, when a user tries to access an SSL protected domain, the web browser prompts the user to provide a client certificate for the domain. Use the **Client certificate trust store** file upload option to define the client-side certificates that you allow to access your custom domain.

  The custom certificate feature in {{site.data.keyword.cloud_notm}} domain management depends on the Server Name Indication (SNI) extension of the Transport Layer Security (TLS) protocol. The client code that accesses {{site.data.keyword.cloud_notm}} applications that are protected by custom certificates must support the SNI extension in the TLS implementation. For more information, see [section 7.4.2 of RFC 4346 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://tools.ietf.org/html/rfc4346#section-7.4.2){:new_window} and [Securing data with TLS](/docs/get-support/appsectls.html).
  {: note}

  #### Client certificate truststore

  The client certificate truststore includes the client certificates for the users who you want to allow access to your application. Upload a client certificate truststore file to enable the option to request a client certificate.

   The following types of certificates are supported in {{site.data.keyword.cloud_notm}}:

      * PEM (pem, .crt, .cer, and .cert)
      * PKCS #7 (p7b, p7r, spc)

  You can set up mutual authentication by uploading a client certificate truststore that includes a public key in its metadata.
  {: tip}

For more information, see [Importing SSL certificates](/docs/infrastructure/ssl-certificates/import-ssl-certificate.html#import-an-ssl-certificate).

To delete a certificate or replace an existing certificate with a new one, follow these steps.

1. Go to **Manage > Account**, and select **Cloud Foundry orgs**.
2. In the action column, select **Domains** from the additional actions menu. In the additional actions menu for the organization, click **Remove from Org**.
