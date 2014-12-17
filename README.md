Boot2GeoportalCluster
=====================

# Présentation
Boot2GeoportalCluster est un automatiseur de création de clusters GeoServer + PostgreSQL (avec PostGIS). L'utilisateur peut accéder à l'[interface web de GeoServer](http://127.0.0.1:8080/geoserver) à travers le serveur web et répartiteur de charges [Dyn-nginx](https://registry.hub.docker.com/u/dduportal/dyn-nginx/).

![Architecture logicielle](https://github.com/cwamgis/Boot2GeoportalCluster/blob/master/images/architecture_logiciel.png)

# Mode d'emploi

1. Ouvrir une invite de commande ;
2. Lancer la commande ```git clone https://github.com/cwamgis/Boot2GeoportalCluster.git``` ;
3. Se placer à la racine du dossier ;
4. Si votre connexion passe par un proxy, éditer proxy/proxy.bat pour correspondre à sa configuration. Puis, le lancer via la commande ```proxy/proxy.bat``` ;
5. Lancer ```vm/run_vm.bat``` : cette commande va configurer et lancer la machine virtuelle sur laquelle nous allons travailler, contenue dans le dossier *vm* ;
6. Lancer ```sh /vagrant/sh_boot2geoportal/run.sh```, préciser le nombre d'instances de GeoServer souhaitées et confirmer. **Attention à ne pas dépasser les capacités de votre machine en lançant trop de GeoServers à la fois, sous peine de crash.** Une fois cette commande exécutée, tous les GeoServers seront lancés et prêts à l'emploi.

# Détails du fonctionnement de run.sh

1. Tue tous les conteneurs existants et les supprime ;
2. Construit les images *geoserver*, *datadir*, *dbserver* si elles n'existent pas déjà ;
  * Sans surprise, geoserver est l'image d'un GeoServer ;
  * datadir est un répertoire partagé par toutes les instances de GeoServer qui contient les données qui seront rentrées dans les GeoServers & les données d'initialisation ;
  * dbserver est l'image d'un serveur Postgre.
3. Lance le fichier *fig.yml*, qui va à son tour créer les conteneurs et définir les liens & interactions entre eux, les ports par lesquels ils communiquent, leurs variables d'environnement, etc. En particulier, il lance NGINX, qui va non seulement gérer la répartition des charges, mais aussi permettre à un utilisateur enregistré de garder la même identité (ie la même session ouverte) quelle que soit l'instance du GeoServer sur laquelle il a été redirigé ;
4. Lance le nombre de GeoServers spécifiés par l'utilisateur ;
5. Affiche les conteneurs.
