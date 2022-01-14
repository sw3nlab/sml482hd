Описание конфигурирования и сборки `Python3.8.12` + `openssl-1.1.1g`

> Процесс сборки выполнялся на SML и в среднем занял от 4 до 6 часов

- Сборка `openssl` (необходимо для корректной работы `Python pip`)

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

- Сборка `Python3.8.12`

```php
apt-get install build-essential checkinstall
apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev \
    libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev
cd /opt
wget https://www.python.org/ftp/python/3.8.12/Python-3.8.12.tgz
tar -xf Python-3.8.12.tgz
cd Python-3.8.12
```

далее необходимо раскоментировать модуль `ssl` в файле `Modules/Setup`

```php
204:# Socket module helper for SSL support; you must comment out the other
205:# socket line above, and possibly edit the SSL variable:
206:SSL=/usr/local/ssl
207:_ssl _ssl.c \
208:    -DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
209:    -L$(SSL)/lib -lssl -lcrypto
```

Проверить адреса библиотек `ssl` и в `Module/Setup` и в `LD_LIBRARY_PATH` 

и можно собирать Python

```php
./configure
make altinstall
```

После сборки можно обновить `pip`

`python3.8 -m pip install --upgrade pip`
