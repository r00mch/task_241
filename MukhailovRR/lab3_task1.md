# 1.Какие файловые системы вы знаете?
Windows: NTFS, FAT32, exFAT, ReFS
Linux/Unix: ext2, ext3, ext4, XFS, Btrfs, ZFS, JFS, ReiserFS
macOS: HFS+, APFS
# 2.Классификация файловых систем
По назначению:
Основные (ext4, XFS, NTFS) - для данных
Журналируемые (ext3, ext4, XFS) - с записью изменений
Сетевые (NFS, CIFS) - для доступа по сети
Виртуальные (procfs, sysfs) - для информации о системе
Временные (tmpfs) - в оперативной памяти
По структуре:
Блочные (ext4, NTFS) - работа с блоками данных
Файловые в памяти (tmpfs)
Базы данных (ReiserFS)
Отличия:
Производительность: XFS быстрее для больших файлов
Надежность: журналируемые устойчивее к сбоям
Возможности: Btrfs/ZFS поддерживают snapshots
Ограничения: FAT32 - файлы до 4GB
# 3.Файловые системы в Linux
## Посмотреть используемые ФС
df -T
Распространенные:
ext4 - стандартная для большинства дистрибутивов
XFS - для серверов, больших файлов
Btrfs - с snapshots и сжатием
ext3 - устаревшая, но надежная
ext2 - без журналирования
# 4.Создание файловой системы на диске
## 1. Найти диск
sudo fdisk -l
## 2. Создать раздел (например, /dev/sdb1)
sudo fdisk /dev/sdb
## в fdisk: n (новый), p (primary), 1 (номер), Enter (размер), w (записать
## 3. Создать ФС ext4
sudo mkfs.ext4 /dev/sdb1
## 4. Создать XFS
sudo mkfs.xfs /dev/sdb1
## 5. Создать FAT32
sudo mkfs.fat -F 32 /dev/sdb1

# 5.Монтирование диска
Монтирование - подключение файловой системы к дереву каталогов.
## 1. Создать точку монтирования
sudo mkdir /mnt/mydisk
## 2. Примонтировать
sudo mount /dev/sdb1 /mnt/mydisk
## 3. Автомонтирование при загрузке=
## Добавить в /etc/fstab:
/dev/sdb1 /mnt/mydisk ext4 defaults 0 0
## 4. Размонтировать
sudo umount /mnt/mydisk
# 6.Специальные файловые системы
procfs - информация о процессах# Монтирована в /proc
cat /proc/mounts | grep proc
sysfs - информация о ядре и устройствах
## Монтирована в /sys
cat /proc/mounts | grep sysfs
mount | grep sysfs
tmpfs - временные файлы в RAM
## Обычно в /dev/shm, /tmp, /run
cat /proc/mounts | grep tmpfs
mount | grep tmpfs
# 7.Информация о системе через cat
Процессор 
## 1. Основная информация о CPU
cat /proc/cpuinfo
## 2. Только модель и ядра
cat /proc/cpuinfo | grep -E "model name|cpu cores"
## 3. Частота процессора
cat /proc/cpuinfo | grep "MHz"
## 4. Информация о кэше
cat /proc/cpuinfo | grep "cache size"
Состояние памяти 
## 1. Полная информация о памяти
cat /proc/meminfo
## 2. Использование памяти
cat /proc/meminfo | grep -E "MemTotal|MemFree|MemAvailable"
## 3. Swap память
cat /proc/meminfo | grep -E "SwapTotal|SwapFree"
## 4. Краткий вывод
cat /proc/meminfo | head -10
Еще команды для вывода инфы о системе 
## Версия ядра
cat /proc/version
## Информация о системе
cat /proc/sys/kernel/{ostype,hostname,osrelease,version}
## Загруженные модули
cat /proc/modules | head -20
## Сетевые интерфейсы
cat /proc/net/dev
## Статус системы
cat /proc/stat
