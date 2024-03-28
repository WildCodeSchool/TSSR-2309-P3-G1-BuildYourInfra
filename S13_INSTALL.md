
# Création d'une sauvegarde du volume qui contient les dossiers partagés des utilisateurs.

Il faut s'assurer d'avoir un disque disponible qui sera dédié aux sauvegardes.

## Installation de Windows Server Backup

Dans le Server Manager sélectionner la coche "Windows Server Backup" et lancer l'installation en conservant les paramètres par défaut.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/sauvegarde/01_installaion_backup.png?raw=true)

Lorsque l'installation est terminée on obtient cette fenêtre :
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/sauvegarde/02_install_backup_terminee.png?raw=true)

## Configuration de Windows Server Backup

Nous choisissons l'option "Custom" 
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/sauvegarde/03_config_winsvr_backup.png?raw=true)

Choix du disque à sauvegarder qui est notre volume partagé "E"
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/sauvegarde/04_config_winsvr_backup.png?raw=true)

Choix de l'heure et du nombre de sauvegardes, nous en sélectionnons une hebdomadaire à 21h00 afin de limiter les sollicitations du système dans la journée.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/sauvegarde/05_config_winsvr_backup.png?raw=true)

Choix des options destination : Notre sauvegarde ira sur le disque "F" que nous trouvons en cliquant sur "Show All Available Disks".
Sur la fenêtre suivante il reste à choisir le disque et valider.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/sauvegarde/06_config_winsvr_backup.png?raw=true)

Valider et attendre la fin de la configuration, cette fenêtre s'affiche.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/sauvegarde/07_config_winsvr_backup.png?raw=true)

Nous pouvons vérifier les paramètres dans cette dernière fenêtre.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/sauvegarde/08_config_winsvr_backup.png?raw=true)



# RAID1 sur Windows Server

Pour cela vous allez avoir besoin de rajouter un disque dans votre VM, sa capacité doit au moins être équivalente à celle du disque qui sera dupliqué grâce au RAID1.

## Disk Management 
Une fois votre disque branché, il faut le configurer dans le "**Disk Management**".   
Pour y accéder, fais un clic droit sur le logo Windows de ta "**Barre Démarrer**".  
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubRAID1/Capture%20d'%C3%A9cran%202023-12-14%20101117.png?raw=true)

Une fois votre "**Disk Management**" ouvert, le "**Disk Management**" détecte votre nouveau disque et vous propose de choisir entre un format MBR ou GPT, il est important de choisir le même format que le disque cible ! Dans notre cas, on utilisera le GPT.  

Une fois votre disque au bon format, vous pouvez constater qu'il est basic, donc nous allons le convertir en disque dynamique.  

Fais un clic droit sur le disque que vous venez d'ajouter et sélectionnez "**Convert to Dynamic Disk**".  
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubRAID1/Capture%20d'%C3%A9cran%202023-12-14%20101208.png?raw=true)

Un petit Wizard Windows va se lancer, suivez les étapes en laissant tout par défaut et terminer la conversion.

Enfin, choisissez le volume que vous voulez dupliqué, faite un clic droit et sélectionnez  "**Add Mirror**" (ce qui correspond à faire un RAID1).  

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubRAID1/Capture%20d'%C3%A9cran%202023-12-14%20101257.png?raw=true)

Dans le Wizard Windows, sélectionnez le disque que vous venez d'implémenter dans votre VM, et terminez la configuration.  

## FAQ
1. A la fin de la configuration de mon miroir, un message d'erreur me dit qu'il n'y a pas assez d'espace sur mon disque alors que celui ci est vide !  
	-  C'est normal. Cela signifie juste qu'il n'y a plus assez d'espace dans le disque de la machine hôte pour effectuer le RAID1, fais de la place dans le disque de ta machine hôte ou son stocké les disques virtuelles et retente l'opération !


# Restriction horaire d'utilisation des machines

## Etablissement des horaires 

Nous pouvons définir les horaires d'accès aux machines des utilisateurs.
Nous pouvons le faire de deux manières. Manuellement, on peut clic droit sur un utilisateur : 
- Propriétés
- Compte
- Horaires d'accès

On arrive sur un tableau d'horaires que l'on peut modifier.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/horairesacces.JPG?raw=true)

