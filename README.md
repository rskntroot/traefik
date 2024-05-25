# traefik in docker swarm

## Setup

traefik sticky loadbalancing for webservice test using whoami & nginx endpoints

initialize swarm
```
docker swarm init
```

deploy stack
```
docker stack deploy -c docker-compose.yml traefik
```

note: must name stack "traefik" due to traefik provider handling of network names

## Testing
```
lost@aml-s905x-cc-101:/opt/docker/traefik$ docker image ls
REPOSITORY       TAG       IMAGE ID       CREATED       SIZE
traefik          latest    7d853441699e   2 days ago    157MB
traefik/whoami   latest    9dd7aa3decf4   2 weeks ago   6.66MB
nginx            latest    8dd77ef2d82e   3 weeks ago   193MB
```
```
lost@aml-s905x-cc-101:/opt/docker/traefik$ docker ps
CONTAINER ID   IMAGE                   COMMAND                  CREATED         STATUS         PORTS     NAMES
452929d48730   traefik/whoami:latest   "/whoami"                7 minutes ago   Up 7 minutes   80/tcp    traefik_whoami.3.6coz87mdulzhaniwerpnw0x0c
21bda105684e   traefik/whoami:latest   "/whoami"                7 minutes ago   Up 7 minutes   80/tcp    traefik_whoami.1.o0hj6f0x1hch1qbfhoaclc101
08397ce330cd   traefik/whoami:latest   "/whoami"                7 minutes ago   Up 7 minutes   80/tcp    traefik_whoami.2.5c0u1weux4wvgcb40wmizpzgo
b2ba1f62e5c1   traefik:latest          "/entrypoint.sh --co…"   7 minutes ago   Up 7 minutes   80/tcp    traefik_traefik.1.qre8dejquwie0dda008rwn623
e45ccb70bbc7   nginx:latest            "/docker-entrypoint.…"   7 minutes ago   Up 7 minutes   80/tcp    traefik_nginx.1.w9fane3zcttrxpsrsvx3ue7jt
```
```
lost@aml-s905x-cc-101:/opt/docker/traefik$ curl -Lk localhost/whoami
Hostname: 21bda105684e
IP: 127.0.0.1
IP: ::1
IP: 172.21.0.8
IP: 172.20.0.7
RemoteAddr: 172.21.0.5:35178
GET /whoami HTTP/1.1
Host: localhost
User-Agent: curl/7.81.0
Accept: */*
Accept-Encoding: gzip
X-Forwarded-For: 10.0.0.2
X-Forwarded-Host: localhost
X-Forwarded-Port: 443
X-Forwarded-Proto: https
X-Forwarded-Server: b2ba1f62e5c1
X-Real-Ip: 10.0.0.2
```
```
lost@aml-s905x-cc-101:/opt/docker/traefik$ curl -Lk localhost/nginx
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
lost@aml-s905x-cc-101:/opt/docker/traefik$
``` 
