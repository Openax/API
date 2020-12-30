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

| FIELD | DESCRIPTION                                     | REQUIRED | SCHEMA |
| ----- | ----------------------------------------------- | -------- | ------ |
| iss   | Domain from redirect_url (without http&#x3A;//) | Yes      | Text   |
| A2    | B2                                              | C2       |        |
| A3    | B3                                              | C3       |        |

## Exchanging authorisation code for access token

Use the authorisation_code, client_assertion and client_id from previous steps to exchange for an access token that can be used on our APIs.

```json http
{
  "method": "post",
  "url": "https://todos.stoplight.io/todos",
  "headers": {
    "Content-Type": "application/x-www-form-urlencoded"
  },
  "body": {
    "grant_type": "authorization_code",
    "code": "<insert authorisation_code>",
    "client_id": "insert client_id"
  }
}
```

```json
curl --request POST \
  --url https://todos.stoplight.io/todos \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data grant_type=authorization_code \
  --data 'code=<insert authorisation_code>' \
  --data 'client_id=insert client_id'
```
