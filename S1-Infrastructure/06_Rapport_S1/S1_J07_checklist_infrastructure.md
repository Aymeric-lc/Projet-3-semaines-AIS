# Checklist infrastructure - Bilan Semaine 1 (J07 H1)

Projet Support N1/N2 et virtualisation - Semaine 1 - J07
Date : 24/11/2026 - Domaine : lab.local - Réseau : 192.168.56.0/24 (Host-Only, vmbr2)

Contrôle croisé des 4 VM en fin de Semaine 1 : état, adressage, résolution DNS, connectivité et snapshots.

---

## 1. État des 4 VM

| VM | IP attendue | OS | Service | Démarre | IP correcte | DNS = .10 | Ping DC (.10) | Service actif |
|---|---|---|---|---|---|---|---|---|
| SRV-AD | 192.168.56.10 | Windows Server 2025 | AD DS + DNS | ☐ | ☐ | lui-même | - | ☐ |
| PC-CLIENT | 192.168.56.20 | Windows 11 | Membre domaine | ☐ | ☐ | ☐ | ☐ | ☐ |
| UBUNTU-SRV | 192.168.56.30 | Ubuntu Server | SSH + HTTP | ☐ | ☐ | ☐ | ☐ | ☐ |
| GLPI-SRV | 192.168.56.40 | Debian 13 | GLPI + SQL | ☐ | ☐ | ☐ | ☐ | ☐ |

## 2. Commandes de vérification

Windows (SRV-AD, PC-CLIENT) :
```powershell
ipconfig /all
nslookup lab.local 192.168.56.10
Test-NetConnection 192.168.56.10 -Port 53
```

Linux (UBUNTU-SRV, GLPI-SRV) :
```bash
ip -br a
cat /etc/resolv.conf
ping -c 3 192.168.56.10
getent hosts lab.local
systemctl status ssh        # UBUNTU-SRV
systemctl status apache2 mariadb   # GLPI-SRV
```

## 3. Matrice de connectivité (ping entre VM)

| De \ Vers | SRV-AD | PC-CLIENT | UBUNTU-SRV | GLPI-SRV |
|---|---|---|---|---|
| SRV-AD | - | ☐ | ☐ | ☐ |
| PC-CLIENT | ☐ | - | ☐ | ☐ |
| UBUNTU-SRV | ☐ | ☐ | - | ☐ |
| GLPI-SRV | ☐ | ☐ | ☐ | - |

Note : les VM Windows peuvent bloquer le ping (ICMP) par défaut - un non-réponse n'est pas forcément une panne, vérifier le service réel (RDP, DNS, HTTP).

## 4. Snapshots S1-VALIDEE

| VM | Snapshot S1-VALIDEE créé | Date |
|---|---|---|
| SRV-AD | ☐ | |
| PC-CLIENT | ☐ | |
| UBUNTU-SRV | ☐ | |
| GLPI-SRV | ☐ | |

## 5. Synthèse

- Nombre de VM fonctionnelles : __ / 4
- Résolution DNS lab.local : ☐ OK sur tous les postes
- Écarts constatés : _______________________________________________
- Actions correctives : _____________________________________________
