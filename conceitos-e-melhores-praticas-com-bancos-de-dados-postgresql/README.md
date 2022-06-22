# Conceitos e melhores práticas com bancos de dados PostgreSQL

## Introdução ao banco de dados PostgreSQL
 - [X] Fundamentos de banco de dados
 - [X] Instalação do PostgreSQL no Ubuntu
 - [X] Instalação do do PostgreSQL no CentOS/ Red Hat
 - [X] Instalação do PostgreSQL no Windows
## Objetos e tipos de dados do PostgreSQL
 - [X] O que é o arquivo postgresql.conf
 - [ ] Conheça a ferramenta PGAdmin
 - [ ] Como administrar usuários no banco de dados
 - [ ] Objetos e comandos do banco de dados
## Fundamentos da Structured Query Language (SQL)
 - [ ] Conheça o DML e o Truncate
 - [ ] Funções agregadas em PostgreSQL
 - [ ] Trabalhando com JOINs
 - [ ] Otimizando o código com CTE
## Comandos avançados da Structured Query Language (SQL)
 - [ ] Como as views auxiliam no acesso ao banco de dados
 - [ ] Conheça um dos principais conceitos de banco de dados: transações
 - [ ] Conheça as funções que podem ser criadas pelo desenvolvedor
 - [ ] Certifique seu conhecimento


## Objetos e tipos de dados do PostgreSQL
 ##### O que é o arquivo postgresql.conf

1. O arquivo postgresql.conf
2. O arquivo pg_hba.conf
3. O arquivo pg_ident.conf
4. Comandos administrativos


## Parte 1: O arquivo postegresql.conf
#### Conceitos e melhores praticas com banco de dados PostgreSQL

#### Definição
 - Arquivo onde esta definicas e armazenadas todas as configurações do servidor PostgreSQL
 - Alguns parâmetros só podem ser alterados com uma reinicialização do banco de dados
 - A view pg_settings, acessada por dentro do banco de dados, guarda todas as configurações atuais


Ao acessar a view pg_settings, é possível visualizar todas as configurações atuais:  
SELECT name, setting, FROM pg_settings

Ou é possível usar o comando   
SHOW [parâmetros]


#### Localização do arquivo postgresql.conf
Por padrão, encontra-se dentor do diretório PGDATA definido no momento da inicialização do cluster de banco de dados.

No sistema operacional Ubuntu, se o PostgreSQL foi instalado a partir do repositorio oficial, o local do arquivo protigresql.conf será diferente do diretório do banco de dados

> /etc/postgresql/[versão]/[nome do cluster]/postgresql.conf


#### Confifurações de conexão

 - LISTE_ADDRESS - Endereço(s) TCP/IP das intrfaces que o servidor prostgreSQL vai escutar/liberar conexões
 - PORT - A porta TCP que o servidos PostgreSQL vai ouvir. O pagrão é 5432.
 - MAX_CONNECTIONS - Número máximo de conexões simultâneas no servidor PostgreSQL
 - SUPERUSER_RESERVED_CONNECTIONS - Números de conexões (slots) reservadas para conexões ao banco de dados de super usuários.

#### Configurações de autenticação

 - AUTHENTICATION_TIMEOUT - Tempo máximo em segundos para o cliente conseguir uma conexão com o servidor.
 - PASSWORD_ENCRYPTION - Algoritimo de criotografia das senha dos novos usuários criados no banco de dados.
 - SSL - Habilita a conexão criptografada por SSL (Somente se o PostgreSQL foi compilado com suporte SSL).

#### Configurações de memório

 - SHARED_BUFFERS - Tamanho da memório compartilhada do servidor PostgreSQL para cache/buffer de tabelas, índices e demais relações.
 - WORK_MEM - Tamanho da memória para operações de agreupamento e ordenações (ORDER BY, DISTINCT, MERGE JOINS).
 - MAINTENANCE_WORK_MEM - Tamanho da memória para operações como VACUUM, INDEX, ALTER TABLE.


## Parte 2: O arquivo pg_hda.conf
Conceitos e melhores práticas com banco de dados PostgreSQL

#### Definição
Arquivo responsável pelo controle de autenticação dos usuários no servidor PostgreSQL.
O portmato de arquivo pode ser:

