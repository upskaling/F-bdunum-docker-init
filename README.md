https://pad.lamyne.org/F@bdunum-docker-init

# Docker01 - Fab du Num - 2021

[TOC]

## Introduction


Vous avez peut être déjà expérimenté ce léger désagrément quand :

> • Vous développez sur une machine et il vous manque les droits root pour installer une dépendance

> • Vous avez passé 5h à installer la dernière version de votre BDD préférée mais que votre chef vous dit que l’appli devra être opérationnelle sur une version précédente

> • Vous avez passé deux semaines à faire une application propre... mais qui ne marche que sur votre machine

Le [SysOp](https://lydra.fr/es-11-quest-ce-quun-ingenieur-devops-un-sysops-ou-admin/) en charge du déploiement vous haïs car votre application est indéployable.

Bref, vous passez ou passerez à un moment de votre vie, au moins autant de temps sur l’aménagement de votre environnement de développement que sur le développement de votre application.

De même, en ce moment, vous développez sûrement vos application en un seul bloc, avec des logiciels et des librairies installées en dur dans votre environnement de développement, ou peut être dans un environnement virtuel... Imaginez que votre application soit déployée à travers le monde et que vous deviez la redévelopper pour n’importe quelle plateforme et OS...

C’est un peu dans cette volonté d’unification et de normalisation que Docker a été créé, en laissant aux développeurs la possibilité de séparer leur application en microservices, adaptifs, légers, universels et scalables, mais aussi aux administrateurs système une grande flexibilité de déploiement et de scalabilité.

Plusieurs projets utilisant Docker vous aideront à mieux comprendre l’outil et à mieux cerner les différentes facettes du développement d’application en microservices.

## Prologue

:::info
Docker01 a pour but de faire manipuler ```docker``` et ```docker-machine```, la base pour comprendre le principe de la conteneurisation de services. Voyez ce
projet comme une initiation. Mais avant de se lancer dans une telle aventure, il est important de vous mettre en garde sur certains points :
:::

• Les exercices se feront sur docker **20.10** minimum. Si vous n’êtes pas sûr de votre version de docker, faites un docker version.

• Se référer à la doc de Docker disponible sur les internets pour l’installer si besoin. [docs.docker.com](https://docs.docker.com/docker-for-windows/install/)

• Prenez le temps de lire la documentation. De toute façon, vous n’aurez pas le choix...

## Recommandations

En parallèle de ce sujet, nous vous recommandons VIVEMENT d’avoir la documentation officielle de Docker à portée de main pour manipuler correctement vos futurs containers. Nous vous demandons aussi de regarder la documentation des containers officiels de chaque service que nous vous demanderons d’implémenter tout au long de ce projet.

Vous allez très vite vous rendre compte que vous n’avez pas besoin de réinventer la roue, chaque fois que vous voulez développer une application ou mettre en place un service.

Voila donc quelques liens très utiles pour votre sujet :

• [L’excellente documentation officielle de Docker](https://docs.docker.com/)

• [Le Registry public de Docker](https://hub.docker.com/)

• [Le Github officiel de Docker qui contient moults projets en devenir](https://github.com/docker)

• [Un très bon conférencier français qui propose beaucoup de ressources en open-source](https://container.training/intro-selfpaced.yml.html#1)

## Consignes

:::danger
Votre dossier de rendu sera composé de 2 dossiers 
    ◦ 00_how_to_docker
    ◦ 01_dockerfiles
:::

pour créer vos dossiers : 

```shell
mkdir 00_how_to_docker 01_dockerfiles
```

Cette première partie est consacrée à la découverte de Docker et de ses options. 

Dans votre dossier de rendu, créez un dossier 00_how_to_docker dans lequel vous allez déposer la solution de chaque exercice.

Vous allez rendre dans le dossier 00_how_to_docker, là ou les commandes correspondantes aux exercices demandés en les nommant par le numéro d’exercice.

![](https://s3.standard.indie.host/pad-lamyne-org/uploads/upload_2fe604428cd50ef7c19d0fa2faa632e8.png)

* par exemple pour le 1er excercice : 
```shell
$ cd 00_how_to_docker
$ echo "docker-machine create -d virtualbox Tatooine" > 01 
```

Si ce n’est pas déjà fait, installez docker, docker-machine et virtualbox.

### Installation de docker-machine

```shell
$ base=https://github.com/docker/machine/releases/download/v0.16.0
$ curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine
$ sudo install /tmp/docker-machine /usr/local/bin/docker-machine

```

:::info
 Pour véfifier que l'installation s'est bien déroulée, on peut utiliser la commande ```docker-machine version```  et ```docker-machine ls``` pour lister les machines docker crées sur notre PC (normalement 0)
 :::
### Installation de virtualbox

```shell
sudo apt install virtualbox
```


## Excercices

:::info
Pour chacun de ces exercices, nous vous demandons de donner la ou les commande(s) shell pour :
:::

1. Créer une machine virtuelle avec ```docker-machine``` utilisant le driver virtualbox et ayant pour nom **Tatooine**
```shell
$ docker-machine create -d virtualbox Tatooine

# crée une Machine Docker avec pour driver virtualbox et pour nom Tatooine
```
OU
```shell
$ docker-machine create --driver virtualbox Tatooine

# l'option -d peut être écrite en version explicite --driver
```

2. Récupérer l’adresse IP de la machine virtuelle **Tatooine**
```shell
docker-machine ip Tatooine
```

3. Assigner les variables spécifiques à la machine virtuelle **Tatooine** dans l’env courant de votre terminal, de sorte que vous puissiez lancer la commande ```docker ps``` sans erreurs. 
Une seule commande est attendue pour fixer les 4 variables d’environnement
```shell
eval $(docker-machine env Tatooine)

# la commande docker-machine env Tatooine affiche les variables d'environnement qui concernent docker
# eval $() permet 
```
4. Récupérer le container ```hello-world``` disponible sur le Docker Hub.
```shell
docker pull hello-world

# pull permet de récupérer l'image hello-world sur le docker Hub
```
5. Lancer le container ```hello-world``` et faire en sorte que le container affiche bien son message d’accueil, puis le quitte.
```shell
docker run hello-world

# docker run ... permet de lancer l'éxécution de l'image
```
6. Lancer un container ```nginx``` disponible sur le Docker Hub en tâche de fond. Le container lancé doit avoir pour nom **padawan**, doit pouvoir redémarrer de lui- même et doit avoir le port 80 du container rattaché au port 5000 de Tatooine. Vous pouvez vérifier le fonctionnement de votre container en allant sur [http://<ip-de-tatooine>:5000](http://<ip-de-tatooine>:5000) comme URL sur votre navigateur internet.
```shell
docker run --detach --name=padawan --restart=always -p 5000:80 nginx
```
    
7. Récupérer l’adresse IP interne du container **padawan** sans lancer son shell et en une commande.
```shell
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' padawan
```
8. Lancer un shell depuis un container ```alpine```, en faisant en sorte que vous puissiez directement interagir avec le container via votre terminal et que le container se supprime à la fin de l’exécution du shell.
```shell
docker run -it --rm alpine 
```
    
9. Depuis le shell d’un container ```debian```, faire en sorte d’installer via le gestionnaire de paquets du container, de quoi compiler un code source en C et le pusher sur un repo git (en veillant avant de bien mettre à jour le gestionnaire de paquets et les paquets présents de base dans le container). Seules les commandes à effectuer dans le container sont demandées pour cet exercice.
10. Créer un volume ```starkiller```
11. Lister les volumes Docker crées sur la machine... je dis bien **VOLUMES**
12. Lancer un container ```mysql``` en tâche de fond. Il devra aussi pouvoir redémarrer de lui- même en cas d’erreur et faire en sorte que le mot de passe root de la base de données soit *Obiwan*. Vous ferez aussi en sorte que la base de données soit stockée dans le volume ```starkiller```, que le container crée directement une base de données qui aura comme nom ```droïde``` et le container s’appellera ```usine-droid```.

13. Afficher les variables d’environnement du container ```usine-droid``` en une seule commande, histoire d’être sûr que vous avez bien configuré votre container.

14. Lancer un container wordpress en tâche de fond. Le container doit avoir pour nom ```naboo```, le port 80 du container doit être bindé au port 8080 de la machine virtuelle et doit pouvoir utiliser le container ```usine-droid``` comme service de base de données. Vous pouvez tenter d’accéder à ```naboo``` sur votre machine via un navigateur en rentrant l’adresse IP de la machine virtuelle comme URL.

:::success
Bravo, vous venez de déployer un site Wordpress fonctionnel en 2 commandes !
:::

15. Lancer un container ```phpmyadmin``` en tâche de fond. Le container doit avoir pour nom ```Kylo-Ren```, le port 80 du container doit être bindé au port 8081 de la machine virtuelle et doit pouvoir faire en sorte d’aller explorer la base de données contenue dans le container ```usine-droid```.
16. Consulter les logs en temps réel du container ```usine-droid``` sans exécuter son shell pour autant.
17. Afficher l’ensemble des containers actuellement actifs sur la machine virtuelle **```Tatooine```**.
18. Relancer le container **```padawan```**
19. Créer un swarm local où la machine virtuelle **```Tatooine```** en est le manager.
20. Créer une autre machine virtuelle avec docker-machine utilisant le driver virtualbox et ayant pour nom **```Alderaan```**
21. Basculer **```Alderaan```** comme esclave du swarm local où **```Tatooine```** est leader (La commande de prise de contrôle de **```Alderaan```** n’est pas demandée).
22. Créer un réseau interne de type overlay que vous nommerez ```alliance-rebelle```.
23. Lancer un SERVICE ```rabbitmq``` qui aura pour nom ```x-wing```. Vous devrez définir un user et un mot de passe spécifiques à l’utilisation du service RabbitMQ, et ceux-ci seront à votre libre convenance. Ce service sera sur le réseau ```alliance-rebelle```.
24. Lister l’ensemble des **services** du swarm local.
25. Forcer l’arrêt et supprimer l’ensemble des services sur le swarm local, **en une seule commande**.
26. Forcer l’arrêt et supprimer l’ensemble des containers (tous états confondus), **en une seule commande**.
27. Supprimer l’ensemble des images de containers stocké sur la machine virtuelle **```Tatooine```**, en une seule commande aussi.
28. Supprimer la machine virtuelle **```Alderaan```** autrement qu’avec un rm -rf
    
## Dockerfiles

:::warning
Alors, c’était bien hein ?
:::
    
Maintenant, il est temps de passer la *seconde*. 
    
Docker vous permet aussi de créer vos PROPRES containers pour vos PROPRES applications ! Et c’est aussi l’objet de ce chapitre. Encore heureux, Docker pense à tout en vous laissant programmer des Dockerfiles. Les Dockerfiles sont des fichiers utilisant une syntaxe spécifique qui réutilise une image de base ou un container existant pour y ajouter vos propres dépendances et vos propres fichiers.
    
Vous allez constater aussi que chaque commande construite par vos Dockerfiles génère un layer, réutilisable dans d’autres Dockerfiles ou d’autres versions du même Dockerfile, pour éviter la redondance de données. 
:::success
Génial non ?
:::    

    
Mais avant de commencer à faire vos propres containers, assurez-vous que votre machine virtuelle soit bien vide de toute image et container actif résiduel. On va partir de la base et faire des applications de plus en plus complexes.
   
:::danger
Créez un dossier 01_dockerfiles dans lequel vous rangerez tous les Dockerfiles demandés ensuite, dans des dossiers séparés (ex00 / ex01 ...).
:::    

Un lien pour bien vous aider à concevoir des Dockerfiles de qualité : [Dockerfile Best Practices](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/)
    
### Exercice 01 : vim/emacs
:::info
• Dossier de rendu : 01_dockerfiles/ex01/
• Fichiers à rendre : Dockerfile
:::

Depuis une image ```alpine```, vous ajouterez à votre container l’éditeur de texte de votre choix entre ```vim ou emacs```, qui se lancera au démarrage de votre container.

    
### Exercice 02 : What does the Fox say ?
:::info
• Dossier de rendu : 01_dockerfiles/ex02/ 
• Fichiers à rendre : Dockerfile
:::
    

Docker peut être pratique pour pouvoir tester une application encore en développement sans créer de la pollution dans vos fichiers. 

Vous allez par ailleurs, devoir concevoir un Dockerfile qui, au build, récupère la version actuelle de [Gitlab - Community Edition](https://about.gitlab.com/install/?version=ce) l’installe avec toutes les dépendances et les configurations nécessaires et lance l’application (HTTPS et SSH). 
    
Le container est jugé valide, si vous pouvez accéder au client web, et si vous êtes capables d’utiliser correctement avec Gitlab et d’interagir via GIT avec ce container. Bien sur, vous ne devrez pas utiliser le container officiel de Gitlab, *ce serait un peu tricher...*
    
## Bonus

:::success
Maintenant que vous avez saisi toute la subtilité de Docker, vous pouvez allègrement vous amuser !
:::
    
À vous de créer vos futurs environnements de travail, tels que :
 * Un environnement de dev pour faire du nodejs, mais en utilisant yarn plutôt que npm
* [PAMP](https://github.com/malkev/pamp) (!) avec docker-compose, mais en utilisant MariaDB plutôt que MySQL
* Recréer une [MEAN](https://code.tutsplus.com/fr/tutorials/introduction-to-the-mean-stack--cms-19918) Stack complète [(la doc ici)](https://github.com/linnovate/mean) 
* Des Dockerfiles pour vos projets de C, Ruby, Go, Ocaml, Rust...
* etc.
    
:::warning
Pensez aussi que vous n’avez encore rien vu de la puissance de Docker. 
:::
    
Vous pouvez :
* Tenter de containeriser une configuration VPN pour vos containers
* Essayer de clusteriser plusieurs machines sur différents services cloud avec Docker Swarm
* Vous lancer dans la construction d’une image de base, comme faire votre propre container debian ou archlinux
* Containeriser le serveur de votre jeu préféré, comme Minecraft ( ?) etc.
Have Fun ! !
###### tags: `Fabdunum` `AdminSys` `docker`  `2021`
