---
tags: [Introduction]
---

# Introduction to API

The API allows you to automate your own business processes.

Whether it is automating payouts to your vendors or fetching transactions into your own application, this documentation is the place to start.
mm

```json
Generate a private and public key
openssl genrsa -out privatekey.pem 1024
openssl req -new -x509 -key privatekey.pem -out publickey.cer -days 1825
```
