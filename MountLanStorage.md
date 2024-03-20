# Сетевое хранилище на OpenWrt

#### Установка необходимых пакетов
```text
opkg update
opkg install kmod-usb-storage
opkg install kmod-fs-f2fs
opkg install f2fs-tools
opkg install block-mount
opkg install samba4-server luci-app-samba4
```
