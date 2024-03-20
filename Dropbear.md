# Нет доступа по ssh
Заходим через браузер System => Administration => SSH Access 
```text
**Interface** = LAN
Password authentication = [X]
Allow root logins with password = [X]
Gateway Ports = [X]
```
> Нажимаем **Save & Apply**

Примечание
Можно произвести перезапуск из браузерной консоли командой
```text
/etc/init.d/dropbear restart
```
