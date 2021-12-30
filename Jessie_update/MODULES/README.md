Модули для подключения Serial конвертеров на чипах `ch341, cp210x, pl2303` и т.д

Модули собраны и проверены с версией ядра 3.14.28 ( kernel314 ) и могут не работать в других версиях.

Подгрузить нужный модуль можно слудующим образом.

```php
wget --no-check-certificate https://github.com/sw3nlab/sml482hd/raw/master/Jessie_update/MODULES/usbserial.ko
wget --no-check-certificate https://github.com/sw3nlab/sml482hd/raw/master/Jessie_update/MODULES/ch341.ko
cp usbserial.ko /lib/modules/3.14.28-1.27/
depmod -a
modprobe usbserial
insmod ch341.ko
```
