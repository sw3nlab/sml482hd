Для того чтобы заставить SML482HD принять участие в распределённых вычислениях на платформе `BOINC`
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


Пример регистрации в отечественном проекте https://gerasim.boinc.ru

Регистриуемся и получаем токен:
`#boinccmd --create_account https://gerasim.boinc.ru ololo@gmail.com my_superpassword my_superlogin`

Добавляем проект в обработку:
`#boinctui --project_attach https://gerasim.boinc.ru 983f234c78b1ff3245` 

Запускаем псевдо-графический интерфейс для отслеживания прогресса вычислений:
`#boinctui`
