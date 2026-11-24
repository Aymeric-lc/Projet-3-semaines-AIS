# Checklist des preuves - Projet Support N1/N2 et virtualisation

Suivi des captures, fiches et exports à produire, jour par jour (d'après la colonne "Preuve attendue" du livret).
On coche au fur et à mesure. Nommage des fichiers : `SX_JXX_description.ext`.

Lab : 4 VM sur Proxmox - SRV-AD (.10), PC-CLIENT (.20), UBUNTU-SRV (.30), GLPI-SRV (.40) - réseau 192.168.56.0/24 isolé, domaine lab.local.

---

## Semaine 1 - Infrastructure

### J01 - Cadrage et préparation
- [ ] Fiche capacité de l'hôte + dimensionnement des 4 VM
- [ ] Capture version de l'hyperviseur + arborescence de travail
- [ ] Plan IP initial + test du réseau isolé

### J02 - Création des VM et snapshots
- [ ] VM AD créée + capture des paramètres
- [ ] 3 autres VM créées + tableau des ressources
- [ ] Captures cartes réseau + snapshots `BASE-VM` sur chaque VM

### J03 - Réseau TCP/IP
- [x] Table IP + captures ipconfig / ip -br a
- [x] Matrice de connectivité avant/après (ping, traceroute)
- [x] Schéma réseau v1 (draw.io)

### J04 - AD DS et DNS
- [x] Fiche état du serveur + snapshot avant promotion
- [x] Capture domaine lab.local + DNS
- [x] Structure des OU + test nslookup

### J05 - Comptes AD et poste client
- [x] ipconfig /all + nslookup lab.local (PC-CLIENT)
- [x] Capture jointure au domaine + liste des comptes de test
- [x] Fiche incident compte + procédure N1

### J06 - Ubuntu : exploitation
- [x] Accès SSH validé + comptes créés
- [x] Capture service HTTP + logs (systemctl, journalctl, curl)
- [x] Procédure de restauration du service web

### J07 - Bilan Semaine 1
- [ ] Checklist infrastructure complétée
- [ ] Diagnostic avant/après de la panne injectée par le formateur
- [ ] Dossier S1 + snapshots `S1-VALIDEE` + auto-évaluation

---

## Semaine 2 - GLPI et support N1

### J08 - Installation Debian et pile GLPI
- [ ] Capture système + ping .10
- [ ] Services actifs + versions relevées (apache/php/mariadb)
- [ ] Checklist pile Web/DB + snapshot

### J09 - Déploiement et sécurisation GLPI
- [ ] Page des contrôles d'installation GLPI
- [ ] Connexion à http://192.168.56.40
- [ ] Captures des accès + matrice des droits

### J10 - Inventaire automatisé
- [ ] Présence des agents sur chaque OS
- [ ] Inventaire GLPI exporté/capturé
- [ ] Table des écarts GLPI/AD + actions

### J11 - Centre de services et qualification N1
- [ ] Structure du centre de services (catégories, groupes, profils)
- [ ] Matrice priorité / SLA (P1 à P4)
- [ ] 3 tickets de test qualifiés complets

### J12 - Incidents Windows et AD (N1)
- [ ] Ticket I-01 (DNS client) + nslookup avant/après
- [ ] Ticket I-02 (compte désactivé/verrouillé) + journal AD sans mot de passe
- [ ] Ticket I-03 (droits NTFS) + capture des permissions

### J13 - Incidents Linux et GLPI Agent
- [ ] Ticket I-04 (agent GLPI arrêté) + preuve nouvelle remontée
- [ ] Ticket I-05 (HTTP indisponible) + curl après correction
- [ ] 2 articles de base de connaissances publiés

### J14 - Bilan Semaine 2 (simulation helpdesk)
- [ ] File GLPI correctement catégorisée (5 demandes)
- [ ] Historique GLPI complet et horodaté
- [ ] Rapport intermédiaire + export des tickets + snapshot S2

---

## Semaine 3 - Support N2 et projet intégrateur

### J15 - Clones et restauration
- [ ] Fiche méthodes (snapshots / clones / OVA) + choix justifié
- [ ] Clone fonctionnel sans IP dupliquée
- [ ] Journal de restauration + tests

### J16 - Services réseau avancés (N2)
- [ ] Schéma des flux + matrice des dépendances
- [ ] Diagnostic reproductible + cause racine (DNS serveur)
- [ ] Ticket escaladé N2 (panne SQL/Apache) + preuves avant/après

### J17 - Sécurisation postes et serveurs
- [ ] Rapport gpresult + paramètres GPO
- [ ] Checklist de durcissement + exceptions justifiées
- [ ] Journal des actions de sécurité

### J18 - Sauvegardes et continuité
- [ ] Plan de sauvegarde (RPO/RTO)
- [ ] Archives vérifiées + stockage hors VM
- [ ] Procès-verbal de restauration

### J19 - Problèmes et changements
- [ ] Fiche problème + arbre des causes (5 pourquoi)
- [ ] Fiche de changement soumise à validation
- [ ] PV du changement + procédure KB v2

### J20 - Projet final (cellule de support mixte)
- [ ] 4 tickets N1 (compte, DNS, agent, web) avec priorité et preuves
- [ ] Tickets résolus / escaladés selon la matrice
- [ ] Grille de recette infrastructure et support

### J21 - Restitution et évaluation
- [ ] Dossier final standardisé (carto, inventaire, tickets, KB, sauvegardes)
- [ ] Démonstration de soutenance (incident tiré au sort + restauration)
- [ ] Note /20 + plan de progression individuel

---

## Dossiers de remise (rappel structure)

- S1-Infrastructure : 01_Architecture, 02_Configurations_VM, 03_AD_DNS, 04_Ubuntu, 05_Snapshots, 06_Rapport_S1
- S2-GLPI : 01_Installation_GLPI, 02_Exports_inventaire, 03_Tickets_N1, 04_SLA, 05_Base_Connaissances, 06_Rapport_S2
- S3-Support-N2 : 01_Clones_Restauration, 02_Incidents_N2, 03_Securite, 04_Sauvegardes, 05_Changement, 06_Rapport_Final

Regle : aucun mot de passe ni secret dans les fichiers (repo public).
