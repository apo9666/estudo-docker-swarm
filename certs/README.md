# Criando certificados auto-assinados para registry Docker

## Gerar certificados

1. Edite o arquivo `openssl-san.cnf`:

2. Execute os comandos:
```bash
# Gerar chave e CSR
openssl req -new -nodes -out meupc.csr -newkey rsa:2048 -keyout meupc.key -config openssl-san.cnf

# Gerar certificado auto-assinado
openssl x509 -req -in meupc.csr -signkey meupc.key -out meupc.crt -days 3650 -extensions req_ext -extfile openssl-san.cnf
```

## Instalar certificados

### Linux
```bash
sudo mkdir -p /etc/docker/certs.d/meupc.test
sudo cp meupc.crt /etc/docker/certs.d/meupc.test/ca.crt
```

### macOS
```bash
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain meupc.crt
```

### Windows
```powershell
Import-Certificate -FilePath meupc.crt -CertStoreLocation Cert:\LocalMachine\Root
```
