# Deployment guide

## Подготовка сервера

### ufw 
Проверим, какие параметры ufw установлены сейчас:
```
sudo ufw status
```
Если разрешен только HTTP-трафик, вывод будет примерно следующим:
```
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
Nginx HTTP                 ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```
Внесем изменения, а именно разрешим HTTPS-трафик. Для этого выполним:
```
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
```

Cнова проверим настройки брандмауэра: 
```
sudo ufw status
```
Если все в порядке, вы увидите, что весь трафик (Nginx Full) разрешен:
```
Status: active

To                   Action      From
--                   ------      ----
OpenSSH              ALLOW       Anywhere
Nginx Full           ALLOW       Anywhere
OpenSSH (v6)         ALLOW       Anywhere (v6)
Nginx Full (v6)      ALLOW       Anywhere (v6)
```


### git
```
sudo apt-get update
sudo apt-get install git
```

### docker
```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
sudo systemctl status docker
```

Если сервер попадает под блокировку докер хаба, то меняем конфиги
```
nano /etc/docker/daemon.json
```

Вносим зеркала
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

Перезагружаем докер
```
sudo systemctl restart docker
```

### nginx 
```
sudo apt install certbot python3-certbot-nginx
```

1. Заносим конфиг для nginx на наш домен:
```
sudo nano /etc/nginx/sites-available/ваш_конфиг
```
2. Туда пишем:
```
server {
    server_name домен www.домен;

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
nginx -t
```
5. Получаем ssl:
```
sudo certbot --nginx -d домен -d www.домен
crontab -e
```
Укажем в крон-задаче следующие параметры:
```
30 3 * * 2 /usr/bin/certbot renew >> /var/log/renew-ssl.log
```

