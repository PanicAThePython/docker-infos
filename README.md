# docker-infos
Repositório com conteúdo aprendido no curso Docker Essencial do TechEduca (YouTube)

## Navegação:

* [conceitos básicos](#conceitos-b%C3%A1sicos)
* [comandos básicos](#comandos-b%C3%A1sicos)
* [comandos de estado do container](#comandos-de-estado-do-container)
* [containers: portas, autores, imagens não oficiais e camadas](#containers-portas-autores-imagens-não-oficiais-e-camadas)
* [volumes](#volumes)
* [tmpfs](#tmpfs)
* [redes no Docker](#redes-no-docker)
* [docker compose](#docker-compose)

## conceitos básicos
* imagem: é o passo a passo, como receita de bolo
* ```docker run``` é a mesma coisa que ```docker create``` seguido de ```docker start```
* FLAGS:
	* -t: ativa terminal;
 	* -i: ativa interatividade;
  	* -d: desatacha container criado;
  	* -e: passada para avisar que valores de variáveis serão definidos. Exemplo: -e NOME=Natalia;
  	* -v: avisar que será criado um volume;
  	* --mount: cria volume;
  	* --build-arg: passar argumentos durante build.

## comandos básicos
```docker container ls``` ou ```docker ps``` : lista containers em execução;

```docker container ls -a``` ou ```docker ps -a``` : lista containers parados;

```docker rm (primeiros 4 dígitos do id do container)``` : remove container;

```docker rmi (nome imagem)``` : remove imagem do repositório local do Docker;

```docker images``` : mostra as imagens locais do Docker;

```docker run (nome-imagem)``` : executa imagem... cria um container de nome aleatório;

```docker run ubuntu echo "Eu gosto mt de Docker"``` : printa a mensagem "Eu gosto mt de Docker" através do container da imagem ubuntu;
```docker exec (id ou nome do container) echo "natalia"``` : printa o nome e não finaliza o container;

```docker run ubuntu ls``` : listar os diretórios presentes dentro do conteiner da imagem ubuntu;

```docker run -i -t ubuntu``` ou ```docker run -it ubuntu```: interação com o container da imagem ubuntu e cria pseudo terminal no container;

```docker run --name (nome-do-container-aqui) -it ubuntu``` : criando um container com nome definido a partir da imagem ubuntu;

```docker rename running-to-paused paused-to-running``` : renomeia container;

```docker run -d -i ubuntu``` : desatachar -> mantém o container rodando;

```docker inspect (nome imagem)``` : mostra detalhes da imagem, camadas etc;

```docker container prune``` : remove todos os container de uma vez;

```docker rmi $(docker images -q)``` : deleta tds as imagens... oq ta no $ retorna tds os ids das imagens;

```ctrl+D``` : atalho pra sair do terminal.

## comandos de estado do container

### create:

Do created é possível ir para running ou deleted.

* ```docker create -it (nome imagem)``` : setou o estado pra created.

### start:

A partir do running é possível ir para stopped ou paused.

* ```docker start (nome container ou id)```: entra em exececução;

* ```docker start -ai (nome ou id)``` : executa container parado com flags de desatachar e interatividade;

* ```docker unpause paused-to-running``` : muda o status de paused para running;

* ```docker restart (nome container)``` : reinicia todos os processos do container.

### delete:

docker kill: faz com que o processo principal do container seja encerrado imediatamente.

* ```docker rm (nome ou id container)``` : estado deleted, dessa forma ele demora um pouco pra deletar pq deleta com segurança;

* ```docker rm -f (id)``` : força container parar e o exclui;

  * ```docker run --name running-to-stopped-kill -dit ubuntu```;
  
  * ```docker kill running-to-stopped-kill``` : matou imediatamente o container.
 
### pause:

A partir do paused é possível ir para o running.

* ```docker stop (id ou 4 dígitos id ou nome do container)``` : para parar o container;

* ```docker stop -t (inserir tempo)``` : parar container no tempo que for informado;

* ```docker run --name ruuning-to-stopped-stop -dit ubuntu```;

  * ```docker stop running-to-stopped-stop``` : vai finalizar o processo principal com o tempo normal e aí encerra (encerra de forma mais amigável);

* ```docker run --name ruuning-to-paused -dit ubuntu```;

   * ```docker pause running-to-paused``` :  pausa o container.

### stop:

A partir do stopped pode ir para start ou deleted.

* ```docker stop stopped-to-running``` : para o container;

## containers: portas, autores, imagens não oficiais e camadas

```docker run dockersamples/static-site``` : nome_user/nome_imagem para quando a imagem não é oficial do Docker;

```docker run -P -d dockersamples/static-site``` : não só cria a porta 80 para o container, como também cria uma porta aleatória para o frontend da aplicação;

```docker port (id)``` : devolve as portas e conexões do container;

```docker run -p 5000:80 -d dockersamples/static-site``` : primeiro é a porta do frontend e depois a do container... flag -d para o terminal não ficar travado;

```docker history dockersamples/static-site:latest | grep AUTHOR``` : devolve o autor da imagem Docker, dá para mudar o valor de author;

```docker run -p 80:80 -d -e AUTHOR="Natalia" dockersamples/static-site``` : a porta 80 da maquina interage com a porta 80 do container e o autor vira natalia;

```docker history nome-imagem``` : mostra tds camadas da img.

## criando imagens: comandos commit, build e run

Para uma imagem ser criada é mais fácil criar a partir de outra, para servir como base, onde geralmente tem SO e dependências.
Criar do zero é muito mais complexo e não foi ensinado no curso.

```docker commit``` : muito custoso, não tão prática, mas importante saber dele;

```docker commit (id container) nome-imagem-nova``` : cria uma nova imagem a partir do container;

```docker build -t "Nome imagem"``` : para quando o arquivo Docker se chama Dockerfile;

```docker build -t "nome imagem" -f nome_arquivo.dockerfile``` : para quando tem um nome diferente;

```docker build -t "nome-imagem":"tag-versao" .``` : o . representa o diretório raiz onde o comando está sendo executado;
	* Exemplo: ```docker run -it -p 8080:8080 minha-app-python-alpine:v1```.

Como subir imagem no Dockerhub:

* fazer o build com o nome de user antes:
	* Estrutura: ```docker build -t nome_user/nome_imagem_criada:numero_ou_nome_versao .```;
* fazer login no Dockerhub com:
	* ```docker login```;
	* pra sair: ```docker logut```;
* ```docker push nome_user/nome_imagem_criada:numero_ou_nome_versao```.

## volumes

São persistentes.

* ```docker run -it -v /nome-volume nome-imagem``` : para criar um volume a partir de uma imagem e executar;

* ```docker volume create nome-volume``` : comando para criar volume;

* ```docker volume ls``` : lista volumes presentes localmente;

* ```docker run -it --mount source=nome-volume,target=/nome-diretorio nome-imagem``` : para utilizar um volume;

* ```docker volume rm nome-volume``` : para excluir volume.

Como acessar esses volumes?

no user root:
* sudo su
* cd /var/lib/docker/volumes/
* cd nome-volume
* cd _data

Criando no mesmo momento que acessa:
* ```docker run -it --mount source=volume-q-ind-n-xiste,target=/novo-diretorio nome-imagem``` : para criar e executar;
* Ou então ```docker run -it -v nome-volume-ainda-n-existe:/novo-dir nome-imagem ```.

## tmpfs

Para salvar os dados na memoria ram, pois são perdidos quando o container é finalizado.

* ```docker run -it --tmpfs=/diretório nome-imagem```;

* ```docker run-it --mount type=tmpfs,destination=/nome-diretorio nome-img```.

## redes no Docker

São três tipos: Bridge, Host e None.
Passar diretamente o ip da máquina não é uma boa prática. É recomendado criar uma nova rede e rodar os containers necessários lá. Para isso, é usado a bridge. Quando a rede não é informada, ela é por padrão a bridge.

Rede host: sempre que quiser inserir um container q usa a interface de rede do docker host, usar ela. Não precisa do mapeamento de porta que nem a bridge. Depois que rodar o container, se escrever apenas localhost no browser, vai mostrar o container... NÃO PRECISA DE PORTA!

Rede none: quando você quer subir um container e ele não está atrelado a nenhuma interface de rede. Não possui endereço de ip, não tem acesso a internet. É usado em contexto isolado, como por exemplo processamento de dados.

```docker nework inspect bridge```;

```docker network create --driver bridge nome-rede-criada-por-vc``` : comando para criar nova rede a partir da bridge;

```docker network disconnect nome-rede nome-container``` : comando para desconectar container de uma rede;

```docker network connect nome-rede nome-container``` : comando para conectar container a uma rede;

```docker run -it --network nome-rede --name nome-container nome-imagem``` : comando para ativar rede e criar container na mesma hora.

## docker compose

Nem sempre o ```docker run``` é uma boa prática. Quando temos muitos containers relacionados com muitos argumentos, esse tipo de prática está suscetível ao erro. Para casos assim, usamos o ```docker compose```.

	"Ferramenta do Docker utilizada para coordenar containers."
 
É um arquivo ```docker-compose.yml``` que irá gerenciar os containers. Pode ser versionado e é menos suscetível a erros.

Para listar composes: ```docker compose ps```.

Exemplo arquivo docker-compose.yml:

	version: '3'
	services:
	  wordpress:
	    image: wordpress:6.2.2
	    container_name: meu_wordpress
	    restart: always
	    environment:
	      - WORDPRESS_DB_HOST=db
	      - WORDPRESS_DB_USER=natalia
	      - WORDPRESS_DB_PASSWORD=123456
	      - WORDPRESS_DB_NAME=wp-example
	    ports:
	      - 80:80
	    volumes:
	      - wordpress:/var/www/html
	    networks:
	      - meu-blog
       	    depends_on:
	      - db
	  db:
	    image: mysql:5.7
	    container_name: meu_mysql
	    restart: always
	    environment:
	      - MYSQL_DATABASE=wp-example
	      - MYSQL_USER=natalia
	      - MYSQL_PASSWORD=123456
	      - MYSQL_ROOT_PASSWORD=123456
	    volumes:
	      - db:/var/lib/mysql
	    ports:
	      - 8080:8080
	    networks:
	      - meu-blog
	networks:
	  meu-blog:
	    driver: bridge
	volumes:
	  wordpress:
	  db:

Para executar, é preciso ir na pasta onde está o arquivo compose e executar ```docker compose up```.

É uma boa prática deixar as variáveis num arquivo ```.env``` e, no lugar de ```environment```, colocar ```env_file``` e inserir o nome ```.env```.

Exemplo:

	env_file:
	  - .env


Outro exemplo de ```docker-compose.yml```:

	version: '3'
	services:
	  mongo:
	    build:
	      context: .
	      dockerfile: mongo.dockerfile
	    container_name: mongo
	    ports:
	      - 27017:27017
	    volumes:
	      - mongo-volume:/data/db
	    networks:
	      - rede-aplicacao
	  mongo-express:
	    build:
	      context: .
	      dockerfile: mongo-express.dockerfile
	    container_name: mongo-express
	    ports:
	      - 8081:8081
	    networks:
	      - rede-aplicacao
	    depends_on:
	      - mongo
	  node1:
	    build:
	      context: .
	      dockerfile: app.dockerfile
	    container_name: node1
	    ports:
	      - 3001:3000
	    networks:
	      - rede-aplicacao
	    depends_on:
	      - mongo
	  node2:
	    build:
	      context: .
	      dockerfile: app.dockerfile
	    container_name: node2
	    ports:
	      - 3002:3000
	    networks:
	      - rede-aplicacao
	    depends_on:
	      - mongo
	  node3:
	    build:
	      context: .
	      dockerfile: app.dockerfile
	    container_name: node3
	    ports:
	      - 3003:3000
	    networks:
	      - rede-aplicacao
	    depends_on:
	      - mongo
	  nginx:
	    image: nginx:stable-alpine
	    container_name: nginx
	    ports:
	      - 80:80
	    networks:
	      - rede-aplicacao
	    volumes:
	      - ./nginx.conf:/etc/nginx/nginx.conf
	    depends_on:
	      - node1
	      - node2
	      - node3
	volumes:
	  mongo-volume:
	networks:
	  rede-aplicacao:
	    driver: bridge
