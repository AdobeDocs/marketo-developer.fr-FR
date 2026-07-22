---
title: Extension mobile Marketo pour  [!DNL Adobe Launch]
feature: Mobile Marketing
description: Installez et configurez l’extension Marketo Mobile SDK dans Adobe Launch pour iOS et Android, y compris la configuration pour les notifications push et les messages in-app.
exl-id: 2f8691ff-0442-45a5-aeba-c91c3af5c711
TQID: https://experienceleague.adobe.com/Bk5GTnQjm6NDosl5Iw6TS-NRjH8owNRUKoE0mZ-H3pY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 303
ht-degree: 0%

---

# Extension Marketo Mobile pour [!DNL Adobe Launch]

Installez l’extension Marketo Mobile SDK dans [!DNL Adobe Launch] pour envoyer des notifications push, des messages in-app ou les deux.

## Conditions préalables

- [Ajoutez une application dans Marketo Admin](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) et obtenez la clé secrète de l’application et l’ID Munchkin.
- Suivez les instructions d’installation fournies dans le portail [!DNL Adobe Launch].
- Facultatif : [Configurer des notifications push](push-notifications.md).

## iOS

### Configurer l&#39;en-tête de pontage Swift

1. Accédez à Fichier > Nouveau > Fichier et sélectionnez « Fichier d’en-tête ».
1. Nommez le fichier « &lt;_ProjectName_>-Bridging-Header ».
1. Accédez à Projet > Cible > Phases de création > Compilateur Swift > Génération de code.
1. Ajoutez le chemin suivant à l’en-tête de pontage d’objectifs :

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Pour Swift, supprimez l&#39;instruction d&#39;importation suivante car les étapes précédentes ajoutent l&#39;en-tête de pontage.

`import Marketo/ALMarketo`

### Appareils de test iOS

Suivez les instructions de la section [&#x200B; Ajout d’appareils de test iOS &#x200B;](installation.md#ios_test_devices).

### Gérer le type d’URL personnalisé dans AppDelegate

Suivez les [instructions d’URL personnalisées](installation.md#ios_test_devices).

### Configuration des notifications push sur iOS

Suivez les [instructions relatives aux notifications push](push-notifications.md). Utilisez le nom de classe « ALMarketo » au lieu de « Marketo ».

## Android

### Configurer les autorisations

Ouvrez `AndroidManifest.xml` et ajoutez les autorisations suivantes. Votre application doit demander les autorisations « INTERNET » et « ACCESS_NETWORK_STATE ». Ignorez cette étape si l’application le demande déjà.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Configuration de ProGuard (en option)

Si votre application utilise ProGuard, ajoutez les lignes suivantes au fichier `proguard.cfg` dans le dossier du projet. Cette configuration exclut le SDK Marketo de l’obscurcissement.

```text
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Appareils de test Android

Suivez les instructions de la section [Appareils de test &#x200B;](installation.md#android_test_devices).

## Configuration des notifications push sur Android

Suivez les [instructions relatives à Android Firebase Cloud Messaging](installation.md#android_firebase_cloud_messaging_support). Utilisez le nom de classe « ALMarketo » au lieu de « Marketo ».

Pour configurer des profils utilisateur, suivez les [&#x200B; instructions relatives aux profils utilisateur &#x200B;](user-profiles.md). Pour configurer des actions personnalisées, suivez les [instructions relatives aux actions personnalisées](custom-actions.md#android_custom_action). Dans les deux ensembles d’instructions, utilisez le nom de classe « ALMarketo » au lieu de « Marketo ».
