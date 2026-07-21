---
title: Installation de l’extension [!DNL Adobe Launch]
feature: Mobile Marketing
description: Installez l’extension Adobe Launch Marketo pour mobile. Suivez les étapes de configuration d’iOS et d’Android, de test des appareils, des autorisations et de FCM pour les notifications push et in-app.
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
TQID: https://experienceleague.adobe.com/UZRHaRBISIZsE6E25Ee7CnnYwyZwi6w2YgOQJ-JL00U
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 753
ht-degree: 0%

---

# Installation de l’extension [!DNL Adobe Launch]

Installez l’extension [!DNL Adobe Launch] Marketo pour envoyer des notifications push, des messages in-app ou les deux.

## Conditions préalables

1. [Ajoutez une application dans Marketo Admin](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) et obtenez la clé secrète de l’application et l’ID Munchkin.
1. [Configurez la propriété dans le [!DNL Adobe Launch] portail](https://experience.adobe.com/#/@amc/data-collection/home).
1. Configurez la clé secrète de l’application et l’ID Munchkin pour la propriété dans le portail [!DNL Adobe Launch].
1. Facultatif : [Configurer des notifications push](push-notifications.md).

## Installation de l’extension Marketo sur iOS

### Configurer l&#39;en-tête de pontage Swift

1. Accédez à [!UICONTROL Fichier] > [!UICONTROL Nouveau] > [!UICONTROL Fichier] et sélectionnez **[!UICONTROL Fichier d’en-tête]**.

1. Nommez le fichier « &lt;_ProjectName_>-Bridging-Header ».

1. Accédez à [!UICONTROL Projet] > [!UICONTROL Cible] > [!UICONTROL Paramètres de build] > [!UICONTROL Compilateur Swift] > [!UICONTROL Génération de code].
1. Ajoutez le chemin suivant à l’en-tête « Pontage d’objectifs » :

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Initialiser l’extension

>[!BEGINTABS]

>[!TAB Objectif C]

Mettez à jour la méthode `applicationDidBecomeActive` comme suit.

```objectivec
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Mettez à jour la méthode `applicationDidBecomeActive` comme suit.

```objectivec
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## Appareils de test iOS

1. Sélectionnez **[!UICONTROL Projet]** > **[!UICONTROL Cible]** > **[!UICONTROL Infos]** > **[!UICONTROL Types d’URL]**.
1. Ajoutez l&#39;identifiant ${PRODUCT_NAME}.
1. Définissez les schémas d’URL sur mkto-&lt;S_secret Key_>.
1. Ajoutez des `application:openURL:sourceApplication:annotation:` à `AppDelegate.m file` pour l’objectif C.

### Gérer le type d’URL personnalisé dans AppDelegate

>[!BEGINTABS]

>[!TAB Objectif C]

```objectivec
#ifdef __IPHONE_10_0
-(BOOL)application:(UIApplication *)application
           openURL:(NSURL *)url
           options:(NSDictionary *)options{
    return [[ALMarketo sharedInstance] application:application
                                         openURL:url
                               sourceApplication:nil
                                      annotation:nil];
}
#endif

- (BOOL)application:(UIApplication *)application
            openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication
         annotation:(id)annotation {
    return [[ALMarketo sharedInstance] application:application
                                         openURL:url
                               sourceApplication:nil
                                      annotation:nil];
}
```

>[!TAB Swift]

```objectivec
func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    return ALMarketo.sharedInstance().application(application, open: url, sourceApplication: nil, annotation: nil)
}
```

>[!ENDTABS]

## Installation de Marketo SDK sur Android

### Configuration de l’extension Android

Suivez les instructions du portail [!DNL Adobe Launch].

### Configurer les autorisations

Ouvrez `AndroidManifest.xml` et ajoutez les autorisations suivantes. Votre application doit demander les autorisations « INTERNET » et « ACCESS_NETWORK_STATE ». Ignorez cette étape si l’application le demande déjà.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Initialiser l’extension

Configuration de ProGuard (en option)

Si votre application utilise ProGuard, ajoutez les lignes suivantes au fichier `proguard.cfg` dans le dossier `project`. Cette configuration exclut le SDK Marketo de l’obscurcissement.

```text
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Appareils de test Android

Ajoutez « MarketoActivity » à `AndroidManifest.xml` dans la balise de l’application.

```xml
<activity android:name="com.marketo.MarketoActivity"  android:configChanges="orientation|screenSize" >
    <intent-filter android:label="MarketoActivity" >
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto" />
    </intent-filter>
</activity>
```

## Prise en charge de Firebase Cloud Messaging

MME SDK pour Android prend en charge l’utilisation directe de Google [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM).

### Ajout de FCM à votre application

1. Intégrez la dernière version de Marketo Android SDK dans l’application Android. Voir les étapes sur [GitHub](https://github.com/Marketo/android-sdk).
1. Configurez l’application Firebase dans la console Firebase.
   1. Créez ou ajoutez un projet dans [&#128279;](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)Firebase Console).
      1. Dans la [console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), sélectionnez **[!UICONTROL Ajouter un projet]**.
      1. Sélectionnez votre projet GCM dans la liste des projets Google Cloud existants, puis sélectionnez **[!UICONTROL Ajouter Firebase]**.
      1. Dans l’écran d’accueil de Firebase, sélectionnez **[!UICONTROL Ajouter Firebase à l’application Android]**.
      1. Indiquez le nom de votre package et SHA-1, puis sélectionnez **[!UICONTROL Ajouter une application]**. Un nouveau fichier `google-services.json` pour votre application Firebase est téléchargé.
      1. Sélectionnez **[!UICONTROL Continuer]** et suivez les instructions détaillées pour ajouter le plug-in Google Services dans Android Studio.

   1. Accédez à **[!UICONTROL Paramètres du projet]** dans [!UICONTROL Présentation du projet].
      1. Sélectionnez l’onglet **[!UICONTROL Général]** et téléchargez `google-services.json`.
      1. Sélectionnez l’onglet **[!UICONTROL Cloud Messaging]**. Copiez les [!UICONTROL clé du serveur] et [!UICONTROL ID de l’expéditeur] et fournissez-les à Marketo.
   1. Configurez FCM dans l’application Android.
      1. Basculez vers la vue Projet dans Android Studio pour afficher le répertoire racine du projet.
         1. Déplacez le fichier `google-services.json` téléchargé dans le répertoire racine du module d’application Android.
         1. Dans la `build.gradle` au niveau du projet, ajoutez ce qui suit :

            ```
            buildscript {
              dependencies {
                classpath 'com.google.gms:google-services:4.0.0'
              }
            }
            ```

         1. Dans le fichier build.gradle de niveau application, ajoutez ce qui suit :

            ```
            dependencies {
              compile 'com.google.firebase:firebase-core:17.4.0'
            }
            // Add to the bottom of the file
            apply plugin: 'com.google.gms.google-services'
            ```

         1. Sélectionnez **[!UICONTROL Synchroniser maintenant]** dans la barre qui s’affiche dans l’IDE.
   1. Modifiez le manifeste de l’application. Le SDK FCM ajoute automatiquement les autorisations requises et la fonctionnalité de récepteur. Supprimez les éléments obsolètes suivants, qui peuvent entraîner la duplication des messages :

      ```xml
      <uses-permission android:name="android.permission.WAKE_LOCK" />
      <permission android:name="<your-package-name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
      <uses-permission android:name="<your-package-name>.permission.C2D_MESSAGE" />
      
      ...
      
      <receiver>
        android:name="com.google.android.gms.gcm.GcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
          <action android:name="com.google.android.c2dm.intent.RECEIVE" />
          <category android:name="<your-package-name> />
        </intent-filter>
      </receiver>
      ```

### FAQ FCM

Ces questions portent sur la prise en charge de Firebase Cloud Messaging.

**Q : Où puis-je trouver des instructions pour mettre à jour vers la dernière version de MME SDK ?** Voir les [instructions d’installation](installation.md) sur le site du développeur de Marketo.

**Q : La mise à jour vers la dernière version de SDK nécessitera-t-elle que je publie une version mise à jour de mon application Android pour mes utilisateurs actuels ?** Non.

**Q : Comment cela affecte-t-il les clients MME existants avec les applications Android publiées qui utilisent Marketo Android SDK ?** Migrez une application cliente Android GCM existante vers Firebase Cloud Messaging (FCM) comme suit :

1. Dans la [console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), sélectionnez **[!UICONTROL Ajouter un projet]**.
1. Sélectionnez votre projet GCM dans la liste des projets Google Cloud existants, puis sélectionnez **[!UICONTROL Ajouter Firebase]**.
1. Dans l’écran d’accueil de Firebase, sélectionnez **[!UICONTROL Ajouter Firebase à l’application Android]**.
1. Indiquez le nom de votre package et SHA-1, puis sélectionnez **[!UICONTROL Ajouter une application]**. Un nouveau fichier google-services.json pour votre application Firebase est téléchargé.
1. Sélectionnez **[!UICONTROL Continuer]** et suivez les instructions détaillées pour ajouter le plug-in Google Services dans Android Studio.

**Q : Pouvons-nous cibler les prospects créés avec l’ancienne SDK Marketo qui utilisait une application GCM ?** Oui. Vous pouvez cibler tous les prospects créés avec Marketo SDK pour les notifications push.
