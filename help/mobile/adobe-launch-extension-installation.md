---
title: '''[!DNL Adobe Launch] Installation de l’extension'
feature: Mobile Marketing
description: '''[!DNL Adobe Launch] Présentation de l’installation de l’extension"'
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
source-git-commit: 2b6fd22becb0eb692dbcf48686c23f7a878cd812
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 0%

---

# [!DNL Adobe Launch] Installation de l’extension

Instructions d’installation pour [!DNL Adobe Launch] Extension Marketo. Les étapes ci-dessous sont requises pour envoyer des notifications push et/ou des messages In-App.

## Conditions préalables

1. [Ajout d’une application dans Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (Obtenez votre clé secrète et votre ID Munchkin)
1. [Configurez la propriété dans [!DNL Adobe Launch] Portail](https://experience.adobe.com/#/@amc/data-collection/home)
1. Configuration de la clé secrète de l’application et de l’ID Munchkin pour la propriété dans le [!DNL Adobe Launch] Portail
1. [Configuration des notifications push](push-notifications.md) (facultatif)

## Installation de l’extension Marketo sur iOS

### Configuration de l’en-tête de liaison Swift

1. Accédez à Fichier > Nouveau > Fichier et sélectionnez &quot;Fichier d’en-tête&quot;.

1. Nommez le fichier &quot;&lt;_ProjectName_>-Bridging-Header&quot;.

1. Accédez à Projet > Cible > Paramètres de génération > Compilateur Swift > Génération de code. Ajoutez le chemin d’accès suivant à l’en-tête de &quot;liaison d’objectifs&quot; :

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Initialisation de l’extension

>[!BEGINTABS]

>[!TAB Objectif C]

Mettez à jour le `applicationDidBecomeActive` , comme ci-dessous

```
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Mettez à jour le `applicationDidBecomeActive` , comme ci-dessous

```
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## Appareils de test iOS

1. Sélectionner [!UICONTROL Projet] > [!UICONTROL Cible] > [!UICONTROL Infos] > Types d’URL.
1. Ajouter un identifiant : ${PRODUCT_NAME}
1. Définition des schémas d’URL : mkto-&lt;s_ecret key_=&quot;&quot;>
1. Inclure `application:openURL:sourceApplication:annotation:` to `AppDelegate.m file` (Objective-C)

### Gestion du type d’URL personnalisé dans AppDelegate

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

## Installation du SDK Marketo sur Android

### Configuration de l’extension Android

Suivez les instructions de la section [!DNL Adobe Launch] Portail

### Configuration des autorisations

Ouvrir `AndroidManifest.xml` et ajoutez les autorisations suivantes. Votre application doit demander les autorisations &quot;INTERNET&quot; et &quot;ACCESS_NETWORK_STATE&quot;. Si votre application demande déjà ces autorisations, ignorez cette étape.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Initialisation de l’extension

Configuration de ProGuard (facultatif)

Si vous utilisez ProGuard pour votre application, ajoutez les lignes suivantes dans votre `proguard.cfg` fichier . Le fichier se trouve dans votre `project` dossier. L’ajout de ce code exclut le SDK Marketo du processus d’obscurcissement.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Appareils de test Android

Ajoutez &quot;MarketoActivity&quot; à `AndroidManifest.xml` à l’intérieur de la balise de l’application.

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

Le SDK (Software Development Kit) MME pour Android a été mis à jour vers un framework plus moderne, stable et évolutif qui offre davantage de flexibilité et de nouvelles fonctionnalités d’ingénierie pour votre développeur d’applications Android.

Les développeurs d’applications Android peuvent désormais utiliser directement Google [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) avec ce SDK.

### Ajout de FCM à votre application

1. Intégrer le dernier SDK Marketo Android dans l’application Android.  Les étapes sont disponibles dans [GitHub](https://github.com/Marketo/android-sdk).
1. Configurez l’application Firebase sur la console Firebase.
   1. Création/ajout d’un projet sur [](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/)Console Firebase.
      1. Dans le [Console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/), sélectionnez [!UICONTROL Ajouter un projet].
      1. Sélectionnez votre projet GCM dans la liste des projets cloud Google existants, puis sélectionnez [!UICONTROL Ajouter Firebase].
      1. Dans l’écran de bienvenue de Firebase, sélectionnez &quot;Ajouter Firebase à votre application Android&quot;.
      1. Indiquez le nom de votre package et SHA-1, puis sélectionnez [!UICONTROL Ajout d’une application]. Une nouvelle `google-services.json` pour votre application Firebase est téléchargé.
      1. Sélectionner [!UICONTROL Continuer] et suivez les instructions détaillées pour ajouter le module externe Google Services dans Android Studio.

   1. Accédez à Paramètres du projet dans Aperçu du projet
      1. Cliquez sur l’onglet &quot;Général&quot;. Téléchargez la `google-services.json` fichier .
      1. Cliquez sur l’onglet &quot;Cloud Messaging&quot;. Copiez &#39;Server Key&#39; et &#39;Sender ID&#39;. Fournissez ces &quot;Clé serveur&quot; et &quot;Identifiant expéditeur&quot; à Marketo.
   1. Configuration des modifications FCM dans l’application Android
      1. Passez en vue Projet dans Android Studio pour afficher le répertoire racine du projet.
         1. Déplacer le téléchargé `google-services.json` dans le répertoire racine du module d’application Android
         1. Au niveau du projet `build.gradle` ajoutez les éléments suivants :

            ```
            buildscript {
              dependencies {
                classpath 'com.google.gms:google-services:4.0.0'
              }
            }
            ```

         1. Dans le fichier build.gradle de niveau application, ajoutez les éléments suivants :

            ```
            dependencies {
              compile 'com.google.firebase:firebase-core:17.4.0'
            } 
            // Add to the bottom of the file 
            apply plugin: 'com.google.gms.google-services'
            ```

         1. Enfin, cliquez sur &quot;[!UICONTROL Synchroniser maintenant]&quot; dans la barre qui s’affiche dans l’ID ;
   1. Modifier le manifeste de votre application Le SDK FCM ajoute automatiquement toutes les autorisations requises et la fonctionnalité de récepteur requise. Veillez à supprimer du manifeste de votre application les éléments obsolètes (et potentiellement dangereux, car ils peuvent entraîner la duplication des messages) suivants :

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

**Q : Où puis-je trouver des instructions pour effectuer la mise à jour vers la dernière version du SDK MME ?** Les instructions se trouvent sur le site des développeurs Marketo [ICI](installation.md).

**Q : La mise à jour vers la dernière version du SDK m’obligera-t-elle à publier une version mise à jour de mon application Android pour mes utilisateurs existants ?** Non.

**Q : Comment cela affecte-t-il les clients MME existants qui ont publié des applications Android intégrées avec le SDK Marketo Android ?** Ils peuvent migrer une application cliente GCM existante sur Android vers Firebase Cloud Messaging (FCM) comme suit :

1. Dans le [Console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/), sélectionnez [!UICONTROL Ajouter un projet].
1. Sélectionnez votre projet GCM dans la liste des projets cloud Google existants, puis sélectionnez **[!UICONTROL Ajouter Firebase]**.
1. Dans l’écran de bienvenue de Firebase, sélectionnez **[!UICONTROL Ajout de Firebase à votre application Android]**.
1. Indiquez le nom de votre package et SHA-1, puis sélectionnez **[!UICONTROL Ajout d’une application]**. Un nouveau fichier google-services.json pour votre
1. L’application Firebase est téléchargée.
1. Sélectionner **[!UICONTROL Continuer]** et suivez les instructions détaillées pour ajouter le module externe Google Services dans Android Studio.

**Q : Pouvons-nous cibler les pistes créées à l’aide de l’ancien SDK Marketo qui utilisait l’application GCM ?** Oui. Toutes les pistes créées à l’aide du SDK Marketo peuvent être ciblées pour envoyer des notifications push.
