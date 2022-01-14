Описание установки `Python3.8.12` + `openssl-1.1.1g`

1) Установка `openssl`

```php
wget https://www.openssl.org/source/openssl-1.1.1g.tar.gz
tar -xf openssl-1.1.1g.tar.gz
cd openssl-1.1.1g.tar.gz
make
make test
make install
```

так же необходимо добавить переменную окружения, с адресом установленного `openssl`
`export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/ssl/lib`
