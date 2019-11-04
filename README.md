
Ambiente de desenvolvimento Docker com Apache + PHP 7.3 com as extensões aos SGBDs MySQL, PostgreSQL, Oracle, demais módulos usuais e o gerenciador composer

**Pré-requisitos:** Ter o Docker e Docker-compose instalados.

[Instalando Docker no Windows 10](https://mundodacomputacaointegral.blogspot.com/2019/10/instalando-o-docker-no-windows.html)

[Instalando Docker e Docker-compose no Linux (qualquer distro)](https://mundodacomputacaointegral.blogspot.com/2019/10/instalando-docker-e-docker-compose-no-Linux.html)

Após ter instalado o Docker e Docker-compose, segue os procedimentos: 

1. Fork do repositório

2. Clonar o repositório forkado

3. Acessar o diretório onde salvou o clone do repositório

4. Para iniciar com container MySQL
   Execute `docker-compose up -d apache2-php7.3 app_mysql`
   
   Para iniciar com container PostgreSQL
   Execute `docker-compose up -d apache2-php7.3 app_pgsql`

5. Adicione no arquivo hosts

   **Windows:** _C:\Windows\System32\drivers\etc\hosts_

   **Linux:** _/etc/hosts_

    `127.0.0.1 app.local`
 
    `127.0.0.1 app2.local`
   
6. Acessar o container PHP para instalar o Laravel Framework
   `docker exec -it apache2-php7.3 bash`
   
   Dentro do container PHP, acessar no diretório _/var/www/html/app_ e execute 

   `rm .gitignore`

   `composer create-project --prefer-dist laravel/laravel .`
   
7. No browser acesse [http://app.local](http://app.local)

8. Esse ambiente de desenvolvido inclui 2 Vhosts no Apache de exemplo para 2 projetos, mas pode ter N vhosts, basta reutilizar o arquivo _vhost.conf_ para o novo arquivo, alterando o _server_name_ e adicionar no _Dockerfile_ do PHP. Lembrar de adicionar no arquivo hosts para cada Vhost do projeto.

9. No Linux para ter permissão no volume _src/app_ e _src/app2_, acessa até o diretório do ambiente e execute:

   `sudo chown -R $(whoami):$(whoami) src/app` 

   `sudo chown -R $(whoami):$(whoami) src/app2`
  
10. No Laravel Framework precisa ajustar as permissões do diretório _storage_ dentro do container PHP e editar o _src/app/.env_ em _APP_NAME=_ para _app.local_ que nesse caso é o ServerName definido no Vhost. Para os demais Vhosts que houver também.

`docker exec -it apache2-php7.3 bash`

`chown -R www-data:www-data app/storage`

Feito!

