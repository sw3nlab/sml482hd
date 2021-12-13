Для удалённой работы неплохо показал себя `tightvncserver`

```php
SML@debian#apt-get install tightvncserver
SML@debian#vncserver
...
set passwd
and view-only-mode passwd
```
Для остановки процесса можно юзать:
`vncserver -kill :1`

Подлючаемся с любого vnc клиента (я использовал android vnc viewer)
если sml имеет адрес 192.168.2.111

в клиенте указываем 192.168.2.111:5901

[!image]()
