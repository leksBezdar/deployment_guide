# Deployment guide

## Создание репозитория
```
git init
git remote add origin git@github.com:<никнейм>/<названиеРепозитория>.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

## Подготовка сервера
### git
```
sudo apt-get update
sudo apt-get install git
```

### docker
https://docs.docker.com/engine/install/ubuntu/
```
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Запуск приложения
Создание образа (коробки) с приложением
```
docker build . --tag fastapi_app
```
Запуск образа в контейнере с пробросом портов для доступа к контейнеру из внешней сети
```
docker run -p 9999:8000 fastapi_app
```