Mais cette méthode serait trop longue pour l'appliquer à chaque utilisateur. Nous avons donc utilisé le script suivant : 
```PowerShell
Function Set-LogonHours{
   [CmdletBinding()]
   Param(
   [Parameter(Mandatory=$True)][ValidateRange(0,23)]$TimeIn24Format,
   [Parameter(Mandatory=$True,ValueFromPipeline=$True,ValueFromPipelineByPropertyName=$True,Position=0)]$Identity,
   [parameter(mandatory=$False)][ValidateSet("WorkingDays", "NonWorkingDays")]$NonSelectedDaysare="NonWorkingDays",
   [parameter(mandatory=$false)][switch]$Sunday,
   [parameter(mandatory=$false)][switch]$Monday,
   [parameter(mandatory=$false)][switch]$Tuesday,
   [parameter(mandatory=$false)][switch]$Wednesday,
   [parameter(mandatory=$false)][switch]$Thursday,
   [parameter(mandatory=$false)][switch]$Friday,
   [parameter(mandatory=$false)][switch]$Saturday
)
Process{
   $FullByte=New-Object "byte[]" 21
   $FullDay=[ordered]@{}
   0..23 | foreach{$FullDay.Add($_,"0")}
   $TimeIn24Format.ForEach({$FullDay[$_]=1})
   $Working= -join ($FullDay.values)
   Switch ($PSBoundParameters["NonSelectedDaysare"])
   {
    'NonWorkingDays' {$SundayValue=$MondayValue=$TuesdayValue=$WednesdayValue=$ThursdayValue=$FridayValue=$SaturdayValue="000000000000000000000000"}
    'WorkingDays' {$SundayValue=$MondayValue=$TuesdayValue=$WednesdayValue=$ThursdayValue=$FridayValue=$SaturdayValue="111111111111111111111111"}
   }
   Switch ($PSBoundParameters.Keys)
   {
    'Sunday' {$SundayValue=$Working}
    'Monday' {$MondayValue=$Working}
    'Tuesday' {$TuesdayValue=$Working}
    'Wednesday' {$WednesdayValue=$Working}
    'Thursday' {$ThursdayValue=$Working}
    'Friday' {$FridayValue=$Working}
    'Saturday' {$SaturdayValue=$Working}
   }

   $AllTheWeek="{0}{1}{2}{3}{4}{5}{6}" -f $SundayValue,$MondayValue,$TuesdayValue,$WednesdayValue,$ThursdayValue,$FridayValue,$SaturdayValue

   # Timezone Check
   if ((Get-TimeZone).baseutcoffset.hours -lt 0){
    $TimeZoneOffset = $AllTheWeek.Substring(0,168+ ((Get-TimeZone).baseutcoffset.hours))
    $TimeZoneOffset1 = $AllTheWeek.SubString(168 + ((Get-TimeZone).baseutcoffset.hours))
    $FixedTimeZoneOffSet="$TimeZoneOffset1$TimeZoneOffset"
   }
   if ((Get-TimeZone).baseutcoffset.hours -gt 0){
    $TimeZoneOffset = $AllTheWeek.Substring(0,((Get-TimeZone).baseutcoffset.hours))
    $TimeZoneOffset1 = $AllTheWeek.SubString(((Get-TimeZone).baseutcoffset.hours))
    $FixedTimeZoneOffSet="$TimeZoneOffset1$TimeZoneOffset"
   }
   if ((Get-TimeZone).baseutcoffset.hours -eq 0){
    $FixedTimeZoneOffSet=$AllTheWeek
   }

   $i=0
   $BinaryResult=$FixedTimeZoneOffSet -split '(\d{8})' | Where {$_ -match '(\d{8})'}

   Foreach($singleByte in $BinaryResult){
    $Tempvar=$singleByte.tochararray()
    [array]::Reverse($Tempvar)
    $Tempvar= -join $Tempvar
    $Byte = [Convert]::ToByte($Tempvar, 2)
    $FullByte[$i]=$Byte
    $i++
   }
   Set-ADUser -Identity $Identity -Replace @{logonhours = $FullByte} 
}
end{
   Write-Output "All Done :)"
}
}
Get-ADUser -SearchBase "OU=01Utilisateurs,DC=ekoloclast,DC=lan" -Filter * | Set-LogonHours -TimeIn24Format @(8,9,10,11,12,13,14,15,16,17) `
                    -Monday -Tuesday -Wednesday -Thursday -Friday -NonSelectedDaysare NonWorkingDays
