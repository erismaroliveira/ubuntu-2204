Autor: Erismar Oliveira<br>
Instagram ErismarDev: https://www.instagram.com/erismardev<br>
YouTube ErismarDev: https://www.youtube.com/erismardev<br>
LinkedIn Erismar Oliveira: https://www.linkedin.com/in/erismar-oliveira/<br>
Github Erismar Oliveira: https://github.com/erismaroliveira<br>
Data de criação: 13/08/2024<br>
Data de atualização: 13/08/2024<br>
Versão: 0.1<br>

Site Oficial do Apache2: https://httpd.apache.org/<br>
Site Oficial do PHP (7.x ou 8.x): https://www.php.net/

Site Oficial do W3C School HTML5: https://www.w3schools.com/html/default.asp<br>
Site Oficial do W3C School CSS: https://www.w3schools.com/css/default.asp<br>
Site Oficial do W3C School JavaScript: https://www.w3schools.com/js/default.asp<br>
Site Oficial do W3C School PHP: https://www.w3schools.com/php/default.asp

O Servidor HTTP (Hypertext Transfer Protocol) Apache ou Servidor Apache ou HTTP Daemon Apache ou somente Apache, é o servidor web livre criado em 1995 por um grupo de desenvolvedores da NCSA, tendo como base o servidor web NCSA HTTPd criado por Rob McCool.

HTML (Hypertext Markup Language) é uma linguagem de marcação utilizada na construção de páginas na Web. Documentos HTML podem ser interpretados por navegadores. A tecnologia é fruto da junção entre os padrões HyTime e SGML. HyTime é um padrão para a representação estruturada de hipermídia e conteúdo baseado em tempo.

PHP (Hypertext Preprocessor, originalmente Personal Home Page) é uma linguagem interpretada livre, usada originalmente apenas para o desenvolvimento de aplicações presentes e atuantes no lado do servidor, capazes de gerar conteúdo dinâmico na World Wide Web.

#01\_ Instalando o Apache2 Server e PHP 8.x<br>

```bash
#atualizando as listas do Apt
sudo apt update

#instalando as dependências do Apache2 Server
sudo apt install git vim perl python2 python3 unzip ghostscript zlib1g zlib1g-dev apt-transport-https

#instalando o Apache2 Server e PHP 8.x
#opção da contra barra (\): criar uma quebra de linha no terminal
sudo apt install apache2 apache2-utils apache2-bin apache2-data php8.1 php8.1-cli php8.1-common \
php8.1-mysql php8.1-opcache php8.1-readline php8.1-common php8.1-bcmath php8.1-curl php8.1-intl \
php8.1-mbstring php8.1-xml php8.1-zip php8.1-soap php-imagick php-json libapache2-mod-php libapr1 \
libapache2-mod-php8.1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap
```

#02\_ Verificando o Serviço e Versão do Apache2 Server e do PHP<br>

```bash
#verificando o serviço do Apache2 Server
sudo systemctl status apache2
sudo systemctl restart apache2
sudo systemctl reload apache2
sudo systemctl stop apache2
sudo systemctl start apache2

#analisando os Log's e mensagens de erro do Servidor do Apache2 (NÃO COMENTADO NO VÍDEO)
#opção do comando journalctl: x (catalog), e (pager-end), u (unit)
sudo journalctl -xeu apache2

#verificando as versões do Apache2 Server e do PHP
#opção do comando apache2ctl: -V (version)
#opção do comando php: -v (version)
sudo apache2ctl -V
sudo php -v
```

#03\_ Verificando a Porta de Conexão do Apache2 Server<br>

