# MySQL
Устрановка MySQL
```
apt install mariadb-server -y
```
Автозапуск
```
sudo systemctl enable --now mariadb.service
```
Зайти в MySQL
```
mysql
```
Создаем нового пользователья в бд
```
CREATE USER 'пользователь'@'localhost' IDENTIFIED BY 'пароль';
```
Дать все права пользователю
```
GRANT ALL PRIVILEGES ON * . * TO 'root'@'localhost';
```
Перезагрузить все привилегии
```
FLUSH PRIVILEGES;
```
Настройка доступа
```
sudo mysql_secure_installation
```
Создаем SSH туннель для MySQl, чтоб подключиться через HeidiSQL
```
ssh -L 3306:localhost:3306 root@ip
```