```shell
echo -n "1039120410"        | openssl dgst -sha3-256 > account_id.sha3-256
echo -n "myUsername"        | openssl dgst -sha3-256 > username.sha3-256
echo -n "Am31nawkMDSmwmekq" | openssl dgst -sha3-256 > password.sha3-256
```
