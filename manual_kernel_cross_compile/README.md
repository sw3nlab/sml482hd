Для самостоятельной сборки, можно взять исходный код ядра и набор кросс-компиляторов из репозитория производителя:

>brcmstb3.3 - (kernel+rootfs)
brcmgcc4.8 - (gcc, ar, ld...)

```php
git clone https://github.com/Broadcom/stbgcc4.8
git clone https://github.com/Broadcom/brcmstb3.3

cd brcmstb3.3/linux
make CC={адрес_кросс_компиляторов_stbgcc/bin/mipsel-linux-gnu-} brcm7231b0_defconfig 
make CC={адрес_кросс_компиляторов_stbgcc/bin/mipsel-linux-gnu-} menuconfig
make CC={адрес_кросс_компиляторов_stbgcc/bin/mipsel-linux-gnu-}

vmlinuz <--- запакованое ядро которое можно закидывать на флешку в fat16 раздел
```

при загрузке необходимо явно указать ядру что откуда брать и куда выводить

>boot -z -elf usbdisk0:vmlinuz 'console=ttyS2,115200n8 rootwait root=/dev/sda5 init=/bin/bash'

init=/sbin/init инициализирует систему но есть много недочётов
