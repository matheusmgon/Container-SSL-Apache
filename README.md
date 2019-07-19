# DOCKER CONTAINER + SSL (LET'S ENCRYPT) + APACHE2

Para fazer a instalação do SSLl com docker, é bem simples:

Vamos utilizar o certbot e forçar o SSL dentro do container com a aplicação WEB.

Toda a Magica é realizada no host, sem a necessidade de inserir instruções dentro do contaier.

No meu caso, utilize o mod_proxy para redirecionar acessos do host para o container, então vou suar o meu como exemplo:

Vhost:


    <VirtualHost xxxxxxxxx.com.br:80>

   	  ServerAdmin matheus@xxxxxx.com.br
  	  RewriteEngine On
  	  ProxyRequests Off
  	  ProxyPreserveHost On
  	  ServerName xxxxxxx.com.br

  	  <Proxy *>
  		    Require all granted
  	  </Proxy>


  	  ProxyPass "/" "http://localhost:9000/" connectiontimeout=5 timeout=30 keepalive=on
  	  ProxyPassReverse "/" "http://localhost:9000/"


  	  ErrorLog ${APACHE_LOG_DIR}/xxxxxx-error.log
  	  CustomLog ${APACHE_LOG_DIR}/xxxxxx-access.log combined
    
    
     </VirtualHost>
  	
Reinicie o Apache
 
    service apache2 restart


Feito isto, vamos para a parte magica

     apt install certbot python-certbot-apache

OK, agora vamos instalar o certificado no Host:
 
    certbot --apache -d xxxxx.com.br -d www.xxxxx.com.br --email email@xxxxx.com.br --agree-tos
 
Perfeito, verifique se no arquivo "/etc/apache2/sites-avaliables/xxxx-le-ssl.conf/ foi feito o apontamento correto para as postas

PS: Se quiser "forçar" o redirect, insira um ".htaccess" dentro da aplicação do Container:
      
      RewriteEngine On
      RewriteCond %{SERVER_PORT} 80
      RewriteRule ^(.*)$ https://xxxxx.com.br/$1 [R,L]
    
    




OBRIGADO
 
