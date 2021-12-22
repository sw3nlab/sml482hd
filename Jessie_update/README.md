![image](https://github.com/sw3nlab/sml482hd/blob/master/Jessie_update/Screenshot_20211223-013936_Termius.png)
Обновится до `jessie` можно следующим образом.

0 - собираем файловую систему `wheezy` через `debootstrap`

1 - устанавливаем только `openssh-server`

2- берём ядро `kernel314` (или собираем его сами)
 из этой директории и заливаем его в раздел FAT-16 на своей флешке вместо ядра `3.3`

3 - Загружаемся... заходим по ssh на приставку.

Удаляем или комментируем строчку в `/apt/sources.list` добавлением знака `#`

```php
#deb http://archive.debian.org/debian wheezy main
```

добавляем адрес репозитория `jessie`

```php
deb http://archive.debian.org/debian jessie main contrib non-free
```

сохраняем и делаем

`apt-get update` затем `apt-get upgrade`


!!! не забываем про то что в бутлоадере `CFE` нужно будет тоже поменять название ядра, с которым вы будете грузится и адрес файловой системы иначе загрузчик просто ничего не загрузит )

`CFE> setenv -p STARTUP "boot -elf usbdisk0:kernel_3_14 'rootwait root=/dev/sda5 init=/sbin/init'"`


>в этом ядре нет проприетарного драйвера фреймбуфера, поэтому КИНА не будет не через hdmi ни через RCA AUX (подробнее в разделе `PROBLEMS`) 
после обновления возможны проблемы например с `aptitude` --> `Segmentation Fault`
