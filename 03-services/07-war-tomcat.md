Autor: Erismar Oliveira<br>
Instagram ErismarDev: https://www.instagram.com/erismardev<br>
YouTube ErismarDev: https://www.youtube.com/erismardev<br>
LinkedIn Erismar Oliveira: https://www.linkedin.com/in/erismar-oliveira/<br>
Github Erismar Oliveira: https://github.com/erismaroliveira<br>
Data de criação: 13/08/2024<br>
Data de atualização: 13/08/2024<br>
Versão: 0.1<br>

Site Oficial do Apache2: https://httpd.apache.org/<br>
Site Oficial do Apache Tomcat: https://tomcat.apache.org/<br>
Site Oficial do OpenJDK: https://openjdk.org/

Site Oficial do W3C School HTML5: https://www.w3schools.com/html/default.asp<br>
Site Oficial do W3C School CSS: https://www.w3schools.com/css/default.asp<br>
Site Oficial do W3C School JavaScript: https://www.w3schools.com/js/default.asp<br>
Site Oficial do W3C School Java: https://www.w3schools.com/java/default.asp

Em engenharia de software, um arquivo WAR é um arquivo JAR usado para distribuir uma coleção de JavaServer Pages, Servlets Java, classes Java, arquivos XML, bibliotecas de tag, páginas web estáticas e outros recursos que, juntos, constituem uma aplicação web.

#01\_ Fazendo o download do WAR do Apache Tomcat Server desenvolvido em JavaEE<br>

```bash
#OBSERVAÇÃO IMPORTANTE: o projeto da Agenda desenvolvida em JavaEE do Prof.
#José de Assis no seu Github está desatualizado, o projeto que está no Github
#foi feito na versão anterior do Java e do Apache TomCAT, para resolver esse
#problema ele compilou um novo WAR que está no meu Github.

#OBSERVAÇÃO: esse novo WAR do Projeto da Agenda foi customizado e melhorado
#pela Prof(a). Sirlene Sanches, criando uma nova estrutura em CSS para deixar
#o ambiente mais bonito.

Acesse o Repositório: https://github.com/professorjosedeassis/javaEE

Clique em: Releases
  Em assets, clique em: agenda.war para fazer o Download.

LINK DE DOWNLOAD DO NOVO ARQUIVO WAR: https://github.com/erismaroliveira/ubuntu-2204/tree/main/war

A) arquivo: agenda.war versão antiga atualizada pela Prof(a). Sirlene Sanches;
B) arquivo: agenda_bootstrap.war versão nova atualizada pela Prof(a). Sirlene Sanches.
```

#02\_ Acessando o Apache TomCAT Server pelo Navegador<br>

```bash
#utilizar os navegadores para testar o WAR TomCAT
firefox ou google chrome: http://endereço_ipv4_ubuntuserver:8080
```

#03\_ Fazendo o Deploy da Aplicação Agenda de Contatos no Apache TomCAT Server<br>

```bash
Clique em: Manager App
  Usuário padrão: admin
  Senha padrão..: pti@2024
<Fazer Login>

A) Em: WAR file to deploy clique em: <Escolher arquivo>
B) Localize o arquivo WAR na pasta: Download clique em: <Abrir>
C) Finalize clicando em: <Deploy>

Após o Deploy da aplicação a nova URL (Uniform Resource Locator) de acesso será:

firefox ou google chrome: http://endereço_ipv4_ubuntuserver:8080/agenda

#OBSERVAÇÃO IMPORTANTE: acessando a aplicação Agenda pela primeira vez será apresentado
#uma mensagem de erro, esse erro está associado ao Banco de Dados que ainda não foi
#criado no MySQL, após a sua criação o sistema irá funcionar perfeitamente.
```

#04\_ Criando a Base de Dados no MySQL Server do projeto da Agenda em JavaEE<br>

```bash
#opções do comando mysql: -u (user), -p (password)
sudo mysql -u root -p
```

