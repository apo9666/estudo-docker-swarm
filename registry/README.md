# Docker Registry 

- O Docker Registry é um serviço para armazenar e distribuir imagens Docker privadas.
- Este repositório contém a configuração (docker-compose.yml) para levantar um registry local.

Como executar
1. Levante o serviço:
```bash
docker stack deploy -c docker-compose.yml registry
```
2. Verifique logs:
```bash
docker service logs registry_registry
```
