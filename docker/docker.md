# Estudos sobre Docker

****

* PID: Provê isolamento dos processos rodando dentro do container
* NET: Provê isolamento das interfaces de rede
* IPC: Provê isolamento da comunicação entre processos e memória compartilhada
* MNT: Provê isolamento do sistemas de arquivo / pontos de montagem
* UTS: Provê isolamento do kernel. Age como se o container fosse outro host

## Docker HUB

Para que o comando "docker run [NAME] funcione, ele precisa encontrar essa imagem. Existe um grande repositório de imagens que é o docker HUB, nele, podemos encontrar diversas imagens. 


## Alguns comandos docker

*docker run [options] image [command] [ARG...]*
*docker [image] pull* - faz o download da imagem
*docker run [image]* - inicializa a imagem
*docker stop [NAME] or [ID] do container*
*docker start [NAME] or [ID] do container*
*docker exec -it [NAME] or [ID] [command]* - ele vai executar o container em modo interativo **(-it)**
*docker pause [NAME] or [ID]*
*docker unpause [NAME] or [ID]*
*docker rm [NAME] or [ID]* - remove o container
*docker run **-d** [image]* - inicializa a imagem em background e não trava o terminal
*docker run -d **-P** [image] - inicializa a imagem em background e disponibiliza uma porta na SO local para acessar.
*docker port [NAME] or [ID]* - informa a porta mapeada em relação ao host
*docker run -d **-p** 1987:80 [image] - inicializa a imagem em background e, diferente, podemos, agora, informar uma porta específica..
*docker inspect [ID]* - teremos diversas informações sobre a imagem.
*docker history [ID]* - teremos todas as camadas que estão formando a imagem.
*docker stop $(docker container ls -q)* - podemos parar todos os containers de uma só vez (-q representa quiet)
*docker container rm $(docker container ls -aq)* - vai remover todos os containers, inclusive os que estão parados
*docker rmi $(docker image ls -aq)* - vai retmover todas as imagens
*docker ps -s* - traz mais um campo com o tamanho da imagem

****

## Observações
- Quando realizamos um docker run image, ele sobe, executa o comando (o bash, no caso do ubuntu) e, se não existir um processo na imagem, encerra.
  
- Quando realizamos o docker stop, nós finalizamos todos os processos que estavam executando no container. Caso não queira, podemos pausar o container e repausar quando desejar. O pause mantém a árvore de processos ativos.
- Também temos o comando *t=0* onde ele para o container instantaneamente, pois, quando não informado, o stop dura 10 segundos para executar.
- Se não usar o comando **-P**, a porta que é disponibilizada pelo docker, :80, por exemplo, é da interface do docker que está isolada e não está mapeada na máquina local. Assim, para mapearmos, utilizamos o -P e o docker disponibiliza uma porta local.
- Se usarmos o **-p** minúsculo, podemos passar uma porta específica. No exemplo acima, **1987:80**, significa que ele irá mapear a porta 1987 na máquina host para a porta 80 da interface do docker.

****

## Imagens

Uma imagem é um conjunto de camadas que, quando juntadas, formam uma imagem. E essas camadas, em sí, são independentes, cada um tem seu respectivo ID.

![imagem com suas camadas read only](https://lh3.googleusercontent.com/proxy/KdopPKTnvShkmr_B4cNA2qh3OKnCGs8Jrgf1HH1z3o2tXHWSzdYf3gC9mGRltq7bctNH8t9Pe4dSOCUWwR0qvz-u8xnCgQpEEZJ_j8G5feh8)

Uma camada, quando criada, é read only, ou seja, não pode ser modificada.

Quando realizamos um download dessa imagem, o docker gera uma camada adicional read/write. **Essa camada é temporada**.

![camada temporária read write](https://docker-unleashed.readthedocs.io/_images/container-layers.jpg)

## Criando a primeira imagem

![fluxo criação da imagem](https://computacaointeligente.com.br/assets/img/posts/docker/df.jpg)

**[Dockerfile - Exemplo](https://docs.docker.com/engine/reference/builder/)**

FROM node:14 - definimos a imagem como base para a nossa imagem

ARG PORT=6000 - Podemos parametrizar a porta e passar a variável **PORT** para o nosso expose, poré, o ARG é executado em momento de build da imagem

ENV PORT=$PORT - Para usar a variável dentro do container, usamos o env. **$PORT é para pegar o valor da variável PORT**

EXPOSE - informar ao usuário em qual porta default nossa aplicação está sendo rodado

WORKDIR /app-node - pasta de diretório padrão

COPY . . - vai copiar todo o diretorio atual para o /app-node definido no workdir

RUN npm install - comando a ser executado assim que a imagem estiver sendo criada

ENTRYPOINT npm start - e quando o container for executado a partir dessa imagem, o comando que irá rodar é npm start

**Comando para gerar a imagem**

docker build -t [nome-da-imagem]:[versão] [contexto a ser executado]
_docker build -b danaraujo/app-node:1.0 ._ - **No meu caso, usei "ponto" por executar no contexto do diretório atual**

**Tamanho da imagem**

Quando passado o comando *docker ps -s*, temos a informação do tamanho da nossa imagem. Com o comando *docker histoty [IMAGEM]* temos o tamanho da nossa imagem local que, se repararmos, será igual ao tamanho da imagem virtual. O que faz total sentido, já que o container é a cópia da imagem original.
 
## Volume

Para persistir dados e não perder quando uma imagem for removida, é criando uma pasta compartilhada entre hosts.

*docker run -it -v /c:/pasta-qualquer:/app ubuntu bash* - assim, qualquer informação que salvarmos dentro da pasta **app** será espelhado na nossa **pasta-qualquer** na nossa máquina local

  [Uma forma mais explícita e verbosa, é usando a tag --mount](https://docs.docker.com/storage/volumes/)
*docker run --mount type=bind,source=/c:/pasta-qualquer,target=/app ubuntu bash*

Existe uma outra forma de criar e gerenciar o volume, que é mais recomendada.

![](https://docs.docker.com/storage/images/types-of-mounts-volume.webp?w=450&h=300)

A imagem acima, mostra que a utilização do volume é uma área gerenciada pelo docker dentro do filesystem.

Assim, é mais seguro alguém mexer dentro do filesystem, pois será gerenciado pelo próprio docker.

*docker volume ps* - verifica os volumes que estão criados
*docker volume create [NAME]* - cria um volume na filesystem do docker

  **tmpfs**

A ideia do tmpfs é persistir dados em memória no host, assim, não serão persistido na camada write/read

comand: *docker run -it --tmpfs=/app ubuntu bash*
  *Ele só funciona no host linux.*