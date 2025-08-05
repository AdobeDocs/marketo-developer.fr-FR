---
title: Installation de l’extension [!DNL Adobe Launch]
feature: Mobile Marketing
description: Présentation de l’installation de l’extension [!DNL Adobe Launch]
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '725'
ht-degree: 0%

---

# Installation de l’extension [!DNL Adobe Launch]

Instructions d’installation pour [!DNL Adobe Launch] extension Marketo. Les étapes ci-dessous sont nécessaires pour envoyer des notifications push et/ou des messages In-App.

## Conditions préalables

1. [Ajout d’une application dans Marketo Admin](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtention de la clé secrète de l’application et de l’ID Munchkin)
1. [Configurer la propriété dans  [!DNL Adobe Launch]  portail](https://experience.adobe.com/#/@amc/data-collection/home)
1. Configurez la clé secrète de l’application et l’ID Munchkin pour la propriété dans le portail [!DNL Adobe Launch]
1. [Configurer des notifications push](push-notifications.md) (facultatif)

## Installation de l’extension Marketo sur iOS

### Configurer l&#39;en-tête de pontage Swift

1. Accédez à [!UICONTROL Fichier] > [!UICONTROL Nouveau] > [!UICONTROL Fichier] et sélectionnez **[!UICONTROL Fichier d’en-tête]**.

1. Nommez le fichier « &lt;_ProjectName_>-Bridging-Header ».

1. Accédez à [!UICONTROL Projet] > [!UICONTROL Cible] > [!UICONTROL Paramètres de build] > [!UICONTROL Compilateur Swift] > [!UICONTROL Génération de code]. Ajoutez le chemin suivant à l’en-tête « Pontage d’objectifs » :

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Initialiser l’extension

>[!BEGINTABS]

>[!TAB Objectif C]

Mettez à jour la méthode `applicationDidBecomeActive` comme ci-dessous

```
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Mettez à jour la méthode `applicationDidBecomeActive` comme ci-dessous

```
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## Appareils de test iOS

1. Sélectionnez **[!UICONTROL Projet]** > **[!UICONTROL Cible]** > **[!UICONTROL Infos]** > **[!UICONTROL Types d’URL]**.
1. Ajouter un identifiant : ${PRODUCT_NAME}
1. Définir Les Schémas D&#39;URL : mkto-&lt;S_secret Key_>
1. Inclure les `application:openURL:sourceApplication:annotation:` à `AppDelegate.m file` (Objectif-C)

### Gérer le type d’URL personnalisé dans AppDelegate

>[!BEGINTABS]

>[!TAB Objectif C]

```
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

```
func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    return ALMarketo.sharedInstance().application(application, open: url, sourceApplication: nil, annotation: nil)
}
```

>[!ENDTABS]

## Installation de Marketo SDK sur Android

### Configuration de l’extension Android

Suivez les instructions du portail [!DNL Adobe Launch]

### Configurer les autorisations

Ouvrez `AndroidManifest.xml` et ajoutez les autorisations suivantes. Votre application doit demander les autorisations « INTERNET » et « ACCESS_NETWORK_STATE ». Si votre application demande déjà ces autorisations, ignorez cette étape.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Initialiser l’extension

Configuration de ProGuard (en option)

Si vous utilisez ProGuard pour votre application, ajoutez les lignes suivantes dans votre fichier `proguard.cfg`. Le fichier se trouve dans votre dossier `project`. L’ajout de ce code exclut le SDK Marketo du processus d’obscurcissement.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Android  Test  Appareils

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

Le kit de développement logiciel MME (SDK) pour Android a été mis à jour vers un framework plus moderne, stable et évolutif qui contient plus de flexibilité et de nouvelles fonctionnalités d&#39;ingénierie pour votre développeur d&#39;applications Android.

Les développeurs d’applications Android peuvent désormais utiliser directement Google [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) avec ce SDK.

### Ajout de FCM à votre application

1. Intégrez la dernière version de Marketo Android SDK dans l’application Android.  Les étapes sont disponibles sur [GitHub](https://github.com/Marketo/android-sdk).
1. Configurez l’application Firebase sur la console Firebase.
   1. Créez/ajoutez un projet sur la console [&#128279;](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)Firebase).
      1. Dans la [console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), sélectionnez **[!UICONTROL Ajouter un projet]**.
      1. Sélectionnez votre projet GCM dans la liste des projets Google Cloud existants, puis sélectionnez **[!UICONTROL Ajouter Firebase]**.
      1. Dans l’écran d’accueil de Firebase, sélectionnez **[!UICONTROL Ajouter Firebase à l’application Android]**.
      1. Indiquez le nom de votre package et SHA-1, puis sélectionnez **[!UICONTROL Ajouter une application]**. Un nouveau fichier `google-services.json` pour votre application Firebase est téléchargé.
      1. Sélectionnez **[!UICONTROL Continuer]** et suivez les instructions détaillées pour ajouter le plug-in Google Services dans Android Studio.

   1. Accédez à **[!UICONTROL Paramètres du projet]** dans [!UICONTROL Présentation du projet]
      1. Cliquez sur l’onglet **[!UICONTROL Général]**. Téléchargez le fichier `google-services.json`.
      1. Cliquez sur l’onglet **[!UICONTROL Cloud Messaging]**. Copiez [!UICONTROL clé du serveur] et [!UICONTROL ID de l’expéditeur]. Fournissez ces [!UICONTROL clé de serveur] et [!UICONTROL ID d’expéditeur] à Marketo.
   1. Configuration des modifications FCM dans l’application Android
      1. Basculez vers la vue Projet dans Android Studio pour afficher le répertoire racine du projet
         1. Déplacez le fichier `google-services.json` téléchargé dans le répertoire racine du module d’application Android
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

         1. Enfin, cliquez sur **[!UICONTROL Synchroniser maintenant]** dans la barre qui s’affiche dans l’ID
   1. Modifier le manifeste de votre application Le SDK FCM ajoute automatiquement toutes les autorisations requises et la fonctionnalité de récepteur requise. Veillez à supprimer les éléments obsolètes (et potentiellement dangereux, car ils peuvent entraîner la duplication des messages) suivants du manifeste de votre application :

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

Questions fréquentes sur la prise en charge de Firebase Cloud Messaging.

**Q : Où puis-je trouver des instructions pour mettre à jour vers la dernière version de MME SDK ?** instructions se trouvent sur le site du développeur de Marketo [ICI](installation.md).

**Q : La mise à jour vers la dernière version de SDK nécessitera-t-elle que je publie une version mise à jour de mon application Android pour mes utilisateurs actuels ?** No.

**Q : Quel est l’impact sur les clients MME existants qui ont publié des applications Android intégrées à Marketo Android SDK ?** Ils peuvent migrer une application cliente GCM existante sur Android vers Firebase Cloud Messaging (FCM) comme suit :

1. Dans la [console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), sélectionnez **[!UICONTROL Ajouter un projet]**.
1. Sélectionnez votre projet GCM dans la liste des projets Google Cloud existants, puis sélectionnez **[!UICONTROL Ajouter Firebase]**.
1. Dans l’écran d’accueil de Firebase, sélectionnez **[!UICONTROL Ajouter Firebase à l’application Android]**.
1. Indiquez le nom de votre package et SHA-1, puis sélectionnez **[!UICONTROL Ajouter une application]**. Un nouveau fichier google-services.json pour votre
1. L’application Firebase est téléchargée.
1. Sélectionnez **[!UICONTROL Continuer]** et suivez les instructions détaillées pour ajouter le plug-in Google Services dans Android Studio.

**Q : Pouvons-nous cibler les prospects créés à l’aide de l’ancienne SDK Marketo qui utilisait l’application GCM ?** Oui. Tous les prospects créés à l’aide de Marketo SDK peuvent être ciblés pour l’envoi des notifications push.
