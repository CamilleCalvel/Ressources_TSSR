# Bash
**ls** – Liste les fichiers et dossiers  
**cd** – Change de répertoire  
**pwd** – Affiche le chemin courant  
**mkdir** – Crée un dossier  
**rm** – Supprime un fichier ou un dossier  
**cp** – Copie des fichiers ou dossiers   
**mv** – Déplace ou renomme des fichiers/dossiers  
**touch** – Crée un fichier vide  
**cat** – Affiche le contenu d’un fichier  
**less** – Lit un fichier page par page   
**nano** / **vim** – Édite un fichier texte  
**echo** – Affiche un texte ou écrit dans un fichier  
**man** – Affiche le manuel d’une commande  
**history** – Liste l’historique des commandes  
**grep** – Recherche dans un fichier  
**find** – Recherche de fichiers  
**tar** – Archive/désarchive des fichiers  
**chmod** – Change les permissions  
**chown** – Change le propriétaire  
**ps / top / htop** – Liste les processus  

**ping** – Tester la connectivité avec une machine  
**traceroute** – Suivre le chemin des paquets jusqu’à une destination  
**curl** – Tester une requête HTTP/HTTPS  
**wget** – Télécharger un fichier depuis Internet   
**ssh** – Connexion sécurisée à une machine distante  
**scp** – Copie sécurisée de fichiers via SSH  
**rsync** – Synchronisation de fichiers/dossiers en réseau  
**ftp / sftp** – Transfert de fichiers  
**telnet** – Tester la connexion à un port spécifique  
**nc** (netcat) – Vérifier les ports ouverts, tester une connexion  
**dig** – Interroger un serveur DNS  
**nslookup** – Résolution de noms (moins moderne que dig)  
**ip addr** – Afficher les adresses IP des interfaces  
**ip route** – Afficher ou modifier la table de routage  
**ifconfig** (ancien, remplacé par ip) – Voir les interfaces réseau  
**ss -tulnp** – Voir les ports TCP/UDP utilisés  
**netstat -tulnp** (ancien, remplacé par ss) – Ports et connexions réseau  
**arp -a** – Table ARP (adresses MAC connues)  
**ethtool eth0** – Infos sur une interface réseau  
**tcpdump** – Sniffer les paquets réseau  

## :arrow_right: Configuration réseau
```bash
nano /etc/network/interfaces
```
- IP dynamique:
   - allow-hotplug nomdeinterface
   - iface nomdeinterface inet dhcp
     
- IP static:
   - allow-hotplug nomdeinterface
   - iface nomdeinterface inet static
   - address x.x.x.x/x
   - gateway x.x.x.x
- Exemple:
<img src="https://github.com/user-attachments/assets/616af6b3-e61d-4317-9722-0f28590ec592" alt="Pictures" width="800" >

## :arrow_right: Configuration DNS
```bash
nano /etc/resolv.conf
```
- Renseigner l'adresse IP de votre serveur DNS
- Exemple:  
<img src="https://github.com/user-attachments/assets/123dd48d-3b9e-4545-a081-27f09311758f" alt="Pictures" width="800" >

## :arrow_right: Configuration nom d'hôte
```bash
nano /etc/hostname
```
- Exemple:
<img src="https://github.com/user-attachments/assets/5e70a0c1-27d3-481f-9ef2-2e9c8bce2adc" alt="Pictures" width="800" >  

## :arrow_right: Ajout à l'Active Directory

- Joindre la machine Debian au domaine Active Directory (AD).  
```bash
apt install packagekit samba-common-bin sssd-tools sssd libnss-sss libpam-sss policykit-1 sssd ntpdate ntp realmd` 
```
- Modifier le fichier `resolv.conf` avec `nameserver Adresse IP du server DNS`, `nameserver 1.1.1.1 (opt)` et `search Nomdedomaine`
Exemple:
<img src="https://github.com/user-attachments/assets/05a59ebe-68e4-4c91-925b-8857765ef12d" alt="Pictures" width="800" >  

- Se connecter au domaine avec la commande:
```bash
realm join --user=administrator/eur Nomdedomaine
```
- Renseigner le mot de passe de l'Administrateur du serveur AD
- Afficher le résumé avec la commande
```bash
realm list
```
<img src="https://github.com/user-attachments/assets/7201c038-fac5-4614-8adf-43a25b6e183c" alt="Pictures" width="300" > 

---
