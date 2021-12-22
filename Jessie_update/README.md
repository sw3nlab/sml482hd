Обновится до `jessie` можно следующим образом.

0 - собираем файловую систему `wheezy` через `debootstrap`

1 - устанавливаем только `openssh-server`

2- берём ядро `3.14` из этой директории и заливаем его вместо `3.3`

3 - Загружаемся... заходим по ssh на приставку.

Уладяем или комментируем строчку в `/apt/sources.list` добавлением знака `#`

```php
#deb http://archive.debian.org/debian wheezy main
```

добавляем адрес репозитория `jessie`

```php
deb http://archive.debian.org/debian jessie main contrib non-free
```

сохраняем и делаем

`apt-get update` затем `apt-get upgrade`

