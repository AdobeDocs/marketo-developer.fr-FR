---
title: Installation
feature: Mobile Marketing
description: Installation des SDK pour Mobile Marketo
exl-id: e0b79d85-3509-46d2-a77d-cee211c5ec7f
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Installation

Instructions d’installation pour Marketo Mobile SDK. Les étapes ci-dessous sont nécessaires pour envoyer des notifications push et/ou des messages In-App.

## Installation de Marketo SDK sur iOS

### Conditions préalables

1. [Ajout d’une application dans Marketo Admin](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtention de la clé secrète de l’application et de l’ID Munchkin)
1. [Configurer des notifications push](push-notifications.md) (facultatif)

### Installation de Framework via CocoaPods

1. Installez CocoaPods. `$ sudo gem install cocoapods`
1. Remplacez le répertoire par le répertoire de votre projet et créez un fichier PDF avec des valeurs par défaut intelligentes. `$ pod init`
1. Ouvrez votre fichier PDF. `$ open -a Xcode Podfile`
1. Ajoutez la ligne suivante à votre fichier PDF. `$ pod 'Marketo-iOS-SDK'`
1. Enregistrez et fermez votre fichier PDF.
1. Télécharger et installer Marketo iOS SDK. `$ pod install`
1. Ouvrez l’espace de travail dans Xcode. `$ open App.xcworkspace`

### Installer Framework à l’aide du gestionnaire de packages Swift

1. Sélectionnez votre projet dans le navigateur de projets et sous « Ajouter une dépendance de package », cliquez sur « + » comme illustré ci-dessous :

   ![Ajouter une dépendance](assets/dependency-manager-add.png)

1. Ajoutez le package Marketo à partir de ce référentiel. Ajoutez cette URL pour ce référentiel : https://github.com/Marketo/ios-sdk.

   ![ URL du référentiel ](assets/dependency-manager-url.png)

1. Ajoutez maintenant le lot de ressources comme illustré ci-dessous : recherchez `MarketoFramework.XCframework` dans le navigateur de projets et ouvrez-le dans le Finder. Effectuez un glisser-déposer des `MKTResources.bundle` pour copier les ressources du bundle.

### Configurer l&#39;en-tête de pontage Swift

1. Accédez à Fichier > Nouveau > Fichier et sélectionnez « Fichier d’en-tête ».

   ![Sélectionnez « Fichier d’en-tête »](assets/choose-header-file.png)

1. Nommez le fichier « &lt;_ProjectName_>-Bridging-Header ».

1. Accédez à Projet > Cible > Phases de création > Compilateur Swift > Génération de code. Ajoutez le chemin suivant à l’en-tête de pontage d’objectifs :

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

   ![Phases de création](assets/build-phases.png)

## Initialiser SDK

Avant de pouvoir utiliser le SDK Marketo iOS, vous devez l’initialiser avec votre ID de compte Munchkin et la Clé secrète de l’application. Vous trouverez chacun de ces éléments dans la zone d’administration Marketo sous « Applications et appareils mobiles ».

1. Ouvrez votre fichier AppDelegate.m (Objective-C) ou le fichier de pontage (Swift) et importez le fichier d’en-tête Marketo.h.

   ```
   #import <MarketoFramework/MarketoFramework.h>
   ```

1. Collez le code suivant dans la fonction `application:didFinishLaunchingWithOptions` : .

   Notez que nous devons transmettre « native » comme type de framework pour les applications natives.

>[!BEGINTABS]

>[!TAB Objectif C]

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance initializeWithMunchkinID:@"munchkinAccountId" appSecret:@"secretKey" mobileFrameworkType:@"native" launchOptions:launchOptions];
```

>[!TAB Swift]

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.initialize(withMunchkinID: "munchkinAccountId", appSecret: "secretKey", mobileFrameworkType: "native", launchOptions: launchOptions)
```

>[!ENDTABS]

1. Remplacez les `munkinAccountId` et `secretKey` ci-dessus à l’aide de votre « ID de compte Munchkin » et de votre « Clé secrète » qui se trouvent dans la section Marketo **[!UICONTROL Admin]** > **[!UICONTROL Applications et appareils mobiles]**.

## Appareils de test iOS

1. Sélectionnez Projet > Cible > Informations > Types d’URL.
1. Ajouter un identifiant : ${PRODUCT_NAME}
1. Définir les schémas d&#39;URL : `mkto-<Secret Key_>`
1. Incluez application:openURL:sourceApplication:annotation: dans le fichier AppDelegate.m (Objective-C)

## Gérer le type d’URL personnalisé dans AppDelegate

>[!BEGINTABS]

>[!TAB Objectif C]

