# Inventaire des snapshots - Lab Proxmox

Projet Support N1/N2 et virtualisation
Hyperviseur : Proxmox VE - Pool projets-ais

Les snapshots permettent de revenir à un état stable (retour arrière). Ce fichier recense les points de restauration créés sur chaque VM ; les captures de l'onglet Snapshots de Proxmox sont dans `captures/`.

---

## Snapshots imposés par le projet

- `BASE-VM` : état propre juste après création de la VM (J02)
- `S1-VALIDEE` : fin de Semaine 1, architecture validée (J07)

## Tableau des snapshots

| VM | IP | Snapshot | Date | État capturé |
|---|---|---|---|---|
| SRV-AD | 192.168.56.10 | BASE-VM | 22/11/2026| OS installé, avant promotion AD |
| SRV-AD | 192.168.56.10 | S1-VALIDEE | 24/11/2026| Fin Semaine 1 |
| PC-CLIENT | 192.168.56.20 | BASE-VM | 22/11/2026 | OS installé, avant jointure domaine |
| PC-CLIENT | 192.168.56.20 | S1-VALIDEE | 24/11/2026 | Fin Semaine 1 |
| UBUNTU-SRV | 192.168.56.30 | BASE-VM | 22/11/2026 | OS installé, avant services |
| UBUNTU-SRV | 192.168.56.30 | S1-VALIDEE | 22/11/2026 | Fin Semaine 1 |
| GLPI-SRV | 192.168.56.40 | BASE-VM | 22/11/2026 | OS installé, avant pile GLPI |
| GLPI-SRV | 192.168.56.40 | S1-VALIDEE | 24/11/2026 | Fin Semaine 1 |

## Captures

| Fichier | Contenu |
|---|---|
| ![](Captures/Snap-AD.png) | Onglet Snapshots de SRV-AD |
| ![](Captures/Snap-Client.png) | Onglet Snapshots de PC-CLIENT |
| ![](Captures/Snap-Ubuntu.png) | Onglet Snapshots de UBUNTU-SRV |
| ![](Captures/Snap-glpi.png) | Onglet Snapshots de GLPI-SRV |

---

Note : sur Proxmox, un snapshot se prend par VM (sélection de la VM > onglet Snapshots > Take Snapshot). Prendre la capture au moment de la création pour garder la trace de l'état à cette étape.
