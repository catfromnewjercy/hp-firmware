*   Настройка системных разделов

    Для установки ОС, нам необходимо на базе контроллера создать конфигурацию RAID в режиме MIRROR - 2 ssd в RAID1, представить его в виде Virtual Disk

*   Все остальные диски нам необходимо предоставить в режиме JBOD, для дальнейшей настройки zfs.

*   Важное примечание: в зависимости от конфигурации сервера, расположение дисков может быть разное, если ситуация не ясна, лучше проконсультироваться.


*   Получаем список доступных блочных устройств в системе

```
# lsblk

# lsblk
NAME                      MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0                       7:0    0  63.9M  1 loop /snap/core20/2182
loop1                       7:1    0  63.9M  1 loop /snap/core20/2318
loop2                       7:2    0    87M  1 loop /snap/lxd/27428
loop3                       7:3    0    87M  1 loop /snap/lxd/28373
loop4                       7:4    0  39.1M  1 loop /snap/snapd/21184
loop5                       7:5    0  38.8M  1 loop /snap/snapd/21759
sda                         8:0    0   2.2T  0 disk 
sdb                         8:16   0   2.2T  0 disk 
sdc                         8:32   0   2.2T  0 disk 
sdd                         8:48   0   2.2T  0 disk 
```

*   Как видим, у нас есть устройства sda, sdb, sdc, sdd размеров 2.2 террабайт

*   Для добавления дисков в пул zfs, необходимо для начала найти для этих дисков нужные нам id

```
# ls -al /dev/disk/by-id/ | grep sda
```

*   Находим такую строчку:

```
lrwxrwxrwx 1 root root    9 Jun 13 15:55 wwn-0x5000039cf83348e9 -> ../../sda
```

*   wwn-0x5000039cf83348e9 - это и есть наш id диска, с которым мы будем взаимодействовах, повторяем процесс для всех дисков предназначенных для zfs, записываем id куда-нибудь для дальнейшего использования.


*   После того, как мы получили disk-id, можем приступать к сборке пула zfs raidz2

```
# zpool create DATASET raidz2 /dev/disk/by-id/wwn-0x5000039cf83348e9 /dev/disk/by-id/wwn-0x5000039cf833466d /dev/disk/by-id/wwn-0x5000039cf83325a5 /dev/disk/by-id/wwn-0x5000039cf833499d /dev/disk/by-id/wwn-0x5000039cf83348f9 /dev/disk/by-id/wwn-0x5000039cf83328f1
```

*   В команде выше мы перечисляем все id дисков, записанные ранее.
*   Нажимает enter

*   После завершения проверяем, что пул создался
```
# zpool status
  pool: DATASET
 state: ONLINE
  scan: scrub repaired 0B in 00:00:00 with 0 errors on Tue Jun 11 21:49:03 2024
config:

        NAME                        STATE     READ WRITE CKSUM
        DATASET                     ONLINE       0     0     0
          raidz2-0                  ONLINE       0     0     0
            wwn-0x5000039cf83348e9  ONLINE       0     0     0
            wwn-0x5000039cf833466d  ONLINE       0     0     0
            wwn-0x5000039cf83325a5  ONLINE       0     0     0
            wwn-0x5000039cf833499d  ONLINE       0     0     0
            wwn-0x5000039cf83348f9  ONLINE       0     0     0
            wwn-0x5000039cf83328f1  ONLINE       0     0     0
            wwn-0x5000039cf832c021  ONLINE       0     0     0
```


*   Проверяем полезный объём данных на пуле

```
# zfs list
NAME      USED  AVAIL     REFER  MOUNTPOINT
DATASET  1.41M  24.9T      236K  /DATASET
```