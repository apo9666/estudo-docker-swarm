
# Estudo: Docker Swarm — Guia rápido e seguro

Este repositório contém um README com um tutorial prático para iniciar e operar um cluster Docker Swarm. O objetivo é fornecer comandos e práticas recomendadas.

## Conteúdo

- Visão geral rápida
- Pré-requisitos
- Inicializar o Swarm (manager)
- Obter tokens e adicionar workers/managers (com placeholders)
- Redes overlay
- Deploy de uma stack com exemplo
- Secrets e variáveis de ambiente
- Escalonamento, logs e troubleshooting
- Boas práticas de segurança

## Pré-requisitos

- Docker instalado em todas as máquinas (recomendado Docker Engine 20.10+).
- Acesso de rede entre nós (portas: 2377, 7946, 4789 e 2376 se usar TLS remota).
- Usuário com permissão para executar comandos docker (ex: membro do grupo docker ou root).

## Iniciar o manager

No nó que será o manager, inicialize o Swarm e informe o endereço que os outros nós devem usar para se conectar.

Aqui estão formas rápidas de obter o IP da máquina

Linux
```bash
# endereço IPv4 primário da máquina
hostname -I | awk '{print $1}'
```

Exemplo (substitua o placeholder):

```bash
docker swarm init --advertise-addr <MANAGER_IP>
```


## Obter token para adicionar nós

No manager, para recuperar o token de join (worker ou manager) execute:

```bash
docker swarm join-token worker   # mostra o comando com o token para workers
docker swarm join-token manager  # mostra o comando com o token para novos managers
```

Saída: o Docker mostrará comandos para juntar workers e managers, por exemplo: 

```bash
docker swarm join --token <WORKER_JOIN_TOKEN> <MANAGER_IP>:2377
```

## Verificar nós no manager

No manager, confirme os nós que fazem parte do cluster:

```bash
docker node ls
```

## Redes overlay

Para serviços distribuídos entre nós use redes overlay. Crie uma rede overlay attachable:

```bash
docker network create --driver overlay --attachable swarm_overlay
```

Por que usar overlay?
- Permite comunicação de contêiner para contêiner entre hosts diferentes.
- Usada por stacks/serviços para se conectar sem expor portas de host desnecessárias.

## Exemplo: deploy de uma stack simples

Crie um arquivo `docker-stack.yml` (exemplo minimal):

```yaml
services:
	web:
		image: nginx:alpine
		ports:
			- "80:80"
		networks:
			- swarm_overlay

networks:
	swarm_overlay:
    name: swarm_overlay
		external: true
```

Depois, no manager:

```bash
docker stack deploy -c docker-stack.yml minha_stack
```

Verificar serviços:

```bash
docker stack ps minha_stack
docker service ls
```

## Secrets e variáveis sensíveis

- Para valores sensíveis use `docker secret` e monte no serviço:

```bash
echo "minha-senha" | docker secret create db_password -

# no compose/stack referencie o secret
# secrets:
#   - db_password
```

- Para variáveis de ambiente não sensíveis, use `.env` ou `env_file` e não versionar esse arquivo (`.gitignore`).

## Escalonamento e atualização de serviços

Escalar um serviço:

```bash
docker service scale minha_stack_web=5
```

Atualizar imagem com deploy sem downtime (rolling update):

```bash
docker service update --image minha-imagem:versao nova_servico
```

## Logs e troubleshooting

Logs do serviço:

```bash
docker service logs minha_stack_web
```

Inspecionar contêiner em um nó:

```bash
docker ps                  # identificar container ID
docker logs <CONTAINER_ID>
docker inspect <CONTAINER_ID>
```

## Boas práticas de segurança

- Nunca armazene tokens ou senhas em repositórios públicos.
- Use `docker secrets` para credenciais sensíveis.
- Proteja o acesso SSH aos nós e use chaves em vez de senhas.
- Considere ativar TLS para a API Docker quando exposta remotamente.
- Limite quais hosts podem se comunicar com o manager via firewall.
