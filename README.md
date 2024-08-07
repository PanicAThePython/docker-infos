# docker-infos
Repositório com conteúdo aprendido no curso Docker Essencial do TechEduca (YouTube)

## conceitos básicos
* imagem: é o passo a passo, como receita de bolo
* ```docker run``` é a mesma coisa que ```docker create``` seguido de ```docker start```

## comandos básicos
```docker container ls``` ou ```docker ps``` : lista containers em execução;

```docker container ls -a``` ou ```docker ps -a``` : lista containers parados;

```docker rm (primeiros 4 dígitos do id do container)``` : remove container;

```docker rmi (nome imagem)``` : remove imagem do repo do Docker;

```docker images``` : mostra as imagens do Docker;

```docker run (nome-imagem)``` : executa imagem;

```docker run ubuntu echo "Eu gosto mt de Docker"``` : printa a mensagem "Eu gosto mt de Docker" através do container da imagem ubuntu;
```docker exec (id ou nome do container) echo "natalia"``` : printa o nome e não finaliza o container;

```docker run ubuntu ls``` : listar os diretórios presentes dentro do conteiner da imagem ubuntu;

```docker run -i -t ubuntu``` ou ```docker run -it ubuntu```: interação com o container da imagem ubuntu e cria pseudo terminal no container;

```docker run --name (nome-do-container-aqui) -it ubuntu``` : criando um container com nome definido a partir da imagem ubuntu;

```docker rename running-to-paused paused-to-running``` : renomeia container;

```docker run -d -i ubuntu``` : desatachar -> mantém o container rodando;

```docker inspect (nome imagem)``` : mostra detalhes da imagem, camadas etc;

```docker container prune``` : remove todos os container de uma vez;

```docker rmi $(docker images -q)``` : deleta tds as imagens... oq ta no $ retorna tds os ids;

```ctrl+D``` : atalho pra sair do terminal.

## comandos de estado do container

create:

Do created tu pode ir pro running ou deleted.

* ```docker create -it (nome imagem)``` : setou o estado pra created;

start:

A partir do running dá pra ir em stopped e paused.

* ```docker start (nome container ou id)```: entra em exec;

* ```docker start -ai (nome ou id)``` : executar container parado e flags de desatachar e interatividade;

* ```docker unause paused-to-running``` : muda pra de paused para running o status;

* ```docker restart (nome container)``` : reinicia tds processos container;

delete:

docker kill: faz com que o processo principal do container seja encerrado imediatamente.

* ```docker rm (nome ou id container)``` : estado deleted, dessa forma ele demora um pouco pra deletar pq tá deltando com segurança;

* ```docker rm -f (id)``` : força container parar e o exclui;

  * ```docker run --name running-to-stopped-kill -dit ubuntu```;
  
  * ```docker kill running-to-stopped-kill``` : matou imediatamente o container;
 
pause:

A partir do paused, dá pra ir pro running.

* ```docker stop (id ou 4 dígitos id ou nome do container)``` : para parar o container;

* ```docker stop -t (inserir tempo)``` : para no tempo que for informado;

* ```docker run --name ruuning-to-stopped-stop -dit ubuntu```;

  * ```docker stop running-to-stopped-stop``` : vai finalizar o processo principal com o tempo normal e aí encerra (encerra de forma mais amigável);

* ```docker run --name ruuning-to-paused -dit ubuntu```;

   * ```docker pause running-to-paused``` :  pausa o container;

stop:

A partir do stopped pode ir para start ou deleted.

* ```docker stop stopped-to-running``` : para o container;

--container site estatico
Docker run dockersamples/static-site //primeiro user/nome imagem -> qnd n é oficial do Docker


Docker run -P -d dockersamples/static-site //n só cria a porta 80 pro container, como cria uma porta aleatória pro front

Docker port (id) //dá as portas e suas conexões

Docker run -p 5000:80 -d dockersamples/static-site /-d pro terminal n ficar travado
//define a porta de front manualmente


Docker history dockersamples/static-site:latest | grep AUTHOR
//devolve o autor do Docker, dá pra mudar


Docker run -p 80:80 -d -e AUTHOR="Natalia" dockersamples/static-site //a porta 80 da maquina interage com a porta 80 do container e o autor vira natalia

docke history nome-imagem -> mostra tds camadas da img

para uma imagem ser criada, a partir de outra q serve como base, q geralmente tem SO e dependências
criar do zero: mt mais complexo, n tá nesse curso

Docker commit -> mt custoso, n tão prática, mas importante saber dele

Docker commit (id container) nome-imagem-nova -> cria uma nova imagem a partir do container


Docker build -t "Nome imagem" . (qnd é Dockerfile)
Docker build -t "nome imagem" -f arquivo.dockerfile (qnd tem nome diferente)

Docker build -t "nome-imagem":"tag-versao" . (diretório raiz)
Docker run -it -p 8080:8080 minha-app-python-alpine:v1 


para subir imagem no Dockerhub:
- fazer o build com o nome de user antes:
	- Docker build -t naweise/imagem:v1 .
- fazer login no Dockerhub com 
	- docker login
	- pra sair:docker logut
- Docker push nome-use/nomeimagem:v1


-e -> serve pra por antes de variáveis pra substituir o valor
exemplo: -e NOME="Natália"

PARA CRIA VOLUMES

Docker run -it -v /nome-volume nome-imagem

persistenia m olumes 
Docker volume create nome-volume
Docker volume ls -> lista volumes

como utilizar o volume?
Docker run -it --mount source=nome-volume,target=/nome-diretorio nome-imagem

como acessar esses volumes?
no user root:
sudo su
cd /var/lib/docker/volumes/
cd nome-volume
cd _data



como excluir volume:
Docker volume rm nome-volume

criando no msm momento q acessa:
Docker run -it --mount source=volume-q-ind-n-xiste,target=/novo-diretorio nome-imagem

 Docker run -it -v nome-volume-ainda-n-existe:/novo-dir nome-imagem


tmpfs
//salvar os dados na memoria ram, são perdidos qnd o container é finalizado
Docker run -it --tmpfs=/diretório nome-imagem

Docker start -ai nome-container //pra reiniciar um continer parado

Docker run-it --mount type=tmpfs,destination=/nome-diretorio nome-img

--build-arg >flag pra passar arg
Docker nework inspect bridge
Docker network create --driver bridge nome-rede-pode-serqqr-coisa


Docker network disconnect nome-rede nome-container


Docker network connect nome-rede nome-container


Docker ru -it --network nome-rede --name nome-conatiner nome-imagem





