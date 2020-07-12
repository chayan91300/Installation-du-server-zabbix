![logo](https://github.com/chayan91300/Installation-du-server-zabbix/blob/master/zabbix.png)

# Installer et configurer le serveur Zabbix


`sudo apt-get install language-pack-fr`

# a. Installer le référentiel Zabbix 
```
# wget https://repo.zabbix.com/zabbix/5.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.0-1+focal_all.deb
# dpkg -i zabbix-release_5.0-1+focal_all.deb
# apt update 
```

# b. Installer le serveur Zabbix, l’interface, l’agent
```
# apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-agent`
```
# c. Créer la base de données initiale
Exécutez ce qui suit sur votre hôte de base de données. 

# mysql -uroot -p 

mot de passe
```
mysql> create database zabbix character set utf8 collate utf8_bin;
mysql> create user zabbix@localhost identified by 'password';
mysql> grant all privileges on zabbix.* to zabbix@localhost;
mysql> flush privileges;
mysql> quit; 
```


# Sur le serveur Zabbix, importez le schéma initial et les données. Vous serez invité à saisir votre mot de passe nouvellement créé.
```
zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -u zabbix -p zabbix -ppassworld -d zabbix`
```
# d. Configurer la base de données pour le serveur Zabbix
```
Edit file: nano/etc/zabbix/zabbix_server.conf 

DBPassword=password`
```
# e. Configurer PHP pour l’interface Zabbix

```
Edit file: nano /etc/zabbix/apache.conf, décompresser et définir le bon fuseau horaire pour vous. 

# php_value date.timezone Europe/Riga (région de votre choix)
|                     |      |
|---------------------|------|
| Max_Execution Time= |   600|
| Max_Input_Time=     |   600|
| Memory_limit=       |  256m|
| Post_Max_Size=      |   32m|
| Upload_Max_FileSize=|    32|
|                     |      |
|                     |      |

```


# f. Démarrer les processus du serveur et de l’agent Zabbix
Démarrez les processus du serveur et de l’agent Zabbix et lancez-le au démarrage du système. 

```
# systemctl restart zabbix-server zabbix-agent apache2
# systemctl enable zabbix-server zabbix-agent apache2 
```
# g. Configure Zabbix frontend 

Connectez-vous à votre interface Zabbix nouvellement installée: http://server_ip_or_name/zabbix


