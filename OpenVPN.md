# Установка OpenVPN на Openwrt
#### Устанавливаем необходимые пакеты в Openwrt через telnet или ssh
```text
opkg update
opkg install openvpn-openssl
opkg install luci-app-openvpn
```
> поледний пакет для веб морды

Скачиваем конфиг OpenVPN

## Идём в веб морду OpenWRT и устанавливаем конфиг OpenVPN

## Идем `Networking => interface => add`
  
**General Setting**
```text
Protokol = Unmanaged
Device = tun0
```
**Firewall Setting**
```text
Create / Assign firewall-zone = Open+Lan 
```

## Идем в `Firewall => add`
```text
Name = Open
Input = accept
Output = accept
Forward = reject
Masquerading = [X]
MSS clamping = [X]
Covered networks open: = Ethernet Adapter: "tun0"
  

Allow forward to destination zones: = Open+Lan
Allow forward from source zones: = Open+Lan
```
Жмем `Save`
Жмем `Save & Apply`

#### ставим пакет ip. Он как правило потянет за собой свою полную версию ip-full. 
```text
opkg install ip
```
  
#### Смотрим общую таблицу маршрутизации
```text
route -n
```

