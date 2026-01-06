# 1.Просмотр журналов SSH
## Просмотр всех логов SSH
sudo journalctl -u ssh
## Или более специфично для sshd
sudo journalctl -u sshd
## За последний час
sudo journalctl -u sshd --since "1 hour ago"
## С ошибками и предупреждениями
sudo journalctl -u sshd -p err..warning
# 2.Вывод журналов в реальном времени
## Все системные логи в реальном времени
sudo journalctl -f
## Только SSH логи в реальном времени
sudo journalctl -u sshd -f
## С определённым идентификатором (например, sshd)
sudo journalctl _SYSTEMD_UNIT=sshd.service -f
# 3.Вывод логов sshd в реальном времени
# Самый простой вариант
sudo journalctl -u sshd -f
## С дополнительными фильтрами (только с сегодняшнего дня)
sudo journalctl -u sshd --since today -f
## С отображением временных меток
sudo journalctl -u sshd -f --output verbose
# 4.Чтение логов systemd без journalctl
## 1.Просмотр через less (декомпрессия на лету)
sudo less /var/log/journal/*/system.journal
## 2. Использование systemd-cat (для чтения через другие инструменты)
sudo journalctl --output=json | jq '.MESSAGE'  # через jq
## 3. Преобразование в текстовый формат
sudo journalctl --output=short > /tmp/systemd_logs.txt
cat /tmp/systemd_logs.txt
## 4. Прямой просмотр через strings
sudo strings /var/log/journal/*/system.journal | grep sshd
## 5. Через dmesg
dmesg | grep ssh
