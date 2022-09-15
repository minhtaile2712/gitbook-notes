# SSL

### Guide

https://www.digicert.com/kb/ssl-support/openssl-quick-reference-guide.htm

### Generate Your Private Key:

openssl genrsa -out client-key.pem 2048

### Creating Your CSR

openssl req -new -key client-key.pem -out client-csr.pem

### Generate Your Certificate Signing Request

openssl x509 -req -in client-csr.pem -signkey client-key.pem -out client-cert.pem

### Remove Unnecessary CSR

rm client-csr.pem

### More

#### Extracting Your Public Key (skip)

openssl rsa -in client-key.pem -pubout -out client-public.pem

#### Verifying CSR Information (skip)

openssl req -text -in client-csr.pem -noout -verify

#### Allow invalid certificates for resources loaded from localhost

chrome://flags/#allow-insecure-localhost
