![image](https://github.com/sw3nlab/sml482hd/blob/master/MODULES/Screenshot_20211217-031358_Termius.png)

Рабочие модули ядра `sml (smartlabs) 3.3.8-3.3` для wi-fi железа на чипах `Realtek` 

Подгружаются через `insmod [modulename.ko]`

```php
SML@debian#wget --no-check-certificate https://github.com/sw3nlab/sml482hd/archive/refs/heads/master.zip
SML@debian#unzip master.zip
SML@debian#insmod sml482hd-master/MODULES/8192eu.ko
```

================================

Модули собраны одним из пользователей `4pda` с ником `shatle03`

https://4pda.to/forum/index.php?showtopic=965786&st=9340#entry97407540


<i> для тех кто желает собирать модули под sml ядро самостоятельно... может попросить у него `linux-headers` с которыми происходила сборка =)</i>
