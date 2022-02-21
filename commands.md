# Настройка дедика
Переключиться на root
```
sudo su -
```
Поменять рекурсивно (для все файлов и папок внутри) права доступа (777 - для всех пользователей) для SFTP
```
chmod -R 777 /var/www
```
Обновляет систему
```
sudo apt update && apt upgrade -y
```
Настройка портов
```
nano /etc/iptables/rules.v4
```
Удаляем ВСЕ строки с port 80 (~ 4 строки)!!!

Добавляем 2 строки после портом 22
```
-A INPUT -p tcp -m tcp --dport 443 -m state --state NEW -j ACCEPT
-A INPUT -p tcp -m tcp --dport 80 -m state --state NEW -j ACCEPT
```
```
iptables -P INPUT ACCEPT
```
```
iptables -P OUTPUT ACCEPT
```
```
iptables -P FORWARD ACCEPT
```
```
iptables -F
```
Отключение SSL ключей
```
passwd root
```
- PermitRootLogin yes
- PubkeyAuthentication no
- PasswordAuthentication yes
```
nano /etc/ssh/sshd_config
```
```
systemctl restart sshd
```