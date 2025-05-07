# comandos-docker# 🐳 Guia Completo de Comandos Docker

Este guia contém os principais comandos Docker divididos por níveis de conhecimento: **básico**, **intermediário** e **avançado**. Ideal para quem está começando ou deseja dominar o uso do Docker no dia a dia.

---

## 📦 Nível Básico

Comandos essenciais para começar a usar o Docker.

### 🔍 Verificar instalação
```bash
docker --version
docker info

docker pull <imagem>          # Baixar uma imagem
docker images                 # Listar imagens locais
docker rmi <imagem>           # Remover imagem

docker run <imagem>                          # Rodar container simples
docker run -it <imagem> /bin/bash            # Rodar container interativo
docker ps                                    # Listar containers em execução
docker ps -a                                 # Listar todos containers (inclui parados)
docker stop <container_id>                   # Parar container
docker start <container_id>                  # Iniciar container parado
docker rm <container_id>                     # Remover container

docker system prune                # Remove containers, imagens e redes não utilizados

docker run -d -p 8080:80 <imagem>           # Rodar container em segundo plano (daemon)
docker run --name meu_container <imagem>    # Nomear container
docker exec -it <container_id> /bin/bash    # Acessar terminal de container em execução

docker volume create meu_volume                   # Criar volume
docker run -v meu_volume:/app/dados <imagem>      # Montar volume
docker volume ls                                  # Listar volumes

docker run -v $(pwd):/app <imagem>         # Montar diretório local no container

docker build -t minha_imagem .             # Criar imagem a partir de Dockerfile

docker-compose up -d                       # Subir containers em segundo plano
docker-compose down                        # Derrubar todos os containers
docker-compose down -v			   # Derrubar todos os containers e limpar volumes

docker logs <container_id>                  # Ver logs do container
docker stats                                # Monitoramento em tempo real

docker network ls                           # Listar redes
docker network create minha_rede           # Criar rede
docker run --network=minha_rede ...        # Usar rede customizada

docker run -u 1001:1001 <imagem>            # Rodar container com usuário específico

docker save -o minha_imagem.tar <imagem>    # Exportar imagem
docker load -i minha_imagem.tar             # Importar imagem

docker inspect <container_id>               # Ver detalhes JSON
docker diff <container_id>                  # Ver alterações no sistema de arquivos

FROM node:18-alpine

FROM golang:1.20 as builder
WORKDIR /app
COPY . .
RUN go build -o app .

FROM alpine
COPY --from=builder /app/app /app
CMD ["/app"]

docker scout quickview <imagem>

docker build -t teste-app .
docker run --rm teste-app npm test

version: "3.8"
services:
  web:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: exemplo


version: "3.8"
services:
  web:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: exemplo

docker-compose --env-file .env up -d

docker system prune -a --volumes

docker run -d \
  --name watchtower \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower


version: "3.8"
services:
  web:
    image: nginx
    ports:
      - "8080:80"


docker run -d -p 8080:80 nome-da-imagem


docker run --network host nome-da-imagem

meu-projeto/
├── Dockerfile
├── docker-compose.yml
└── index.html

<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Docker Externo</title>
</head>
<body>
  <h1>Olá da rede!</h1>
</body>
</html>

FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html

version: "3.8"

services:
  web:
    build: .
    ports:
      - "8080:80"  # Expondo porta do container para o host

docker-compose up --build -d

http://localhost:8080

Verifique se o firewall está liberando a porta 8080.

Linux: sudo ufw allow 8080/tcp

Windows: libere nas configurações do Firewall.

Verifique se os dispositivos estão na mesma sub-rede (10.130.0.0/16).

Tente pingar o IP do host a partir do outro dispositivo.

from zipfile import ZipFile

# Conteúdo dos arquivos
files = {
    "Dockerfile": '''FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
''',
    "docker-compose.yml": '''version: "3.8"

services:
  web:
    build: .
    ports:
      - "8080:80"
''',
    "index.html": '''<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Docker Externo</title>
</head>
<body>
  <h1>Olá da rede!</h1>
</body>
</html>
'''
}

# Criação do zip
with ZipFile("docker_rede_externa.zip", "w") as zipf:
    for filename, content in files.items():
        with open(filename, "w", encoding="utf-8") as f:
            f.write(content)
        zipf.write(filename)

