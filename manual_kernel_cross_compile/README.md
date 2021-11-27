Для самостоятельной сборки, можно взять исходный код ядра и набор кросс-компиляторов из репозитория производителя:

>brcmstb3.3 - (kernel+rootfs)
>brcmgcc4.8 - (gcc, ar, ld...)

```php
git clone https://github.com/Broadcom/stbgcc4.8
git clone https://github.com/Broadcom/brcmstb3.3

cd brcmstb3.3/linux
make CROSS_COMPILE={адрес_кросс_компиляторов_stbgcc4.8/bin/mipsel-linux-gnu-} brcm7231b0_defconfig <--- аттачим дефолтный конфиг этого камня
make CROSS_COMPILE={адрес_кросс_компиляторов_stbgcc4.8/bin/mipsel-linux-gnu-} menuconfig <--- конфигурируем, добавляем плюшки
make CROSS_COMPILE={адрес_кросс_компиляторов_stbgcc4.8/bin/mipsel-linux-gnu-} <--- компилируем ядро можно с ключём -j равному кол.ву ядер вашего процессора

ls
vmlinuz <--- на выходе запакованое ядро которое можно закидывать на флешку в fat16 раздел, если нет ошибок в процессе компиляции
```

при загрузке необходимо явно указать ядру что откуда брать и куда выводить:

`CFE>boot -z -elf usbdisk0:vmlinuz 'console=ttyS2,115200n8 rootwait root=/dev/sda5 init=/bin/bash'`

- `console=ttyS2,115200n8` <--- UART пины на плате SML (в какую консоль выводить лог загрузки)
- `rootwait` - ожидание определения ядром всех блочных устройств
- `root` - на каком разделе находится файловая система
- `init` - первый демон, обычно init, но можно и hello_world =)

init=/sbin/init - из сборки `debootstrap` инициализирует систему но есть недочёты (непонятное поведение ssh демона)

...

27.11.2021 (5:48) 
