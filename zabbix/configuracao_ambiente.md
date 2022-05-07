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
dpkg -i zabbix-sender_6.0.4-1+ubuntu20.04_amd64.deb;\
dpkg-reconfigure locales
~~~

### Configurando Mysql e banco de dados inicial

~~~
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'zabbix';
mysql_secure_installation
### 1BsIM2s5kDPst2GZe9vHA3TSIoCgCGNHVoaKw2w3jCE
mysql -uroot -p
password
mysql> 
~~~
~~~
mysql -uroot -p'1BsIM2s5kDPst2GZe9vHA3TSIoCgCGNHVoaKw2w3jCE'
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by '1BsIM2s5kDPst2GZe9vHA3TSIoCgCGNHVoaKw2w3jCE';
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

# Instalação e configuração do Grafana

~~~
clear ;\
echo "BAIXANDO REPOSITORIOS E CHAVES PARA O GRAFANA" ; sleep 2 ;\
apt-get install -y apt-transport-https ; \
apt-get install -y software-properties-common wget ;\
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add - ;\
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list ;\
clear ;\
apt-get update ; clear ;\
clear ;\
echo "INSTALANDO O GRAFANA" ; sleep 2 ;\
apt-get install grafana -y ;\
systemctl daemon-reload ; sudo systemctl start grafana-server ; sudo systemctl enable grafana-server ;\ 
clear ;\
echo "INSTALANDO O PLUGIN DO ZABBIX NO GRAFANA" ; sleep 2 ;\
grafana-cli plugins install alexanderzobnin-zabbix-app
~~~



~~~
