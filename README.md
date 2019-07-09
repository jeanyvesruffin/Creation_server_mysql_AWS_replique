# Creation_server_mysql_AWS_replique
Creation server mysql sur Amazon Web Service avec replication Master/ Slave

# Commandes Linux/ Unix
![Cheatsheet command line Linux](https://files.fosswire.com/2007/08/fwunixrefshot.png "Cheatsheet command line Linux")

![Architecture client server](https://user-images.githubusercontent.com/37696862/60921632-3e474700-a29b-11e9-9770-47642e2c8414.png)



# Creation d'un compte AWS

https://aws.amazon.com/fr/

> - Cliquer en haut du bandeau du site pour choisir la localité du server.
Nous choisirons Paris.
> - Connectez vous à la console.
> - Démarrez une machine virtuelle avec EC2

## Création d'une machine virtuelle.

-  Cliquez sur AMI communauté, système d'exploitation Debian et sélectionner
une version 64 bits recente (ex: debian-stretch-hvm-x86_64-gp2-2019-06-18-79269 - ami-0791a60e93530af8a)

# ATTENTION !! Il est necessaire de garder IMPERATIVEMENT le fichier qui sera télechargé
Ce fichier sera LA CLE d'acces à nos serveurs.

# Création de nouvelles instances de votre serveur mysql.
https://docs.aws.amazon.com/fr_fr/quickstarts/latest/vmlaunch/step-2-connect-to-instance.html

- Allez dans le dossier ou se trouve le fichier .pem
- Donner les priviléges au fichier

                chmod 400 my-key-pair.pem
                
- Utiliser la commande qui permettra de créer une instance

                ssh -i /path/my-key-pair.pem ec2-user@public_dns_name
                
- Aller dans la maison de root
                
                $cd /root
  
  - Editer les prefs du compte root pour le bashrc

                $nano .bashrc
                
  
![Param bashrc](https://user-images.githubusercontent.com/37696862/60921873-cc233200-a29b-11e9-9c33-08909a36d624.png)

- Se déconnecter et se reconnecter 

                $ exit
                $ sudo su
                $ cd /root
                
- Vérifier la disponibilité de l'alias l

                $ l
                
- Mettre à jour le système

                $ apt update
                $ apt upgrade
                
- Installer les utilitaires usuels du système

                $ apt install nmap zip dnsutils net-tools tzdata lynx curl git screen locate ncdu

- Télécharger la dernière version de webmin

                $ wget http://prdownloads.sourceforge.net/webadmin/webmin_1.910_all.deb

- Installer le paquet en local

                $ dpkg -i webmin_1.910_all.deb 

- Finir l'installation des bibliothèques manquantes

                $ apt -f install
                
- Vérifier l'heure

                $ date

- Configureau le fuseau le cas échéant

                $ dpkg-reconfigure tzdata

- Création d'un user

                $ adduser user
                
- Création en mode user du dossier www et d’un index dans /home/user/. Débrayer temporaire en mode user pour la création

                $ su user
                $ mkdir /home/user/www
                $ nano /home/user/www/index.html
                $ exit

- Changement récursif de propriétaire et c'est apache le nouveau proprio

                $ chown -R www-data:www-data /home/user/www
                $ usermod -a -G www-data user

- Changement récursif des droits pour le dossier www pour ftp et samba

                $ chmod -R 775 /home/user/www

- Installer apache 2.4

                $ apt install apache2

- Activation du module SSL

                $ a2enmod ssl
                $ a2ensite default-ssl

- Relancer le service ( systemctl restart apache2)

                $ service apache2 restart

- Activer des modules populaires

                $ a2enmod rewrite
                $ service apache2 restart


- Installation de let’s encrypt/certbot
                
                $ cd /root
                $ wget https://dl.eff.org/certbot-auto
                $ chmod a+x certbot-auto
                $ ./certbot-auto

- Configuration apache ultime -> the site.conf, un seul fichier

                $ nano /etc/apache2/sites-available/thesite.conf

                <VirtualHost *:80>
                    ServerName tcsql1.cefim-formation.org
                    #ServerAlias www.domain.tld
                    RewriteEngine on
                    RewriteCond %{HTTPS} !on
                    RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}
                
                </VirtualHost>
                
                <VirtualHost *:443>
                
                    ServerName tcsql1.cefim-formation.org
                    #ServerAlias www.domain.tld
                
                    DocumentRoot /home/user/www/
                
                    <Directory /home/user/www>
                   	 Options -Indexes
                   	 AllowOverride all
                   	 Order allow,deny
                   	 allow from all
                    </Directory>
                
                    SSLEngine on
                    SSLCertificateFile /etc/letsencrypt/live/tcsql1.cefim-formation.org/cert.pem
                    SSLCertificateKeyFile /etc/letsencrypt/live/tcsql1.cefim-formation.org/privkey.pem
                    SSLCertificateChainFile /etc/letsencrypt/live/tcsql1.cefim-formation.org/chain.pem
                    SSLProtocol all -SSLv2 -SSLv3
                    SSLHonorCipherOrder on
                    SSLCompression off
                    SSLOptions +StrictRequire
                    SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA
                    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
                
                    LogLevel warn
                    ErrorLog ${APACHE_LOG_DIR}/tcsql1.cefim-formation.org-error.log
                    CustomLog ${APACHE_LOG_DIR}/tcsql1.cefim-formation.org-access.log combined
                
                </VirtualHost>


                
   
                    


