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




## support

If you've found this useful, you can buy me a 🍺️ or 🍕️ via the [stellar][stellar] network:

* Payment address: [xcmats*keybase.io][xcmatspayment]
* Stellar account ID: [`GBYUN4PMACWBJ2CXVX2KID3WQOONPKZX2UL4J6ODMIRFCYOB3Z3C44UZ`][addressproof]

<br />




## license

**locatrust** is released under the Apache License, Version 2.0. See the
[LICENSE](https://github.com/drmats/locatrust/blob/master/LICENSE)
for more details.




[stellar]: https://learn.stellar.org
[xcmatspayment]: https://keybase.io/xcmats
[addressproof]: https://keybase.io/xcmats/sigchain#d0999a36b501c4818c15cf813f5a53da5bfe437875d92262be8d285bbb67614e22
