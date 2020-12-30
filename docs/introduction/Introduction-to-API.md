---
tags: [Introduction]
---

# Introduction to API

The API allows you to automate your own business processes.

Whether it is automating payouts to your vendors or fetching transactions into your own application, this documentation is the place to start.


## Setting up your app to access API

You will need to authorise your application to access your Openax account via API. You can do this via the [API settings page](url).

First, create a public/private key-pair. Copy and paste the `.cer` file into the certificate field "X509 public key".

Include here your OAuth Redirect URI, where you will be redirected after consenting the application to access your Openax account.

### Generate a private and public key

```json

openssl genrsa -out privatekey.pem 1024
openssl req -new -x509 -key privatekey.pem -out publickey.cer -days 1825
```

## Consenting the application

Click "Enable API access to your account" from the Openax APP UI and you will be redirected to the /app-confirm URL. Authorise the application using 2FA.

You will then be redirected again, this time where you will obtain an authorisation_code. A client_id will also be created for your account and surfaced via UI.


## Generating a client assertion

To exchange your authorisation code for access token you should also provide a client_assertion JWT. It should be signed with you private key.

See a sample JWT header and payload to the right.

Note: to check that your JWT is valid, please use this [JWT debugging tool](https://jwt.io/#debugger-io) before attempting to register.

JWT Header
```json
{
  "alg": "RS256",
  "typ": "JWT"
}
```

JWT Payload
```json
{
  "iss": <insert your_domain>,
  "sub": <insert client_id>,
  "aud": "https://openax.com",
  "exp": 1606851519
}
```

FIELD | DESCRIPTION | REQUIRED | SCHEMA
---------|----------|----------|-------
 iss | Domain from redirect_url (without http://) | Yes | Text
 A2 | B2 | C2
 A3 | B3 | C3