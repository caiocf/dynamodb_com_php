# dynamodb_com_php
Este passos abaixo mostrar como criar uma maquina PHP integrado com o dynamodb da AWS

a) Crie uma ROLE para EC2 no painel IAM e associe a uma police de `AmazonDynamoDBFullAccess`

b) Crie uma instancia EC2( recomenda-se na região de N. Virginia para não precisar alterar o codigo php)  na AWS e associe a este ROLE do passo anterior e libere as portas de entrada 22/tcp, 80/tcp


c) Instalando Apache e PHP e Git na instancia EC2.
```bash
[root@ip-172-31-48-200 /]# yum update -y
[root@ip-172-31-48-200 /]# yum install httpd php git -y
[root@ip-172-31-48-200 /]# service httpd start
[root@ip-172-31-48-200 /]# chkconfig httpd on
[root@ip-172-31-48-200 /]# cd /var/www/html
[root@ip-172-31-48-200 /]# echo "<?php phpinfo();?>" > home.php
[root@ip-172-31-48-200 /]# git clone https://github.com/caiocf/dynamodb_com_php.git
```

d) Instalando Gerenciador de Dependencias do PHP Composer (https://getcomposer.org/download/)
```bash
[root@ip-172-31-48-200 /]# cd /var/www/html
[root@ip-172-31-48-200 /]# php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
[root@ip-172-31-48-200 /]# php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
[root@ip-172-31-48-215 /]# php -r "unlink('composer-setup.php');"
```

e) Instalando SDK AWS para o PHP usando o Composer:
```bash
[root@ip-172-31-48-200 /]# cd /var/www/html
[root@ip-172-31-48-200 /]# php -d memory_limit=-1 composer.phar require aws/aws-sdk-php
```

f) Criando as tabelas e dados:

Cria estrutura de tabelas:
http://54.162.90.100/dynamodb/createtables.php

Faz o inserção de dados:
http://54.162.90.100/dynamodb/createdata.php
