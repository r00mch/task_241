# 1. Установка iptables 
Проверил, у меня стояли таблесы уже

<img width="762" height="90" alt="image" src="https://github.com/user-attachments/assets/8216299d-937e-4b0a-b1ea-3c0ee1d57f4c" />

# 2.Проверка текущих правил
## в более читаемом формате с номерами правил
sudo iptables -L -n --line-numbers
<img width="775" height="206" alt="image" src="https://github.com/user-attachments/assets/fcf3cf0b-ed0b-4768-b5b7-5c9a6d5875c1" />
# 3.Почему пропадает возможность подключение 
Основные причины:
Политика по умолчанию DROP на INPUT - все входящие соединения блокируются
Нет правила разрешающего порт 22 - SSH работает на 22 порту TCP
Неправильная последовательность правил - правило DROP стоит перед ACCEPT
# 4.Смоделировал проблему, чтобы все попытки подключения блокировались 
Изначально видно, что подключиться удалось, изменил айпитаблесы, теперь вывел ошибку 
<img width="1294" height="225" alt="image" src="https://github.com/user-attachments/assets/d8b5ffcf-76f5-4e00-a0ba-4417f1f9276c" />
# 5.Восстанавливаем возможность подключения 
## 1. Временно возвращаем ACCEPT
sudo iptables -P INPUT ACCEPT
## 2. Добавляем необходимые правила в правильном порядке
sudo iptables -F INPUT  # Очищаем цепь INPUT
## 3. Разрешаем loopback
sudo iptables -A INPUT -i lo -j ACCEPT
## 4. Разрешаем установленные соединения
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
## 5. Разрешаем SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
## 6. Теперь безопасно ставим DROP
sudo iptables -P INPUT DROP
## 7. Проверяем
sudo iptables -L -n --line-numbers
<img width="1060" height="637" alt="image" src="https://github.com/user-attachments/assets/2a0c3c1c-7326-4a17-b4cc-d97aba533143" />
На скрине два окна, в одном чекал соеденение. Видно, поосле испраления правил соеденение с сервером установил 
# Это будет TCP протокол или UDP?
Ответ: SSH использует TCP протокол, порт 22.
# Сохранение 
## Сохраняем текущие правила
sudo iptables-save > /etc/iptables/rules.v4
## Сохраняются ли правила после перезагрузки?
Правила iptables хранятся в памяти и сбрасываются при перезагрузке
# Как сохранить?
## Устанавливаем iptables-persistent
sudo apt install iptables-persistent -y
## Сохраняем правила
sudo netfilter-persistent save
## Включаем автозагрузку
sudo systemctl enable netfilter-persistent
теперь правила сохранены 
