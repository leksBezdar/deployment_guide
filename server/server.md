# Начальная подготовка сервера для деплоя

## Установка необходимых пакетов и зависимостей

Примечание: все представленные команды предназначены для Ubuntu Server и выполняются под root.


### server
Перед началом нужно проверить и обновить установленные пакеты.
Выполним команду:
```
apt update && apt upgrade -y
```
 Ждем окончания всех процессов и приступаем к следующему шагу.


### nginx 
1. Установим nginx командой:
```
apt install nginx
```
2. Включим его сервис:
```
systemctl enable nginx
```
3. Проверим:
```
service nginx status
```
Вот основные команды управления nginx:
```
systemctl start nginx - Запуск
systemctl stop nginx - Отключение
systemctl restart nginx - Перезапуск
systemctl reload nginx - Перезагрузка
systemctl status nginx - Проверка состояния службы
nginx -t - Тестирование конфигурации
```


### Брандмауэр (UFW)
1. Установим утилиту UFW:
```
apt install ufw
```
2. Проверим список доступных приложений:
```
ufw app list
```
3. Запустим брандмауэр и разрешим передачу трафика по Nginx и OpenSSH:
```
ufw enable
ufw allow 'Nginx Full'
ufw allow 'OpenSSH'
```
4. Проверим, что все сделано верно:
```
ufw status
```
Вот примерный вывод команды:
```
Status: active

To                   Action      From
--                   ------      ----
OpenSSH              ALLOW       Anywhere
Nginx Full           ALLOW       Anywhere
OpenSSH (v6)         ALLOW       Anywhere (v6)
Nginx Full (v6)      ALLOW       Anywhere (v6)
```


### PostgreSQL
1. Установим PostgreSQL:
```
apt install postgresql postgresql-contrib
```
2. Зайдем в оболочку:
```
sudo -u <user> psql
```
3. Установим пароль для пользователя:
```
\password
```
4. Дважды вводим пароль(из соображений безопасности он не отображается).
5. И выходим:
```
\q
#OR
exit
```


### git 
Установим git, если он не установлен:
```
apt install git
```


### docker
1. Вставляем команды:
```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
sudo systemctl status docker
```

2. Если сервер попадает под блокировку докер хаба, то меняем конфиги
```
nano /etc/docker/daemon.json
```

3. Вносим зеркала
```
{
  "registry-mirrors": [
    "https://mirror.gcr.io",
    "https://daocloud.io",
    "https://c.163.com/",
    "https://registry.docker-cn.com"
  ]
}
```

4. Перезагружаем докер
```
sudo systemctl restart docker
```

### SSL
Будем использовать бесплатные SSL сертификаты от Let's Encrypt.
Установим все необходимое:
```
apt install certbot python3-certbot-nginx
```


## Деплой


### nginx 
1. Заносим конфиг для nginx на наш домен:
```
nano /etc/nginx/sites-available/ваш_конфиг
```
2. Туда пишем:
```
server {
    server_name домен;

    location / {
        proxy_pass http://localhost:порт/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

}
```
3. Создаем символическую ссылку на конфиг:
```
sudo ln -s /etc/nginx/sites-available/ваш_конфиг /etc/nginx/sites-enabled/
```
4. Перезапускаем nginx:
```
sudo systemctl restart nginx
```
5. Тестируем:
```
nginx -t
```


### SSL
1. Получаем сертификаты:
```
certbot --nginx -d домен
```
2. Зайдем в crontab:
```
crontab -e
```
3.  Для автоматического продления сертификатов укажем в крон-задаче следующие параметры:
```
30 3 * * 2 /usr/bin/certbot renew >> /var/log/renew-ssl.log
```
