### ====== DISTRIBUTED.NET ======

SML482HD может принять участие в распределённых вычислениях на платформе:

https://www.distributed.net/

> Вычислительные мощности участников проекта `OGR-28` занимаются доказательством оптимальности линейки Голомба 28-го порядка, который на 4 сентября 2021 года выполнен на 87,87% Подробнее: https://ru.wikipedia.org/wiki/%D0%9B%D0%B8%D0%BD%D0%B5%D0%B9%D0%BA%D0%B0_%D0%93%D0%BE%D0%BB%D0%BE%D0%BC%D0%B1%D0%B0

Практическое применение результатов вычислений:
> Одним из практических применений линейки Голомба, является использование её в фазированных антенных решётках — например, в радиотелескопах. Антенны с конфигурацией [0 1 4 6] можно встретить в базовых станциях сотовой связи стандарта CDMA.

Список поддерживаемых в проекте архитектур внушителен:

https://www.distributed.net/Download_clients


Забираем клиента 
-распакрвываем;
-смотрим хэлп;
-делаем бенчмарки;
-конфигурируем;
-запускаем;
```php
wget http://http.distributed.net/pub/dcti/current-client/dnetc-linux-mips32el.tar.gz
tar -xf dnetc-linux-mips32el.tar.gz
./dnetc --help
./dnetc -benchmark
./dnetc -config
./dnetc
```

### ======= BOINC =======

<details>
  <summary>Вычисления на платформе BOINC (на 26.01.2022 нет заданий для устройств с mips архитектурой)</summary>
Для того чтобы заставить TV-приставку `SML482HD` принять участие в распределённых вычислениях на платформе `BOINC`
нужно:

Установить следующие пакеты:
```php
#apt-get install boinc-client boinctui
```
пакет `boinctui` это консольный псевдо-графический интерфейс, доступен в Jessie (но впринипе можно и без него)


Регистрируемся в интересном для нас проекте:

Список проектов можно посмотреть тут:
https://boinc.berkeley.edu/projects.php


Зарегистрироватся можно как на сайте проекта, так и через установленый выше `boinc-client`

вот некоторые консольные команды `boinc-client`'a:

`#boinccmd --run_benchmarks` <-- запуск тестов для определения вычислительных мощностей

`#boinccmd --get_messages` <---- вывод сообщений boinc ядра

`#boinccmd --lookup_account URL email password` <--- получение токена (если есть аккаунт в проекте)

`#boinccmd --create_account URL_проекта email password login` <--- регистрация в проекте 

(если регистрация прошла успешно, получаем token вида 04b168............876 для добавления )

`#boinccmd --project_attach URL_проекта token` <-- добавление проекта к списку



### Пример регистрации в отечественном проекте https://gerasim.boinc.ru

Регистриуемся и получаем токен:

`#boinccmd --create_account https://gerasim.boinc.ru ololo@gmail.com my_superpassword my_superlogin`

Добавляем проект в обработку:

`#boinctui --project_attach https://gerasim.boinc.ru 983f234c78b1ff3245` 

Запускаем псевдо-графический интерфейс для отслеживания прогресса вычислений:

`#boinctui`


> сегодня 26.01.2022 на данный момент мною не было найдено ни одного проекта который бы выдавал задания устройствам c `mipsel` архитектурой. =(
в то время как многие проекты выдают задания микрокомпьютерам `Raspberry Pi` с `ARM` архитектурой.
возможно после переговоров с организаторами проектов они добавят такую возможность. но когда это будет и будет ли никто не знает )  
</details>
