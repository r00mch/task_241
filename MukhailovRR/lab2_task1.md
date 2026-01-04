# 1.Добавление пользователей user1 и user2
sudo adduser user1
## В процессе создаст домашнюю директорию и запросит пароль
## Проверим оболочку user1
sudo grep user1 /etc/passwd

<img width="539" height="45" alt="image" src="https://github.com/user-attachments/assets/e8cef1e8-3a61-4245-9815-a08d5157721f" />

## Вывод: user1:x:1001:1001:,,,:/home/user1:/bin/bash
## Добавляем user2 с оболочкой sh
sudo adduser user2 --shell /bin/sh
## Проверим оболочку user2
sudo grep user2 /etc/passwd

<img width="591" height="41" alt="image" src="https://github.com/user-attachments/assets/0180d686-c20b-4f4e-bb5f-64f2ba2b45ec" />

## Вывод: user2:x:1002:1002:,,,:/home/user2:/bin/sh

# 2.Назначение групп
## Назначим user1 группу администраторов (sudo)
sudo usermod -aG sudo user1
## Добавим user2 в группу user1
sudo usermod -aG user1 user2

## Проверим группы
groups user1
groups user2 

<img width="686" height="134" alt="image" src="https://github.com/user-attachments/assets/b4f83b98-8e07-45f5-9b9c-86368d954f16" />

# 3.Что такое права доступа?
Права доступа — это система, определяющая, кто и что может делать с файлом/директорией.
Состоит из трёх категорий:
u (user/владелец)
g (group/группа)
o (others/остальные)
И трёх типов прав:
r (read/чтение, 4)
w (write/запись, 2)
x (execute/выполнение, 1)
## Переключимся на user1
sudo su - user1
## Выведем права в домашней директории
ls -la ~/

<img width="843" height="363" alt="image" src="https://github.com/user-attachments/assets/97db1f07-3cba-4a11-8b6f-6378af7d5baf" />

# 4.Как изменить права на файлы?
cd ~
touch all_access.txt
echo "Этот файл доступен всем" > all_access.txt

## Установим все права всем (777 в восьмеричной системе)
chmod 777 all_access.txt
## Проверка 
ls -l all_access.txt

<img width="660" height="173" alt="image" src="https://github.com/user-attachments/assets/d2b2d3dc-59c6-4a79-87bd-f4bbec27460a" />


## -rwxrwxrwx 1 user1 user1 ... all_access.txt
## rwx для владельца, rwx для группы, rwx для остальных

# 5.Учётная запись встроенного администратора
## Посмотреть информацию о root
root — суперпользователь (UID=0, GID=0), имеет полный доступ ко всей системе.
sudo grep root /etc/passwd

<img width="458" height="50" alt="image" src="https://github.com/user-attachments/assets/887476da-341d-43f9-838c-813367426b76" />

# 6.Как выполнить команду от имени администратора?
## sudo перед командой (требует права sudo)
sudo command

# 7.Есть ли ограничения у суперпользователя?
ограничений у суперюзера нету, за исключением случаев когда диск переполнен/сломан, используются неизменяемые файлы

# 8.Удаление юзер2 через юзер1
sudo userdel -r user2

<img width="807" height="246" alt="image" src="https://github.com/user-attachments/assets/1b92729c-58a2-424e-b814-2e1056dcbfef" />

# 9.Изменение владельца папки/файла
mkdir test_folder
echo "test" > test_folder/file.txt
## Сейчас владелец - user1
ls -ld test_folder/
## Изменим владельца на root
sudo chown root:root test_folder/
## Проверим
ls -ld test_folder/
## Меняем только владельца (не группу)
sudo chown user1 test_folder/
## Меняем только группу
sudo chgrp sudo test_folder/
## Изменяем рекурсивно для всей папки
sudo chown -R user1:user1 test_folder/

<img width="788" height="246" alt="image" src="https://github.com/user-attachments/assets/290f36e1-bacb-434f-afac-c1351803876d" />