```bash
#OBSERVAÇÃO IMPORTANTE: no Ubuntu Server as Regras de Firewall utilizando o comando:
#iptables ou: ufw está desabilitado por padrão (INACTIVE), caso você tenha habilitado
#algum recurso de Firewall é necessário fazer a liberação do Fluxo de Entrada, Porta
#e Protocolo TCP do Serviço corresponde nas tabelas do firewall e testar a conexão.

#opção do comando lsof: -n (network number), -P (port number), -i (list IP Address), -s (alone directs)
sudo lsof -nP -iTCP:'80' -sTCP:LISTEN
```

#04\_ Localização dos Arquivos de Configuração do Apache2 Server e do PHP 8.x<br>

```bash
/etc/apache2/                  <-- Diretório de configuração do Apache 2 Server
/etc/apache2/apache2.conf      <-- Arquivo de configuração do Apache 2 Server
/etc/apache2/sites-available/  <-- Diretório padrão dos Sites Acessíveis do Apache 2 Server
/etc/apache2/conf-available/   <-- Diretório padrão das Configurações Acessíveis do Apache 2 Server
/etc/php/                      <-- Diretório de configuração do PHP 7.x ou 8.x
/etc/php/8.1/apache2/php.ini   <-- Arquivo de configuração do PHP 8.x do Apache 2 Server
/var/www/html/                 <-- Diretório padrão das Hospedagem de Site do Apache 2 Server
/var/log/apache2/              <-- Diretório padrão dos Logs do Apache 2 Server
```

#05\_ Adicionando o Usuário Local no Grupo Padrão do Apache2 Server<br>

```bash
#adicionando o seu usuário no grupo do Apache2
#opções do comando usermod: -a (append), -G (groups), $USER (environment variable)
#OBSERVAÇÃO IMPORTANTE: você pode substituir a variável de ambiente $USER pelo
#nome do usuário existente no sistema para adicionar no Grupo desejado.
sudo usermod -a -G www-data $USER

#fazendo login em um novo grupo do Apache2
newgrp www-data

#verificando os identificadores de usuário e grupos
id

#verificando informações do grupo WWW-DATA
sudo getent group www-data

#recomendo fazer logout do usuário para testar as permissões de grupos
#OBSERVAÇÃO: você pode utilizar o comando: exit ou tecla de atalho: Ctrl+D
exit

#OBSERVAÇÃO IMPORTANTE: caso a conexão SSH trave, utilize os caracteres de escape para
#finalizar conexões SSH.
#caracteres: ~ (til) e . (ponto)
~.
```

#06\_ Criando um diretório de Teste do HTML e PHP no Apache2 Server<br>

```bash
#acessando o diretório padrão dos Sites do Apache2 Server (DocumentRoot)
cd /var/www/html

#criando o diretório de teste das páginas HTML e PHP
#opção do comando mkdir: -v (verbose)
sudo mkdir -v teste

#alterando as permissões do diretório de teste
#opção do comando chmod: -R (recursive), -v (verbose), 2775 (Set-GID=2,User=RWX,Group=RWS,Other=R-X)
sudo chmod -Rv 2775 teste/

#alterando o dono e grupo do diretório de teste
#opção do comando chown: -R (recursive), -v (verbose), root (User), . (separate), www-date (group)
sudo chown -Rv root.www-data teste/

#acessando o diretório criado de teste
cd teste
```

#07\_ Criando páginas HTML e PHP para testar o Apache2 Server<br>

```bash
#OBSERVAÇÃO IMPORTANTE: nesse exemplo vamos editar os arquivos teste.html, teste.php e phpinfo.php
#utilizando o Editor de Texto em Linha de Comando Vim.

#OBSERVAÇÃO IMPORTANTE: no Microsoft Windows utilizando o Powershell no processo de copiar e colar
#o código HTML ou PHP ele desconfigura o código, recomendo no Windows utilizar o software PuTTY ou
#Git Bash para editar os códigos ou copiar e colar. No Linux Mint e macOS essa falha não acontece.

#OBSERVAÇÃO: tanto no Microsoft Windows como no GNU/Linux (Linux Mint, Ubuntu Desktop, etc...) ou no
#macOS recomendo sempre utilizar o Editor de Texto em Modo Gráfico IDE Microsoft Visual Studio, por
#padrão ele já entende toda a codificação HTML, PHP, JavaScript, JSON, etc..., facilitando a criação
#e modificação dos arquivos desse curso.

#criando o arquivo em HTML
#OBSERVAÇÃO: ALTERAR O NOME DO ARQUIVO PARA O SEU PRIMEIRO NOME TUDO EM MINÚSCULO
sudo vim seu_nome.html
INSERT
```

