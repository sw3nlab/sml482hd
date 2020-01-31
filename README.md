### NFS - Debian Wheezy boot proccess from OpenWRT/LEDE route.

(Процесс запуска Debian Wheezy на ТВ приставке SML482HD с загрузкой ядра и файловой системы по сети)

### Необходимые шаги:
- (1) Поднять и настроить TFTP сервер и NFS сервер на Linux хосте или роутере (192.168.2.1).
- (2) Загрузить ядро (zImage) и файловую систему (rootfs) на хост (192.168.2.1)
- (3) Настроить загрузчик SML482HD (CFE) на загрузку ядра и файловой системы с хоста.


Для самостоятельной сборки файловой системы можно использовать bash скрипт.
https://github.com/ZubairLK/mkdebianrfs


(3) Подключаемся к UART приставки и останавливаем загрузку:
`CTRL+i`
Проверяем переменную окружения `STARTUP` .

`CFE> printenv`

Заменяем старое значение: 

`show_logo;cls;boot -z -elf nandflash0.kernel:||boot -z -elf nandflash0.backup_kernel:||boot -z -elf flash0.ro_kernel:||boot -z -elf 192.168.2.1:zImage` 


на загрузку из сети:
```php
CFE>setenv -p STARTUP "boot -z -elf 192.168.2.1:zImage"
```
Перезагружаем SML:
```php
CFE>reboot
```

