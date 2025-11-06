<details><summary><h1>Définition</h1></summary>  

La cryptologie
La cryptographie 
La cryptanalyse 
La confidentialité, l'authenticité et l'intégrité 


Le chiffrement 
Le chiffrement symétrique 
Le chiffrement asymétrique 
Le hashage 

Un certificat 
Une autorité de certification 
Une PKI 
Le protocole SSL 
Le protocole TLS 

---
</details>

<details><summary><h1>Pratique</h1></summary>  
  
<details><summary><h2>Chiffrement symétrique</h2></summary> 

### Prérequis

sudo apt update
sudo apt install -y openssl hexedit

**Préparer le fichier**

Sur machine A (ou les deux pour tester) :

mkdir -p ~/crypto_tests
cd ~/crypto_tests
echo "je suis en AIS chez Simplon" > ais.txt
cat ais.txt

**Chiffrer avec AES-256-CBC**
openssl enc -aes-256-cbc -salt -in ais.txt -out ais_aes256cbc.enc

**Chiffrer avec ChaCha20**
openssl enc -chacha20 -in ais.txt -out ais_chacha20.enc

> Remarque les méthodes de chiffrement gcm et blowfish sont devenues obsolètes. Elles ne seront donc pas utilisés dans cette procédure

**Comparer la taille des fichiers chiffrés**
ls -lh ais*


**Modifier un fichier chiffré avec hexedit**
hexedit ais_aes256cbc.enc
> Dans hexedit : navigue (flèches), change un octet (ex : tape 00 -> FF), sauvegarde : Ctrl+X puis Y

**Déchiffrer pour tester la détection d’erreur**
openssl enc -d -aes-256-cbc -in ais_aes256cbc.enc -out test_dechiffre.txt

**Déchiffrer tous les fichiers et comparer**
AES :
openssl enc -d -aes-256-cbc -in ais_aes256cbc.enc -out dec_aes256cbc.
txt
Chacha20 :
openssl enc -d -chacha20 -in ais_chacha20.enc -out dec_chacha20.txt 

---
</details>

<details><summary><h2>Chiffrement asymétrique</h2></summary>

Sur la Machine 1 :
cd /srv/partage
echo "je suis en AIS chez Simplon_Campus_Distanciel" > ais.txt
se placer dans un dossier local sécurisé pour la clé privée :
mkdir -p ~/keys
chmod 700 ~/keys
Générer la clé privée
openssl genpkey -algorithm RSA -out private.pem -pkeyopt rsa_keygen_bit
s:2048
puis sur le dossier partagé :
Générer la clé publique à partir de la clé privée
openssl rsa -pubout -in private.pem -out public.pem
Échanger la clé publique
Copie public.pem dans le dossier partagé (déjà fait automatiquement si tu es
dans /srv/partage )
La Machine 2 pourra maintenant chiffrer un fichier pour toi
Chiffrer le fichier avec la clé publique
Sur la Machine 2
openssl pkeyutl -decrypt -inkey ~/keys/private.pem -in ais_encrypted.bin -
chiffrement asymétrique 1
out ais_decrypted.txt
Le fichier ais_encrypted.bin est chiffré
Il peut être envoyé à la Machine 1 pour déchiffrement
Déchiffrer avec la clé privée
Sur la Machine 1 :
openssl pkeyutl -decrypt -inkey private.pem -in ais_encrypted.bin -out ais_
decrypted.txt
Vérifie le contenu :
cat ais_decrypted.txt
Modifier le fichier chiffré et tester la détection d’erreur
hexedit ais_encrypted.bin
Modifie quelques octets → sauvegarde et quitte
Puis tente de déchiffrer avec la clé privée
Tu devrais avoir une erreur ou du texte corrompu, preuve de la détection
de modification.

---
</details>
  
<details><summary><h2>Hachage</h2></summary>  

Sur la machine 1 :
echo "je suis chez moi mais j’apprends mieux" > simplon.txt
cat simplon.txt
Calculer le hash avec SHA256 (via OpenSSL)
openssl dgst -sha256 simplon.txt
openssl dgst → permet de générer un digest (empreinte)
sha256 → choisit l’algorithme SHA-256
Calculer le hash avec SHA512
openssl dgst -sha512 simplon.txt
Vérification de l’intégrité
Modifie le fichier (ajoute un mot par exemple) :
echo "!" >> simplon.txt
Recalcule SHA256 ou SHA512 :
openssl dgst -sha256 simplon.txt
Optionnel : sauvegarder le hash dans un fichier
openssl dgst -sha256 simplon.txt > simplon_sha256.txt

---
</details>

</details>
