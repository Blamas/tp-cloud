# TP 1 - Introduction au Cloud Computing [^1]
Au cours de ce TP, vous manipulerez des VMs en utilisant l'interface Horizon qui permet d'administrer la plateforme Cloud OpenStack. Vous allez utiliser pour le TP une image `Docker Ready` sur laquelle vous allez vous connecter en utilisant SSH. 

## Instancier une VM 

### Étape préliminaire  : 
Démarrez votre poste sous GNU/Linux (Ubuntu). Connectez vous ensuite à la plateforme d’administration Horizon de Openstack via votre navigateur en saisissant dans la barre de navigation l’URL suivante : [https://cloud-info.univ-lyon1.fr/horizon](https://cloud-info.univ-lyon1.fr/horizon)

Pour se connecter à votre futur instance de machine virtuelle vous aurez besoin d’un jeu de clé SSH (clés publique/privé). Pour cela: 

* Accédez à la section  `Key Pairs` et créer une paire de clés. Donnez lui un nom et sélectionnez le type SSH Key.

:warning: **Sauvegardez minutieusement** le fichier avec l’extension `.pem` généré. Il contient entre autre votre clé privé.


### Creation de l'instance :

Comme mentionner au début, nous allons utiliser une image Docker qui sera relativement petite. Pour cela, allez sur l’onglet `instances` puis `Launch Instance`. Ensuite : 
* Donnez un nom à votre instance.
* Nous allons pas créer de volume, cochez `No` et séléctionnez une image `Ubuntu 20.04.3 LTS - Docker Ready`
* Choisissez `m1.small` comme type `Flavor` de VM.
* Dans la section `Key Pair` séléctionnez la clé que vous avez crée.
* Maintenant, vous pouvez lancer  votre VM.

**Bravo!** Vous venez de créer votre VM :clap: !

* Quelle est votre adresse IP ?
* Quel status a votre VM ? Comment l'éteindre ?

Parcourez les differents onglets de l'interface Horizon.
* Quelles ressources avez vous attribué à votre VM (VCPUs, Disk, RAM)?
* Quelle est votre adresse MAC ?

Connectez-vous via SSH en spécifiant le nom de votre clé à l’aide de l’option `–i` de SSH. 

```
$ ssh –i fichier.pem ubuntu@192.168.X.X 
```
* Est-ce que vous arrivez à vous connecter? Comment remédier à cela ?

Vérifiez en « pingant »  votre VM ou celle d’un(e) de vos camarades (vous aurez besoin de son adresse IP (cf. interface Horizon). Cela devrait fonctionner de  votre VM mais aussi de votre poste à  la maison si celui-ci est bien connecté via le VPN.
* Pourquoi le ping fonctionne ?

**Faites vérifier vos réponses par l'encadrante :mag:**

##  Installation de logiciels : 
Usage de la VM : Nous allons à présent utiliser la VM. Vous remarquerez par vous même qu’il n’y a pas différences avec l’administration d’une machine classique. 
```
$ sudo apt update
$ sudo apt install apache2 libapache2-mod-php
```
* À quoi sert la commande `apt update`
* Comment trouver le nom des paquets à installer ?

Une fois installé, ouvrez un nouvel onglet de votre navigateur et connectez vous à votre serveur web Apache fraichement installé. 
```
 http://adresseIPdeVotreVM/
```
* Est-ce que ça marche ? 
* D'où vient le problème ?

**Faites vérifier vos réponses par l'encadrante :mag:**

## SSH 
Sur votre VM, observez le fichier `authorized_keys` avec la commande suivante : 
```
cat ~/.ssh/authorized_keys
```
* Que contient ce fichier ? 
* Ou peut-on trouver cette information sur l'interface Horizon ?

Essayer maintenant de vous connecter à la VM de votre voisin en utilisant son IP. 
* Comment peut-on acceder aux autres VMs ?

Maintenant nous allons instancier une VM en utilisant une clé générer avec `ssh` sur votre machine locale. **Supprimez** votre premiere VM sur l'intrface Horizon ainsi que la clé SSH crée.


Générez un jeu de clé en utilisant SSH (pas besoin de passphrase). Rendez vous ensuite dans la section **Paires de clés** de l'interface Horizon et **Importez une clé publique**.   Copiez/colez la clé publique  et nommer la. 
* Quelle commande vous permet de generer des clés SSH ?
* Ou se trouve votre **clé publique** ?

Creez une nouvelle instance en utilisant la nouvelle clé et connectez-vous à la VM.

Observez le dossier: 
```
ls ~/.ssh
```
* Que contient le fichier `known_hosts` ?
* Pourquoi la VM ne contient pas le fichier `known_hosts` ?
* Que permet de faire la commande `ssh-copy-id`

Si vous avez répondu aux questions, vous auriez une idée de comment se connecter à la VM de votre voisin, sois de votre machine locale ou de la VM.
* Connectez-vous à la machine de votre voisin

**Faites vérifier vos réponses par l'encadrante :mag:**

Jusqu'ici nous avons utilisé la clé privé afin de nous connecter à la VM. Or, si vous perdez votre clé vous n'aurez plus accée à votre VM. Pour cela nous allons utiliser un Mot de passe pour acceder à la VM.
* Que retourne la commande suivante : 
``` 
sudo cat /etc/shadow | grep ubuntu
```
Modifier le mot de passe avec la commande `sudo passwd ubuntu`.
Observez le contenu du dossier `/etc/ssh`

* Quelle est la difference entre `sshd_config` et `ssh_config` ?

Dans `/etc/ssh/sshd_config` remplacez :
```
PermitRootLogin without-password
PasswordAuthentication no
```
par :
```
PermitRootLogin yes
PasswordAuthentication yes
```
Enregistrez et testez.

Ça ne marche pas! vérifier le status de SSH sur votre VM pour savoir pourquoi.
```
systemctl status ssh
```
* Quelle commande permet de résoudre le problème ?

**Faites vérifier vos réponses par l'encadrante :mag:**

##  Installation de logiciels (Suite) :
Nous allons retenter notre expérience avec `Apache`. Installer sur votre nouvelle VM le logiciel avec la commande précédente. 

* Pourquoi ça marche cette fois-ci ? 
* Ou peut-on vérifier cette information (:bulb: à l'aide l'interface Horizon) ?
* Modifiez la page d'Apache qui s'affiche et vérifier que vos modifications ont bien été prises en compte (:bulb: Pour savoir quel fichier modifier lisez la page d'accueil par défaut d'Apache).

### Petite partie Snapshot : 
* Retour sur l'interface Horizon, réalisez un snapshot de votre instance.
* Créez une seconde VM à partir du snapshot réalisé.
* Connectez vous sur le serveur Apache sur votre seconde VM.
```
 http://New_IP_VM/
```

**Bravo!** Vous avez fini le TP :clap: !

**:warning: Supprimez** la seconde VM que vous avez crée.

**Faites vérifier vos réponses par l'encadrante :mag:**

[^1]: This lab is in tribute to Fabien Rico.