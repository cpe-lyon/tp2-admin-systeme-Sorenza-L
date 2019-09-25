***``Sorenza Laplume``***

# Compte rendu TP2

## Exercice 1. Variables d’environnement

**1. Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur ?**

Les commandes tapées par l'utilisateur se trouve dans le dossier contenu dans **PATH**.

**2. Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans votre répertoire personnel ?**

La variable d'environnement qui permet à la commande cd tapée sans argument de ramener dans le répertoire personnel est ***PATH***.

**3. Explicitez le rôle des variables LANG, PWD, OLDPWD, SHELL et _.**

La variable ***LANG*** détermine la langue que les logiciels utilisent pour communiquer avec l'utilisateur.

La variable ***PWD*** détermine le répertoire courant de l'interpréteur de commande.

La variable ***OLDPWD*** indique le dernier changement effectué via la commande ***cd*** dernièrement.

La variable ***SHELL*** détermine l'interpréteur de commande préféré de l'utilisateur tel qu'il est défini dans le fichier "/etc/passwd".

**4. Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe.**

Pour créer la variable locale j'utilise les commandes suivantes :
> MY_VAR="LAPLUME"; printenv MY_VAR

La commande printenv MY_VAR m'affiche LAPLUME. Lorsque je teste la commande :
> echo MY_VAR

je vois apparaitre LAPLUME.

**5. Tapez ensuite la commande bash. Que fait-elle ? La variable MY_VAR existe-t-elle ? Expliquez. A la fin de cette question, tapez la commande exit pour revenir dans votre session initiale.**

La variable **MY_VAR** n'existe plus après avoir tapé la commande bash. La commande bash sort de la session initiale et ouvre un autre shell. La variable MY_VAR est une variable locale donc elle n'est pas visible sur tout les shell.


**6. Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez.**

Pour transformer la variable MY_VAR en variable d'environnement j'utilise les commandes suivantes :
>export MY_VAR="LAPLUME"; printenv MY_VAR

La variable **MY_VAR** est présente car c'est une variable d'environnement ainsi elle est présente sur tout les shell.

**7. Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace. Afficher la valeur de NOMS pour vérifier que l’affectation est correcte.**

> export- NOMS="LAPLUME LARRAUFIE"; printenv $NOMS
>echo $NOMS

Nous pouvons voir afficher LAPLUME LARRAUFIE.

**8. Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2 !” (où binôme1 et binôme2 sont vos deux noms) en utilisant la variable NOMS.**

>echo "Bonjour à vous deux, $NOMS !"
Cette commande nous affiche **Bonjour à vous deux, LAPLUME LARRAUFIE !**

**9. Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande unset ?**

La commande **unset** efface de la mémoire les variables passées en paramètre.

**10. Utilisez la commande echo pour écrire exactement la phrase : $HOME = chemin (où chemin est votre dossier personnel d’après bash)**

La commande est :
>echo '$HOME'=$HOME
Ce qui m'affiche **$HOME = /home/slapl**


**`Programmation Bash`**

## Exercice 2. Contrôle de mot de passe

**Écrivez un script testpwd.sh qui demande de saisir un mot de passe et vérifie s’il correspond ou non au contenu d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par l’utilisateur ne doit pas s’afficher.**

<pre>
#!/bin/bash

#Variables
PASSWORD="qwerty"

#Programme principal
echo "Saisir un mot de passe : "
read -s mdp
if [$PASSWORD = $mdp]; then
   echo "Le mot de passe est correct"
else 
   echo "Le mot de passe est incorrect"
fi

#fin du script

</pre>

## Exercice 3. Expressions rationnelles

**Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètre est un nombre réel :**
**function is_number()**
**{**
**re='^[+-]?[0-9]+([.][0-9]+)?$**'
**if ! [[ $1 =~ $re ]] ; then**
**return 1**
**else**
**return 0**
**fi**
**}**

<pre>
#!/bin/bash
#Déclaration de ma fonction

function is_number()
{
    re='^[+-]?[0-9]+([.][0-9]+)?$'
    if ! [[ $1 =~ $re ]] ; then
        return 1
    else
        return 0
fi
}

#Appel de ma fonction
is_number $1
if [ $? = 0]; then
       echo "$1 est un nombre réel"
else 
       echo "$1 n'est pas un nombre réel"
fi

#Fin du script
</pre>

***Si je lance mon script avec is_number 2 cela m'affiche 2 est un nombre réel. Quand je remplace le paramètre par p je peut lire, p n'est pas un nombre réel.***

## Exercice 4. Contrôle d’utilisateur

**Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné en paramètre du script. Si le script est appelé sans nom d’utilisateur, il affiche le message : ”Utilisation : nom_du_script nom_utilisateur”, où nom_du_script est le nom de votre script récupéré automatiquement (si vous changez le nom de votre script, le message doit changer automatiquement)**

<pre>
#!/bin/bash
#Mon script username

valide=$(cat /etc/passwd | grep - c slapl)

if [ $valide = 0 ]; then
    echo "$valide Utilisateur valide"
 [ -z "nom_user" ]; then
    echo "Utilisation : $0 nom_utilisateur"
else
    echo "Utilisateur invalide"

fi

#Fin du script
</pre>

> ./username.sh
renvoie nom_utlisateur. 

Lorsque je change la ligne suivant :
> valide=$(cat /etc/passwd | grep - c lilou)
ce qui me renvoie Utilisateur invalide.


## Exercice 5. Factorielle
Écrivez un programme qui calcule la factorielle d’un entier naturel passé en paramètre (on supposera que
l’utilisateur saisit toujours un entier naturel).

<pre>
#!/bin/bash
 fact()
 {
    n=$1
     if [ $n = 0 ]; then
        echo 1
    else
        echo $(( n * `fact $((n-1))`))
    fi
 }
echo `fact $1`

#Fin du script
</pre>

## Exercice 6. Le juste prix
**Écrivez un script qui génère un nombre aléatoire entre 1 et 1000 et demande à l’utilisateur de le deviner.
Le programme écrira ”C’est plus !”, ”C’est moins !” ou ”Gagné !” selon les cas (vous utiliserez $RANDOM).**

<pre>
#!/bin/bash
#Mon script juste_prix

echelle=1000
nombre=$RANDOM
let "nombre %= $echelle"
echo "Saisissez un nombre compris entre 1 et 1000: "
read nb

while [ $nb -ne $nombre ]
do

    if [ $nb -lt $nombre ]; then
        echo "C'est plus !"

    elif [ "$nb -gt $nombre" ]; then
        echo "C'est moins !"
    fi

        echo -e "\n Saisir un autre nombre :"
        read new
        nb=new
done

if [ $nb -eq $nombre ]; then
    echo "Gagné !"
fi

# Fin du script

</pre>
