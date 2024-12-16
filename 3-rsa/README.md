Create RSA Private Key

```shell
openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:2048
```

Extract RSA Public Key

```shell
openssl rsa -pubout -in private_key.pem -out public_key.pem
```

Check RSA Private, Public Key

```shell
openssl rsa -in private_key.pem -text -noout
openssl rsa -pubin -in public_key.pem -text -noout
```
