Для того чтобы заставить SML482HD принять участие в распределённых вычислениях на платформе `BOINC`
нужно:

Установить следующие пакеты:
```php
#apt-get install boinc-client boinctui
```
пакет `boinctui` это консольный графический интерфейс, доступен в Jessie (но впринипе можно и без него)

Регистрируемся в интересном для нас проекте:

Список проектов можно посмотреть тут:
https://boinc.berkeley.edu/projects.php


Зарегистрироватся можно как на сайте проекта, так и через установленый выше `boinc-client`



`#boinccmd --run_benchmarks` <-- запуск тестов для определения вычислительных мощностей

`#boinccmd --get_messages` <---- вывод сообщений boinc ядра