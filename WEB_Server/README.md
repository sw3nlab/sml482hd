Установка и настройка аудио-плеера с веб-интерфейсом

phptts <-- https://github.com/sw3nlab/phptts

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

после рестарта вебсервера, можно заходить по ip-адресу приставки (например 192.168.2.111) в панель управления плеером:
>http://192.168.2.111/phptts/

PHPTTS - это графический интерфейс написаный на `php` и использующий более низкоуровневые Open Source утилиты (wget, madplay, amixer и т.д)

он позволяет:
+ удалённо управлять воспроизведением/громкостью
+ добавлять свои .mp3 файлы в директорию phptts 
+ добавлять/удалять ip-адреса онлайн радиостанций.
+ конвертировать текст в речь в меню `Голос`


По умолчанию доступно 3 радиостанции EuropaPlus и Radio Record 

(ip-поток радиорекорда можно удалить если он неактивен)

Пример функции `Голос` с синтезом речи:
https://youtu.be/2lZVKWMA5UY
