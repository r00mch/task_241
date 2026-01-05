# 1.Что такое systemd юнит?
Systemd юнит — это конфигурационный файл, который описывает, как systemd должен управлять системными ресурсами.
Основные типы юнитов:
.service — фоновые службы/демоны (nginx, ssh, mysql)
.socket — сетевые сокеты для активации по запросу
.timer — задачи по расписанию (аналог cron)
.mount — точки монтирования файловых систем
.device — управление устройствами
.target — группы юнитов (аналог runlevels)

# 2.Проверка статуса systemd юнита
sudo systemctl status ssh
<img width="908" height="156" alt="image" src="https://github.com/user-attachments/assets/f319545e-6d1c-4adb-ac9d-413c3716c069" />

# 3.Остановка сервиса
## 1.Сначала проверим текущий статус
sudo systemctl status nginx

## 2.Остановим сервис nginx
sudo systemctl stop nginx

## 3.Проверим что сервис остановился
sudo systemctl status nginx
<img width="1017" height="420" alt="image" src="https://github.com/user-attachments/assets/72a69612-41e4-482f-bd0c-8ca083c64adc" />

# 4.Перезапуск сервиса
sudo systemctl restart nginx
<img width="1265" height="650" alt="image" src="https://github.com/user-attachments/assets/f36d2d82-d253-4c26-b668-a40ada599da7" />

# 5.Удаление с автозагрузки
sudo systemctl disable ssh

#№ 2. Проверим статус автозагрузки
sudo systemctl is-enabled ssh
#№ Вернёт: disabled
<img width="885" height="169" alt="image" src="https://github.com/user-attachments/assets/80ae9b62-9ec1-4696-9a69-f5e5d881c6bd" />

# 6. Возврат в автозагрузку
## 1. Вернём SSH в автозагрузку
sudo systemctl enable ssh
## 2. Проверим
sudo systemctl is-enabled ssh
<img width="872" height="262" alt="image" src="https://github.com/user-attachments/assets/5feedf0e-5bc3-4eb0-8a65-1e78f4c02935" />

# 7.Что такое таймеры?
Systemd таймеры — это юниты для запуска задач по расписанию
Преимущества таймеров:

Могут запускать любые systemd юниты (не только команды)
Имеют зависимости между юнитами
Логи интегрированы в systemd journal
Более точное планирование
## Посмотреть все таймеры
systemctl list-timers

