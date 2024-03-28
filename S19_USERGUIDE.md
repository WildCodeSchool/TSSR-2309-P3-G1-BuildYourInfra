# Guide d'utilisation : 

## Nmap : 

Nmap nous permet de "sniffer" les différents ports du réseau et de sortir un ensemnble d'informations utiles pour des attaques ultérieures en profondeur.   
L'outil existe en local sur Kali Linux. Nous pouvons le retrouver dans le menu en haut à gauche.

Une fois sur la console, voici quelques lignes de commande possibles pour sniffer un réseau : 
```
nmap 192.168.10.0/24
# Scan complet du réseau 192.168.10.0/24
nmap -sS 192.168.10.0/24
# Scan en mode discret du réseau, plus dur à détecter
nmap -sp 192.168.10.0/24
# Balayage ping du réseau pour voir les connexions possibles
```

Il en existe beaucoup d'autres avec plus de possibilités.   
Après un scan de quelques secondes, nmap affiche le résultat sur le CLI.  


## Hydra : 

Hydra nous permet de faire des attaques par force brute sur un réseau, afin de récupérer des mots de passe.  
C'est aussi un outil aussi natif de Kali Linux.  

On peut se créer un dossier de listes de mots : 
```
cd /home/kali 
mkdir passwords-and-usernames
git clone https://github.com/danielmiessler/SecLists
cp -r SecLists/Usernames/*.txt  /home/kali/passwords-and-usernames/
cp -r SecLists/Passwords/*.txt  /home/kali/passwords-and-usernames/
```

On peut utiliser un ensemble d'options avec notre attaque :   
    -s = port  
    -l = utilisateur simple    
    -L = utilisateurs extraient d’une liste   
    -P = mots de passe extraient d’une liste  
    -T = nombre d’essai par seconde (4, étant une valeur recommandé, pour outrepasser certains IPS vieillissant et non mis à jours)  
    -V = Afficher tout les essais dans la fenêtre du terminal   


Pour l'utilisateur, on utilise **l** si on le connaît (et on le renseigne), sinon on utilise **L** et on renseigne une liste de mots pour le trouver.   
De même pour le mot de passe, **p** si on le connaît et **P** si on ne le connaît pas et on renseigne une liste de mots pour l'attaque.  
Pour plus d'infos : 
```
hydra --help
```

La syntaxe générale est  :
```
hydra -l < nom d'utilisateur > -P < mot de passe > < protocole > : // < ip >
```

Exemple dans notre réseau : 
```
hydra -l wilder -P /usr/share/wordlists/rockyou.txt 192.169.10.91 ssh
# Attaque par brute force de notre serveur GLPI avec SSH installé et configuré sur l'utilisateur   
 connu wilder en utilisant la liste de mots rockyou.txt sur le protocole SSH.  
```

Hydra nous affiche le résultat après l'attaque.  

# Snort

Pour les besoins de notre projets nous avons établie les règles suivantes :

```
alert icmp any any -> any any (msg: "ICMP Detected"; sid: 1000001; rev:1) 

alert tcp $HOME_NET [!22,!80] -> any [!22,!80] (msg:"Scan Port"; sid:1000003;) 

alert tcp any any -> $HOME_NET 80 (flags: S; msg:"DDOS OSKOOR"; flow: stateless; threshold: type both, track by_dst, count 70, seconds 10; sid:1000004;) 

alert tcp any any -> $HOME_NET 22 (flags: S+; threshold: type both, track by_src, count 5, seconds 60;msg:"SSH Brute Force Attack"; sid:1000007;)
```

La première règle nous alerte  en cas de ping sur le réseaux.

La deuxième règle nous alerte si un scan de port est lancé sur tous les ports exceptés les ports 22 et 80, pour ne pas que cette règle n'apparaisse pas pendant les deux tests suivants.

La troisième nous alerte en cas de tentative de DDOS sur le port 80, si il a plus de 70 requête en moins de 10 secondes.

Enfin, la dernière règle nous préviens en cas de tentative d'attaque sur le service SSH de nos machines, si il y a plus de 5 requête en 60 secondes

# Socialphish

Une fois le répertoire GitHub sur votre Kali effectuer les commande suivante : 
```Shell
Cd socialphish
chmod 755 socialphish.sh
./socialphish.sh
```

Une fois le script lancé, vous verrez ce menu apparaître :

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GHSocialPhish/Capture%20d'%C3%A9cran%202024-02-02%20112349.png?raw=true)

Sélectionnez la page que vous voulez clonez, nous choisirons Netflix : 08. 
Ensuite le port de votre faux site, nous laisserons le port par défaut : 8080

Le script initialise le serveur pour votre fausse page et vous donne un lien, si vous le diffusez à votre victime, celle ci verra la page ci-dessous apparaitre.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GHSocialPhish/Capture%20d'%C3%A9cran%202024-02-02%20112609.png?raw=true)

Quand celle ci cliquera sur le bouton "Sign In", la victime sera redirigé vers la vrai page de netflix.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GHSocialPhish/Capture%20d'%C3%A9cran%202024-02-02%20112625.png?raw=true)

Et nous récupérerons sur notre script ses identifiants de connexion sauvegardé dans un fichier texte.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GHSocialPhish/Capture%20d'%C3%A9cran%202024-02-02%20112402.png?raw=true)
