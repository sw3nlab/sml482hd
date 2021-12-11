# SML 482 HD Debian GNU/Linux (Wheezy) boot proccess.

Описание процесса запуска Debian GNU/Linux (Wheezy) на ТВ приставке SML 482 HD.

![image](https://github.com/sw3nlab/sml482hd/blob/master/2010-01-01-043528_720x576_scrot.png)

Вариант (1):

<details>

  <summary> <b> Загрузка с Флешки (с ядром sml) /Boot from usb flash drive (sml kernel)</b> </summary>
  
  
  ##### 0) Разметка флешки 4Gb 
 - размечать удобнее утилитой `gparted`
 - для быстродейтвия системы (операции чтение/запись) лучше подходят флеш накопители 10 класса
  - bootloader CFE не видит накопители с GPT , необходимо предварительно преобразовать GPT в MBR утилитой `gdisk`
  
  ```php
  [===== Primary =====|===================Extended==================]
  [===================|=============================================]
  [=====50Mb FAT16====|================ 3.95Gb (EXT2) ==============] 
  [=======[sml]=======|================== [rootfs] =================]
  ```
  
  ##### 1) Сборка файловой системы 
```bash
  sudo apt-get install binfmt-support qemu qemu-user-static debootstrap bzip2
  sudo debootstrap --arch=mipsel --no-check-gpg wheezy rootfs http://archive.debian.org/debian/
  ```
  
  
 ##### 2) монтирование файловой системы, установка пароля, установка ssh и иксов 
  ```bash
sudo mount -t proc proc rootfs/proc
sudo mount -t sysfs sysfs rootfs/sys
sudo mount -o bind /dev rootfs/dev
sudo mount --bind /dev/pts/ rootfs/dev/pts/
sudo cp /usr/bin/qemu-mipsel-static rootfs/usr/bin/
sudo chroot rootfs /bin/bash
  
root@debian# passwd root
root@debian# apt-get update 
root@debian# apt-get install openssh-server
root@debian# apt-get install xorg lxde-core lightdm
.......
  ```
  
##### 3) копируем ядро `sml` на флешку в главный раздел primary (50Mb fat16), 
  а файловую систему `rootfs` в расширеный раздел (3.95Gb ext2) !
  
  ```php
sudo cp sml /media/USER/FLASH_DRIVE_FAT16
cd rootfs
sudo cp -a * /media/USER/FLASH_DRIVE_EXT2
  ```
  
  Подключаемся к приставке по UART (останавливаем загрузку CTRL+I) и меняем директивы бутлоадера CFE на:
  ```php
  CFE> setenv -p STARTUP "show_logo; cls; sleep 3000; boot -z -elf usbdisk0:sml"
  CFE> reboot
  ```

##### перезагружаем приставку

</details>


Вариант (2):
<details>
  <summary> <b> Загрузка по сети (с ядром zImage) / Boot from Lan (zImage kernel) </b> </summary>

### Необходимые шаги:
- **(0)** Собрать файловую систему (rootfs) и зарузить вместе с ядром (zImage) на хост (192.168.2.1)
- **(1)** Поднять и настроить TFTP сервер и NFS сервер на Linux хосте или роутере (Для примера: 192.168.2.1).
- **(2)** Настроить загрузчик SML482HD (CFE) на загрузку ядра и файловой системы с хоста.

**(0)**
Cборка файловой системы `rootfs`
Собираем от root'a командой: 
`sudo debootstrap --arch=mipsel --no-check-gpg rootfs http://archive.debian.org/debian/`

Собраную фс пакуем:
> sudo tar -cvzf wheezy-roofs.tar.gz rootfs

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

![image](https://github.com/sw3nlab/sml482hd/blob/master/cpuinfo.jpg)
![image](https://github.com/sw3nlab/sml482hd/blob/master/freedoom.jpg)
  
</details>

Сборка собственного ядра / Cross compile from source:
https://github.com/sw3nlab/sml482hd/tree/master/manual_kernel_cross_compile


Вопросы/Предложения:<br/>
https://discord.com/invite/vcUt6kP

*** Донаты направляйте в благотворительные организации в вашей локации... =)
