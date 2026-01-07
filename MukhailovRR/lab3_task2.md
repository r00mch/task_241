# 1.Структура каталогов Linux

ls -la /

<img width="754" height="374" alt="image" src="https://github.com/user-attachments/assets/41e9c28c-cc2a-4344-8986-d78123daeb24" />

# 2.Папки пользователей 

ls -la /home/

<img width="624" height="131" alt="image" src="https://github.com/user-attachments/assets/649c82cc-3eb2-4ca5-8a64-e58f15036e3c" />

# 3.Домашняя папка суперюзера

sudo ls -la /root

<img width="770" height="263" alt="image" src="https://github.com/user-attachments/assets/59f6e1da-1d4f-4374-9b71-66c8858a7b22" />

# 4.Основные конфигурационные файлы

ls -la /etc/

<img width="971" height="573" alt="image" src="https://github.com/user-attachments/assets/6a4f802e-27ec-423e-ab46-805f9405879b" />

# 5.Разница между /bin, /sbin, /usr/bin, /usr/sbin

ls /bin | head -10

<img width="502" height="237" alt="image" src="https://github.com/user-attachments/assets/867dafe5-eec8-4dee-b0c6-ce32bacb1725" />

ls /sbin | head -10

<img width="621" height="244" alt="image" src="https://github.com/user-attachments/assets/70a11ad9-d541-49f8-a4ab-5633ea7af2dd" />

ls /usr/bin | head -10

<img width="628" height="240" alt="image" src="https://github.com/user-attachments/assets/a520314f-8998-480b-8bdd-f5f3a906c720" />

ls /usr/sbin | head -10

<img width="556" height="254" alt="image" src="https://github.com/user-attachments/assets/8ebfd4de-6629-4b8d-be3a-3755f1d7c76c" />

Каталог /bin для всех пользователей, содержит основные команды для работы в single user mode: ls, cp, bash, cat
Каталог /sbin только для root, содержит системные утилиты для администрирования и восстановления: fdisk, iptables 
Каталог /usr/bin для всех пользователей, содержит основные пользовательские программы^ python3, ssh
Каталог /usr/sbin только для root, содержит системные утилиты: sshd, nginx, useradd
