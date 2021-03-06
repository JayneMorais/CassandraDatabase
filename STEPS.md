# Intalação, Configuração e Execução do Cassandra Database

-----
## Instalação Docker

#### Capturando chave do repositorio

``curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add-``

#### Adicionando o repositorio do docker a lista de fontes de software

``sudo add-apt-repository deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable``

#### Atualizando a lista de software 

``sudo apt-get update``

#### Instalando o Docker 

``sudo apt install docker-ce``

#### Verificando instalação do Docker

``sudo systemctl status docker``




-----

## Instalando Docker-Compose

O docker compose funciona como uma "agregador de containers" onde é possível através de um arquivo YML 
executar multiplos containers Docker

#### Realizando o download dos Binarios

``curl -L https://github.com/docker/compose/releases/download/1.25.5/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose``

#### Transformando em executável

``chmod +x /usr/local/bin/docker-compose``

#### Verificação da instalação do Docker-Compose

```docker-compose -v```


### Configuração para execução de Docker sem sudo

#### Criando grupo do Docker

```sudo groupadd docker```

#### Adiciona o atual usuário ao grupo Docker

```sudo gpasswd -a $USER docker```

Para finalizar faça o Log Out ou Log In.




-----

## CassandraDB via Docker-Compose

#### Download YML do cluster do Cassandra

Navegue até a pasta desejada e realize o download através do comando

``wget https://github.com/bitnami/bitnami-docker-cassandra/blob/master/docker-compose-cluster.yml -O docker-compose.yml``



------
### Instalando o drive do cassandra para Python 3.6


#### Criando um novo ENV

```conda create --name cassandra```

**Obs.** O nome cassandra é o nome escolhido para exemplo, mas pode ser qualquer outro

#### Inicializando ENV

```source activate cassandra```

#### Instalando o driver do cassandra DB

```pip install cassandra-driver```


-----

# Execução da Aplicação


-----

### Executando cluster do cassandra

```sudo docker-compose up -d```

* ```up``` executa o YML encontrado na pasta
* ```-d``` executa o docker compose em background


#### Acessando o CQLSH do container 

```docker exec -it projeto\_cassandra\_1 cqlsh -u cassandra```

senha: cassandra





#### Deletando Keyspaces existentes

```DROP KEYSPACE reviews_db;```

#### Conferindo

```DESCRIBE KEYSPACES```


### Inicialização do enviroment conda 

#### Caso não esteja no enviroment:

```source activate cassandra```

#### Definindo arquivo de aplicação Flask

```(cassandra) jayne@jayne-X555LD:~/Documentos/MESTRADO/BD/projeto$ export FLASK_APP=app.py```

#### Executando aplicação Flask

Navegue até o diretório do projeto e execute:

```flask run```


#### Listar os conteiners

```docker-compose ps```


#### Criando base de dados 

       CREATE KEYSPACE reviews_db 
       WITH REPLICATION = { 
        'class' : 'SimpleStrategy', 
        'replication_factor' : 2
       };

#### Criando table

       CREATE TABLE reviews_db.reviews (
       id UUID, 
       gostou INT, 
       usuario_id TEXT, 
       filme_id TEXT,
       comentario TEXT,
       criado_em TIMESTAMP,
       atualizado_em TIMESTAMP,
       PRIMARY KEY (id));

**Obs.** A senha pre definida econtra-se no docker-compose.yml, sendo: cassandra


####  Verificando Dados  existentes

- Descrição do Cluster

```DESCRIBE CLUSTER```

- Listar Schemas do Cluster

```DESCRIBE SCHEMA```

- Listar Keyspaces do Cluster

```DESCRIBE KEYSPACES```

-  Descrição de um Keyspace específico

```DESCRIBE KEYSPACE <keyspace_name>```

- Listagem das tabelas

```DESCRIBE TABLES```

- Listagem das tabelas de um Kespace Específico

```DESCRIBE TABLE reviews_db.reviews```


## Arquivos

### Executar o terminal do container

```sudo docker exec -it projeto_cassandra_1 /bin/bash```

### Log de eventos

Utilizando o cassandra diretamente instalando na máquina (sem a versão container do bitnami)

Os arquivos de log de evento estarão na pasta: /var/lib/cassandra/commitlog

porém no nosso caso, os arquivos de logs estão localizados dentro do container na pasta: /bitnami/cassandra/data/commitlog

### Arquivos SSTable
