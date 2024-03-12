# Подключение USB-флешки для расширения памяти в Openwrt
#### Устанавливаем необходимые пакеты в Openwrt через telnet или ssh
```text
opkg update
opkg install block-mount kmod-usb-core kmod-usb2 kmod-usb-ohci kmod-usb-storage kmod-fs-ext4 vsftpd
```
Подготовка USB-флешки:
Как правила на OpenWRT нет ничего. По этому устанавливаем DOS (мне нравится делать через нее)
```text
opkg install cfdisk
```
проверяем как смонтирован диск
```text
fdisk -l
```
обычно система пишет название дисков (бренд и размер) но с китайцами может и не отображать в некоторых случаях.
В моем случаи это DataTrevel 2.0 (1GB)

#### Запускаем DOS и делаем разметку диска
```text
cfdisk /dev/sda 
```
Выбираем `dos` 

#### Создаем разделы 
- [X] /dev/sda1   
выбираем "***type***" далее строка _82 Linux swap / Solatis_ - swap раздел  
- [X] /dev/sda2  
основной раздел куда будем ставить "***/overlay***"  
- [X] /dev/sda3  
основной раздел куда будем ставить "***для файлов***"  

_Далее заходим в пункт "**Write**" пишем "**yes**" и далее "**Quit**"_  

#### Далeе пишем команды для нормальной работы usb drive
```text
# mkswap /dev/sda1
# swapon /dev/sda1
# mkfs.ext4 /dev/sda2
# mkfs.ext4 /dev/sda3
```

#### Cоздадим папоку
```text
mkdir /mnt/sda2
```

#### Монтируем наш основной раздел в папку /mnt/sda2
```text
mount /dev/sda2 /mnt/sda2
```
Переносим содержимое каталога **/overlay** на наш раздел
```text
tar -C /overlay -cvf - . | tar -C /mnt/sda2 -xf -
```

### Пишем конфиг для монтирования в `/etc/config/fstab`

```text
nano -c /etc/config/fstab
```
   
Вставляем следующее:
```text
config 'mount'
option uuid 'fe154992-578f-430b-a7c9-3f38ce037a70'
option target '/overlay'
option enabled '1'

config 'mount'
option target '/mnt/usb'
option uuid '7a7bf810-b7ce-4241-91eb-583a4b56105b'
option enabled '1'

config 'global'
option anon_swap '0'
option anon_mount '0'
option auto_swap '1'
option auto_mount '1'
option delay_root '5'
option check_fs '0'

config 'swap'
option device '/dev/sda1'
option enabled '1'

config 'mount'
option target '/mnt/sda2'
option uuid 'fe154992-578f-430b-a7c9-3f38ce037a70'
option enabled '0'

config 'mount'
option target '/mnt/sda3'
option uuid '7a7bf810-b7ce-4241-91eb-583a4b56105b'
option enabled '0'
```
> Нажми `CTRL+O` далее `Enter`, далее `CTRL+X`
> Все uuid необходимо использовать из вашего системы.
