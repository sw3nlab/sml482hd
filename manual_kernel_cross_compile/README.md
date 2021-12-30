![image](https://github.com/sw3nlab/sml482hd/blob/master/manual_kernel_cross_compile/menuconfig.png)


Для самостоятельной сборки, можно взять исходный код ядра и набор кросс-компиляторов из репозитория производителя Broadcom:

stblinux-3.3 - (kernel+rootfs)  <---> https://github.com/Broadcom/stblinux-3.3

brcmgcc-4.8 - (gcc, ar, ld...) <---> https://github.com/Broadcom/stbgcc-4.8

```php
wget https://github.com/Broadcom/stbgcc-4.8/releases/download/stbgcc-4.8-1.7/stbgcc-4.8-1.7.tar.bz2
tar -xf stbgcc-4.8-1.7.tar.bz2
git clone https://github.com/Broadcom/stblinux-3.3

cd stblinux-3.3/linux
make ARCH=mips CROSS_COMPILE={адрес_кросс_компиляторов_stbgcc4.8/bin/mipsel-linux-} bcm7231b0_defconfig <--- аттачим дефолтный конфиг этого камня
make ARCH=mips CROSS_COMPILE={адрес_кросс_компиляторов_stbgcc4.8/bin/mipsel-linux-} menuconfig <--- конфигурируем, добавляем плюшки
make ARCH=mips CROSS_COMPILE={адрес_кросс_компиляторов_stbgcc4.8/bin/mipsel-linux-} <--- компилируем ядро можно с ключём -j равному кол.ву ядер вашего процессора

ls
vmlinuz <--- на выходе получим запакованое ядро которое можно закидывать на флешку в fat16 раздел, если нет ошибок в процессе компиляции

#Сборка модулей ядра
make ARCH=mips CROSS_COMPILE={адрес_кросс_компиляторов_stbgcc4.8/bin/mipsel-linux-} modules

```

при загрузке необходимо явно указать ядру что откуда брать и куда выводить:

`CFE>boot -z -elf usbdisk0:vmlinuz 'console=ttyS2,115200n8 rootwait root=/dev/sda5 init=/sbin/init'`

- `console=ttyS2,115200n8` <--- UART пины на плате SML (в какую консоль выводить лог загрузки)
- `rootwait` - ожидание определения ядром всех блочных устройств
- `root` - в каком разделе находится файловая система
- `init` - первый демон, обычно init, но можно и hello_world =)


тестирование в процессе...


