## Docker

1º Passo -> Entender e Analisar os requerimentos que nossa aplicação necessita;
2º Passo -> Mapear o que nossa aplicação tem que fazer;

-- Steps

1º Instalar Node.js
2º Instalar Typescript
3º Instalar Dependencias
4º Compilar Projeto
5º Rodar o Projeto em uma porta específica

### Docker
- Ter pequenas aplicações como sistemas operacionais (Com suas próprias dependencias, seu próprio linux);
- Compartilhar informações entre containers, compartilhar rede e alguns caso, recursos;

- Precisamos de um arquivo de receita, que define como será executado e instaldo em nosso projeto (Quem segue os steps) -> DockerFile

- Após a criação do nosso docker file, precisamos compilar a nossa imagem
  `docker build -t heroes-api .`
  - `-t` -> Nome da imagem que ele vai criar
  - ` .` -> Caminho de onde está o dockerfile

  - Após construir nossa imagem, precisamos rodar de fato nossa aplicação
  `docker run -p 3000:3000 heroes-api`
  - `p` 
        1º Parametro -> Porta que meu sistema vai visualizar
        2º Parametro -> Porta de dentro do container
 - `e`
        Variáveis de ambiente do projeto

- Baixamos a imagem e colocamos para rodar o mongoDB
`docker run -p 27017:27017 -d --name mongodb mongo:4`

-`-d` - Roda em segundo plano 
    ** Importante: Se você tiver com uma imagem de mesmo nome irá dar erro. 
    
    || Para listar as imagens: `docker image list`
    || Para listar os containers: `docker ps`
    || Para listar os containers rodando e parados: `docker ps -a`
    || Para listar os container rodando e parados e IDs: `docker ps -aq`
    || Para parar um container: `docker stop ID`

    - Precisamos linkar o nosso mongoDB com à nossa Aplicação:  `--link`

    -Passamos o MONGO_URL o nome do nosso container, internamenye ele vai converter essa string para o IP interno do MongoDB

```shell
docker run -p 3000:4000 \
-e PORT=4000 \
-e MONGO_URL=mongodb \
--link mongodb:mongodb \
heroes-api
```

- Podemos entrar no nosso container e visualizar seu código
1º ` docker ps` e pegar o id;
2º ` docker exec -it ID /bin/sh `
`exec` -> Executa um comando especifico e entra no container

##Subir nossa imagem para o docker hub{
 |-> Criar conta no dockerhub.com
 |-> Logar no terminal: `docker login `
 |-> Compilar nossa imagem ou tag-a-la
  #Colocamos um nome de usuário do docker hub, o nome do projeto e falamos a versão de nossa imagem
 ` docker build -t viniciusmartinss/heroes-api-vinicius:v1 .`
 #Podemos Tag-a-la para uma nova imagem
  docker tag existente novoNome | Criamos uma cópia da imagem com novo nome
  `docker tag heroes-api viniciusmartinss/heroes-api-vinicius:v1` 

#fazer upload
  ` docker push viniciusmartinss/heroes-api-vinicius:v1`
  ##}

#Remover imagem localmente 
` docker rmi viniciusmartinss/heroes-api-vinicius:v1`
` docker rmi heroes-api`

-- Neste momento a imagem nao existe localmente e busca no docker hub, no caso na minha conta ou no do usuario que passar.
```shell
docker run -p 3000:4000 \
-e PORT=4000 \
-e MONGO_URL=mongodb \
--link mongodb:mongodb \
viniciusmartinss/heroes-api-vinicius:v1
```
## ----
Para evitar rodar comando bash o tempo todo, temos uma forma automatizada de agregar serviço.
Agrupar o mongodb, apis, eventos, serviços tudo em um lugar, para que ao digitar um unico comando, subir nossa infra completa.
Este arquivo é chamado `docker-compose.yml`

- Após a criação e serviços no docker-compose
`docker-compose up` > Para levantar todos os serviços
- `docker-compose up --build` > Para levantar e reconstruir as imagens

- `docker-compose up -d heroes-api` > Para levantar apenas o heroes-api e rodar em segundo plano 

- `docker-compose down` > Para matar todos os serviços, volumes e networks

- Para não perder as informações de dados de aplicações, criamos volumes. Informamos a pasta local que será espelhada para a pasta container.
- Para começar a enxergar é necessário limpar as aplicações com 
 `docker-compose down` e `docker-compose up`

``` yaml 
    - volumes:
        ./db:/data/db
```

- Podemos também criar volumes virtuais usando a aba volumes do docker-compose

``` yml 
    - volumes:
        mongo_data: {}
```
    Desta forma, criamos um disco virtual usando a aba volumes do docker-compose
``` yml 
    - volumes: mongo_data:/data/db 
```

- Para visualizar onde nosso volume foi criado
1. ` docker volume ls`
2. `docker volume inspect ID`

Para ganhar um painel bonitão, colocamos uma interface para visualizar nossas instancias do docker
`PORTAINER`

## Rodando
- `docker-compose-up`
- Ir para `localhost:9000` e visualizar os containers 
