# Commandes Cisco les Plus Utiles

## 1. Commandes de Base

- `enable` : Passe en mode privilégié.
- `disable` : Quitte le mode privilégié.
- `configure terminal` : Passe en mode de configuration globale.
- `exit` : Quitte le mode actuel.
- `end` : Retourne directement en mode privilégié.
- `write memory` / `copy running-config startup-config` : Sauvegarde la configuration active dans la configuration de démarrage.
- `show running-config` : Affiche la configuration en cours.
- `show startup-config` : Affiche la configuration de démarrage.
- `do show ?` : Affiche les options disponibles du menu précédent sans quitter le menu actuel.

## 2. Gestion des Interfaces

- `show ip interface brief` : Affiche un résumé des interfaces et de leur statut.
- `interface GigabitEthernet 0/1` : Accède à l'interface GigabitEthernet 0/1.
- `ip address 192.168.1.1 255.255.255.0` : Assigne une adresse IP à l'interface.
- `no shutdown` : Active une interface.
- `shutdown` : Désactive une interface.

## 3. Configuration des VLANs

- `show vlan brief` : Affiche la liste des VLANs et leurs interfaces associées.
- `vlan 10` : Crée le VLAN 10.
- `name Marketing` : Assigne un nom au VLAN.
- `interface GigabitEthernet 0/2` : Accède à l'interface.
- `switchport mode access` : Configure l'interface en mode access.
- `switchport access vlan 10` : Assigne le VLAN 10 à l'interface.
- `switchport mode trunk` : Configure l'interface en mode trunk.
- `switchport trunk allowed vlan 10,20` : Ajoute les VLANs 10 et 20 au trunk.

## 4. Routage

- `ip route 0.0.0.0 0.0.0.0 192.168.1.254` : Configure une route par défaut.
- `show ip route` : Affiche la table de routage.
- `router ospf 1` : Active le protocole OSPF.
- `network 192.168.1.0 0.0.0.255 area 0` : Ajoute un réseau à OSPF.
- `router bgp 65001` : Configure le protocole BGP avec l'AS 65001.
- `neighbor 192.168.2.1 remote-as 65002` : Configure un voisin BGP.

## 5. Gestion et Surveillance

- `show version` : Affiche la version du système.
- `show interfaces` : Affiche les détails des interfaces.
- `show ip arp` : Affiche la table ARP.
- `show mac address-table` : Affiche la table des adresses MAC.
- `show logging` : Affiche les logs.
- `debug ip icmp` : Active le débogage des paquets ICMP.
- `ping 192.168.1.1` : Teste la connectivité avec une adresse IP.
- `traceroute 8.8.8.8` : Affiche le chemin vers une adresse IP.

## 6. Configuration des Accès et Sécurité

- `enable secret MonMotDePasse` : Configure un mot de passe d'accès privilégié.
- `line vty 0 4` : Accède à la configuration des sessions Telnet/SSH.
- `password MonMotDePasse` : Définit un mot de passe pour Telnet/SSH.
- `login` : Active l'authentification sur les lignes VTY.
- `service password-encryption` : Crypte les mots de passe dans la configuration.
- `access-list 10 permit 192.168.1.0 0.0.0.255` : Crée une ACL autorisant le réseau 192.168.1.0.
- `access-list 10 deny any` : Bloque tous les autres trafics.
- `show access-lists` : Affiche les listes de contrôle d'accès (ACL).

## 7. Gestion des Utilisateurs

- `username admin privilege 15 secret MonMotDePasse` : Crée un utilisateur avec un mot de passe sécurisé.
- `show users` : Affiche les utilisateurs connectés.
- `who` : Liste les utilisateurs actifs.

## 8. Configuration SSH

- `hostname MonRouteur` : Définit le nom d'hôte du routeur.
- `ip domain-name mondomaine.com` : Configure un domaine pour l'authentification SSH.
- `crypto key generate rsa` : Génère une clé RSA pour SSH.
- `ip ssh version 2` : Active la version 2 de SSH.
- `line vty 0 4` : Accède à la configuration des sessions SSH.
- `transport input ssh` : Active SSH uniquement.
- `login local` : Utilise la base locale des utilisateurs pour l'authentification SSH.

- `hostname MonRouteur` : Définit le nom d'hôte du routeur.
- `ip domain-name mondomaine.com` : Configure un domaine pour l'authentification SSH.
- `crypto key generate rsa` : Génère une clé RSA pour SSH.
- `ip ssh version 2` : Active la version 2 de SSH.
- `line vty 0 4` : Accède à la configuration des sessions SSH.
- `transport input ssh` : Active SSH uniquement.
- `ssh -l "nomde l'utilisateur" 'ip adress'` : permet de se connecter en ssh depuis le prompt à un routeur /switch.

## 9. Sauvegarde et Restauration

- `copy running-config startup-config` : Sauvegarde la configuration active.
- `copy startup-config running-config` : Charge la configuration de démarrage.
- `copy running-config tftp` : Sauvegarde la configuration sur un serveur TFTP.
- `copy tftp running-config` : Restaure la configuration depuis un serveur TFTP.

Ces commandes couvrent les bases essentielles pour configurer et gérer un équipement Cisco. Elles permettent de travailler efficacement sur les switchs et routeurs Cisco.

