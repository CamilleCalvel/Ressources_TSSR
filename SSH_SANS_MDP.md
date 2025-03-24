# SSH AVEC IDENTIFICATION AUTOMATIQUE

## UBUNTU -> UBUNTU

ON EXÉCUTE LE TERMINAL SUR LES DEUX MACHINES

- On vérifie qu'on a OpenSSH serveur sur le client :  
  `sshd -V` (*Pour vérifier si installé, sinon installer avec :*)  
  `sudo apt install openssh-server`  
  `sshd -V` (*Pour vérifier*)  
  `systemctl status ssh` (*Si pas activé :*)  
  `systemctl enable ssh` (*Active le service au démarrage*)  
  `systemctl status ssh`  
  `systemctl status sshd` (*Si tout est bon, tout est vert et "enabled"*)  

- On met les machines sur le même réseau (VM en pont) et on redémarre le service :  
  `sudo systemctl restart Networking` (ou `NetworkManager`)  
  `ip a` (*Pour récupérer l'IP*)  
  `ping (adresse ip)` (*On vérifie si la communication est ok*)  

- On se connecte depuis le serveur pour vérifier que tout fonctionne :  
  `ssh client@ip` (*On confirme et on entre le mot de passe*)  
  `exit`  

- On crée une clé SSH :  
  `ssh-keygen -t ecdsa`  
  `yes`  
  `passphrase non obligatoire`  
  `ls -a` (*Pour vérifier le fichier .ssh => lieu de stockage de la clé SSH*)  

- On déploie la clé SSH publique depuis le serveur vers le client :  
  `ssh-copy-id -i /home/user/.ssh/id_ecdsa.pub client@ip`  
  `On entre le mot de passe` (*Si tout est ok, on se connecte*)  

- On vérifie que la clé correspond sur les deux machines :  
  `cat /home/wcs/.ssh/id_ecdsa.pub` (*Sur le serveur*)  
  `cat .ssh/authorized_keys` (*Sur le client*)  

- On se connecte sans mot de passe :  
  `ssh client@ip` (*Si tout est ok, aucun mot de passe demandé*)  

---

## WINDOWS -> WINDOWS

ON EXÉCUTE POWERSHELL EN ADMIN SUR LES DEUX MACHINES = toujours un mot de passe sur le client pour SSH sur Windows

- On vérifie les services du client :  
  `get-service ssh-agent`  

- On active la fonctionnalité client-SSH sur les machines :  
  => Système => Fonctionnalités facultatives = vérifier la présence du client SSH  

- On vérifie la fonctionnalité serveur-SSH sur le client :  
  => Système... => Ajouter une fonctionnalité = serveur OpenSSH  

- On vérifie le service sur le client avec :  
  `get-service sshd` (*Si pas activé :*)  
  `get-service sshd | Set-Service -StartupType automatic` (*Puis on démarre :*)  
  `Restart-Service sshd`  
  `get-service sshd` (*Si tout est ok, le service est en "running"*)  

- On met les machines sur le même réseau IP (VM en pont) :  
  `ipconfig` (*On récupère l'IP du client*)  

- On teste la connexion SSH depuis le serveur :  
  `ssh client@ip powershell` (*Si tout est bon, on entre le mot de passe => on précise PowerShell*)  
  `exit`  

- Sur le serveur, on crée la clé SSH :  
  `ssh-keygen -t ecdsa`  
  (*On va vérifier où est la clé et si tout est ok :*)  
  `Set-Location c:\Users\client\.ssh\`  
  `get ChildItem` (*On vérifie la clé*)  

- On déploie la clé SSH publique depuis le serveur vers le client :  
  `get-content -path .\.ssh\id_ecdsa.pub` (*On copie la clé*)  
  `ssh client@ip` (*On se connecte en SSH au client*)  
  `add-content -path .ssh\authorized_keys -value "la clé"` (*On vérifie dans le fichier du client*)  

- On va dans le répertoire Program Data sur le client :  
  `C:\ProgramData\ssh\sshd_config` (*élément masqué dans affichage classique*)  
  `On commente la ligne MATCH group administrator` (*On sauvegarde et on redémarre le service :*)  
  `Restart-Service sshd`  

- On se connecte en SSH et normalement pas de mot de passe demandé :  
  `ssh client@ip`  

---

## UBUNTU -> WINDOWS

ON EXÉCUTE LE TERMINAL ET POWERSHELL EN ADMIN SUR LES MACHINES RESPECTIVES  
=> Même vérification pour OpenSSH que précédemment

- On vérifie si on peut se connecter en SSH :  
  `ssh client@ip powershell` (*Si tout est ok, on quitte*)  
  `exit`  

- On déploie la clé SSH depuis Ubuntu vers Windows :  
  `cat .ssh/id_ecdsa.pub` (*On copie la clé*)  
  `ssh client@ip` (*On se connecte en SSH au client*)  
  `add-content -path .ssh\authorized_keys -value "la clé"` (*On colle la clé*)  

- On va dans Program Data sur le client :  
  `C:\ProgramData\ssh\sshd_config` (*Élément masqué dans affichage classique*)  
  `On commente la ligne MATCH group administrator` (*On sauvegarde et on redémarre le service :*)  
  `Restart-Service sshd`  

- On se connecte en SSH et normalement pas de mot de passe demandé :  
  `ssh client@ip`  

---

## WINDOWS -> UBUNTU

ON EXÉCUTE LE TERMINAL ET POWERSHELL EN ADMIN SUR LES MACHINES RESPECTIVES  
=> Même vérification pour OpenSSH que précédemment

- On vérifie si on peut se connecter en SSH :  
  `ssh client@ip` (*Si tout est ok, on quitte*)  
  `exit`  

- On déploie la clé SSH depuis Windows vers Ubuntu :  
  `get-content -path .\.ssh\id_ecdsa.pub` (*On copie la clé*)  
  `ssh client@ip` (*On se connecte en SSH au client*)  
  `nano .ssh/authorized_keys` (*On colle la clé*)  

- On se connecte en SSH et normalement pas de mot de passe demandé :  
  `ssh client@ip`
