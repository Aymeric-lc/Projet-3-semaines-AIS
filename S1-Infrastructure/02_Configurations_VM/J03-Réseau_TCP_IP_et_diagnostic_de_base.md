# Documentation technique - Réseau TCP/IP et diagnostic de base

Projet Support N1/N2 et virtualisation - Semaine 1 - J03 (H1/H2/H3)  
**Technicien :** [Aymeric] - **Date :** 18/11/2026[cite: 1]  
**Domaine cible :** `lab.local` - **Réseau :** `192.168.56.0/24` (Host-Only)[cite: 1, 5, 6]

---

## Contexte

Configuration de l'adressage IP statique de l'infrastructure virtuelle sur un réseau privé hôte (Host-Only). L'objectif est d'établir la connectivité inter-machines, de valider la résolution de noms et de distinguer une panne réseau (couche IP/masque) d'un problème de résolution DNS.

---

## Architecture et plan d'adressage (H1)

Mise en place des adresses IP fixes en /24 (255.255.255.0) pour l'ensemble des machines virtuelles du laboratoire :

| Machine | Système d'exploitation | Interface | Adresse IP | Masque | Rôle |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **SRV-AD** | Windows Server | Ethernet | 192.168.56.10 | /24 | Contrôleur de domaine / DNS |
| **PC-CLIENT** | Windows 11 | Ethernet | 192.168.56.20 | /24 | Poste client |
| **ubuntu-srv** | Linux (Ubuntu) | ens18 | 192.168.56.30 | /24 | Serveur Linux |
| **glpi-srv** | Linux (Debian/Ubuntu) | ens19 | 192.168.56.40 | /24 | Serveur GLPI[cite: 5, 6] |

### Vérification des interfaces IP

**Sur SRV-AD** (Windows) :
```powershell
ipconfig /all
```

Preuve :

()[Ipconfig-SRV-AD.png]

**Sur ubuntu-srv** (Linux) :

```bash
ip -br a
ip route
getent hosts
```

Preuve : 

()[IpBrief-Ubuntu.png]

**Sur glpi-srv **(Linux) :

```bash
ip -br a
ip route
getent hosts
```

Preuve :

()[IpBrief-GLPI.png]


## Diagnostic et tests de connectivité (H2)

1. **Tests ICMP (Ping inter-VM)**
Vérification de la connectivité de niveau 3 (couche réseau) entre le serveur SRV-AD et les autres machines du sous-réseau 192.168.56.0/24 :

()[Ping-SRV-AD.png]


2. **Matrice de connectivité**
Source \ DestinationSRV-AD (.10)PC-CLIENT (.20)ubuntu-srv (.30)glpi-srv (.40)SRV-AD (.10)-OK   OK   OK   PC-CLIENT (.20)OK-OKOKubuntu-srv (.30)OK   OK-OKglpi-srv (.40)OK   OKOK-

3. **Test de résolution DNSVérification du fonctionnement du service DNS depuis SRV-AD** :

()[Nslookup-SRV-AD.png]



