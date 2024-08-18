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

O Tomcat é um servidor web Java, mais especificamente, um container de servlets. O Tomcat implementa, dentre outras de menor relevância, as tecnologias Java Servlet e JavaServer Pages e não é um container Enterprise JavaBeans. Desenvolvido pela Apache Software Foundation, é distribuído como software livre.

#01\_ Instalando as Dependências do Apache Tomcat Server<br>

```bash
#atualizando as lista do apt
sudo apt update

#OBSERVAÇÃO IMPORTANTE: no Ubuntu Server 22.04.x temos as versões disponíveis do OpenJDK e do
#OpenJRE: 8, 11, 17, 18, 19 e 21, cuidado na versão do Java que você está usando no seu projeto
#e a compatibilidade de versão do Apache TomCAT em relação ao OpenJDK e OpenJRE.

#instalando as dependências do Java OpenJDK e OpenJRE utilizadas no Apache Tomcat
#OBSERVAÇÃO: OpenJDK é uma implementação livre e gratuita da plataforma Java hoje da Oracle
#OBSERVAÇÃO: OpenJRE é um software necessário para que os programas em Java funcionem corretamente.
sudo apt install git vim openjdk-21-jdk openjdk-21-jre software-properties-common build-essential
```

#02\_ Verificando as Versões do Java OpenJDK e OpenJRE instalado<br>

```bash
#verificando as versões de Java instalado
#opção do comando grep: -i (ignore-case)
#opção do redirecionador |: Conecta a saída padrão com a entrada padrão de outro comando
sudo java -version
sudo apt list --installed | grep -i openjdk
sudo update-alternatives --list java
sudo update-java-alternatives --list
```

#03\_ Download do Apache Tomcat Server 10.1.x do site Oficial<br>

```bash
#OBSERVAÇÃO IMPORTANTE: recomendo que o procedimento abaixo seja feito utilizando o usuário:
#Root do Ubuntu para facilitar a instalação e configuração do Apache Tomcat Server 10.1.x.

#Link Oficial das versões do Apache Tomcat Server: https://dlcdn.apache.org/tomcat/

#mudando para o usuário Root do Ubuntu Server
#opção do comando sudo: -i (login)
sudo -i

#download da última versão do Apache TomCAT Server (link atualizado em 07/08/2024)
#OBSERVAÇÃO IMPORTANTE: o tempo todo o Apache TomCAT Server sofre alteração, antes
#de fazer o download do arquivo verifique a versão no link: https://dlcdn.apache.org/tomcat/
#opção do comando wget: -v (verbose), -O (output file)
wget -v -O /tmp/tomcat10.tar.gz https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.28/bin/apache-tomcat-10.1.28.tar.gz
```

#04\_ Descompactando e instalando o Apache Tomcat 10.1.x<br>

```bash
#descompactando o download do arquivo do Apache TomCAT
#opção do comando tar: -x (extract), -z (gzip), -v (verbose), -f (file), -C (directory)
tar -xzvf /tmp/tomcat10.tar.gz -C /tmp

#movendo o conteúdo do diretório do Apache TomCAT para o diretório /opt
#opção do comando mv: -v (verbose)
mv -v /tmp/apache-tomcat* /opt/tomcat
```

#05\_ Atualizando os arquivos de configuração do Apache Tomcat Server 10.1.x<br>

```bash
#download dos principais arquivos de configuração do Apache TomCAT Server
#opção do comando wget: -v (verbose), -O (output file)

#download do arquivo de configuração do Servidor Apache Tomcat
wget -v -O /opt/tomcat/conf/server.xml https://raw.githubusercontent.com/erismaroliveira/ubuntu-2204/main/conf/server.xml

#download do arquivo de configuração dos Usuários do Apache Tomcat
wget -v -O /opt/tomcat/conf/tomcat-users.xml https://raw.githubusercontent.com/erismaroliveira/ubuntu-2204/main/conf/tomcat-users.xml

#download do arquivo de configuração do Contexto do Apache Tomcat
wget -v -O /opt/tomcat/conf/context.xml https://raw.githubusercontent.com/erismaroliveira/ubuntu-2204/main/conf/context.xml

#download do arquivo de configuração do Meta Dados do Contexto Manager do Apache Tomcat
wget -v -O /opt/tomcat/webapps/manager/META-INF/context.xml https://raw.githubusercontent.com/erismaroliveira/ubuntu-2204/main/conf/context.xml

#download do arquivo de configuração do Meta Dados do Contexto Host Manager do Apache Tomcat
wget -v -O /opt/tomcat/webapps/host-manager/META-INF/context.xml https://raw.githubusercontent.com/erismaroliveira/ubuntu-2204/main/conf/context.xml

#download do arquivo de configuração do Meta Dados do Contexto Examples do Apache Tomcat
wget -v -O /opt/tomcat/webapps/examples/META-INF/context.xml https://raw.githubusercontent.com/erismaroliveira/ubuntu-2204/main/conf/context.xml

#download do arquivo de configuração do Meta Dados do Contexto Docs do Apache Tomcat
#NÃO COMENTADO OU BAIXADO NO VÍDEO (ADICIONADO DEPOIS PARA CORRIGIR O ERRO DA DOCUMENTAÇÃO)
wget -v -O /opt/tomcat/webapps/docs/META-INF/context.xml https://raw.githubusercontent.com/erismaroliveira/ubuntu-2204/main/conf/context.xml

#download do arquivo de configuração da Inicialização do Apache Tomcat
#OBSERVAÇÃO IMPORTANTE: NESSE ARQUIVO NA LINHA: 11 FICA A CONFIGURAÇÃO DA VERSÃO
#DO OPENJDK UTILIZADO, POR PADRÃO FOI ATUALIZADO PARA A VERSÃO 21.x NO DIA: 13/05/2024
wget -v -O /etc/systemd/system/tomcat10.service https://raw.githubusercontent.com/erismaroliveira/ubuntu-2204/main/conf/tomcat10.service
```

