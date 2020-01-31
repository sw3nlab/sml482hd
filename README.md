### NFS - Debian Wheezy boot proccess from OpenWRT/LEDE route.

(Процесс запуска Debian Wheezy на ТВ приставке SML482HD с загрузкой ядра и файловой системы по сети)

> Необходимые шаги:
- Поднять и настроить TFTP сервер и NFS сервер на Linux хосте или роутере (192.168.2.1).
- Загрузить ядро (zImage) и файловую систему (rootfs) на хост (192.168.2.1)
- Настроить загрузчик SML482HD (CFE) на загрузку ядра и файловой системы с хоста.







Для самостоятельной сборки файловой системы можно использовать bash скрипт.
https://github.com/ZubairLK/mkdebianrfs



1) Connect to UART and STOP boot proccess.
`CTRL+i`


```php
CFE>setenv -p STARTUP "boot -z -elf 192.168.2.1:zImage"
CFE>reboot
``` 