```

Ce script permet d'établir les mêmes horaires à tous les employés de l'entreprise, c'est-à-dire de 8h à 18h du lundi au vendredi.


## Mise en place de la GPO associée

En plus de l'établissement des horaires, nous allons mettre en place une GPO qui déconnecte les employés de leur session après les heures indiquées.
Il s'agit d'une GPO s'appliquant aux ordinateurs.
Le chemin d'édition de cette GPO est le suivant : 
- Configuration ordinateur
- Windows Settings
- Paramètres de sécurité
- Stratégies locales
- Options de sécurité
- Serveur réseau Microsoft : Déconnecter les clients à l'ouverture de session heures expirent
- Enabled


La GPO a bien été montée et s'applique à l'ensemble des ordinateurs du domaine.
# Dossier Partagé déployé par GPO
## Prérequis matériel
Pour commencer nous allons ajouter, un nouveau disque sur notre serveur, qui nous servira de stockage pour tout les dossiers partagés des utilisateurs, services, et départements. on utilisera le format GPT, et allouons tout l'espace disponible dans un seul volume E:\\

## Création et mise en réseaux de la Racine de notre partage utilisateurs

Dans celui-ci, nous allons créer un dossier utilisateurs qui nous servira à stocker et partager tout les dossiers personnel de nos utilisateurs.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/CreationMiseenPartageduDossier/Capture%20d'%C3%A9cran%202023-12-14%20145505.png?raw=true)

Nous faisons un clic droit > Propriétés sur le dossier sur le lecteur E:\\ .
Dans l'onglet Partage, nous allons partager le dossier sur le réseaux.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/CreationMiseenPartageduDossier/Capture%20d'%C3%A9cran%202023-12-14%20153233.png?raw=true)

Nous allons ajouter le groupe "**Authentificated Users**" aux personnes pouvant accéder au dossier et nous leur donnons les droits de lecture/écriture.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/CreationMiseenPartageduDossier/Capture%20d'%C3%A9cran%202023-12-14%20145712.png?raw=true)

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/CreationMiseenPartageduDossier/Capture%20d'%C3%A9cran%202023-12-14%20145741.png?raw=true)

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/CreationMiseenPartageduDossier/Capture%20d'%C3%A9cran%202023-12-14%20145815.png?raw=true)

Nous faisons un clic droit > Propriétés sur le dossier partagé sur le réseaux dans \\\\\<NomdevotreServeur>.
Dans l'onglet Sécurité, cliqué sur Avancé.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/CreationMiseenPartageduDossier/Capture%20d'%C3%A9cran%202023-12-14%20150053.png?raw=true)

On commence par désactiver l'héritage. 

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/CreationMiseenPartageduDossier/Capture%20d'%C3%A9cran%202023-12-14%20150310.png?raw=true)

Puis on va ajouter dans les permissions le groupe "**CREATOR OWNER**"

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/CreationMiseenPartageduDossier/Capture%20d'%C3%A9cran%202023-12-14%20150550.png?raw=true)

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/CreationMiseenPartageduDossier/Capture%20d'%C3%A9cran%202023-12-14%20150604.png?raw=true)

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/CreationMiseenPartageduDossier/Capture%20d'%C3%A9cran%202023-12-14%20150633.png?raw=true)


Enfin on lui donne le "**Full Control**" sur le dossier
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/CreationMiseenPartageduDossier/Capture%20d'%C3%A9cran%202023-12-14%20150657.png?raw=true)



## Création et configuration de la GPO
Nous allons reprendre la création de GPO depuis le début.
Clic droit sur "**Group Policy Object**" > "**New**"

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/GPO/Capture%20d'%C3%A9cran%202023-12-14%20150747.png?raw=true)

Nous allons l'appeler "**UserDossierPerso**" 

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/GPO/Capture%20d'%C3%A9cran%202023-12-14%20150836.png?raw=true)

Nous allons utiliser les groupes de filtrage suivant pour appliqué notre GPO :
- "**Domain Computer**" : Obligatoire lors d'une GPO affectant les utilisateurs.
- "**GRPUtilisateurs**" : Notre groupe reprenant tout les utilisateurs de notre AD.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/GPO/Capture%20d'%C3%A9cran%202023-12-14%20151004.png?raw=true)

Comme cette GPO s'appliquera sur les utilisateurs, on désactive les configuration ordinateurs.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/GPO/Capture%20d'%C3%A9cran%202023-12-14%20151046.png?raw=true)

Nous allons configurer 3 règles dans cette GPO :
- "**Folder**" : qui nous permet de crée automatiquement un dossier n'appartenant qu'a l'utilisateurs de la session en cours sur la machine client
- "**Drivers Maps**" : qui va automatiquement mapper le dossier crée en tant que lecteur M:\
- "**Shortcut**" : qui va créer un raccourci sur le bureau de l'utilisateurs pour accéder plus rapidement à son dossier personnel 
### Règle Folder

Pour nous rendre a cette règle suivez le chemin suivant : 
- "**User Configuration**" > "**Preferences**" > "**Windows Settings**" > "**Folders**"

Faites un clic droit sur la fenêtre et sélectionner New > Folders

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/GPO/Capture%20d'%C3%A9cran%202023-12-14%20151313.png?raw=true)

Dans l'option "**Path**" nous renseignons le chemin suivant :
- \\\\\<NomdevotreServeur>\utilisateurs\\%LogonUser%

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/GPO/Capture%20d'%C3%A9cran%202023-12-14%20151411.png?raw=true)

La variable "**%LogonUser%**" permet de créer un dossier qui aura automatiquement le nom de notre utilisateurs connecté.

On se rend ensuite dans l'onglet "**Common**" et on coche la cas "**Run in logged-on ...**"

On valide la règle et passons à la suivante : "**Drive Maps**"

### Drive Maps

Pour nous rendre a cette règle suivez le chemin suivant : 
- "**User Configuration**" > "**Preferences**" > "**Windows Settings**" > "**Drive Maps**"

Faites un clic droit sur la fenêtre et sélectionner "**New**" > "**Mapped Drive**"

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/GPO/Capture%20d'%C3%A9cran%202023-12-14%20151501.png?raw=true)

Dans l'option "**Location**", nous renseignons le chemin que nous avons utilisé lors de la création automatique du dossier grâce à la variable "**%LogonUser%**".

Dans l'option "**Label as**", on renseigne le nom que le lecteur aura dans notre cas "**Dossier Perso**".

Dans l'option "**Driver Letter**", dans notre cas :
- pour les utilisateurs on utilisera le lecteur "**I:\\**"
- pour les services on utilisera le lecteur "**M:\\**"
- pour les départements on utilisera le lecteur "**N:\\**"

Ensuite on utilisera les options "**Show this drive**" et "**Show all drive**" pour afficher les lecteurs.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/GPO/Capture%20d'%C3%A9cran%202023-12-14%20151528.png?raw=true)

Enfin comme pour la règle des "**Folders**", on va dans l'onglet Common et on active "**Run in logged-on ...**"

### Shortcut

Pour nous rendre a cette règle suivez le chemin suivant : 
- "**User Configuration**" > "**Preferences**" > "**Windows Settings**" > "**Shortcut**"

Faites un clic droit sur la fenêtre et sélectionner "**New**" > "**Shortcut**"

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/GPO/Capture%20d'%C3%A9cran%202023-12-14%20151550.png?raw=true)

Dans l'option "**Name**", on renseigne le nom du raccourcie "**ESPACE PERSO**".

Dans l'option "**Target Path**", nous renseignons le chemin que nous avons utilisé lors de la création automatique du dossier grâce à la variable "**%LogonUser%**".

Dans l'option "**Icon File Path**", on peut modifier l'icône du raccourcie grâce a la bibliothèque disponible dans les "**...**"

Ensuite on utilisera les options "**Show this drive**" et "**Show all drive**" pour afficher les lecteurs.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/GPO/Capture%20d'%C3%A9cran%202023-12-14%20151603.png?raw=true)
### Application de la GPO 

Pour terminer, on applique notre GPO sur l'OU "**01Utilisateurs**"

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/GPO/Capture%20d'%C3%A9cran%202023-12-14%20152224.png?raw=true)

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/GPO/Capture%20d'%C3%A9cran%202023-12-14%20152242.png?raw=true)

Notre GPO est terminé et elle est appliqué correctement 

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubDossierPartage/GPO/Capture%20d'%C3%A9cran%202023-12-14%20152254.png?raw=true)
## Divers
On applique la même méthodologie pour les département et services on modifie juste les chemins d'accès au différent dossier correspondant au différent département ou services
