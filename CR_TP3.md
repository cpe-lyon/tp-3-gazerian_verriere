GAZERIAN Nicolas
VERRIERE Nicolas
4ETI

**Exercice 1. Commandes de base** 

**Question 1)** 
*Quels sont les 5 derniers paquets installés sur votre machine?*

On peut voir le contenu de l'historique des installations grace aux commandes:

    cd /var/log/apt
    cat history.log | grep " install " | tail -5
    
La réponse renvoyée est la liste des 5 derniers paquets installés avec leur date et heure d'installation ainsi que d'autres paramètres.


**Question 2)** 
*Utiliser **dpkg** et **apt** pour compter le nombre de paquets installés.*
*Comment explique-t-on la (petite) différence de comptage?*

Pour dpkg : 

    sudo dpkg-query -f '${binary:Package}\n' -W | wc -l
    
Résultat :

    543


Pour apt :

    apt list --installed | wc -l    
    
Résultat :

    544 
    
L'écart d'1 est dû à la ligne "En train de lister..." qui s'écrit lors d'une commande apt. (On vérifie cela grâce au paramètre head)
Cette ligne doit être prise en compte alors qu'il ne faudrait pas?



**Question 3)**
*Combien de paquets sont disponibles en téléchargement?*

Grâce à la commande :

    apt cache -stats

Nombre de paquets disponibles : 87939

Nombre de paquets déjà installés : 543

Donc nombre de paquets disponibles au téléchargement : **87896**
    
    
    
**Question 4)**
*Créer un alias “maj” qui met à jour le système*

    alias maj='apt update'
    
    
    
**Question 5)**
*A quoi sert le paquet **fortunes**? Installez-le.*

Le paquet fortune permet de renvoyer des citations ou des proverbes à l'ouverture du terminal ou à l'appel de la commande fortune.
Pour l'installer on utilise la commande :

    sudo apt install fortunes


**Question 6**
*Quels paquets proposent de jouer au sudoku?*

C'est grâce au paquet gnome-sudoku que l'on peut jouer au jeu.


**Question 7**
*Lister les derniers paquets installés explicitement avec la commande* **apt install** 

Code :
    dpkg -L apt install | tail -5




**Exercice 2)** 
*A partir de quel paquet est installée la commande* **ls** *?* Comment obtenir cette information en une seule commande, pour n’importe quel programme ? 
 
 Après avoir trouvé le chemin qui mène de root à ls, on écrit :

    dpkg -S /usr/share/man/man1/ls.1.gz

Résultat :

    coreutils

*Utilisez la réponse à pour écrire un script appelé origine-commande prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.*

Dans nano, nous écrivons :

    #!/bin/bash
    echo $(which -a $1 | tail -1 | xargs dpkg -S)
    
    
Résultat :

    coreutils: /bin/ls




**Exercice 3)**

Script :

        #!/bin/bash
        result=$(dpkg -l | grep " $1 ")
        if [ -z "$results" ]; then
          echo "NON INSTALLE"
        else 
          echo "INSTALLE"
        fi




**Exercice 4)**
*Lister les programmes livrés avec coreutils. A quoi sert la commande ’[’ et comment aﬀicher ce qu’elle retourne?*

Code :

    apt show coreutils


La commande '[' permet de comparer des chaines de caractères, des nombres et vérifie des propriétés de fichiers. Elle simplifie le code pour ne pas avoir a écrire "test".


    
**Exercice 5) aptitude**
*Installez le paquet* **emacs** *à l’aide de la version graphique d’aptitude.*

Commandes :

    sudo apt install aptitude
    sudo apt update aptitude
    aptitude
    
Après avoir cherché emacs dans la liste graphique des paquets, on le télécharge avec '+'.




**Exercice 6)  Installation d’un paquet par PPA**



**Exercice 7) Création de dépôt personnalisé**
**Création d’un paquet Debian avec dpkg-deb**

*1) Dans le dossier scripts créé lors du TP 2, créez un sous-dossier origine-commande où vous créerez un sous-dossier DEBIAN, ainsi que l’arborescence usr/local/bin où vous placerez le script écrit à l’exercice 2. *

L'arborescence est créée grâce à la suite de commandes suivante : 

        cd scripts
        mkdir origine-commande
        cd origine-command
        mkdir DEBIAN
        mkdir usr
        cd usr
        mkdir local
        cd local 
        mkdir bin
        cd ../..
        cd DEBIAN

*2) Dans le dossierDEBIAN, créez un fichiercontrolavec les champs suivants :*

La commande vim nous permet de créer un fichier et de le paramétrer, on écrit donc :

        vim control
        
        Package: origine-commande 
        Version: 0.1 
        Maintainer: Gazerian_Verriere
        Architecture: all 
        Description: Cherche l'origine d'une commande
        Section: utils 
        Priority: optional 
        
        
*3.Revenez dans le dossier parent de origine-commande (normalement, c’est votre $HOME) et tapez la commande suivante pour construire le paquet : dpkg-deb --build origine-commande .


Pour retourner dans HOME, on utilise la commande :

        cd ~
        
