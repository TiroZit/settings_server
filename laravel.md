# Laravel
Установка необходимых компонентов
```
sudo apt install -y php-mbstring php-xml php-fpm php-zip php-common php-fpm php-cli unzip curl nginx
```
Установка Composer
```
sudo curl -s https://getcomposer.org/installer | php
```
```
sudo mv composer.phar /usr/local/bin/composer
```
Установка
```
cd /var/www/сайт
```
```
sudo composer global require laravel/installer
```
```
sudo composer create-project --prefer-dist laravel/laravel example
```
Права
```
sudo chmod -R 755 /var/www/сайт/example
```
```
sudo chown -R example_user:example_user /var/www/сайт/example
```
Установка примера проекта
```
cd example
```
```
composer install
```
Настройка nginx
```
sudo nano /etc/nginx/sites-enabled/
```
Замените
- `server_name _;`
- `/var/www/сайт/example/public` - root path

Добавьте
```
  location / {
    try_files $uri $uri/ /index.php?$query_string;
  }
```
Перезагрузка nginx
```
sudo systemctl restart nginx
```