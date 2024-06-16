*   Клонируем репозиторий

```console
~$ git clone https://github.com/catfromnewjercy/hp-firmware.git
```


*   Для установки ОС на сервер, нам необходимо сделать либо загрузочную флешку с установщиком с помощью Balena Etcher, или подгрузить ISO в Virtual CD/DVD в iLO Сервера.
*   После авторизации переходим во вкладку Firmware & OS Software
*   После загрузки с ISO, необходимо нажать Try to install Ubuntu
*   Появится меню выбора языка, выбрать English


![Alt text](/instruction/ubuntu_install/pictures/1choose_language.png?raw=true "Language Screen")

*   Отказываемся от обновления установщика на 22.04

![Alt text](/instruction/ubuntu_install/pictures/2update_ask1.png?raw=true "UpdateAsk Screen")

*   В следующем окне выбираем тип установки Ubuntu server

![Alt text](/instruction/ubuntu_install/pictures/3server_select.png?raw=true "Server Screen")

*   Выбираем layout

![Alt text](/instruction/ubuntu_install/pictures/4layout.png?raw=true "Layout")

*   Настраиваем нужный нам сетевой интерфейс или оставляем всё в DHCP, нажимаем Done

![Alt text](/instruction/ubuntu_install/pictures/5network.png?raw=true "Network")

*   Указываем ссылку на зеркало репозиториев

![Alt text](/instruction/ubuntu_install/pictures/6repository.png?raw=true "Repo")

*   Выбираем наш диск и автоматическую разметку и жмёшь Done

![Alt text](/instruction/ubuntu_install/pictures/7disk.png?raw=true "Network")

*   Задаём пользовательские настройки

![Alt text](/instruction/ubuntu_install/pictures/8hostnameanduser.png?raw=true "Network")

*   Жмём Done, потом Install, дожидаемся конца установки, отцепляем загрузочный ISO или вытаскиваем флешку и ожидаем загрузку ОС, после которой логинимся под своим пользвателем

*   После загрузки ОС, нам необходимо расширить / раздел (root), так как установщик размечает только 100 Gb области диска, для этого необходимо выполнить следующие шаги:

1. Проверяем размер текущего Logical Volume:

```
# lvs
  LV        VG        Attr       LSize    Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  ubuntu-lv ubuntu-vg -wi-ao---- <100.16g
```
Как видим, размер вольюма 100 Gb

2. Проверяем доступный объём в Physical Volume

```
# pvs
  PV         VG        Fmt  Attr PSize    PFree
  /dev/sda3  ubuntu-vg lvm2 a--  <742.16g    0
```
3. Увеличиваем Logical Volume Size

```
# lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
```
4. Увеличиваем размер файловой системы

```
# resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```