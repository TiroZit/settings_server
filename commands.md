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
# Сервер nginx

Ставит зависимости

```
apt install dpkg-dev build-essential gnupg2 git gcc cmake libpcre3 libpcre3-dev zlib1g zlib1g-dev openssl libssl-dev curl unzip -y
```


Добавляет GPG ключ nginx

```
curl -L https://nginx.org/keys/nginx_signing.key | apt-key add -
```

Добавляет репозитории nginx

```
nano /etc/apt/sources.list.d/nginx.list
```

```
deb http://nginx.org/packages/ubuntu/ focal nginx
deb-src http://nginx.org/packages/ubuntu/ focal nginx
```

Обновляет репозитории

```
apt update -y
```

Скачивает исходники nginx

```
cd /usr/local/src
apt source nginx
```

Ставит зависимости для сборки

```
apt build-dep nginx -y
```

Скачивает Brotli

```
git clone --recursive https://github.com/google/ngx_brotli.git
```

Обновляет правила сборки

```
cd /usr/local/src/nginx-*/
nano debian/rules
```

Найти блоки

- `config.env.nginx`
- `config.env.nginx_debug`

Добавить новый ключ в каждом `./configure`

```
--add-module=/usr/local/src/ngx_brotli
```

Компилирует и собирает nginx

```
dpkg-buildpackage -b -uc -us
```

Проверяет deb-файлы

```
ls /usr/local/src/*.deb
```

Устанавливает nginx из deb-файлов

```
dpkg -i /usr/local/src/*.deb
```

## Установка PHP-FPM
```
sudo apt -y install php7.4 php7.4-cli php7.4-fpm php7.4-json php7.4-pdo php7.4-mysql php7.4-zip php7.4-gd php7.4-mbstring php7.4-curl php7.4-xml php-pear php7.4-bcmath
```
```
systemctl status php7.4-fpm.service
```

## Продолжение настройки nginx
Настраивает nginx

```
nano /etc/nginx/nginx.conf
```

Проверяет конфиг nginx

```
nginx -t
```

Запускает nginx

```
systemctl start nginx
```
```
systemctl status nginx
```

Фиксит ошибку с PID

```
mkdir /etc/systemd/system/nginx.service.d
```
```
printf "[Service]\nExecStartPost=/bin/sleep 0.1\n" > /etc/systemd/system/nginx.service.d/override.conf
```
```
systemctl daemon-reload
```
```
systemctl restart nginx
```

Проверяет Brotli

```
curl -H 'Accept-Encoding: br' -I http://localhost
```


Создаёт папки для конфига

```
mkdir -p /etc/nginx/sites-available/
```
```
mkdir -p /etc/nginx/sites-enabled/
```

Создает папку с сайтом
```
mkdir -p /var/www/example.com/html
```

Правит конфиг

```
nano /etc/nginx/sites-available/tirozit.tk.conf
```

Включает конфиг

```
ln -s /etc/nginx/sites-available/tirozit.tk.conf /etc/nginx/sites-enabled/
```

Проверяет конфиг

```
nginx -t
```

Рестартует nginx

```
systemctl restart nginx
```
# Кэширование

Создает папки
```
mkdir /usr/local/nginx
```
```
mkdir /usr/local/nginx/cache
```
```
mkdir /usr/local/nginx/cache2
```

# SSL сертификат от CloudFlare

Необходимо сгенирировать сертификат в CloudFlare, потом установить его на сервер

Сертификат:
```
nano /etc/ssl/домен.pem
```
Ключ:
```
nano /etc/ssl/домен.key
```

# MySQL
Устрановка MySQL
```
apt install mariadb-server -y
```
Зайти в MySQL
```
mysql
```
Создаем нового пользователья в бд
```
CREATE USER 'root'@'localhost' IDENTIFIED BY 'password';
```
Дать все права пользователю
```
GRANT ALL PRIVILEGES ON * . * TO 'root'@'localhost';
```
Перезагрузить все привилегии
```
FLUSH PRIVILEGES;
```
Создаем SSH туннель для MySQl, чтоб подключиться через HeidiSQL
```
ssh -L 3306:localhost:3306 root@ip
```