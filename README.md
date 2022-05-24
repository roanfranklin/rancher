## rancher

Instalar o RANCHER em um docker local para testes. Ele irá gerenciar/monitorar o seu Kubernetes.

### Requesitos

docker
docker-compose
kubectl

### docker-compose.yml

**OBS.:** O comando *--acme-domain* é para levantar com o certificado!

```yaml
version: '3.3'

services:
  server:
    image: rancher/rancher
    restart: unless-stopped
    privileged: true
    ports:
      - '80:80'
      - '443:443'
      - '6443:6443'
    volumes:
      - rancher-vol:/var/lib/rancher
    command: --acme-domain rancher.seudominio.com.br
  
volumes:
  rancher-vol:
```

Iniciar o RANCHER com o docker-compose:

```bash
docker-compose up -d
```

verificando se está tudo certo:
```bash
docker ps

docker-compose logs -f
```

### Opcional

Voce pode usar o comando docker para levantar seu RANCHER sem o docker-compose.

```bash
docker run --privileged -d --restart=unless-stopped -p 80:80 -p 443:443 --name=rancher-server rancher/rancher
```

ou:

```bash
docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 -p 6443:6443 \
  --privileged \
  --name rancher \
  rancher/rancher:v2.4.15 \
  --acme-domain rancher.seudominio.com.br
```

### Observação Importante

Caso você não tenha domínio e queira levantar o rancher como teste em sua máquina local, tente remover o *--acme-domain*.