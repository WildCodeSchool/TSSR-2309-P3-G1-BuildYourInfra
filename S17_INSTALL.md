# Guide d'installation des deux serveurs ADCORE :

## Configuration EFI et mise en place du RAID 1 : 

Dans virtualbox, avant le démarrage de la nouvelle machine, cocher dasn **Système** la case **EFI**. Cela permettra de créer le disk 0 au format GPT et non MBR.  

Ajouter un second disque.

Une fois la machine allumée, on tape 15 pour aller sur l'invite de commande :
- diskpart
- list disk
- select disk 0
- convert dynamic
- select disk 1
- convert gpt
- convert dynamic
- select volume 0
- add disk=1

Le disque 0 est maintenant en mirroring avec le disque 1.


## Installation : 

On configure tout d'abord le DNS dans le menu (8).    
Pour l'installation de ces serveurs, on utilise le script suivant : 

```PowerShell
###################################################################  Initialisation Variable  ##########################################################################



$arg1 = read-host "Donner un nom au serveur CORE" 

$arg2 = read-host "Donner une adresse IP" 

$arg3 = read-host "Donner l'adresse IP de la Gateway"

$arg4 = read-host "Renseigner le nom de Domaine"



#############################################################################  MAIN  ###################################################################################



Rename-Computer -NewName $arg1 -Force

New-NetIPAddress -IPAddress "$arg2" -PrefixLength "24" -InterfaceIndex (Get-NetAdapter).ifIndex -DefaultGateway "$arg3"

Set-DnsClientServerAddress -InterfaceIndex (Get-NetAdapter).ifIndex -ServerAddresses ("$arg4")

Install-WindowsFeature -Name RSAT-AD-Tools -IncludeManagementTools -IncludeAllSubFeature

Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools -IncludeAllSubFeature

install-WindowsFeature -Name DNS -IncludeManagementTools -IncludeAllSubFeature

Install-ADDSDomainController -DomainName "$arg4" -Credential (Get-Credential)

Get-ADDomainController -Identity $arg1

Restart-Computer

```

On passe ensuite sur S17_USERGUIDE.md pour la répartition des rôles FSMO.
