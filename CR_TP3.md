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
    
L'écart d'1 est du à la ligne "En train de lister..." qui s'écrit lors d'une commande apt. (On vérifie cela grâce au paramètre head)
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
On affiche ce qu'elle retourne 


    
**Exercice 5) aptitude**
*Installez le paquet* **emacs** *à l’aide de la version graphique d’aptitude.*

Commandes :

    sudo apt install aptitude
    sudo apt update aptitude
    aptitude
    
Après avoir cherché emacs dans la liste graphique des paquets, on le télécharge avec '+'.




**Exercice 6)  Installation d’un paquet par PPA**






