sml482hd
NFS - Debian Wheezy boot proccess from OpenWRT/LEDE router
(Процесс запуска Debian Wheezy на ТВ приставке SML482HD с загрузкой ядра и файловой системы по сети)


1) Connect to UART and STOP boot proccess.
`CTRL+i`


```php
CFE>setenv -p STARTUP "boot -z -elf 192.168.2.1:zImage"
CFE>reboot
``` 