Puis on crée le paquet avant la commande de l'énoncé. Nous ne savons pas vraiment comment transcrire dans ce compte-rendu ce qui a été fait.



**Création du dépôt personnel avec reprepro**
*1) Dans votre dossier personnel, commencez par créer un dossier repo-cpe. Ce sera la racine de votre dépôt*
*2) Ajoutez-y deux sous-dossiers : conf (qui contiendra la configuration du dépôt) et packages (qui contiendra nos paquets)*

La création d'un dossier et de ces sous-dossiers se fait, comme toujours, grâce àaux commandes mkdir et cd, comme suit :

        mkdir repo-cpe
        cd repo-cpe
        mkdir conf
        mkdir packages
        

*3) Dans conf, créez le fichier distributions.*

Pour suivre les consignes de l'énoncé, nous écrivons :

        cd conf
        vim distributions 
        
        Origin: Gazerian_Verriere_Depot
        Label: repo-cpe
        // Suite: stable
        Codename: cosmic  
        Architectures: i386 amd64 
        Components: universe 
        Description: Test_Depot_TP3_AdminSys


*4) Dans le dossier repo-cpe, générez l’arborescence du dépôt.*

Pour cela, on utilise la commande :

        reprepro -b . export5
        

*5) Copiez le paquet origine-commande.deb créé précédemment dans le dossier packages du dépôt, puis,à la racine du dépôt, inscrivez votre paquet dans le dépôt.*

On effectue d'abord ces commandes :

        cp ~/scripts/origine-commande.deb ./packages
        cp ~/scripts/origine-commande.deb
        
Puis :

        reprepro -b . includedeb cosmic origine-commande.deb 

Afin que le paquet soit placé dans notre dépot.



*6) Il faut à présent indiquer à apt qu’il existe un nouveau dépôt dans lequel il peut trouver des logiciels. Pour cela, créez (avec sudo) dans le dossier /etc/apt/sources.list.d le fichier repo-cpe.list contenant :
deb file:/home/VOTRE_NOM/repo-cpe cosmic multiverse (cette ligne reprend la configuration du dépôt, elle est à adapter au besoin)*

On ouvre ce fichier dans vim pour pouvoir le configurer correctement :

        sudo vim /etc/apt/sources.list.d/repo-cpe.list
        deb file: /home/Gazerian_Verriere/repo-cpe cosmic multiverse

*7) Lancez la commande sudo apt update.*

Cette commande permet de prendre en compte les paquets signés. Le nôtre est configuré presque entièrement, mais nous devons le signer. C'est l'objet de la prochaine partie de l'exercice.



**Signature du dépôt avec GPG**
*GPG est la version GNU du protocole PGP (Pretty Good Privacy), qui permet d’échanger des données demanière sécurisée. Ce système repose sur la notion de clés de chiffrement asymétriques (une clép ublique et une cléprivée)*

*1) Commencez par créer une nouvelle paire de clés*

Pour cela on utilise la commande:

        gpg --gen-key
        Real name: Gazerian_Verriere
        Email address: nicolas.verriere@cpe.fr
        o
        passphrase : EZ
        
*2) Ajoutez à la configuration du dépôt (fichier distributions) la ligne suivante :*

        SignWith: yes
        
*3) Ajoutez la clé à votre dépôt :*

        reprepro --ask-passphrase -b . export

Nous comprenons cette commaned comme la nécéssité de demander la passphrase du dépot aux utilisateurs.

*4) Ajoutez votre clé publique à votre dépôt avec la commande :*

        gpg --export -a nicolas.verriere@cpe.fr > public.key
        
*5) Enfin, ajoutez cette clé à la liste des clés fiables connues de apt:* 

        sudo apt-key add public.key

Ces deux dernières commandes permettent d'identifier la clé du dépôt comme une clé valide. (On ajoute la nouvelle à la banque des clés connues par apt dans la dernière commande.)



**Exercice 8) Installation d’un logiciel à partir du code source**
*Lorsqu’un logiciel n’est disponible ni dans les dépôts officiels, ni dans un PPA, ou encore parce qu’on souhaite n’installer qu’une partie de ses fonctionnalités, on peut se tourner vers la compilation du code source. Malheureusement, cette installation ”à la main” fait qu’on ne propose pas des bénéfices de la gestion de paquets apportée par dpkg ou apt. Heureusement, il est possible de transformer un logiciel installé ”à la main” en un paquet, et de le gérer ensuite avec apt ; c’est ce que permet par exemple checkinstall.*


*1) Commencez par cloner le dépôt git suivant :*

        git clone https://github.com/jubalh/nudoku
        
Ceci permet de récupérer en local le code source du logiciel nudoku.
On crée le clône du contenu de l'adresse http pour avoir une copie en dur sur l'ordinateur plutôt que sur le réseau internet.



Comme je vous l'ai expliqué dans un mail le vendredi 6, nous avons eu des problèmes d'interface pour terminer le TP. (Nous avons lié 3 captures d'écran à ce .md. Ce sont les mêmes que vous avez reçues par mail.) 

Nous n'avons pas pu avancer plus dans le TP.
