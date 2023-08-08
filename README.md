виртуальные машины в стенде: centralRouter, inetRouter, inetRouter2, centralServer<br>
<b>Задачи:</b><br><br>
реализовать knocking port<br>
<br>
centralRouter может попасть на ssh inetRouter через knock скрипт (пример в материалах).<br>
<br>
добавить inetRouter2, который виден(маршрутизируется (host-only тип сети для виртуалки)) с хоста или форвардится порт через локалхост.<br>
<br>
запустить nginx на centralServer.
пробросить 80й порт на inetRouter2 8080.
дефолт в инет оставить через inetRouter.

<h2>схема сети будет такой:</h2>
<img src=".//screenshots//scheme.png"></img>
<img src=".//screenshots//scheme0.png"></img>

В плейбуке реализовано:<br>
в ролях nginx и routing - проброс tcp порта 80 c centralServer, где nginx, на порт 8888 адреса 127.1 хостовой машины, через inetRouter2 - при помощи маскарадинга<br>
в роли knock-script - ограничение подключения к inetRouter при помощи knock скрипта через настройку iptables на inetRouter, добавление скрипта на centralServer.<br>

Подключение c centralRouter на inetRouter возможно только того как на centralRouter отработает скрипт:<br>

```bash
./knock-script.sh 192.168.255.1 5432 5433 5434
```