# Openobserve-docker-compose

Быстрая установка
Перед установкой направьте на ip домен

**1. Требованя к серверу**

Минимум

* 1 CPUs
* 2 RAM
* 10 Gb

Ubuntu 24.04

**2. Подготовка сервера**

Установка docker
```
curl -s https://raw.githubusercontent.com/6Ministers/Openobserve-docker-compose/master/setup.sh | sudo bash -s
```

Клоинируем репозиторий
```
curl -s https://raw.githubusercontent.com/6Ministers/Openobserve-docker-compose/master/download.sh | sudo bash -s openobserve
```
**3. Настройка запуска**

Переходим в каталог
```
cd openobserve
```
Редактируем настройки caddyfile

```
https://subdomain.your-domain:443 {
    reverse_proxy :5080
	# tls admin@example.org
	encode zstd gzip
	file_server
	
}
```

Укажите ваш логин и пароль в docker-compose
     `ZO_ROOT_USER_EMAIL: "admin@domain.com"`
     `ZO_ROOT_USER_PASSWORD: "password"`

```
services:
  caddy:
    image: caddy:alpine
    restart: unless-stopped
    container_name: caddy
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./certs:/certs
      - ./config:/config
      - ./data:/data
      - ./sites:/srv
    network_mode: "host"

  openobserve:
    image: public.ecr.aws/zinclabs/openobserve:latest
    restart: unless-stopped
    environment:
      ZO_ROOT_USER_EMAIL: "admin@domain.com"
      ZO_ROOT_USER_PASSWORD: "password"
    ports:
      - "5080:5080"
    volumes:
      - ./openobserve-data:/data
```
**5. Запуск**

**Полезные команды**

Запуск:
```
sudo docker-compose up -d
```
Перезапуск:
```
sudo docker-compose restart
```
Остановка и стартр:
```
sudo docker-compose down && sudo docker-compose up -d
```
Остановка:
```
sudo docker-compose down
```
