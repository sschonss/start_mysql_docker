# Como subir um banco de dados MySQL com Docker
## Conceitos básicos
---

### O que é Docker?

Se você está lendo esse artigo, provavelmente já sabe o que é Docker, mas para aqueles que não sabem, Docker é uma plataforma de código aberto que facilita a criação, o empacotamento e a execução de aplicativos em contêineres. Os contêineres permitem que você empacote rapidamente um aplicativo pronto para produção em qualquer ambiente, incluindo desktops, VMs, servidores, clusters, etc.

Com o Docker, você pode separar seus aplicativos de sua infraestrutura e tratá-los como um aplicativo gerenciável. Ao fazer isso, seus aplicativos são mais portáteis e eficientes.

### O que é um banco de dados?

Um banco de dados é um sistema computadorizado de manutenção de registros. Um banco de dados é uma coleção de dados inter-relacionados, chamados de dados, que são armazenados de forma estruturada e organizada para permitir o acesso, a recuperação, a modificação ou a exclusão de dados.

### O que é MySQL?

MySQL é um sistema de gerenciamento de banco de dados relacional de código aberto (RDBMS) baseado na linguagem de consulta estruturada (SQL). O MySQL é executado em praticamente todas as plataformas, incluindo Linux, UNIX e Windows. Embora possa ser usado em uma ampla gama de aplicativos, o MySQL é mais frequentemente associado a aplicativos baseados na Web e comércio eletrônico e é uma parte importante da pilha de código aberto LAMP (Linux, Apache, MySQL, Perl / PHP / Python).

## Requisitos

- Docker instalado
- Docker Compose instalado

## Introdução

Vamos criar um banco de dados MySQL com Docker. Para isso, vamos utilizar o Docker Compose, que é uma ferramenta para definir e executar aplicativos Docker de vários contêineres. Com o Compose, usamos um arquivo YAML para configurar os serviços do aplicativo. Então, com um único comando, podemos criar e iniciar todos os serviços do aplicativo a partir da configuração.

## Estruturando o projeto

Vamos criar uma pasta chamada `mysql-docker` e dentro dela vamos criar um arquivo chamado `docker-compose.yml`. Dentro desse arquivo, vamos definir o serviço do MySQL.

```yml

version: '3.1'

services:
  db:
    image: mysql:5.7
    container_name: mysql-docker
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: database
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    ports:
      - 3306:3306
    volumes:
      - ./mysql/data:/var/lib/mysql
```

Vamos entender o que cada linha desse arquivo faz.

- `version: '3.1'`: Versão do Docker Compose que estamos utilizando.

- `services`: Define os serviços que serão utilizados.

- `db`: Nome do serviço. Aqui poderia ser qualquer nome.

- `image: mysql:5.7`: Imagem do MySQL que vamos utilizar. Nesse caso, vamos utilizar a versão 5.7, é possível utilizar outras versões. Para saber mais, acesse o [Docker Hub](https://hub.docker.com/_/mysql).

- `container_name: mysql-docker`: Nome do container que vamos criar. Aqui poderia ser qualquer nome.

- `restart: always`: Sempre que o container for reiniciado, o serviço será iniciado.

- `environment`: Define as variáveis de ambiente que serão utilizadas. Nesse caso, estamos definindo o usuário, senha e o nome do banco de dados. Lembrando que as variaveis de ambiente podem ser definidas de diversas formas, para saber mais, acesse a [documentação](https://docs.docker.com/compose/environment-variables/).

- `ports`: Define a porta que será utilizada para acessar o banco de dados. Nesse caso, estamos utilizando a porta 3306. A primeira porta é a porta do host e a segunda é a porta do container.

- `volumes`: Define o volume que será utilizado. Nesse caso, estamos definindo que os dados do banco de dados serão salvos na pasta `mysql/data` do projeto. Pense que o volume é um link simbólico entre a pasta do projeto e a pasta do container. Aqui é onde será salvo o datadir do MySQL. Para saber mais, acesse a [documentação](https://docs.docker.com/storage/volumes/).

## Criando o banco de dados

Nesse momento, já podemos criar o banco de dados. Para isso, vamos executar o seguinte comando:

```bash
docker-compose up
```

Esse comando vai criar o container e iniciar o serviço do MySQL. Para verificar se o container foi criado, execute o seguinte comando:

```bash
docker ps
```

Esse comando vai listar todos os containers que estão em execução.

## Acessando o banco de dados

Você pode acessar o banco de dados de diversas formas, seja utilizando um cliente MySQL ou acessando o container. Para acessar o container, execute o seguinte comando:

```bash
docker exec -it mysql-docker bash
```

Vamos entender o que cada parte desse comando faz.

- `docker exec`: Executa um comando dentro do container.

- `-it`: Define que o comando será executado no modo interativo.

- `mysql-docker`: Nome do container que vamos acessar. Para saber o nome do container, execute o comando `docker ps`. Lembrando que definimos o nome do container no arquivo `docker-compose.yml`.

- `bash`: Comando que vamos executar dentro do container. Nesse caso, vamos executar o bash.

Agora que estamos dentro do container, podemos acessar o banco de dados utilizando o seguinte comando:

```bash
mysql -u user -p
```

Vamos entender o que cada parte desse comando faz.

- `mysql`: Comando para acessar o banco de dados.

- `-u user`: Define o usuário que vamos utilizar para acessar o banco de dados. Como definimos o usuário no arquivo `docker-compose.yml`, vamos utilizar o usuário `user`. Se você não definir o usuário, o usuário padrão é o `root`.

- `-p`: Define que o usuário vai digitar a senha para acessar o banco de dados.

Agora, vamos digitar a senha que definimos no arquivo `docker-compose.yml`. Se tudo der certo, você vai estar dentro do banco de dados.

## Conclusão

Nesse artigo, vimos como criar um banco de dados MySQL com Docker. Vimos como utilizar o Docker Compose para criar o container e como acessar o banco de dados. Espero que esse artigo tenha sido útil para você. Até a próxima!

## Atenção

Arquivos de configuração e senhas não devem ser versionados. Esse projeto é apenas para fins didáticos.

A extensão `.yml` possui uma padronização de espaços e tabs. Caso você utilize o VSCode, recomendo que instale a extensão [YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) para que o arquivo fique com uma aparência melhor.

## Referências

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)
- [MySQL](https://www.mysql.com/)