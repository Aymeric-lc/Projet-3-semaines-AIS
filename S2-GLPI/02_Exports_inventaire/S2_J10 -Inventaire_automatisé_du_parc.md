# J10 - Inventaire automatisé du parc

> Projet Support N1/N2 et virtualisation - Semaine 2
> Responsable : Grégory Datis - 24 et 25/11/2026
 
Les deux postes équipés d'un agent, UBUNTU-SRV et PC-CLIENT, remontent dans GLPI (`192.168.56.40`). La comparaison avec l'AD montre 4 machines et uniquement des écarts légitimes.

## Sommaire
 
- [H1 - Installation des agents et URL d'inventaire](#h1---installation-des-agents-et-url-dinventaire)
- [H2 - Remontées vérifiées dans GLPI](#h2---remontées-vérifiées-dans-glpi)
- [H3 - Comparaison GLPI / AD](#h3---comparaison-glpi--ad)
- [Auto-contrôle](#auto-contrôle)

---

## H1 - Installation des agents et URL d'inventaire
 
**Résultat :** l'agent GLPI tourne en service sur UBUNTU-SRV et PC-CLIENT, avec l'URL `http://192.168.56.40/`.
**Preuve attendue :** présence des agents sur chaque OS.
 
| Machine | Méthode d'installation | Vérification | État |
| --- | --- | --- | --- |
| UBUNTU-SRV (`.30`) | Installeur Perl Linux officiel | `systemctl status glpi-agent` | `active (running)` |
| PC-CLIENT (`.20`) | MSI x64 en mode silencieux | `Get-Service glpi-agent` | `Running` |

### Ubuntu
 
```bash
cd /tmp
VER=$(curl -s https://api.github.com/repos/glpi-project/glpi-agent/releases/latest
wget https://github.com/glpi-project/glpi-agent/releases/download/$VER/glpi-agent-$VER-linux-installer.pl
sudo perl glpi-agent-$VER-linux-installer.pl --server="http://192.168.56.40/" --runnow
systemctl status glpi-agent --no-pager
cat /etc/glpi-agent/conf.d/00-install.cfg     # server = http://192.168.56.40/
sudo glpi-agent --force
sudo journalctl -u glpi-agent -n 40 --no-pager
```

La version est récupérée automatiquement depuis GitHub pour installer la dernière publiée, qui intègre les correctifs de sécurité récents.

### Windows 11
 
```powershell
msiexec /i GLPI-Agent-x.xx-x64.msi /quiet SERVER="http://192.168.56.40/" RUNNOW=1 ADD_FIREWALL_EXCEPTION=1
Get-Service glpi-agent
```
 
<img width="439" height="153" alt="capture presence glpi-agent WIN" src="https://github.com/user-attachments/assets/3eb902c4-40fb-4391-8f8e-3ec475db4f90" />

*Service glpi-agent en Running sur PC-CLIENT.*

### Incident rencontré : 403 Forbidden
 
| Étape | Constat |
| --- | --- |
| Symptôme | `glpi-agent --force` : `communication error: 403 Forbidden` au prolog |
| Lecture | Réseau et URL corrects : GLPI répond mais refuse |
| Cause 1 | Inventaire désactivé dans *Administration > Inventaire* |
| Cause 2 | Authentification des agents en OAuth (GLPI 11) sans identifiants côté agent |
| Correction | Inventaire activé ; authentification passée à "Aucune" par l'administrateur GLPI |
| Vérification | `server0 answer shows it supports GLPI Agent protocol`, puis `New inventory from ubuntu-srv` |

<img width="692" height="261" alt="Ubuntu" src="https://github.com/user-attachments/assets/01192a62-1f62-464c-aec3-19ec7765c86c" />

*Sur UBUNTU-SRV : le 403 avant correction, puis l'inventaire envoyé après activation.*

> [!IMPORTANT]
> **Exception justifiée :** authentification des agents désactivée temporairement, le lab étant isolé sur `192.168.56.0/24` sans exposition Internet. Passage en OAuth prévu par le responsable GLPI (Aymeric).

---
 
## H2 - Remontées vérifiées dans GLPI
 
**Résultat :** *Parc > Ordinateurs* liste 2 machines, avec un nom, un OS et une date de remontée corrects.
**Preuve attendue :** inventaire GLPI exporté ou capturé.
 
| Nom GLPI | OS | IP attendue | Dernière modification (GLPI) | Modèle |
| --- | --- | --- | --- | --- |
| ubuntu-srv | Ubuntu 26.04.1 LTS | 192.168.56.30 | 2026-11-24 09:55 | QEMU Standard PC (Q35 + ICH9, 2009) |
| PC-CLIENT | Microsoft Windows 11 Professionnel | 192.168.56.20 | 2026-11-24 14:00 | QEMU Standard PC (Q35 + ICH9, 2009) |
 
IP à confirmer dans l'onglet *Ports réseau* de chaque fiche. Export : icône de téléchargement de la liste, format CSV ou PDF.
 
<img width="732" height="177" alt="capture inventaire WIN exporté" src="https://github.com/user-attachments/assets/7fc2a03d-7987-4d48-b725-8c27ea3365bc" />

*Parc > Ordinateurs : les deux machines remontées par les agents.*

---

## H3 - Comparaison GLPI / AD
 
**Résultat :** sur 4 machines, une seule apparaît dans les deux outils (PC-CLIENT), et les 3 écarts sont légitimes.
**Preuve attendue :** table des écarts GLPI/AD et actions.
 
L'AD recense les objets du domaine, GLPI mesure l'état réel des machines qui ont un agent. Une machine hors domaine absente de l'AD n'est pas une anomalie.
 
```powershell
Get-ADComputer -Filter * -Properties OperatingSystem,IPv4Address,LastLogonDate |
  Select-Object Name,OperatingSystem,IPv4Address,LastLogonDate |
  Format-Table -AutoSize
```

<img width="739" height="178" alt="get-adcomputer" src="https://github.com/user-attachments/assets/28f07e3e-86cc-4bd9-a52f-4480d18dcffe" />

*Get-ADComputer sur SRV-AD : 2 objets ordinateur dans le domaine lab.local.*

| Machine | IP | AD | GLPI | Écart légitime | Explication | Action |
| --- | --- | --- | --- | --- | --- | --- |
| PC-CLIENT | .20 | Oui (Windows 11 Professionnel) | Oui | Pas d'écart | Membre du domaine, agent installé | Aucune |
| SRV-AD | .10 | Oui (Windows Server 2025 Standard) | Non | Oui | Aucun agent prévu sur le contrôleur de domaine | Option : installer l'agent pour un parc complet |
| ubuntu-srv | .30 | Non | Oui (Ubuntu 26.04.1 LTS) | Oui | Serveur Linux non joint au domaine | Aucune |
| GLPI-SRV | .40 | Non | Non | Oui | Serveur Debian hors domaine, sans agent | Option : installer l'agent sur le serveur GLPI |

> [!TIP]
> L'AD indique un `LastLogonDate` au 22/11/2026 pour SRV-AD et PC-CLIENT, alors que les deux tournent le 25/11. L'attribut n'est répliqué que tous les 9 à 14 jours environ. Il ne prouve pas qu'une machine est inactive : on se fie à la date de remontée GLPI et à un ping.
 
---

## Auto-contrôle
 
- [ ] Les trois tâches sont réalisées ou l'écart est expliqué
- [ ] Les preuves sont datées
- [ ] Les configurations et incidents sont reproductibles
- [ ] Les secrets sont exclus des captures
