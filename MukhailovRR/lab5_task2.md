# Скрипт /usr/local/bin/create_info_files.sh
#!/bin/bash

## Переходим в домашнюю директорию пользователя
cd ~
FOLDER_NAME="system_info_collection"
if [ ! -d "$FOLDER_NAME" ]; then
    mkdir -p "$FOLDER_NAME"
fi
## Создаём файлы 1-4
for i in {1..4}; do
    if [ ! -f "$FOLDER_NAME/file$i.txt" ]; then
        touch "$FOLDER_NAME/file$i.txt"
    fi
done
## Информация в файл 1
echo "Дата: $(date)" > "$FOLDER_NAME/file1.txt"
## Информация в файл 2
echo "Ядро: $(uname -r)" > "$FOLDER_NAME/file2.txt"
echo "Хост: $(hostname)" >> "$FOLDER_NAME/file2.txt"
## Информация в файл 3
ls -la ~/ > "$FOLDER_NAME/file3.txt"
## Информация в файл 4
echo "Пользователь: $(whoami)" > "$FOLDER_NAME/file4.txt"
echo "Директория: $(pwd)" >> "$FOLDER_NAME/file4.txt"
free -h | grep Mem >> "$FOLDER_NAME/file4.txt"

# Systemd юнит /etc/systemd/system/create-info.service
[Unit]
Description=Create system info files
After=network.target

[Service]
Type=oneshot
User=info_user
Group=info_user
WorkingDirectory=/home/info_user
ExecStart=/usr/local/bin/create_info_files.sh

[Install]
WantedBy=multi-user.target
# Systemd таймер /etc/systemd/system/create-info.timer
[Unit]
Description=Run create-info every 5 minutes
Requires=create-info.service

[Timer]
OnBootSec=1min
OnUnitActiveSec=5min
Unit=create-info.service

[Install]
WantedBy=timers.target
# Создание пользователя
sudo useradd -m -s /usr/sbin/nologin info_user
# Включение и проверка
sudo systemctl daemon-reload
sudo systemctl enable --now create-info.timer
sudo systemctl start create-info.service

## Проверка
sudo ls -la /home/info_user/system_info_collection/
sudo cat /home/info_user/system_info_collection/file1.txt

# Проверка логов
sudo journalctl -u create-info.service -n 5
sudo systemctl list-timers --all | grep create-info
