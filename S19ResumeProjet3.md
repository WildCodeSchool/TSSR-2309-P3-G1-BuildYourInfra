# 1. Schéma de l'Infra
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/BilanEkoloclastV5.drawio.png?raw=true)
# 2. Tableau de Synthèse Matériels
| Nom des Machines | Type | OS | Fonction | Adresse IP + Masque | Nombre de Disques + Espace libre par disque | RAM |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| G1-PARIS-ServerAD | VM | Win2022 | Serveur AD, Maitre Schéma + Maitre d'Affectation | 192.168.10.5/24 | 4 Disques<br>Disque 1 et 2 Monté en RAID1<br>1 : 32go 8,53 libre<br>2 : 32go 8,53 libre<br>3 : 32go 31.86go libre<br>4 : 32go 29,79go libre | 8GB<br>82.29% |
| G1-PARIS-ServerADCore1 | VM | Win2022 Core | Serveur Réplication, Maitre RID + PDC | 192.168.10.80/24 | 1 disque 32go 23.8go libre | 2GB<br>53.45% |
| G1-PARIS-ADCORE2 | VM | Win2022 Core | Serveur Réplication, Maitre d'Infrastructure | 192.168.10.85/24 | 1 disque 32go 20go libre | 2GB<br>55.93% |
| G1-PARIS-CLIWIN2 | VM | Win10 | Test utilisateur AD + Accès interface Web des différents serveurs | 192.168.10.100/24 | 1 disque<br>32go 7.22go libre | 4GB<br>79.29% |
| G1-PARIS-ClientUbuntu | VM | Ubuntu22-04LTS | Accès interface Web des différents serveurs | 192.168.10.20/24 | 1 disque<br>32go 17go libre | 8GB<br>70.38% |
| G1-PARIS-ServWSUS | VM | Win2022 | Serveur de Gestion de Mise a jour | 192.168.10.50/24 | 1 disque<br>32go 17.3go libre | 4GB<br>56.76% |
| G1-PARIS-SRVGLPI | VM | Debian 12.04 | Serveur de Ticketing + Gestion de Parc | 192.168.10.91/24 | 2 disque en RAID1<br>1 : 50go<br>43go libre<br>2 : 50 go 43go libre<br> | 4GB<br>14.92% |
| G1-PARIS-ZIMBRA | VM | Ubuntu 18.04 | Serveur de Messagerie | 192.168.10.60/24 | 2 disque<br>1 : 32go<br>18go libre<br>2 : 32 go disque non utilisé | 8GB<br>79.02% |
| G1-PARIS-FREEPBX | VM | FreePBX | Serveur de téléphonie IP | 192.168.10.90/24 | 1 disque 50go 36go libre | 4GB<br>85.71% |
| G1-PARIS-ServerPassbolt | CT | Debian 12.02 | Serveur de Gestion de Mot de Passe | 192.168.10.15 | 1 disque 8Go 6.3go libre | 512MiB<br>22.98% |
| G1-PARIS-Snort | VM | Ubuntu 22.04 | Serveur de Gestion IDS IPS | 192.168.10.123/24 | 1 disque 32go<br>17go Libre  | 4GB<br>87.03%  |
| G1-PARIS-KALI | VM | Kali | Client pour Pain Test pour notre réseaux | 192.168.10.124/24 | 1 disque 32go<br>16go libre | 4GB<br>25.07% |
| G1-PARIS-pfsense-V2 | VM | Pfsense 2.7.1 | Pare-feu | Lan 192.168.10.2/24<br>Wan 192.168.1.239/24 | 1 disque 4go<br>1,7go libre | 2GB<br>72.78% |
| G1-BORDEAUX-pfsense-v2 | VM | Pfsense 2.7.1 | Pare-feu | Lan 172.17.1.1/24<br>Wan 192.168.1.241/24 | 1 disque 4go<br>1,7go libre | 2GB<br>61.70% |
| G1-BORDEAUX-CLIWIN | VM | Win10 | Test utilisateur AD + Acces interface Web des différent serveur | 172.17.1.50/24 | 1 disque 32go 10,2go libre | 4GB<br>80.77% |
| G1-BORDEAUX-RODC | VM | Win2022 | RODC | 172.17.1.5/24 | 1 disque 32go<br>17.1go libre | 4GB<br>83.32% |

# 3. Synthèse Documentation

| Nom de la Machine | Logiciel ou App | Statut Install | Statut UserGuide |
| ---- | ---- | ---- | ---- |
| G1-PARIS-ServerAD | Active Directory<br>DNS<br>DHCP<br>WSB<br>Ping Castle<br>Purple Knight<br>BloodHound | A jour<br>Inexistante (non nécessaire)<br>Inexistante (non nécessaire)<br>A jour<br>A jour<br>A jour<br>A jour | A jour<br>Inexistante (non nécessaire)<br>Inexistante (non nécessaire)<br>A jour<br>A jour<br>A jour<br>A jour |
| G1-PARIS-ServerADCore1 | Active Directory + Role FSMO | A jour | A jour<br> |
| G1-PARIS-ServerADCore2 | Active Directory + Role FSMO | A jour | A jour<br> |
| G1-PARIS-CLIWIN2 | RAS | RAS | RAS |
| G1-PARIS-ClientUbuntu | RAS | RAS | RAS |
| G1-PARIS-ServWSUS | Windows Server Update Service   | A jour | A jour |
| G1-PARIS-SRVGLPI | GLPI | A jour | A jour |
| G1-PARIS-ZIMBRA | Zimbra | GitHub BillU | GitHub BillU |
| G1-PARIS-FREEPBX | FreePBX | Inexistante ? | Inexistante ? |
| G1-PARIS-ServerPassbolt | Passbolt   | A mettre à jour (VM non fonctionnel donc a mettre a jour quand ca marchera )  | Inexistante (VM non fonctionnel donc pas de doc fourni) |
| G1-PARIS-Snort | Snort |  |  |
| G1-PARIS-KALI | OS Kali | Inexsitant (VM donné par notre Formateur) | Inexsitant (VM donné par notre Formateur) |
| G1-PARIS-pfsense-V2 | PfSense | Inexsitant (VM donné par notre Formateur) | Inexsitant (VM donné par notre Formateur) |
| G1-BORDEAUX-pfsense-v2 | PfSense | Inexsitant (VM donné par notre Formateur) | Inexsitant (VM donné par notre Formateur) |
| G1-BORDEAUX-CLIWIN | RAS | RAS | RAS |
| G1-BORDEAUX-RODC | Active Directory | Inexistante (VM non fonctionnel donc pas de doc fourni) | Inexistante (VM non fonctionnel donc pas de doc fourni) |