```sql
/* Criando o Banco de Dados DBAgenda */
CREATE DATABASE dbagenda;

/* Criando o Usuário Agenda com a Senha Agenda do Banco de Dados Agenda*/
/* OBSERVAÇÃO IMPORTANTE: POR MOTIVO DE SEGURANÇA SERÁ CRIADO UM USUÁRIO LOCALHOST NO
BANCO DE DADOS MYSQL, USUÁRIOS REMOTOS SOMENTE SE O SERVIDOR DE BANCO DE DADOS NÃO
ESTIVER NO MESMO SERVIDOR */
CREATE USER 'dbagenda'@'localhost' IDENTIFIED WITH mysql_native_password BY 'dbagenda';
GRANT USAGE ON *.* TO 'dbagenda'@'localhost';
GRANT ALL PRIVILEGES ON dbagenda.* TO 'dbagenda'@'localhost';
FLUSH PRIVILEGES;

/* Verificando o Usuário Agenda criado no Banco de Dados MySQL Server*/
SELECT user,host FROM mysql.user WHERE user='dbagenda';

/* Acessando o Banco de Dados DBAgenda */
USE dbagenda;

/* Criando a Tabela Contatos no Banco de Dados DBAgenda */
CREATE TABLE contatos (
	idcon int NOT NULL AUTO_INCREMENT,
	nome varchar(50) NOT NULL,
	fone varchar(15) NOT NULL,
	email varchar(50) DEFAULT NULL,
	PRIMARY KEY (idcon)
);

/* Verificando as informações das Tabelas */
SHOW TABLES;
DESC contatos;

/* Saindo do Banco de Dados */
exit
```

#05\_ Testando o acesso a Base de Dados DBAgenda com o usuário dbagenda<br>

```bash
#opções do comando mysql: -u (user), -p (password)
sudo mysql -u dbagenda -p
```

```sql
/* comandos básicos de verificação da base de dados e tabelas do MySQL */
SHOW DATABASES;
USE dbagenda;
SHOW TABLES;
DESC contatos;
exit
```

#06\_ Acessando novamente a Aplicação Agenda via Navegador e adicionando Registros<br>

```bash
#utilizar os navegadores para testar o WAR do Apache TomCAT
firefox ou google chrome: http://endereço_ipv4_ubuntuserver:8080/agenda
```

#07\_ Fazendo o Backup e Restore do Banco de Dados DBAgenda no MySQL Server<br>

```bash
#OBSERVAÇÃO IMPORTANTE: para esse teste, recomendo adicionar vários registros no Banco
#de Dados do DBAgenda, para verificar os procedimentos de Dump do Banco e Restore das
#informações.

#fazendo o backup do banco de dados DBAgenda
#opções do comando mysqldump: -u (user), -p (password), database
#opção do redirecionador de saída >: Redireciona a saída padrão (STDOUT)
sudo mysqldump -u root -p dbagenda > bkp-dbagenda.sql

#verificando o conteúdo do arquivo backupeado
sudo less bkp-dbagenda.sql

#opções do comando mysql: -u (user), -p (password)
sudo mysql -u dbagenda -p
```

```sql
/* comandos básicos de verificação da base de dados e tabelas do MySQL */
SHOW DATABASES;
USE dbagenda;
SHOW TABLES;

/* verificando todos os registros da Tabela Contatos */
SELECT * FROM contatos;

/* removendo todos os registros da Tabela Contatos */
TRUNCATE TABLE contatos;
SELECT * FROM contatos;
exit
```

```bash
#OBSERVAÇÃO IMPORTANTE: ATUALIZAR A PÁGINA DO SISTEMA DE AGENDA NO SEU NAVEGADOR PARA
#VERIFICAR QUE TODOS OS REGISTRO FORAM DELETADOS DO BANCO DE DADOS.
firefox ou google chrome: http://endereço_ipv4_ubuntuserver:8080/agenda

#restaurando o backup do banco de dados DBAgenda
#opções do comando mysql: -u (user), -p (password)
#opção do redirecionador de entrada <: Redireciona a entrada padrão (STDIN)
sudo mysql -u root -p dbagenda < bkp-dbagenda.sql

#opções do comando mysql: -u (user), -p (password)
sudo mysql -u dbagenda -p
```

```sql
/* comandos básicos de verificação da base de dados e tabelas do MySQL */
SHOW DATABASES;
USE dbagenda;
SHOW TABLES;

/* verificando todos os registros da Tabela Contatos */
SELECT * FROM contatos;
exit
```

```bash
#acessar novamente a aplicação para verificar se voltou os registros
firefox ou google chrome: http://endereço_ipv4_ubuntuserver:8080/agenda
```
