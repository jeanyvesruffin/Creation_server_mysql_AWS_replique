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

                $nano /root/.bashrc
                
  
![Param bashrc](https://user-images.githubusercontent.com/37696862/60921873-cc233200-a29b-11e9-9c33-08909a36d624.png)


                
   
                    