```
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options{

    return [[Marketo sharedInstance] application:app
                                         openURL:url
                                         options:options];
}
```

>[!TAB Swift]

```
private func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool
    {
        return Marketo.sharedInstance().application(app, open: url, options: options)
    }
```

>[!ENDTABS]

## Installation de Marketo SDK sur Android

### Conditions préalables

1. [Ajout d’une application dans Marketo Admin](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtention de la clé secrète de l’application et de l’ID Munchkin)
1. [Configurer des notifications push](push-notifications.md#android_setup_push) (facultatif)
1. [Télécharger Marketo SDK pour Android](https://codeload.github.com/Marketo/android-sdk/zip/refs/heads/master)

### Configuration d’Android SDK avec Gradle

1. Dans le fichier build.gradle de niveau application, sous la section dependencies, ajoutez :

`implementation 'com.marketo:MarketoSDK:0.8.9'`

1. Le fichier de `build.gradle` racine doit comporter

   ```
   buildscript {
       repositories {
           google()
           mavenCentral()
       }
   ```

1. Synchroniser le projet avec les fichiers Gradle

### Configurer les autorisations

Ouvrez `AndroidManifest.xml` et ajoutez les autorisations suivantes. Votre application doit demander les autorisations « INTERNET » et « ACCESS_NETWORK_STATE ». Si votre application demande déjà ces autorisations, ignorez cette étape.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Initialiser SDK

1. Ouvrez la classe Application ou Activity dans votre application et importez le SDK Marketo dans votre activité avant setContentView ou dans Application Context.

   ```java
   // Initialize Marketo
   Marketo marketoSdk = Marketo.getInstance(getApplicationContext());
   marketoSdk.initializeSDK("native","munchkinAccountId","secretKey");
   ```

1. Configuration de ProGuard (en option)

   Si vous utilisez ProGuard pour votre application, ajoutez les lignes suivantes dans votre fichier `proguard.cfg`. Le fichier se trouve dans le dossier du projet. L’ajout de ce code exclut le SDK Marketo du processus d’obscurcissement.

   ```
   -dontwarn com.marketo.*
   -dontnote com.marketo.*
   -keep class com.marketo.`{ *; }
   ```

## Appareils de test Android

Ajoutez « MarketoActivity » au fichier `AndroidManifest.xml` dans la balise de l’application.

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
      1. Dans la [console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), sélectionnez `Add Project`.
      1. Sélectionnez votre projet GCM dans la liste des projets Google Cloud existants, puis sélectionnez `Add Firebase`.
      1. Dans l&#39;écran d&#39;accueil de Firebase, sélectionnez `Add Firebase to your Android App`.
      1. Indiquez le nom de votre package et SHA-1, puis sélectionnez `Add App`. Un nouveau fichier `google-services.json` pour votre application Firebase est téléchargé.
      1. Sélectionnez `Continue` et suivez les instructions détaillées pour ajouter le plug-in Google Services dans Android Studio.

   1. Accédez aux « Paramètres du projet » dans la Présentation du projet
      1. Cliquez sur l’onglet « Général ». Téléchargez le fichier « google-services.json ».
      1. Cliquez sur l’onglet « Cloud Messaging ». Copiez « Clé du serveur » et « Identifiant de l’expéditeur ». Fournissez ces champs « Clé du serveur » et « ID de l’expéditeur » à Marketo.
   1. Configuration des modifications FCM dans l’application Android
      1. Basculez vers la vue Projet dans Android Studio pour afficher le répertoire racine du projet
         1. Déplacez le fichier « google-services.json » téléchargé dans le répertoire racine du module d’application Android
         1. Dans le fichier build.gradle au niveau du projet, ajoutez ce qui suit :

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

         1. Enfin, cliquez sur « Synchroniser maintenant » dans la barre qui s’affiche dans l’ID
   1. Modifier le manifeste de votre application Le SDK FCM ajoute automatiquement toutes les autorisations requises et la fonctionnalité de récepteur requise. Veillez à supprimer les éléments obsolètes (et potentiellement dangereux, car ils peuvent entraîner la duplication des messages) suivants du manifeste de votre application :

      ```xml
      <uses-permission android:name="android.permission.WAKE_LOCK" />
      <permission android:name="<your-package-name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
      <uses-permission android:name="<your-package-name>.permission.C2D_MESSAGE" />
      
      ...
      
      <receiver>
        android:name="com.google.android.gms.gcm.GcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND"
        <intent-filter>
          <action android:name="com.google.android.c2dm.intent.RECEIVE" />
          <category android:name="<your-package-name> />
        </intent-filter>
      </receiver>
      ```