```
local         database  user  auth-method [auth-options]
host          database  user  address     auth-method  [auth-options]
hostssl       database  user  address     auth-method  [auth-options]
hostnossl     database  user  address     auth-method  [auth-options]
hostgssenc    database  user  address     auth-method  [auth-options]
hostnogssenc  database  user  address     auth-method  [auth-options]
host          database  user  IP-address  IP-mask      auth-method  [auth-options]
hostssl       database  user  IP-address  IP-mask      auth-method  [auth-options]
hostnossl     database  user  IP-address  IP-mask      auth-method  [auth-options]
hostgssenc    database  user  IP-address  IP-mask      auth-method  [auth-options]
hostnogssenc  database  user  IP-address  IP-mask      auth-method  [auth-options]
```

## Método de autenticação

 - trust - A conexão é permitida incondicionalmente. Este método permite a qualquer um que possa se conectar ao servidor de banco de dados PostgreSQL se autenticar como o usuário do PostgreSQL que for desejado, sem necessidade de senha.
 - reject - A conexão é rejeitada incondicionalmente. É útil para "eliminar por filtragem" certos hospedeiros de um grupo.
 - md5 - Requer que o cliente forneça uma senha criptografada pelo método md5 para autenticação. 
 - crypt - Requer que o cliente forneça uma senha criptografada através de crypt() para autenticação. Deve-se dar preferência ao método md5 para os clientes com versão 7.2 ou posterior, mas os clientes com versão anterior a 7.2 somente suportam crypt.
 - password - Requer que o cliente forneça uma senha não criptografada para autenticação. Uma vez que a senha é enviada em texto puro pela rede, não deve ser utilizado em redes não confiáveis. 
 - krb4 - É utilizado Kerberos V4 para autenticar o usuário. Somente disponível para conexões TCP/IP. 
 - krb5 - É utilizado Kerberos V5 para autenticar o usuário. Somente disponível para conexões TCP/IP.
 - ident - Obtém o nome de usuário do sistema operacional do cliente (para conexões TCP/IP fazendo contato com o servidor de identificação no cliente, para conexões locais obtendo a partir do sistema operacional) e verifica se o usuário possui permissão para se conectar como o usuário de banco de dados solicitado consultando o mapa especificado após a palavra chave ident. .
 - pam - Autenticação utilizando o serviço Pluggable Authentication Modules (PAM) fornecido pelo sistema operacional. 
 - GSS - Generic Security service application program interface
 - SSPI - Security support provider interfice - somente para windows
 - KRB5 - Kerberos V5
 - PEER - Uriliza o usuário do siostema operacional do cliente
 - IDENT - Utiliza o usuário do sistema operacional do cliente via server
 - LDAP - LDAP server
 - RADIUS - Radius server
 - CERT - autencicação via certificado SSL do cliente

```
# Database administrative login by Unix domain socket
local   all             postgres                                peer

# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     peer    map=testmap
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
# IPv6 local connections:
host    all             all             ::1/128                 md5
```


## Parte 3: O arquivo pg_ident.conf
#### Conceitos e melhores práticas com banco de dados postigreSQL

## Descrição
Arquivo responsável por mapear os usuários do sitema operacional com o s usuários do banco de dados.  
Localizado no diretório de dados PGDATA de sua instalação.  
A opção ident deve ser utilizada no arquivo pg_hdb.conf

```
# Put your actual configuration here
# ----------------------------------
# MAPNAME       SYSTEM-USERNAME         PG-USERNAME
User123         LinuxUser               PGUser
```

## Parte 4 - Comandos administrativos
#### Conceitos e melhores práticas com banco de dados PostgreSQL

# Ubuntu

#### Listando os Clusters
Use o comando pg_lsclusters para listar as instalações. No meu caso há as versões 11 e 14. A 11 havia instalado noutro momento.

```
elder@server01:~$ sudo pg_lsclusters 
Ver Cluster Port Status                Owner    Data directory              Log file
11  main    5432 down,binaries_missing postgres /var/lib/postgresql/11/main /var/log/postgresql/postgresql-11-main.log
14  main    5433 online                postgres /var/lib/postgresql/14/main /var/log/postgresql/postgresql-14-main.log
```


##### pg_clt e pg_ctlcluster
 
As duas ferramentas são usadas para controlar o PostgreSQL. A diferença é que:

pg_ctl é um comando do PostgreSQL.

pg_ctlcluster é um programa feito pelo Debian em Perl como substituto do pg_ctl. Lembre-se disso, pois em outras distros não haverá uma ferramenta pg_ctlcluster.  
*** Start, Stop, Status, Restart de clusters postgreSQL ***

