# Instalação e configuração do Zabbix

~~~sh
fonte: https://www.zabbix.com/br/download?zabbix=6.0&os_distribution=ubuntu&os_version=20.04_focal&db=mysql&ws=apache
~~~
### Instale o repositório Zabbix e Instale o servidor, o frontend e o agente Zabbix
~~~sh
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-1+ubuntu20.04_all.deb ; \
dpkg -i zabbix-release_6.0-1+ubuntu20.04_all.deb ; \
apt update -y
~~~
~~~sh
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent -y ; \
apt install mysql-server; \
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix/zabbix-sender_6.0.4-1%2Bubuntu20.04_amd64.deb; \
dpkg -i zabbix-sender_6.0.4-1+ubuntu20.04_amd64.deb


~~~

### Configurando Mysql e banco de dados inicial

~~~
# mysql -uroot -p
password
mysql> 
~~~
~~~
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'zabbix';
grant all privileges on zabbix.* to zabbix@localhost;
quit;
~~~

#### No servidor do Zabbix, importe o esquema inicial e os dados. Vocá será solicitado a inserir a senha que foi criada anteriormente.
~~~
zcat /usr/share/doc/zabbix-sql-scripts/mysql/server.sql.gz | mysql -uzabbix -p zabbix
~~~

#### Configure o banco de dados para o servidor Zabbix
~~~
nano /etc/zabbix/zabbix_server.conf
~~~
~~~
DBPassword=zabbix
~~~

### Inicie o servidor Zabbix e os processos do agente
~~~
systemctl restart zabbix-server zabbix-agent apache2 ; systemctl enable zabbix-server zabbix-agent apache2
~~~

### Acesso o zabbix via navegador 
~~~
http://server_ip_or_name/zabbix
~~~
