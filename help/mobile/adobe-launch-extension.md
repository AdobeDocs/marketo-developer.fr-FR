---
title: "Extension mobile Marketo pour [!DNL Adobe Launch]"
feature: Mobile Marketing
description: "Extension mobile Marketo pour [!DNL Adobe Launch] overview"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---


# Extension mobile Marketo pour [!DNL Adobe Launch]

Instructions d’installation du SDK Mobile Marketo dans [!DNL Adobe Launch]. Les étapes ci-dessous sont requises pour envoyer des notifications push et/ou des messages In-App.

## Conditions préalables

- [Ajout d’une application dans Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (Obtenez votre clé secrète et votre ID Munchkin)
- Suivez les instructions de la section [!DNL Adobe Launch] Portal pour l’installation
- [Configuration des notifications push](push-notifications.md) (facultatif)

## iOS

### Configuration de l’en-tête de liaison Swift

1. Accédez à Fichier > Nouveau > Fichier et sélectionnez &quot;Fichier d’en-tête&quot;.
1. Nommez le fichier &quot;&lt;_ProjectName_>-Bridging-Header&quot;.
1. Accédez à Projet > Cible > Créer des phases > Compilateur Swift > Génération de code. Ajoutez le chemin suivant à l’en-tête de liaison d’objectif :

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Pour les utilisateurs Swift : supprimez l’instruction d’importation suivante, car l’en-tête de liaison est ajouté aux étapes ci-dessus.

`import Marketo/ALMarketo`

### Appareils de test iOS

Suivez les instructions de la section [Ajout de périphériques de test iOS](installation.md#ios_test_devices)

### Gestion du type d’URL personnalisé dans AppDelegate

Suivez les instructions [here](installation.md#ios_test_devices)

### Configuration des notifications push sur iOS

Suivez les instructions [here](push-notifications.md) et utilisez le nom de classe &quot;ALMarketo&quot; au lieu de &quot;Marketo&quot;.

## Android

### Configuration des autorisations

Ouvrir `AndroidManifest.xml` et ajoutez les autorisations suivantes. Votre application doit demander les autorisations &quot;INTERNET&quot; et &quot;ACCESS_NETWORK_STATE&quot;. Si votre application demande déjà ces autorisations, ignorez cette étape.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Configuration de ProGuard (facultatif)

Si vous utilisez ProGuard pour votre application, ajoutez les lignes suivantes dans votre `proguard.cfg` fichier . Le fichier se trouve dans le dossier de votre projet. L’ajout de ce code exclut le SDK Marketo du processus d’obscurcissement.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Appareils de test Android

Suivez les instructions [here](installation.md#android_test_devices)

## Configuration des notifications push sur Android

Suivez les instructions [here](installation.md#android_firebase_cloud_messaging_support) et utilisez le nom de classe &quot;ALMarketo&quot; au lieu de &quot;Marketo&quot;.

Pour configurer des profils utilisateur, suivez les instructions. [here](user-profiles.md) et pour les actions personnalisées suivez les instructions [here](custom-actions.md#android_custom_action). Dans les instructions suivantes, utilisez le nom de classe &quot;ALMarketo&quot; au lieu de &quot;Marketo&quot;.
