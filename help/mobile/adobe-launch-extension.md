---
title: Extension mobile Marketo pour  [!DNL Adobe Launch]
feature: Mobile Marketing
description: Installez et configurez l’extension Marketo Mobile SDK dans Adobe Launch pour iOS et Android, y compris la configuration pour les notifications push et les messages in-app.
exl-id: 2f8691ff-0442-45a5-aeba-c91c3af5c711
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---

# Extension Marketo Mobile pour [!DNL Adobe Launch]

Instructions d’installation pour l’extension Marketo Mobile SDK dans [!DNL Adobe Launch]. Les étapes ci-dessous sont nécessaires pour envoyer des notifications push et/ou des messages In-App.

## Conditions préalables

- [Ajout d’une application dans Marketo Admin](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtention de la clé secrète de l’application et de l’ID Munchkin)
- Suivez les instructions fournies dans le portail [!DNL Adobe Launch] pour l’installation
- [Configurer des notifications push](push-notifications.md) (facultatif)

## iOS

### Configurer l&#39;en-tête de pontage Swift

1. Accédez à Fichier > Nouveau > Fichier et sélectionnez « Fichier d’en-tête ».
1. Nommez le fichier « &lt;_ProjectName_>-Bridging-Header ».
1. Accédez à Projet > Cible > Phases de création > Compilateur Swift > Génération de code. Ajoutez le chemin suivant à l’en-tête de pontage d’objectifs :

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Pour les utilisateurs de Swift : supprimez l&#39;instruction d&#39;importation suivante, car l&#39;en-tête de pontage est ajouté dans les étapes ci-dessus.

`import Marketo/ALMarketo`

### Appareils de test iOS

Suivez les instructions de la section [ Ajout d’appareils de test iOS ](installation.md#ios_test_devices)

### Gérer le type d’URL personnalisé dans AppDelegate

Suivez les instructions [ici](installation.md#ios_test_devices)

### Configuration des notifications push sur iOS

Suivez les instructions [ici](push-notifications.md) et utilisez le nom de classe « ALMarketo » au lieu de « Marketo »

## Android

### Configurer les autorisations

Ouvrez `AndroidManifest.xml` et ajoutez les autorisations suivantes. Votre application doit demander les autorisations « INTERNET » et « ACCESS_NETWORK_STATE ». Si votre application demande déjà ces autorisations, ignorez cette étape.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Configuration de ProGuard (en option)

Si vous utilisez ProGuard pour votre application, ajoutez les lignes suivantes dans votre fichier `proguard.cfg`. Le fichier se trouve dans le dossier du projet. L’ajout de ce code exclut le SDK Marketo du processus d’obscurcissement.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Appareils de test Android

Suivez les instructions [ici](installation.md#android_test_devices)

## Configuration des notifications push sur Android

Suivez les instructions [ici](installation.md#android_firebase_cloud_messaging_support) et utilisez le nom de classe « ALMarketo » au lieu de « Marketo »

Pour configurer des profils utilisateur, suivez les instructions [ici](user-profiles.md) et pour les actions personnalisées, suivez les instructions [ici](custom-actions.md#android_custom_action). Dans les instructions suivantes, utilisez le nom de classe « ALMarketo » au lieu de « Marketo »
