# 1. Установка Samba 
## Обновление пакетов и установка Samba
sudo apt update
sudo apt install samba samba-common-bin -y
## Проверка установки
samba --version

<img width="388" height="44" alt="image" src="https://github.com/user-attachments/assets/a33544b9-a756-4096-9268-b2cbe16052d2" />

# 2.Что такое общая папка и зачем она нужна?
Общая папка (Shared Folder) - это директория в сети, к которой могут подключаться несколько пользователей с разных компьютеров.
Зачем нужна:
Обмен файлами между компьютерами в сети
Совместная работа над документами
Централизованное хранение данных
Доступ к файлам с разных операционных систем (Windows, Linux, macOS)
Создание сетевых архивов и бэкапов
# 3.Общая папка без пароля, только чтение
## Создаем папку
sudo mkdir -p /srv/share/readonly

## Настраиваем права
sudo chmod 755 /srv/share/readonly
sudo chown nobody:nogroup /srv/share/readonly
<img width="549" height="46" alt="image" src="https://github.com/user-attachments/assets/e0239544-3a41-4e01-8fc6-edd498339b4b" />


## Добавляем в конфигурацию Samba
sudo tee -a /etc/samba/smb.conf << 'EOF'
[readonly_share]
   comment = Public Read-Only Share
   path = /srv/share/readonly
   browseable = yes
   writable = no
   guest ok = yes
   read only = yes
   create mask = 0644
   directory mask = 0755
EOF
<img width="813" height="463" alt="image" src="https://github.com/user-attachments/assets/56b3c501-c3a6-48ed-8e19-9e00ae105148" />
# 5.Общая папка с паролем, чтение и запись
## Создаем пользователя для Samba
sudo useradd -M -s /usr/sbin/nologin sambauser
sudo smbpasswd -a sambauser
<img width="721" height="156" alt="image" src="https://github.com/user-attachments/assets/d14abfac-787c-4ce7-8d79-7dfc3d138f87" />

## Создаем папку
sudo mkdir -p /srv/share/secure
## Настраиваем права
sudo chmod 770 /srv/share/secure
sudo chown sambauser:sambauser /srv/share/secure
## Добавляем в конфигурацию
sudo tee -a /etc/samba/smb.conf << 'EOF'
<img width="700" height="156" alt="image" src="https://github.com/user-attachments/assets/72d66cd5-187e-48dc-8f4d-d65ff4af086f" />

[secure_share]
   comment = Secure Share with Password
   path = /srv/share/secure
   browseable = yes
   writable = yes
   valid users = sambauser
   read only = no
   create mask = 0770
   directory mask = 0770
EOF

# 6.Общая папка для группы с полными правами
## Создаем группу и пользователей
sudo groupadd project_group
sudo useradd -M -s /usr/sbin/nologin -G project_group user1
sudo useradd -M -s /usr/sbin/nologin -G project_group user2

## Добавляем пользователей в Samba
sudo smbpasswd -a user1
sudo smbpasswd -a user2

## Создаем папку
sudo mkdir -p /srv/share/project

## Настраиваем права
sudo chmod 2770 /srv/share/project   ---- sticky bit для наследования группы
sudo chown root:project_group /srv/share/project

## Добавляем в конфигурацию
sudo tee -a /etc/samba/smb.conf << 'EOF'

[project_share]
   comment = Project Group Share
   path = /srv/share/project
   browseable = yes
   writable = yes
   valid users = @project_group
   read only = no
   create mask = 0770
   directory mask = 0770
   force group = project_group
EOF
<img width="778" height="690" alt="image" src="https://github.com/user-attachments/assets/cf1ccbb4-902f-4121-a367-e062b1a633dd" />
# 7. Общая папка с разными правами для разных групп
## Создаем группы
sudo groupadd managers
sudo groupadd employees
sudo groupadd contractors

## Создаем пользователей
sudo useradd -M -s /usr/sbin/nologin -G managers manager1
sudo useradd -M -s /usr/sbin/nologin -G employees employee1
sudo useradd -M -s /usr/sbin/nologin -G contractors contractor1

## Добавляем в Samba
sudo smbpasswd -a manager1
sudo smbpasswd -a employee1
sudo smbpasswd -a contractor1

## Создаем папку
sudo mkdir -p /srv/share/department

sudo chmod 2770 /srv/share/department
sudo chown root:managers /srv/share/department

sudo tee -a /etc/samba/smb.conf << 'EOF'

[department_share]
   comment = Department Share with Different Permissions
   path = /srv/share/department
   browseable = yes
   
   write list = @managers
   read list = @managers
   valid users = @managers, @employees
   
   ## указываем через отрицание
   
   # Подрядчикам доступ запрещен
   invalid users = @contractors

   create mask = 0770
   directory mask = 0770
   force group = managers
   
   inherit permissions = yes
   inherit owner = yes
EOF
<img width="1167" height="677" alt="image" src="https://github.com/user-attachments/assets/55ba35ee-f154-4b8b-bce6-ba585ac265f6" />
# Перезапуск Samba и проверка
## Проверяем конфигурацию
sudo testparm
## Перезапускаем Samba
sudo systemctl restart smbd nmbd
## Проверяем статус
sudo systemctl status smbd
## Просмотр общих папок
sudo smbclient -L localhost -U%

# Тестирование доступа 
## Тест 1: Анонимный доступ (только чтение)
smbclient //localhost/readonly_share -N

## Тест 2: Доступ с паролем
smbclient //localhost/secure_share -U sambauser

## Тест 3: Доступ для группы project
smbclient //localhost/project_share -U user1

## Тест 4: Проверка разных прав
smbclient //localhost/department_share -U manager1  # полный доступ
smbclient //localhost/department_share -U employee1  # только чтение
smbclient //localhost/department_share -U contractor1  # доступ запрещен
<img width="741" height="573" alt="image" src="https://github.com/user-attachments/assets/5020251b-90f3-428b-a8d0-81b3d90812ef" />
