# Traefik

[Traefik](https://doc.traefik.io/traefik) is um opensource proxy reverso para publicação de serviços e de fácil experiência. Ele trabalha recebendo solicitações e distribuindo para os serviços alvos os quais a própria ferramenta descobre através das notações providas e configurações, sendo um dos principais diferenciais em comparação com outras ferramentas. Além disso, o Traefik oferece um painel integrado para gerenciamento (visualização) e possui suporte a gerenciamento automático de certificado TLS/SSL, além de outros recursos.

## Sumário

- [Traefik](#traefik)
  - [Sumário](#sumário)
  - [1. Requisitos e Dependências](#1-requisitos-e-dependências)
  - [2. Instalação](#2-instalação)
    - [2.1. Portas](#21-portas)
    - [2.2. Volumes](#22-volumes)
    - [2.3. Redes](#23-redes)
  - [3. Adicionando Containers na Rede](#3-adicionando-containers-na-rede)


## 1. Requisitos e Dependências

- [Docker e Docker-Compose](https://docs.docker.com/)

## 2. Instalação

### 2.1. Portas

```yml
# stack.docker-compose.yml

# Em "services.app".
# Comente/Descomente (e/ou altere) as portas/serviços que você deseja prover.

ports:
# Porta para HTTP.
  - '80:80'
# Porta para HTTPS.
  - '443:443'
# Porta para o Traefik Dashboard/API (UI). Não recomendado usar sem autenticação.
# Esse conteúdo pode expor dados sensíveis.
  - '8080:8080'
```

### 2.2. Volumes

```yml
# stack.docker-compose.yml

# Em "services.app".
# Aponte para os locais corretos.

volumes:
# Socket do "Docker Daemon".
  - /var/run/docker.sock:/var/run/docker.sock
# Arquivo de configuração estática.
  - $(pwd)/traefik.yml:/etc/traefik/traefik.yml:ro
# Pasta para logs do Traefik
  - $(pwd)/traefik_log:/var/log/traefik
```

### 2.3. Redes

```yml
# stack.docker-compose.yml

# Em "networks.traefik-net.ipam".
# Altere o valores caso necessário. 

config:
# Endereço da rede.
  - subnet: 172.18.0.0/28
```

## 3. Adicionando Containers na Rede

```yml
# No docker-compose do serviço alvo.

# Adicione a rede.
networks:
  traefik-net:
    name: traefik-net
    external: true

# Em "services.<service_name>".
networks:
  - traefik-net
```