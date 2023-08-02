виртуальные машины в стенде: centralRouter, inetRouter, inetRouter2, centralServer
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