##### Reconfigure seus locales:
> dpkg-reconfigure locales

#### Crie o cluster corretamente (minha versão é a 9.3):
> pg_createcluster 9.3 main --start
 
#### Inicie o PostgreSQL:
> /etc/init.d/postgresql start

# CentOS
#### Comandos administrativos

# systemctl <action> <cluster>
 
  - start - Inicia o cluster do porstegreSQL
  - status - Mosta status do cluster PostegreSQL
  - stop - para o closter
  - restarte - testarta o cluster
 
 # Binários do PostegreSQL
 
  - createdb
  - createuser
  - dropdb
  - dropuser
  - initdb
  - pg_ctl
  - pg_basebackup
  - pg_dump / pg_dumpall
  - pg_restote
  - psql
  - reindexdb
  - vacuumdb
  
 
# Arctetura / Hierarquia
 
 #### Cluster
 Coleção de banco de dados que compartilham as mesmas configurações(arquivos de configuração) do PostgreSQL e do sistema operacional (porta,listen_addresses, etc).
 
 #### Banco de dados  (database)
 Conjunto de schemas com seus objetos/relações (tabelas, funcões, views,etc)
 
 #### schemas
 Conjunto de objeto/relações (tabelas, funçoes views, etc)
 
 
 
# PGAdmin
 #### Conceitos e melhores práticas do banco de dados PostegreSQL
 
 

# Parte 1:  Database , schemas e objatos
 
 #### Database
 É o banco de dados.  
 Grupos de schemas e seus objetos, como tabelas, types, views, funções, entre outros.  
 Seus Schemas e objetos não podem ser compartilhados entre si.  
Cada database é separado um do outro compartilhando apenas usuários/roles  e configurações de cluster ProstegreSQL.
 
 #### Schemas
 É um grupo de pbjetos, como tabelas, types, views, funções, entre outros.  
 É possívle relacionar objetos entre diversos schemas.  
 Por exemplo: schema public e schema curso podem  ter tabelas com o mesmo nome(treste por exemplo) relacionando-se entre si.
 
 #### Objetos
 São as tabelas, views, funções, types, sequences, entre outros, pertencentes ao schemas.
 
 
```
 CREATE DATABASE name
    [ [ WITH ] [ OWNER [=] user_name ]
           [ TEMPLATE [=] template ]
           [ ENCODING [=] encoding ]
           [ LOCALE [=] locale ]
           [ LC_COLLATE [=] lc_collate ]
           [ LC_CTYPE [=] lc_ctype ]
           [ TABLESPACE [=] tablespace_name ]
           [ ALLOW_CONNECTIONS [=] allowconn ]
           [ CONNECTION LIMIT [=] connlimit ]
           [ IS_TEMPLATE [=] istemplate ] ]
 
 ```
 
 ```
 ALTER DATABASE name [ [ WITH ] option [ ... ] ]

where option can be:

    ALLOW_CONNECTIONS allowconn
    CONNECTION LIMIT connlimit
    IS_TEMPLATE istemplate

ALTER DATABASE name RENAME TO new_name

ALTER DATABASE name OWNER TO { new_owner | CURRENT_ROLE | CURRENT_USER | SESSION_USER }

ALTER DATABASE name SET TABLESPACE new_tablespace

ALTER DATABASE name SET configuration_parameter { TO | = } { value | DEFAULT }
ALTER DATABASE name SET configuration_parameter FROM CURRENT
ALTER DATABASE name RESET configuration_parameter
ALTER DATABASE name RESET ALL
 ```

 ```
 DROP DATABASE [ IF EXISTS ] name [ [ WITH ] ( option [, ...] ) ]

where option can be:

    FORCE
 ```

# Schemas
 
 ```
 CREATE SCHEMA schema_name [ AUTHORIZATION role_specification ] [ schema_element [ ... ] ]
 CREATE SCHEMA AUTHORIZATION role_specification [ schema_element [ ... ] ]
 CREATE SCHEMA IF NOT EXISTS schema_name [ AUTHORIZATION role_specification ]
 CREATE SCHEMA IF NOT EXISTS AUTHORIZATION role_specification

where role_specification can be:

    user_name
  | CURRENT_ROLE
  | CURRENT_USER
  | SESSION_USER
 
```
 
 ```
 ALTER SCHEMA name RENAME TO new_name
 ALTER SCHEMA name OWNER TO { new_owner | CURRENT_ROLE | CURRENT_USER | SESSION_USER }
 ```
 
 ```
 DROP SCHEMA [name]
 ```
 
