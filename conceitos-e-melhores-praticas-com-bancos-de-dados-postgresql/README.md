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















