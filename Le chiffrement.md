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

```bash
sudo apt update
sudo apt install -y openssl hexedit
```

**Préparer le fichier**

Sur machine A (ou les deux pour tester) :

```bash
mkdir -p ~/crypto_tests
cd ~/crypto_tests
echo "je suis en AIS chez Simplon" > ais.txt
cat ais.txt
```

**Chiffrer avec AES-256-CBC**

```bash
openssl enc -aes-256-cbc -salt -in ais.txt -out ais_aes256cbc.enc
```

**Chiffrer avec ChaCha20**

```bash
openssl enc -chacha20 -in ais.txt -out ais_chacha20.enc
```

> 💡 Les méthodes de chiffrement **GCM** et **Blowfish** sont obsolètes et ne seront pas utilisées dans cette procédure.

**Comparer la taille des fichiers chiffrés**

```bash
ls -lh ais*
```

<p align="center">
<img src="https://github.com/user-attachments/assets/f38abd03-4453-4211-b211-167375fdf4ac" alt="Pictures" width="600" >
</p>

**Modifier un fichier chiffré avec hexedit**

```bash
hexedit ais_aes256cbc.enc
```

> Dans `hexedit` : navigue avec les flèches, change un octet (ex : tape `00` → `FF`), puis sauvegarde avec **Ctrl+X**, **Y**.

**Déchiffrer pour tester la détection d’erreur**

```bash
openssl enc -d -aes-256-cbc -in ais_aes256cbc.enc -out test_dechiffre.txt
```

<p align="center">
<img src="https://github.com/user-attachments/assets/faaafd38-0ead-4900-99f8-a178f4c78166" alt="Pictures" width="700" >
</p>

**Déchiffrer tous les fichiers et comparer**

```bash
# AES
openssl enc -d -aes-256-cbc -in ais_aes256cbc.enc -out dec_aes256cbc.txt

# Chacha20
openssl enc -d -chacha20 -in ais_chacha20.enc -out dec_chacha20.txt
```

---
</details>

<details><summary><h2>Chiffrement asymétrique</h2></summary>

**Créer le fichier ais.txt (sur la machine A)**

```bash
cd /srv/partage
echo "je suis en AIS chez Simplon_Campus_Distanciel" > ais.txt
```

**Générer la clé privée (private.pem) — sur A**

Se placer dans un dossier local sécurisé :

```bash
mkdir -p ~/keys
chmod 700 ~/keys
openssl genpkey -algorithm RSA -out ~/private.pem -pkeyopt rsa_keygen_bits:2048
chmod 600 ~/private.pem
```

**Générer la clé publique depuis la privée (public.pem) — sur A**

```bash
openssl rsa -pubout -in private.pem -out public.pem
```

Échanger la clé publique :

```bash
cp ~/public.pem ~/shared_from_A/
# ou
scp ~/public.pem user@B_host:~/
```

**Chiffrer le fichier avec la clé publique**

Sur la Machine B :

```bash
openssl pkeyutl -encrypt -pubin -inkey public.pem -in ~/ais.txt -out ~/ais.enc
```

**Déchiffrer avec la clé privée (sur A, qui a private.pem)**

```bash
openssl pkeyutl -decrypt -inkey ~/private.pem -in ~/ais.txt.enc -out ~/ais_decrypted.txt
cat ais_decrypted.txt
```

**Modifier le fichier chiffré à l’aide de hexedit**

```bash
hexedit ais.enc
```

> Change un octet (ex: `00` → `FF`), sauvegarde (**Ctrl+X**, **Y**).

Puis tente de déchiffrer avec la clé privée :

```bash
openssl pkeyutl -decrypt -inkey ~/private.pem -in ~/ais.txt.enc -out ~/ais_decrypted_after_edit.txt 2>err.txt || true
cat err.txt
```

---
</details>
  
<details><summary><h2>Hachage</h2></summary>  

Sur la machine A :

```bash
echo "je suis chez moi mais j’apprends mieux" > simplon.txt
cat simplon.txt
```

**Calculer un hash SHA256 (avec l’utilitaire système)**

```bash
sha256sum ~/simplon.txt
```

<p align="center">
<img src="https://github.com/user-attachments/assets/c2f0e181-30f3-497f-81f2-6ef02922aacb" alt="Pictures" width="700" >
</p>

**Calculer un hash SHA256 avec openssl**

```bash
openssl dgst -sha256 simplon.txt
```

<p align="center">
<img src="https://github.com/user-attachments/assets/5fac4146-e07a-4ec6-86af-8d9b5523cc85" alt="Pictures" width="700" >
</p>

- openssl dgst → permet de générer un digest (empreinte)
- sha256 → choisit l’algorithme SHA-256

**Calculer un hash SHA512**

```bash
sha512sum simplon.txt
# ou
openssl dgst -sha512 simplon.txt
```

<p align="center">
<img src="https://github.com/user-attachments/assets/ed281562-cf0e-4b0a-baf2-2816a071b267" alt="Pictures" width="700" >
</p>

**Vérification de l’intégrité**
Modifie le fichier (ajoute un mot par exemple) :

```bash
echo "!" >> simplon.txt
# Recalculer SHA256 ou SHA512 :
sha256sum simplon.txt
```

- Comparer au hash initial — s'il est différent, l'intégrité est rompue
  
Avant modification :
<p align="center">
<img src="https://github.com/user-attachments/assets/c2f0e181-30f3-497f-81f2-6ef02922aacb" alt="Pictures" width="700" >
</p>

Après modification :
<p align="center">
<img src="https://github.com/user-attachments/assets/10f3c011-b9de-494a-aeac-ef5d71b23c39" alt="Pictures" width="700" >
</p>

**Créer une empreinte dans un fichier (fichier de signature/empreinte)**

Créer fichier contenant le hash (format attendu par sha256sum -c)

```bash
sha256sum simplon.txt > simplon.sha256
cat simplon.sha256
```

<p align="center">
<img src="https://github.com/user-attachments/assets/7f4b589e-074f-4359-855a-c7800f55e0df" alt="Pictures" width="700" >
</p>


**Vérifier l’empreinte (vérification automatique)**

```bash
sha256sum -c ~/simplon.txt.sha256
```

> Sortie attendue : **OK** ou **Réussi**

<p align="center">
<img src="https://github.com/user-attachments/assets/6c8c5213-51b5-48fd-94ed-8655581f41fe" alt="Pictures" width="600" >
</p>


---
</details>

</details>
