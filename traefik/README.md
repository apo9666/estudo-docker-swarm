# Traefik - Proxy Reverso para Docker Swarm

Traefik é um proxy reverso moderno e load balancer que facilita o deploy de microsserviços. Ele integra nativamente com Docker Swarm e automaticamente configura rotas baseadas em labels.

## Pré-requisitos

- Docker instalado
- Docker Swarm inicializado

## Configuração Inicial

1. Criar rede overlay para o Swarm:

```bash
docker network create -d overlay swarm_overlay
```

2. Configurações do Traefik veja `docker-compose.yml`


3. Deploy com:

```bash
docker stack deploy -c docker-compose.yml traefik
```

4. Ver logs

```bash
docker service logs traefik_traefik
```

## Verificação

- Dashboard Traefik: https://traefik.meupc.test