#### Melhores práticas
 
 > CREATE SCHEMA IF NOT EXISTS schema_name [AUTHORIZATION role_specification]
 
 > DROP SCHEMA IF EXISTS [name]

 
 # Parte 2: Tabelas, columas e tipos de dados
 
 Definição de tabela é conjunto de dados dispostos em colunas e linhas referente a um objetivo comum.
 
 As colunas são concideradas como "campos das tabelas", como atributos da tabela.
 
 As linhas de uma tabela são chamadas também de tuplas, e é onde estão contidos os valores, os dados.
 
 
 ## Primary key / Chave primária / PK
 No conceito de modelo de  dados relacionados e obedecendo as regras de normalização, uma PK é um conjunto de um ou mais campos que nunca se repetem em uma tabela e que seus valores garantem a integridade dos dados únicos e a utilização do mesmo como referência para relacionamento entre demais tabelas.
 
  - não pode haver duas ocorrências de uma mesma entidade com o mesmos conteúdo ba PK
  - A chave primária não pode ser composta por atributos opcionail, ou seja, atributos qua aceite nulo.
  - Os atributos identificadores devem ser o conjunto minimo que pode identificar cada instância de uma entidade.
  - Não devem ser usados chaves externas
  - Não deve conter informaçõe volátil.
 
 
 ## Foreign Key / Chave estrangeira / FK
 Campo, ou conjunto de campos que são referências de chaves primárias de outras tabelas ou da mesma tabela.  
 Sua principal função é garantir a integridade referencial entra tabelas.
 

Name	| Aliases |	Description
---------- | --------------| -----------------------------------------
bigint |	int8	| signed eight-byte integer
bigserial |	serial8	| autoincrementing eight-byte integer
bit [ (n) ] |  |	fixed-length bit string
bit varying [ (n) ] |	varbit [ (n) ] |	variable-length bit string
boolean |	bool |	logical Boolean (true/false)
box | |	rectangular box on a plane
bytea | |	binary data (“byte array”)
character [ (n) ] |	char [ (n) ] |	fixed-length character string
character varying [ (n) ] |	varchar [ (n) ] |	variable-length character string
cidr | | 	IPv4 or IPv6 network address
circle | |	circle on a plane
date | |	calendar date (year, month, day)
double precision |	float8 |	double precision floating-point number (8 bytes)
inet | | 	IPv4 or IPv6 host address
integer |	int, int4 |	signed four-byte integer
interval [ fields ] [ (p) ] | |	 	time span
json | |	textual JSON data
jsonb | |	binary JSON data, decomposed
line | |	infinite line on a plane
lseg | |	line segment on a plane
macaddr | |	MAC (Media Access Control) address
macaddr8 | |	MAC (Media Access Control) address (EUI-64 format)
money | |	currency amount
numeric [ (p, s) ] |	decimal [ (p, s) ] |	exact numeric of selectable precision
path | |	geometric path on a plane
pg_lsn | |	PostgreSQL Log Sequence Number
pg_snapshot | |	user-level transaction ID snapshot
point | |	geometric point on a plane
polygon | |	closed geometric path on a plane
real |	float4 |	single precision floating-point number (4 bytes)
smallint |	int2 |	signed two-byte integer
smallserial |	serial2 |	autoincrementing two-byte integer
serial |	serial4 |	autoincrementing four-byte integer
text | |	variable-length character string
time [ (p) ] [ without time zone ] | | time of day (no time zone)
time [ (p) ] with time zone |	timetz |	time of day, including time zone
timestamp [ (p) ] [ without time zone ] | |	date and time (no time zone)
timestamp [ (p) ] with time zone |	timestamptz |	date and time, including time zone
tsquery | |	text search query
tsvector	| | text search document
txid_snapshot | |	user-level transaction ID snapshot (deprecated; see pg_snapshot)
uuid | |	universally unique identifier
xml | |	XML data

 
# Parte 3: DML e DDL
 
 #### DML - Data manipulation Language
 Linguagem de manipulação de dados  
 INSERT, UPDATE, DELETE, SELECT  
 * o select, alguns consideram como DML, outros como DQL, que significa query language, ou linguagem de consulta de dados
 
 #### DDl - Dara definition laguage
 Linguagem de definição de dados  
 CREATE, ALTER, DROP
 
 URL:/ https://github.com/drobcosta/digital_innovation_one
 
 
 
 
 
 
 
 
 
 
