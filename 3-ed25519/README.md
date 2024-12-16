Create ED25519 Public Key

```shell
openssl genpkey -algorithm ED25519 -out ed25519_private_key.pem
```

Extract ED25519 Public Key

```shell
openssl pkey -in ed25519_private_key.pem -pubout -out ed25519_public_key.pem
```

Check ED25519 Private, Public Key

```shell
openssl pkey -in ed25519_private_key.pem -text -noout
openssl pkey -pubin -in ed25519_public_key.pem -text -noout
```