```html
<!-- Início do código HTML: declaração do tipo de arquivo que será enviado para a navegador -->
<!DOCTYPE html>
<!-- Tag HTML: Define a raiz de um documento HTML -->
<html lang="pt-br">
  <!-- Tag HEAD: Define um cabeçalho para um documento ou seção -->
  <head>
    <!-- Tag TITLE: Define um título para o documento -->
    <title>Teste da Linguagem HTML</title>
    <!-- Tag META: Define metadados sobre um documento HTML -->
    <meta charset="utf-8" />
    <!-- Fechamento da Tag: HEAD -->
  </head>
  <!-- Tag BODY: Define o corpo do documento -->
  <body>
    <!-- Tag H1: Define títulos HTML -->
    <!-- Tag BR: Define uma única quebra de linha -->
    <h1>Teste da Linguagem HTML (HyperText Markup Language)</h1>
    Autor: Erismar Oliveira<br />
    Editado por: SEU NOME AQUI<br />
    <!-- Tag: A Define um hiperlink -->
    LinkedIn:
    <a href="https://www.linkedin.com/in/erismar-oliveira/">Erismar Oliveira</a
    ><br />
    Instagram:
    <a href="https://www.instagram.com/erismardev">Erismar Dev</a><br />
    YouTube:
    <a href="https://www.youtube.com/erismardev">Erismar Dev</a><br />
    <!-- Fechamento da Tag: BODY -->
  </body>
  <!-- Fechamento da Tag: HTML -->
</html>
```

```bash
#salvar e sair do arquivo
ESC SHIFT :x <Enter>

#criando o arquivo em PHP
#OBSERVAÇÃO: ALTERAR O NOME DO ARQUIVO PARA O SEU PRIMEIRO NOME TUDO EM MINÚSCULO
sudo vim seu_nome.php
INSERT
```

```php
<!DOCTYPE html>
	<html lang="pt-br">
		<head>
			<title>Teste da Linguagem PHP</title>
			<meta charset="utf-8">
		</head>
		<body>
			<!-- Início do script PHP: ?php -->
			<?php
				// Função ECHO: Imprimir uma ou mais strings na saída padrão
				echo '<h1>Teste da Linguagem HTML (HyperText Markup Language)</h1>';
				echo 'Autor: Erismar Oliveira<br>';
				echo 'Editado por: SEU NOME AQUI<br>';
				echo 'LinkedIn: https://www.linkedin.com/in/erismar-oliveira/<br>';
				echo 'Instagram: https://www.instagram.com/erismardev/<br>';
				echo 'YouTube: https://youtube.com/erismardev<br>';
			// Fechamento do Script PHP
			?>
		</body>
	</html>
```

```bash
#salvar e sair do arquivo
ESC SHIFT :x <Enter>

#criando o arquivo de informações do PHP
sudo vim phpinfo.php
INSERT
```

```php
<?php
	// Função do PHP para gerar a página de documentação e parâmetros do PHP
	phpinfo();
?>
```

```bash
#salvar e sair do arquivo
ESC SHIFT :x <Enter>
```

#08\_ Testando o Apache2 Server e o PHP no navegador<br>

```bash
#utilizar os navegadores para testar suas páginas
firefox ou google chrome: http://endereço_ipv4_ubuntuserver
firefox ou google chrome: http://endereço_ipv4_ubuntuserver/teste/
```
