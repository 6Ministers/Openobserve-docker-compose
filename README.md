# Openobserve-docker-compose

Установка docker
```
curl -s https://raw.githubusercontent.com/6Ministers/Openobserve-docker-compose/master/setup.sh | sudo bash -s
```

Клоинируем репозиторий
```
curl -s https://raw.githubusercontent.com/6Ministers/Openobserve-docker-compose/master/download.sh | sudo bash -s openobserve
```

Переходим в каталог
```
cd openobserve
```
Редактируем настройки caddy

```
https://subdomain.your-domain:443 {
    reverse_proxy :5080
	# tls admin@example.org
	encode zstd gzip
	file_server
	
}
```

Укажите ваш логин и пароль
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
