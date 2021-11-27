Для самостоятельной сборки, можно взять исходный код ядра и набор кросс-компиляторов из репозитория производителя:

>brcmstb3.3 - (kernel+rootfs)
brcmgcc4.8 - (gcc, ar, ld...)

```php
git clone https://github.com/Broadcom/stbgcc4.8
git clone https://github.com/Broadcom/brcmstb3.3

cd brcmstb3.3/linux
make CC={адрес_кросс_компиляторов_stbgcc/bin/mipsel-linux-gnu-} brcm7231b0_defconfig <--- аттачим дефолтный конфиг этого камня
make CC={адрес_кросс_компиляторов_stbgcc/bin/mipsel-linux-gnu-} menuconfig <--- конфигурируем, добавляем плюшки
make CC={адрес_кросс_компиляторов_stbgcc/bin/mipsel-linux-gnu-} <--- компилируем ядро можно с ключём -j равному кол.ву ядер вашего процессора

vmlinuz <--- запакованое ядро которое можно закидывать на флешку в fat16 раздел
```

при загрузке необходимо явно указать ядру что откуда брать и куда выводить:

>boot -z -elf usbdisk0:vmlinuz 'console=ttyS2,115200n8 rootwait root=/dev/sda5 init=/bin/bash'

UART пины на плате - это `console=ttyS2,115200n8`
rootwait - ожидание монтирования фс
root - на каком разделе фс
init - первый демон 

init=/sbin/init инициализирует систему но есть много недочётов
