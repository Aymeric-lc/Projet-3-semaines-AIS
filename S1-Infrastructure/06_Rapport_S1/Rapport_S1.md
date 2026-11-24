# Rapport de Semaine 1 - Infrastructure

Projet Support N1/N2 et virtualisation - AIS - RNCP37680 - CP1
Groupe : [noms du groupe] - Date : 24/11/2026
Formateur : Mohamed Amine Harabi

---

## 1. Contexte et objectif

Construction d'un laboratoire de 4 machines virtuelles sur Proxmox, sur un réseau isolé lab.local, afin de mettre en place une infrastructure de base : contrôleur de domaine (AD DS + DNS), poste client Windows, serveur Linux, et serveur GLPI. Objectif de fin de semaine : une infrastructure fonctionnelle, reproductible et documentée, capable de résoudre les noms et d'authentifier les comptes du domaine.

## 2. Architecture réalisée

| Machine | Rôle | IP (Host-Only) | OS |
|---|---|---|---|
| SRV-AD | AD DS + DNS | 192.168.56.10 | Windows Server 2025 |
| PC-CLIENT | Membre du domaine | 192.168.56.20 | Windows 11 |
| UBUNTU-SRV | SSH + HTTP | 192.168.56.30 | Ubuntu Server |
| GLPI-SRV | GLPI + SQL | 192.168.56.40 | Debian 13 |

Réseau : 192.168.56.0/24, Host-Only (bridge Proxmox vmbr2), isolé, sans passerelle. DNS : SRV-AD (192.168.56.10). Domaine : lab.local. Mises à jour via adaptateur NAT secondaire ponctuel.

Schéma : voir `01_Architecture/cartographie_reseau_v2.png`.

## 3. Travaux réalisés (J01 à J06)

- J01 : cadrage, inventaire de l'hôte, création du réseau Host-Only
- J02 : création des 4 VM et snapshots BASE-VM
- J03 : adressage IP fixe, tests de connectivité, cartographie réseau v1
- J04 : installation AD DS + DNS, promotion du domaine lab.local, création des OU et groupes
- J05 : configuration et jointure de PC-CLIENT, comptes de test, incident compte N1
- J06 : exploitation Ubuntu (SSH, service web), incident de service

## 4. État des livrables

| Livrable | Dossier | État |
|---|---|---|
| Architecture (schéma v2) | 01_Architecture | ☐ |
| Configurations VM | 02_Configurations_VM | ☐ |
| AD / DNS (fiches + preuves) | 03_AD_DNS | ☐ |
| Ubuntu (journaux) | 04_Ubuntu | ☐ |
| Snapshots S1-VALIDEE | 05_Snapshots | ☐ |
| Rapport S1 | 06_Rapport_S1 | ☐ |

## 5. Difficultés rencontrées

- Mise en place et partage du Proxmox au sein de l'équipe
- _______________________________________________

## 6. Auto-évaluation

- Ce qui a bien fonctionné : _______________________________________
- Ce qui reste à améliorer : La communication et le partage des tâches (surtout avec Gregory DatiS)
- Compétences travaillées : virtualisation, réseau TCP/IP, AD DS, DNS, exploitation Linux, support N1

## 7. Conclusion

Infrastructure de Semaine 1 fonctionnelle et validée (snapshots S1-VALIDEE). Base prête pour la Semaine 2 (déploiement du centre de services GLPI).
