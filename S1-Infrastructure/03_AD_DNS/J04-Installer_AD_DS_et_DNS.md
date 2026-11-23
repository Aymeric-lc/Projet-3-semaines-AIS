# Installation AD DS et DNS

Projet Support N1/N2 et virtualisation - Semaine 1 - J04 (H1 / H2 / H3)
Technicien : [Nom Prénom] - Date : 23/11/2026
Domaine : lab.local - Serveur concerné : SRV-AD (192.168.56.10)

---

## Contexte

Déploiement des rôles AD DS (Active Directory Domain Services) et DNS sur le serveur SRV-AD, afin de centraliser les identités, gérer l'authentification et assurer la résolution de noms du domaine lab.local. L'objectif est de préparer l'annuaire (unités d'organisation et groupes) et de vérifier le bon fonctionnement des services.

## H1 - État de départ et préparation

Avant l'installation et la promotion du rôle contrôleur de domaine :

- Nom d'hôte : SRV-AD
- Adressage IPv4 statique : 192.168.56.10 (masque /24, 255.255.255.0) sur l'interface Ethernet
- Précaution : création d'un snapshot de la VM avant la promotion au rang de contrôleur de domaine, avec uniquement des comptes et identifiants de test
- Vérification de l'état du rôle via PowerShell administrateur :

```powershell
Get-WindowsFeature AD-Domain-Services
```

## H2 - Installation et promotion au domaine lab.local

Installation du rôle AD DS et des outils d'administration (qui intègrent la gestion DNS) :

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

Promotion du serveur SRV-AD en contrôleur de domaine d'une nouvelle forêt lab.local, via l'assistant du Gestionnaire de serveur (redémarrage automatique après la promotion).

### Preuve : rôles AD DS et DNS actifs et domaine lab.local

Dans le Gestionnaire de serveur (Serveur local), on vérifie que le nom de l'ordinateur est bien SRV-AD, que le domaine est lab.local, que l'adresse Ethernet est 192.168.56.10, et que les rôles AD DS et DNS apparaissent dans le volet de gauche.

![Rôles AD DS et DNS - domaine lab.local](captures/j04_roles_adds_dns.png)

## H3 - Organisation de l'annuaire et groupes

Dans la console Utilisateurs et ordinateurs Active Directory (dsa.msc), création d'une structure d'unités d'organisation (OU) sous lab.local, pour structurer les objets et faciliter l'administration et la délégation :

- OU `lab.local` (sous la racine lab.local)
- OU `Ordinateurs`
- OU `Utilisateurs` (contenant le groupe de sécurité global `GrpUtilisateurs`)

### Preuve : structure des OU et groupe de sécurité

![Structure des OU et groupe de sécurité](captures/j04_ou_groupes.png)

---

## Captures

| Fichier | Contenu |
|---|---|
| captures/j04_roles_adds_dns.png | Rôles AD DS et DNS actifs, domaine lab.local |
| captures/j04_ou_groupes.png | Structure des OU et groupe GrpUtilisateurs |
