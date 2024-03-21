# Сетевое хранилище на OpenWrt

#### Установка необходимых пакетов
```text
opkg update
opkg install kmod-fs-cifs kmod-crypto-hmac kmod-crypto-md5 kmod-crypto-misc
opkg install kmod-nls-utf8 kmod-nls-base cifsmount
opkg install kmod-usb-storage kmod-fs-f2fs f2fs-tools block-mount samba4-server luci-app-samba4
```


#### Cоздадим папку куда будем монтировать сетевый диски
```text
mkdir /mnt/win
mkdir /mnt/win/arhiv
```

#### Монтируем сетевой диск  
для **UNIX/Darwin**  
Подключение сетевых дисков с аутентификацией
```text
mount -t cifs //cifs-server/share /localfolder -o user=username,password=password
```
Тоже самое, но с дополнительными, уточняющими параметрами
```text
mount -t cifs //cifs-server/share /localfolder -o unc=\\\\cifs-server\\share,ip=IP-Address,user=john,pass=doe,dom=workgroup
```
\
для **Windows**  
Анонимное или гостевое подключение сетевых дисков
```text
mount -t cifs '\\cifs-server\share' /localfolder -o guest,iocharset=utf8,file_mode=0777,dir_mode=0777,nounix,noserverino
```

Подключение сетевых дисков с аутентификацией
```text
mmount -t cifs '\\192.168.1.10/Arhiv' /mnt/win/arhiv -o iocharset=utf8,file_mode=0777,dir_mode=0777,nounix,noserverino,username=User,password=password
```
