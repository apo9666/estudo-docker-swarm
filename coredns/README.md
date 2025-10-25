# CoreDNS

## O que é
CoreDNS é um servidor DNS flexível e extensível escrito em Go. Ele serve como resolver ou autoridade e pode ser estendido com plugins (forward, cache, proxy, etc.). É comumente usado em clusters Kubernetes e em infraestruturas que precisam de DNS customizável.

## Comandos úteis:
- Subir: `docker compose up -d`
- Ver logs: `docker compose logs -f coredns`
- Testar (usando dig): `dig @127.0.0.1 google.com`
- Parar/remover: `docker compose down`
