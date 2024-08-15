Autor: Erismar Oliveira<br>
Instagram ErismarDev: https://www.instagram.com/erismardev<br>
YouTube ErismarDev: https://www.youtube.com/erismardev<br>
LinkedIn Erismar Oliveira: https://www.linkedin.com/in/erismar-oliveira/<br>
Github Erismar Oliveira: https://github.com/erismaroliveira<br>
Data de criação: 13/08/2024<br>
Data de atualização: 13/08/2024<br>
Versão: 0.1<br>

Site Oficial do MySQL: https://www.mysql.com/<br>
Site Oficial do MariaDB: https://mariadb.org/<br>
Site Oficial do Workbench: https://www.mysql.com/products/workbench/

Site Oficial do W3C School MySQL: https://www.w3schools.com/mysql/default.asp

O MySQL é um sistema de gerenciamento de banco de dados, que utiliza a linguagem SQL como interface. É atualmente um dos sistemas de gerenciamento de bancos de dados mais populares da Oracle Corporation, com mais de 10 milhões de instalações pelo mundo.

#01\_ Instalando o MySQL Server e Client<br>

```bash
#atualizando as listas do Apt
sudo apt update

#instalando o MySQL Server e Client
sudo apt install git vim libproj22 proj-data mysql-server-8.0 mysql-client-8.0
```

#02\_ Verificando o Serviço e Versão do MySQL Server<br>

```bash
#verificando o serviço do MySQL Server
sudo systemctl status mysql
sudo systemctl restart mysql
sudo systemctl stop mysql
sudo systemctl start mysql

#verificando as versões do MySQL Server e Client
sudo mysqld --version
sudo mysql --version
```

#03\_ Verificando a Porta de Conexão do MySQL Server<br>

```bash
#OBSERVAÇÃO IMPORTANTE: no Ubuntu Server as Regras de Firewall utilizando o comando:
#iptables ou: ufw está desabilitado por padrão (INACTIVE), caso você tenha habilitado
#algum recurso de Firewall é necessário fazer a liberação do Fluxo de Entrada, Porta
#e Protocolo TCP do Serviço corresponde nas tabelas do firewall e testar a conexão.

#opção do comando lsof: -n (network number), -P (port number), -i (list IP Address), -s (alone directs)
sudo lsof -nP -iTCP:'3306' -sTCP:LISTEN
```

#04\_ Localização dos Arquivos de Configuração do MySQL Server<br>

```bash
/etc/mysql                          <-- Diretório de configuração do SGBD MySQL Server
/etc/mysql/mysql.conf.d/mysqld.cnf  <-- Arquivo de configuração do Servidor SGBD do MySQL Server
/etc/mysql/mysql.conf.d/mysql.cnf   <-- Arquivo de configuração do Cliente SGBD do MySQL Client
/var/log/mysql                      <-- Diretório padrão dos Logs do SGBD Mysql Server
/var/lib/mysql                      <-- Diretório da Base de Dados padrão do SGBD MySQL Server
```

#05\_ Acessando o MySQL Server utilizando o MySQL Client (Console)<br>

```bash
#OBSERVAÇÃO IMPORTANTE: por padrão o usuário Root do MySQL Server não tem senha para
#se logar no MySQL Client Console, sendo necessário fazer a configuração de segurança.

#opções do comando mysql: -u (user), -p (password)
sudo mysql -u root -p
```

#06\_ Aplicando a segurança de acesso do usuário Root no MySQL Server<br>

```sql
/* OBSERVAÇÃO IMPORTANTE: O MYSQL TAMBÉM É CASE SENSITIVE, CUIDADO COM O NOME DA BASE
DE DADOS, TABELAS, COLUNAS, USUÁRIOS, ETC... NO MOMENTO DA CRIAÇÃO OU ATUALIZAÇÃO */

/* visualizando as bases de dados do MySQL */
SHOW DATABASES;

/* utilizando a base de dados mysql */
USE mysql;

/* mostrando as tabelas criadas na base de dados mysql */
SHOW TABLES;

/* selecionando o dados da tabela user, filtrando somente as colunas: user e host */
SELECT user,host FROM user;

/* alterando a senha do usuário Root Localhost */
/* OBSERVAÇÃO: ALTERAR A SENHA DO USUÁRIO ROOT CONFORME A SUA NECESSIDADE */
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'pti@2024';

/* alterando as permissões do usuário Root Localhost */
GRANT ALL ON *.* TO 'root'@'localhost';

/* aplicando todas as mudanças na base de dados */
FLUSH PRIVILEGES;

/* saindo do MySQL Client Console */
exit
```

```bash
#testando novamente o acesso ao MySQL Server agora com senha
#opções do comando mysql: -u (user), -p (password)
sudo mysql -u root -p
```

#07\_ Criando um usuário DBA (Data Base Administrator) no MySQL Server<br>

```sql
/* criando o usuário DBA Localhost */
/* OBSERVAÇÃO: ALTERAR A SENHA DO USUÁRIO DBA CONFORME A SUA NECESSIDADE */
CREATE USER 'dba'@'localhost' IDENTIFIED WITH mysql_native_password BY 'pti@2024';

/* alterando as permissões do usuário DBA Localhost */
GRANT ALL ON *.* TO 'dba'@'localhost';

/* aplicando todas as mudanças na base de dados */
FLUSH PRIVILEGES;

/* Verificando o Usuário DBA criado no Banco de Dados MySQL Server*/
SELECT user,host FROM mysql.user WHERE user='dba';

/* saindo do MySQL Client Console */
exit
```

