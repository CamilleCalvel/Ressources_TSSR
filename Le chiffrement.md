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
<img width="574" height="108" alt="Capture d&#39;écran 2025-11-05 160734" src="https://github.com/user-attachments/assets/f38abd03-4453-4211-b211-167375fdf4ac" />


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

**Créer le fichier ais.txt (sur la machine A)**
cd /srv/partage
echo "je suis en AIS chez Simplon_Campus_Distanciel" > ais.txt

**Générer la clé privée (private.pem) — sur A**
Se placer dans un dossier local sécurisé pour la clé privée :
mkdir -p ~/keys
chmod 700 ~/keys
Générer la clé privée
2048 bits (plus sûre que 1024)
openssl genpkey -algorithm RSA -out ~/private.pem -pkeyopt rsa_keygen_bits:2048
protéger la clé privée
chmod 600 ~/private.pem

**Générer la clé publique depuis la privée (public.pem) — sur A**

openssl rsa -pubout -in private.pem -out public.pem
Échanger la clé publique
Copie public.pem dans le dossier partagé (déjà fait automatiquement si tu es
dans /srv/partage ) 
cp ~/public.pem ~/shared_from_A/

Ou par scp :
scp ~/public.pem user@B_host:~/

La Machine 2 pourra maintenant chiffrer un fichier pour toi

**Chiffrer le fichier avec la clé publique**

Sur la Machine 2
openssl pkeyutl -encrypt -pubin -inkey public.pem -in ~/ais.txt -out ~/ais.enc

Il peut être envoyé à la Machine A pour déchiffrement

**Déchiffrer avec la clé privée (sur A, qui a private.pem)**

openssl pkeyutl -decrypt -inkey ~/private.pem -in ~/ais.txt.enc -out ~/ais_decrypted.txt

Vérifie le contenu :
cat ais_decrypted.txt

**Modifier le fichier chiffré à l’aide de hexedit**
hexedit ais.enc

Modifie quelques octets → sauvegarde et quitte
> Dans hexedit, change 1 octet (ex: remplace un 00 par FF) ; quitter en sauvegardant (Ctrl+X puis y)
Puis tente de déchiffrer avec la clé privée
openssl pkeyutl -decrypt -inkey ~/private.pem -in ~/ais.txt.enc -out ~/ais_decrypted_after_edit.txt 2>err.txt || true
afficher un éventuel message d'erreur
cat err.txt

---
</details>
  
<details><summary><h2>Hachage</h2></summary>  

Sur la machine 1 :
echo "je suis chez moi mais j’apprends mieux" > simplon.txt
cat simplon.txt

**Calculer un hash SHA256 (avec l’utilitaire système)**
sha256sum ~/simplon.txt

**Calculer un hash SHA256 avec openssl**

openssl dgst -sha256 simplon.txt

openssl dgst → permet de générer un digest (empreinte)
sha256 → choisit l’algorithme SHA-256

**Calculer un hash SHA512**

sha512sum simplon.txt
ou avec openssl
openssl dgst -sha512 simplon.txt

**Vérification de l’intégrité**
Modifie le fichier (ajoute un mot par exemple) :
echo "!" >> simplon.txt

Recalcule SHA256 ou SHA512 :
sha256sum simplon.txt

Comparer au hash initial — s'il est différent, l'intégrité est rompue

**Créer une empreinte dans un fichier (fichier de signature/empreinte)**

Créer fichier contenant le hash (format attendu par sha256sum -c)
sha256sum simplon.txt > simplon.sha256

Le fichier contiendra: <hash>
cat simplon.sha256

**Vérifier l’empreinte (vérification automatique)**

sha256sum -c ~/simplon.txt.sha256
> sortie attendue: OK ou Réussi

<img width="678" height="49" alt="Capture d&#39;écran 2025-11-06 002309" src="https://github.com/user-attachments/assets/6c8c5213-51b5-48fd-94ed-8655581f41fe" />
<img width="774" height="42" alt="Sans titre" src="https://github.com/user-attachments/assets/4ee6a633-6799-4b0f-8c9a-bc4996f113cc" />

---
</details>

</details>
