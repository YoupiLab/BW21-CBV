# 📸 ESP32 Camera → Google Drive Uploader

Ce projet permet de capturer des images avec une caméra embarquée (type ESP32/AMB82) et de les envoyer automatiquement vers Google Drive via un script Google Apps Script.

---

## 🚀 Fonctionnalités

* Connexion WiFi automatique
* Capture d’image depuis la caméra
* Encodage en Base64
* Upload sécurisé via HTTPS
* Horodatage automatique des fichiers
* Envoi vers un dossier Google Drive spécifique
* Capture en boucle (mode surveillance)

---

## 🧰 Technologies utilisées

* ESP32 / AMB82 SDK
* WiFi (WiFi.h)
* NTPClient (horodatage)
* WiFiUDP
* Base64 encoding
* Google Apps Script (backend upload)
* HTTPS via `WiFiSSLClient`

---

## ⚙️ Configuration

### 1. WiFi

```cpp
char ssid[] = "YOUR_WIFI";
const char password[] = "YOUR_PASSWORD";
```

---

### 2. Google Apps Script

Remplace le chemin du script :

```cpp
String myScript = "/macros/s/YOUR_SCRIPT_ID/exec";
```

Configurer également :

```cpp
String myFoldername = "&myFoldername=YOUR_FOLDER_NAME";
```

---

### 3. Paramètres caméra

```cpp
VideoSetting config(768, 768, CAM_FPS, VIDEO_JPEG, 1);
```

* Résolution : 768x768
* Format : JPEG
* FPS : dépend de `CAM_FPS`

---

## 📂 Structure du projet

```
.
├── main.ino
├── README.md
```

---

## 🔄 Fonctionnement global

1. Connexion au WiFi
2. Initialisation caméra
3. Capture d'image
4. Encodage Base64
5. Génération timestamp
6. Envoi vers Google Drive via POST HTTPS
7. Répétition en boucle

---

## 🧠 Détails techniques

### 📸 Capture d'image

```cpp
Camera.getImage(0, &img_addr, &img_len);
```

* `img_addr` : adresse mémoire de l'image
* `img_len` : taille en bytes

---

### 🔐 Encodage Base64

L'image est convertie en string Base64 puis encodée en URL :

```cpp
base64_encode(output, (input++), 3);
```

---

### 🌐 Upload HTTP POST

```http
POST /macros/s/.../exec HTTP/1.1
Host: script.google.com
Content-Type: application/x-www-form-urlencoded
```

Payload :

```
myFoldername=XXX&myFilename=TIMESTAMP.jpg&myFile=BASE64_DATA
```

---

### 🕒 Horodatage

Format :

```
YYYYMMDD_HHMMSS.jpg
```

Exemple :

```
20260501_153045.jpg
```

---

## 🔁 Boucle principale

```cpp
void loop()
{
    Camera.getImage(0, &img_addr, &img_len);
    uploadToGoogleDrive();
    delay(500);
}
```

➡️ Capture toutes les 500 ms

---

## ⚠️ Limitations / Points d’attention

* ⚡ Consommation mémoire élevée (Base64)
* 📡 Dépend fortement de la stabilité WiFi
* ⏱️ Latence possible côté Google Script
* 🔐 Pas de gestion avancée des erreurs HTTP
* 📷 Capture continue → charge CPU + réseau

---

## 🛠️ Améliorations possibles

* Ajouter un buffer intelligent
* Réduire la fréquence de capture
* Ajouter détection de mouvement
* Compression d’image
* Retry en cas d’échec
* Authentification sécurisée (token)

---

## 📌 Prérequis

* Carte ESP32 / AMB82 compatible caméra
* Caméra fonctionnelle
* Compte Google + Google Apps Script configuré
* Connexion WiFi stable

---

## 📄 Licence

Libre d’utilisation pour projets éducatifs et expérimentaux.

---

## 👨‍💻 Auteur

Projet embarqué IoT orienté surveillance et automatisation cloud.

---

Si tu veux, je peux aussi te générer :

* le script Google Apps Script complet ✅
* une version optimisée (moins de RAM)
* ou une architecture pro (queue + batch upload)
