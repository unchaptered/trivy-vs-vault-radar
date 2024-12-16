```shell
echo -n "1039120410"        | openssl dgst -sha256 > account_id.sha256
echo -n "myUsername"        | openssl dgst -sha256 > username.sha256
echo -n "Am31nawkMDSmwmekq" | openssl dgst -sha256 > password.sha256
```