#06\_ Criando o Usuário de Serviço do Apache Tomcat Server 10.1.x<br>

```bash
#criando o usuário de serviço do Apache TomCAT
#opção do comando useradd: -m (create-home), -d (home-dir), -U (user-group), -s (shell)
useradd -m -d /opt/tomcat -U -s /bin/false tomcat
```

#07\_ Alterando as Permissões do Diretório do Apache Tomcat Server 10.1.x<br>

```bash
#alterando as permissões de dono e grupo
#opção do comando chown: -R (recursive), -v (verbose), tomcat:tomcat (user and group)
chown -Rv tomcat:tomcat /opt/tomcat

#alterando as permissões de acesso a arquivos e diretórios
#opção do comando chmod: -R (recursive), -v (verbose), u+x (user added execute/search)
chmod -Rv u+x /opt/tomcat/bin
```

#08\_ Habilitando o Serviço do Apache Tomcat Server 10.1.x<br>

```bash
#habilitando o serviço do Apache Tomcat Server
systemctl daemon-reload
systemctl enable tomcat10
systemctl start tomcat10

#saindo do perfil do usuário Root
exit
```

#09\_ Verificando o Serviço e Versão do Apache Tomcat Server 10.1.x<br>

```bash
#verificando o serviço do Apache Tomcat Server
sudo systemctl status tomcat10
sudo systemctl restart tomcat10
sudo systemctl stop tomcat10
sudo systemctl start tomcat10

#analisando os Log's e mensagens de erro do Servidor do TomCAT (NÃO COMENTADO NO VÍDEO)
#opção do comando journalctl: x (catalog), e (pager-end), u (unit)
sudo journalctl -xeu tomcat10

#verificando a versão do Apache Tomcat Server
sudo bash /opt/tomcat/bin/version.sh
```

#10\_ Verificando a Porta de Conexão do Apache Tomcat Server 10.1.x<br>

```bash
#OBSERVAÇÃO IMPORTANTE: no Ubuntu Server as Regras de Firewall utilizando o comando:
#iptables ou: ufw está desabilitado por padrão (INACTIVE), caso você tenha habilitado
#algum recurso de Firewall é necessário fazer a liberação do Fluxo de Entrada, Porta
#e Protocolo TCP do Serviço corresponde nas tabelas do firewall e testar a conexão.

#opção do comando lsof: -n (network number), -P (port number), -i (list IP Address), -s (alone directs)
sudo lsof -nP -iTCP:'8080' -sTCP:LISTEN
```

#11\_ Localização dos Arquivos de Configuração do Apache Tomcat Server<br>

```bash
/opt/tomcat                        <-- Diretório de configuração do Apache Tomcat Server
/opt/tomcat/bin                    <-- Diretório do binário (executável) do Apache Tomcat Server
/opt/tomcat/conf                   <-- Diretório das configurações do Apache Tomcat Server
/opt/tomcat/conf/server.xml        <-- Arquivo de configuração do Servidor do Apache Tomcat Server
/opt/tomcat/conf/tomcat-users.xml  <-- Arquivo de configuração dos Usuários do Apache Tomcat Server
/opt/tomcat/conf/context.xml       <-- Arquivo de configuração do Contexto do Apache Tomcat Server
/opt/tomcat/logs                   <-- Diretório dos Logs do Apache Tomcat Server
/opt/tomcat/webapps                <-- Diretório das Aplicações Web do Apache Tomcat Server
```

#12\_ Adicionado o Usuário Local no Grupo Padrão do Apache Tomcat Server<br>

```bash
#opções do comando usermod: -a (append), -G (groups), $USER (environment variable)
#OBSERVAÇÃO IMPORTANTE: você pode substituir a variável de ambiente $USER pelo
#nome do usuário existente no sistema para adicionar no Grupo desejado.
sudo usermod -a -G tomcat $USER

#fazendo login em um novo grupo do TOMCAT
newgrp tomcat

#verificando os identificadores de usuário e grupos
id

#verificando informações do grupo TOMCAT
sudo getent group tomcat

#recomendo fazer logout do usuário para testar as permissões de grupos
#OBSERVAÇÃO: você pode utilizar o comando: exit ou tecla de atalho: Ctrl +D
exit
```

#13\_ Editando o arquivo de configuração de usuários do Apache Tomcat Server

```bash
#editando o arquivo de criação de usuários do Tomcat
sudo vim /opt/tomcat/conf/tomcat-users.xml
INSERT
```

```xml
<!-- alterar os valores das variável a partir da linha: 30 -->
<!-- Configuração do Usuário, Senha e Papéis de administrador do Servidor Web Tomcat -->
<!-- Para criar novos usuários no Apache TomCAT Server é só copiar a linha abaixo e colar -->
<!-- na próxima linha alterando o nome, senha e papeis do novo usuário -->
<user username="admin" password="pti@2024" roles="manager-gui,manager,admin-gui,admin,tomcat,role1"/>
```

```bash
#salvar e sair do arquivo
ESC SHIFT : x <Enter>

#reiniciando e verificando o serviço do Apache Tomcat Server
sudo systemctl restart tomcat10
sudo systemctl status tomcat10
```

#14\_ Testando o Apache Tomcat Server no navegador<br>

```bash
#utilizar os navegadores para testar o Apache TomCAT
firefox ou google chrome: http://endereço_ipv4_ubuntuserver:8080
```

#15\_ Administrando o Apache Tomcat Server<br>

```bash
Clique em: Manager App
	Usuário padrão: admin
	Senha padrão..: pti@2024
<Fazer Login>
```
