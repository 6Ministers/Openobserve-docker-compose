# Openobserve-docker-compose

Быстрая установка
Перед установкой направьте на ip домен

## 1. Требованя к серверу

Минимум

* 1 CPUs
* 2 RAM
* 10 Gb

Ubuntu 24.04

## 2. Подготовка сервера

Установка docker
```
curl -s https://raw.githubusercontent.com/6Ministers/Openobserve-docker-compose/master/setup.sh | sudo bash -s
```

Клоинируем репозиторий
```
curl -s https://raw.githubusercontent.com/6Ministers/Openobserve-docker-compose/master/download.sh | sudo bash -s openobserve
```
## 3. Настройка запуска

Переходим в каталог
```
cd openobserve
```
Редактируем настройки `caddyfile`

```
https://subdomain.your-domain.com:443 {
    reverse_proxy :5080
	# tls admin@example.org
	encode zstd gzip
	file_server
	
}
```

Укажите ваш логин и пароль в `docker-compose.yml`
* `ZO_ROOT_USER_EMAIL: "admin@domain.com"`
* `ZO_ROOT_USER_PASSWORD: "password"`

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
## 4. Запуск

Запуск:
```
docker-compose up -d
```
Переходим по адресу вашего домена

`https://subdomain.your-domain.com`

Вводим логин и пароль:

* `EMAIL: "admin@domain.com"`
* `PASSWORD: "password"`

  ![image](https://github.com/user-attachments/assets/edacc175-3a23-46b2-8d7e-52f7b86039bd)


## 5. Подключаем чтение логов удаленного сервер 

Инструкции по настройке агента сборов лога находятся в разделе:

![image](https://github.com/user-attachments/assets/8eaa7339-27e8-4322-8f67-075555c9628d)

Обсуждение в техподдержке:

https://github.com/openobserve/agents/discussions/11

## Полезные команды

Запуск:
```
docker-compose up -d
```
Перезапуск:
```
docker-compose restart
```
Остановка и стартр:
```
docker-compose down && sudo docker-compose up -d
```
Остановка:
```
docker-compose down
```
