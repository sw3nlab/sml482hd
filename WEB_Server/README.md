Установка и настройка аудио-плеера 

phptts <-- https://github.com/sw3nlab/phptts

с web-интерфейсом

> !!! для работы необходима usb звуковая карта и ядро sml_sound

```php
#apt-get install apache2 php5
#cd /var/www
#wget --no-check-certificate https://github.com/sw3nlab/phptts/archive/refs/heads/master.zip
#unzip master.zip
#mv phptts-master/ phptts
#usermod -a -G audio www-data
#service apache2 restart
```

после рестарта вебсервера, можно заходить по ip-адресу приставки в панель управления плеером,
+ добавлять свои .mp3 файлы в директорию phptts 
+ или добавлять ip-адреса онлайн радиостанций:
>http://192.168.2.111/phptts/

По умолчанию доступно 3 радиостанции EuropaPlus и Radio Record (ip-поток радиорекорда можно удалить если он неактивен)
