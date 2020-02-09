### NFS - Debian Wheezy boot proccess from OpenWRT/LEDE router.

(Процесс запуска Debian Wheezy на ТВ приставке SML482HD с загрузкой ядра и файловой системы по сети)


![image](https://github.com/sw3nlab/sml482hd/blob/master/cpuinfo.jpg)
![image](https://github.com/sw3nlab/sml482hd/blob/master/freedoom.jpg)


> на данный момент ищем способы загрузки ядра и файловой системы с флешки
(!!! currently looking for ways to boot the kernel and file system from a flash drive)
push request+

### Необходимые шаги:
- **(0)** Собрать файловую систему (rootfs) и зарузить вместе с ядром (zImage) на хост (192.168.2.1)
- **(1)** Поднять и настроить TFTP сервер и NFS сервер на Linux хосте или роутере (Для примера: 192.168.2.1).
- **(2)** Настроить загрузчик SML482HD (CFE) на загрузку ядра и файловой системы с хоста.

**(0)**
Для самостоятельной сборки файловой системы можно использовать bash скрипт из этого репозитория или официального:
https://github.com/ZubairLK/mkdebianrfs
Собираем от root'a командой: 
> sudo ./mkdebianrfs.sh mipsel wheezy wheezy-rootfs

Собраную фс пакуем:
> sudo tar -cvzf wheezy-roofs.tar.gz wheezy-rootfs

Загружаем и распаковываем на NFS сервере (192.168.2.1)
> tar -xvzf wheezy-rootfs.tar.gz

**(1)** Установка и настройка NFS сервера:
```php
opkg update
opkg install nfs-kernel-server
vi /etc/exports
/nfs/smart_nfs/ *(rw,insecure,no_root_squash,subtree_check)
/etc/init.d/nfs start
```
проверить работоспособность NFS можно примонтировав свежезалитую rootfs к себе: 
>mount -t nfs 192.168.2.1:/nfs/wheezy-rootfs/ /home


**(1.1)** Настройки TFTP сервера для роутера на базе OpenWRT/LEDE:

https://github.com/alghanmi/openwrt_netgear-wndr3700/wiki/TFTP-Server-on-Your-OpenWRT-Router

```php
uci set dhcp.@dnsmasq[0].enable_tftp=1
uci set dhcp.@dnsmasq[0].tftp_root=/mnt/storage/tftp
uci set dhcp.@dnsmasq[0].dhcp_boot=pxelinux.0
#Commit changes
uci commit dhcp
#Restart Dnsmasq
/etc/init.d/dnsmasq restart
```


**(2)** Подключаемся к UART приставки и останавливаем загрузку:
`CTRL+i`
Проверяем переменную окружения `STARTUP` .

`CFE> printenv`

Заменяем старое значение: 

`show_logo;cls;boot -z -elf nandflash0.kernel:||boot -z -elf nandflash0.backup_kernel:||boot -z -elf flash0.ro_kernel:||boot -z -elf 192.168.2.1:zImage` 


на загрузку из сети:
```php
CFE>setenv -p STARTUP "show_logo;cls;boot -z -elf 192.168.2.1:zImage"
```
> !!! функция show_logo необходима для дальнейшей инициализации графики без неё не стартанут иксы ;(

Перезагружаем SML:
```php
CFE>reboot
```
Видим лог загрузки ядра... если не видим значит стоит проверить настройки TFTP сервера на хосте.

После загрузки ядра следует выбрать , загрузку файловой системы из NFS нажав 1
и указать адрес сервера и путь к файловой системе
```php
NFS SERVER IP:192.168.2.1
NFS PATH:/nfs/wheezy-rootfs/
y/N: y
```
Если загрузка прошла успешно, но всё зависло без приглашения на ввод пароля, то в собраной файловой системе 
следует внести изменения в файл `/etc/inittab`
Заменить следующую строку:
```php
# The default runlevel.
id:2:initdefault:
```
На:
```php
# The default runlevel.
id:1:initdefault:
```
После ребута система загрузится в однопользовательском режиме.
После этого можно стартовать иксы, ssh и загружать `prboom`

```php
screen -q
startx
ctrl+a+d
/etc/init.d/ssh start
```
и
```php
apt-get update
apt-get install prboom
```


