# locatrust

Own Certificate Authority (CA) with multi-domain SSL certificates
generation recipe (localhost development).

<br />




## root key (with passphrase)
```bash
$ openssl genrsa -des3 -out rootCA.key 4096
```

## root Certificate Authority (to be imported to the browser)
```bash
$ openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 3650 -out rootCA.crt
```

## server key
```bash
$ openssl genrsa -out ssl.key 4096
```

## server Certificate Signing Request
```bash
$ openssl req -new -key ssl.key -out ssl.csr -config ssl.cnf
```

## server Certificate signed by root CA
```bash
$ openssl x509 -req -in ssl.csr \
    -CA rootCA.crt -CAkey rootCA.key -CAcreateserial \
    -days 3650 -sha256 -out ssl.cert -extfile ssl.cnf -extensions v3_req
```

## server Certificate signed by root CA (subsequent calls)
```bash
$ openssl x509 -req -in ssl.csr \
    -CA rootCA.crt -CAkey rootCA.key -CAserial \
    -days 3650 -sha256 -out ssl.cert -extfile ssl.cnf -extensions v3_req
```

<br />




## license

**locatrust** is released under the Apache License, Version 2.0. See the
[LICENSE](https://github.com/drmats/locatrust/blob/master/LICENSE)
for more details.
