# Cahier de Bord Numérique – Android

Projet Android natif (conteneur WebView) basé sur le fichier HTML fourni. L'interface et les fonctionnalités métier du HTML sont conservées. L'icône de l'application reprend l'image fournie.

## Compatibilité
- Android 6.0 (API 23) minimum.
- Cible Android 15 (API 35).
- Le fonctionnement dépend aussi de la version du composant Android System WebView installé sur le téléphone.
- L'import de PDF utilise le sélecteur de fichiers Android. Les fonctions d'impression/PDF passent par le service d'impression Android.

Il n'existe pas d'APK unique garantissant le fonctionnement sur absolument toutes les versions Android. Cette configuration vise Android 6.0 et versions ultérieures.

## Construire depuis GitHub (recommandé)
1. Décompresser le ZIP.
2. Créer un dépôt GitHub et y téléverser tous les fichiers du dossier.
3. Ouvrir l'onglet **Actions**, puis lancer **Android Release** avec **Run workflow**.
4. Télécharger l'artefact `Cahier-de-Bord-Numerique-APK` à la fin de l'exécution.

Pour une publication installable et distribuable, configurez une clé de signature (voir ci-dessous). Sans clé, Gradle produit un APK release non signé, utile pour vérifier la compilation mais pas pour une distribution finale.

## Signature release GitHub
Créer une clé privée une seule fois, dans un environnement sécurisé :

```bash
keytool -genkeypair -v -keystore cahier-release.jks -alias cahier -keyalg RSA -keysize 2048 -validity 10000
base64 -w 0 cahier-release.jks
```

Dans GitHub : **Settings → Secrets and variables → Actions → New repository secret**, ajouter :
- `ANDROID_KEYSTORE_BASE64` : résultat de la commande base64
- `ANDROID_KEYSTORE_PASSWORD` : mot de passe du keystore
- `ANDROID_KEY_ALIAS` : `cahier` (ou l'alias choisi)
- `ANDROID_KEY_PASSWORD` : mot de passe de la clé

Ne jamais publier le fichier `.jks`, ni les mots de passe. Relancer ensuite le workflow. L'APK Release généré sera signé.

## Construire avec Termux
La construction Android complète sur téléphone nécessite Java 17, Gradle et un Android SDK correctement installé/configuré. Ce n'est pas toujours simple sur Termux. La méthode GitHub Actions ci-dessus est la méthode conseillée depuis Android.

Si un SDK Android est déjà configuré :
```bash
pkg update
pkg install openjdk-17 gradle
gradle :app:assembleRelease
```
Le fichier sera créé dans `app/build/outputs/apk/release/`.

## Ouvrir dans Android Studio
Ouvrir le dossier décompressé comme projet Gradle. Installer Android SDK Platform 35 et JDK 17, puis lancer **Build → Generate Signed Bundle / APK** ou `:app:assembleRelease`.

## Données et précautions
- Les données de l'application sont conservées dans le stockage local WebView du téléphone. Elles ne sont pas automatiquement synchronisées entre appareils.
- Désinstaller l'application ou effacer ses données peut supprimer les données locales. Faire des sauvegardes régulières depuis les fonctions disponibles dans l'application.
- Le PDF.js du HTML source est chargé depuis cdnjs : une connexion Internet est nécessaire pour cette fonction d'import PDF.
- L'identifiant Android est `bf.quantumzacks.cahierdebord`. Le nom affiché est « Cahier de Bord Numérique ».
