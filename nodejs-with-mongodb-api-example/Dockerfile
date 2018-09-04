#Seguindo nossos steps, precisamos instalar o node.js
 #Está imagem será procurada localmente, caso não encontrada, nosso projeto será baixado do dockerhub.com, repositório de imagens públicas do Docker
    # padrão nomedaimagem: TAG (Tag serão suas versões, em nosso caso uma distribuição linux) O alpine é o sistema operacional linux
FROM node:10-alpine 
    #From só é usado como o S.O|Ambiente base!

#Mapeamos nosso repositório local para dentro do host
 #Host -> É o CONTAINER

#Copiamos toda nossa pasta local ".", para uma pasta dentro do docker chamada "/src"
 ADD . /src

#precisamos entrar na pasta que criamos
WORKDIR /src

#Instalamos o typescript globalmente | IMPORTANTE: Não existe SUDO no Docker
 RUN npm i -g typescript

#Instalamos as dependencias
 RUN npm install 

#Copilamos o projeto
 RUN npm run build

 #Para a execução final, Utilizamos o comando CMD 
  # A DIFERENÇA DO CMD É QUE SÓ SERÁ RODADO QUANDO O CONTAINER FOR SER EXECUTADO
    #Este comando pode ser substituido
     CMD npm start
     
 #Expomos a porta 3000
     EXPOSE 3000