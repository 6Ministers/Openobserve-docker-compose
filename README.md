# Openobserve-docker-compose


```
curl -s https://raw.githubusercontent.com/6Ministers/Openobserve-docker-compose/master/setup.sh | sudo bash -s
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
      - /data:/data
volumes:
  data:
```
