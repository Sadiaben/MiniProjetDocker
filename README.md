

 **MINI PROJET DOCKER BOOTCAMP 18**


  ![Mini project](https://github.com/Sadiaben/project2/blob/main/php-dev.png "Mini project")

Projet student-list

Veuillez trouver la consigne en cliquant [mini-projet-docker-18](https://github.com/diranetafen/student-list)

Prénom : Sadia

Nom de famille : Ben touirad

Formation : 18ème Bootcamp DevOps d'Eazytraining

Période : mars-avril-mai

date : 14 mars 2024

**Objectives**

L'objectif était de :

1\. Créer un conteneur pour chaque module.

2\. Établir des interactions entre ces conteneurs.

3\. Mettre en place un registre privé pour stocker ces conteneurs

**BUILD AND TEST**

  – Cloner le projet dans un répertoire /miniprojet-docker
  
      1 git clone https://github.com/diranetafen/student-list.git
  ![](https://github.com/Sadiaben/project2/blob/main/1.png "")
  
  
  1\. Changez de répertoire et créez l'image du conteneur API
  
  Voici le contenu de Dockerfile
  ![](https://github.com/Sadiaben/project2/blob/main/2.png "")
  
  2\. build l’image
  
      cd /miniprojet-docker/student-list/simple\_api
      docker build -t simple-api .
  
  ![](https://github.com/Sadiaben/project2/blob/main/3.png "")
  
  
  3\. Créez un réseau de type bridge pour que les deux conteneurs puissent se contacter par leurs noms grâce aux fonctions DNS
  
      1 docker network create simple-api.network --driver=bridge
      
     ![](https://github.com/Sadiaben/project2/blob/main/4.png "").
  
  4\. Exécutez le conteneur de l'API backend avec ces arguments
  
      1 docker run --rm -d --name=simple-apisim --network=student\_list.network -v ./simple\_api/:/data/ api.student\_list.img
      
    ![](https://github.com/Sadiaben/project2/blob/main/5.png "").
  
  Mettez à jour le fichier index.php :
  ![](https://github.com/Sadiaben/project2/blob/main/6.png "")
  
  5\. Lancez le conteneur de l'application web :
  
      1 docker run --rm -d --name=webapp\_student -p 80:80 --network=simple-api.network -v ./website/:/var/www/html -e USERNAME=toto -e PASSWORD=python php
  ![](https://github.com/Sadiaben/project2/blob/main/14.png "")
          
  
  6\. Testez l'API via le frontend
  
  6\.1 via la ligne de commande :
  
      1 docker exec webapp\_student curl -u toto:python -X GET http://simple-api\_student:5000/pozos/api/v1.0/get\_student\_age
  ![](https://github.com/Sadiaben/project2/blob/main/7.png "")
  
  6\.2 via le navigateur web
  ![](https://github.com/Sadiaben/project2/blob/main/8.png "")


  
**INFRASTRUCTURE AS CODE**

1\. Supprimer tous les conteneurs que nous avons créés

![](https://github.com/Sadiaben/project2/blob/main/9.png "")


2\. construire le docker-compose puis exécuter la commande suivante

    1 docker-compose up -d
  ![](https://github.com/Sadiaben/project2/blob/main/10.png "")
    
  ![](https://github.com/Sadiaben/project2/blob/main/11.png "")

**REGISTER DOCKER**

- Pour le registre, j'ai opté pour l'image registry:2, tandis que pour son interface utilisateur frontend, j'ai choisi joxit/docker-registry-ui:static, en passant également plusieurs variables

d'environnement

    1 NGINX\_PROXY\_PASS\_URL=http://pozos-registry:5000
    2 DELETE\_IMAGES=true
    3  REGISTRY\_TITLE=Pozos


 lancer le fichier docker-compose.registry.yml

    1 docker-compose -f docker-compose.registry.yml up -d
    
![](https://github.com/Sadiaben/project2/blob/main/12.png "")

![](https://github.com/Sadiaben/project2/blob/main/13.png "")

