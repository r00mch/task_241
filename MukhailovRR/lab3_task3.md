# 1.Содержимое fstab

cat /etc/fstab

Что хранится в fstab?

Файл /etc/fstab содержит информацию о файловых системах, которые должны монтироваться автоматически при загрузке системы. 

<img width="780" height="214" alt="image" src="https://github.com/user-attachments/assets/34478ced-26a9-4a46-91b2-8977d7c02daa" />

# 2.Добавление нового диска в VirtualBox

Добавим диск на выключенной системе в VM, создал VDI диск 

# 3.Проверка нового диска системой

lsblk | grep -v "loop\|sr0"

показывает все созданные диски, на скрине видно новый дист с 5.2 гб 

<img width="740" height="134" alt="image" src="https://github.com/user-attachments/assets/ca6f534a-8f81-41b0-b633-6ae1a1caac5b" />

# 4.Создание раздела и файловой системы

## 1. Создаем раздел на новом диске (предположим это /dev/sdb)

<img width="942" height="537" alt="image" src="https://github.com/user-attachments/assets/5d1af2ca-54a6-4eb9-bf62-ea6870d354ce" />

## 2.Создание файловой системы 

sudo mkfs.ext4 /dev/sdb1

<img width="788" height="271" alt="image" src="https://github.com/user-attachments/assets/387c2270-68cd-431f-b5be-f40f4899c40d" />

sudo blkid /dev/sdb1

провреим создание

<img width="1063" height="47" alt="image" src="https://github.com/user-attachments/assets/3791a6e4-12a5-423d-8ebf-824592d2c8ca" />

# 5.Монтирование диска

## 1. Создаем точку монтирования

sudo mkdir /mnt/newdisk

## 2. Монтируем диск

sudo mount /dev/sdb1 /mnt/newdisk

## 3. Проверяем монтирование

df -h /mnt/newdisk

mount | grep newdisk

<img width="650" height="155" alt="image" src="https://github.com/user-attachments/assets/6b20e1f5-4eb6-474a-a28d-efe504495a2d" />

# 6.Создание файлов на диске

## Переходим в каталог

cd /mnt/newdisk

## Создаем файлы

sudo touch file{1..5}.txt

sudo echo "Test data" > testfile.txt - реализовал с помощью tee тк выяснил что владелец root и права ограничены 

sudo mkdir testdir

## Смотрим содержимое

ls -la

<img width="1013" height="485" alt="image" src="https://github.com/user-attachments/assets/472839fb-7f24-4f8e-9106-c9e4c24bd624" />

# 7. Размонтировка и проверка  

## Выходим из каталога

cd /

## Размонтируем

sudo umount /mnt/newdisk

## Проверяем, что диск отмонтирован

ls /mnt/newdisk/  # усто

## Снова монтируем

sudo mount /dev/sdb1 /mnt/newdisk

## Проверяем файлы

ls /mnt/newdisk/  # Файлы на месте

<img width="781" height="133" alt="image" src="https://github.com/user-attachments/assets/487d1b5e-fc60-4bec-9708-62aedb4799ab" />

# 8.Добавляем в fstab для автомонтирования

## Получаем UUID нового диска

NEW_UUID=$(sudo blkid -s UUID -o value /dev/sdb1)

echo "UUID нового диска: $NEW_UUID"

## Добавляем в fstab

echo "UUID=$NEW_UUID /mnt/newdisk ext4 defaults 0 0" | sudo tee -a /etc/fstab

## Проверяем добавление

tail -2 /etc/fstab

<img width="1025" height="196" alt="image" src="https://github.com/user-attachments/assets/4a5ea5cf-c450-4923-9b4e-19e3785e8675" />

# 9.Проверка корректности fstab 

## Проверяем синтаксис

sudo mount -a

## проверяем монтирование

mount | grep newdisk

## Перезагружаем для проверки

sudo reboot

# 10.После перезагрузки проверяем:

## Проверяем, что диск автомонтировался

mount | grep newdisk

## Проверяем файлы

ls -la /mnt/newdisk/

<img width="792" height="354" alt="image" src="https://github.com/user-attachments/assets/ab0b5a11-1b9d-4aee-9024-64d1bd84b9e3" />


