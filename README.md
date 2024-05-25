# traefik in docker swarm

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