```bash
#se logando com o usuário dba para testar a conexão com o MySQL Server
#opções do comando mysql: -u (user), -p (password)
sudo mysql -u dba -p
```

```sql
/* visualizando as bases de dados do MySQL */
SHOW DATABASES;

/* saindo do MySQL Client Console */
exit
```

#08\_ Adicionando o Usuário Local no Grupo Padrão do MySQL Server<br>

```bash
#opções do comando usermod: -a (append), -G (groups), $USER (environment variable)
#OBSERVAÇÃO IMPORTANTE: você pode substituir a variável de ambiente $USER pelo
#nome do usuário existente no sistema para adicionar no Grupo desejado.
sudo usermod -a -G mysql $USER

#fazendo login em um novo grupo do MySQL
newgrp mysql

#verificando os identificadores de usuário e grupos
id

#verificando informações do grupo MYSQL
sudo getent group mysql

#recomendo fazer logout do usuário para testar as permissões de grupos
#OBSERVAÇÃO: você pode utilizar o comando: exit ou tecla de atalho: Ctrl +D
exit

#opções do comando mysql: -u (user), -p (password)
mysql -u dba -p
```

```sql
/* testando os direitos do usuário DBA no MySQL Server */
SHOW DATABASES;
USE mysql;
SHOW TABLES;
exit
```

#09\_ Permitindo o Root do MySQL se Logar Remotamente no MySQL Client Console<br>

```bash
#fazendo o backup do arquivo de configuração do MySQL Server
#opção do comando cp: -v (verbose)
sudo cp -v /etc/mysql/mysql.conf.d/mysqld.cnf /etc/mysql/mysql.conf.d/mysqld.cnf.old

#editar o arquivo de configuração do MySQL Server
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
INSERT

	#alterar a linha: 31 variável do: bind-address = 127.0.0.1 para: 0.0.0.0
	bind-address = 0.0.0.0

	#comentar a linha:32 da variável do: mysqlx-bind-address
	#mysqlx-bind-address = 127.0.0.1

#salvar e sair do arquivo
ESC SHIFT :x <Enter>

#testando o arquivo de configuração do MySQL SERVER (NÃO COMENTADO NO VÍDEO)
#opção do comando mysqld: --validate-config (checked for problems without running the server)
sudo mysqld --validate-config

#reiniciar o serviço do MySQL Server
sudo systemctl restart mysql
sudo systemctl status mysql

#analisando os Log's e mensagens de erro do Servidor do MySQL (NÃO COMENTADO NO VÍDEO)
#opção do comando journalctl: x (catalog), e (pager-end), u (unit)
sudo journalctl -xeu mysql

#acessar o MySQL Server como Root
sudo mysql -u root -p
```

```sql
/* criando o usuário Root Remoto do MySQL Server */
/* OBSERVAÇÃO IMPORTANTE: QUANDO UTILIZADO O CARÁCTER % (PORCENTAGEM) O MYSQL
ENTENDE QUE O USUÁRIO PODE ACESSAR O SERVIDOR DE QUALQUER ORIGEM, DIFERENTE DA
OPÇÃO LOCALHOST QUE SÓ PERMITE O ACESSO LOCAL. CUIDADO COM ESSA OPÇÃO */
/* OBSERVAÇÃO: ALTERAR A SENHA DO USUÁRIO ROOT CONFORME A SUA NECESSIDADE */
CREATE USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'pti@2024';
GRANT ALL ON *.* TO 'root'@'%';
FLUSH PRIVILEGES;

/* verificando o usuário Root Remoto do MySQL Server */
USE mysql;
SELECT user,host FROM user;
exit
```

#10\_ Conectando no MySQL Server utilizando o MySQL Workbench<br>

```bash
#OBSERVAÇÃO IMPORTANTE: após a conexão com o MySQL Server utilizando MySQL Workbench somente o
#Banco de Dados Sys (Sistema) é mostrado em Esquemas, os demais Banco de Dados utilizados pelo
#MySQL Server não são mostrados por motivo de segurança.

#Link para download do MySQL Workbench: https://dev.mysql.com/downloads/workbench/

#conectando com o usuário Root Remoto do MySQL no Workbench
MySQL Connections: +
  Connection Name: wsvaamonde
  Connection Method: Standard (TCP/IP)
  Parameters:
    Hostname: 192.168.1.20 (alterar o endereço IPv4 do seu servidor)
    Port: 3306
    Username: root
    Password:
      Store in Keychain
        Password: pti@2024 (alterar a senha do usuário root do seu servidor)
    <OK>
  <Test Connection>
    <OK>
<OK>
```

#11\_ Integrando o MySQL Server com o Visual Studio Code VSCode<br>

```bash
#OBSERVAÇÃO IMPORTANTE: CONFORME COMENTADO NO VÍDEO E MOSTRADO, NA EXTENSÃO DO VSCODE NÃO APARECE
#NENHUM BANCO DE DADOS PADRÃO DO MYSQL SERVER, SOMENTE OS BANCOS DE DADOS CRIADOS PELO USUÁRIO,
#POR MOTIVO DE SEGURANÇA.

#instalando a Extensão do MySQL Server no VSCode
VSCode
  Extensões
    Pesquisar
      MySQL (Database manager for MySQL/MariaDB, PostgreSQL, SQLite, Redis and ElasticSearch)
        Instalar

#configurando a conexão com o MySQL Server no VSCode
VSCode
  Database
    <Create Connection>
      Name: UbuntuServer
      Server Type:
        MySQL
          Host: 192.168.1.20 (alterar o endereço IPv4 do seu servidor)
          Port: 3306
          Username: root
          Password: pti@2024 (alterar a senha do usuário root do seu servidor)
    <Save>
```
