---
title: PhoneGap
feature: Mobile Marketing
description: Configurez le plug-in Marketo PhoneGap avec Cordova, configurez Firebase Cloud Messaging, activez les notifications push iOS et Android, suivez les notifications et initialisez SDK.
exl-id: 99f14c76-9438-4942-9309-643bca434d07
TQID: https://experienceleague.adobe.com/eFAwR7r5IE6vKigsEWrJdCmC3VrfB-nl0h8x7Vgt1VY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 775
ht-degree: 2%

---

# PhoneGap

Intégrez le plug-in Marketo PhoneGap à une application Cordova.

## Conditions préalables

1. [Ajoutez une application dans Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) et obtenez la clé secrète de l’application et l’ID Munchkin.
1. Configurez les notifications push pour [&#128279;](push-notifications.md) ou [Android](push-notifications.md).
1. [Installez PhoneGap/Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Instructions d’installation

1. Configurez le plug-in Marketo PhoneGap.

   Accédez au répertoire de l’application PhoneGap et exécutez la commande suivante pour ajouter le module externe Marketo :

   `$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Installez le plug-in FCM.

   `$ cordova plugin add cordova-plugin-fcm`

   Exécutez la commande suivante pour vérifier que le module externe a été ajouté :

   `$ cordova plugin ls com.marketo.plugin 0.X.0 "MarketoPlugin" cordova-plugin-fcm 2.1.2 "FCMPlugin"`

**Migration vers une version plus récente (facultatif)**

Pour supprimer un module externe existant, exécutez la commande suivante :

`$ cordova plugin remove com.marketo.plugin`

Pour ajouter à nouveau le module externe, exécutez la commande suivante :

`$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

**Cordova version 8.0.0 (Cordova@Android7.0.0) et ultérieures**

Après avoir créé la plateforme Cordova Android, ouvrez l’application dans Android Studio. Mettez à jour la valeur `dirs` dans le fichier `Marketo.gradle` du dossier `com.marketo.plugin` .

```groovy
repositories{
  jcenter()
  flatDir{
      dirs '../app/src/main/aar'
   }
}
```

Ajoutez les plateformes cibles pour l’application : `$cordova platform add android` `$ cordova platform add ios`

Vérifiez les plateformes ajoutées : `$cordova platform ls`

1. Prise en charge de Firebase Cloud Messaging

1. Configurez l’application Firebase dans la console Firebase.
   1. Créez ou ajoutez un projet dans [&#128279;](https://console.firebase.google.com/)Firebase Console).
      1. Dans la [console Firebase](https://console.firebase.google.com/), sélectionnez **[!UICONTROL Ajouter un projet]**.
      1. Sélectionnez votre projet GCM dans la liste des projets Google Cloud existants, puis sélectionnez **[!UICONTROL Ajouter Firebase]**.
      1. Dans l’écran d’accueil de Firebase, sélectionnez Ajouter Firebase à l’application Android.
      1. Indiquez le nom de votre package et SHA-1, puis sélectionnez **[!UICONTROL Ajouter une application]**. Un nouveau fichier `google-services.json` pour votre application Firebase est téléchargé.
   1. Accédez à **[!UICONTROL Paramètres du projet]** dans [!UICONTROL Présentation du projet].
      1. Sélectionnez l’onglet **[!UICONTROL Général]** et téléchargez le fichier « google-services.json ».
      1. Sélectionnez l’onglet **[!UICONTROL Cloud Messaging]**. Copiez les [!UICONTROL clé du serveur] et [!UICONTROL ID de l’expéditeur] et fournissez-les à Marketo.
   1. Configurez FCM dans l’application PhoneGap.
      1. Déplacez le fichier « google-services.json » téléchargé dans le répertoire racine du module d’application PhoneGap.
      1. Supprimez le fichier « MyFirebaseInstanceIDService » de l’emplacement `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` (obsolète).
      1. Modifiez le fichier &#39;MyFirebaseMessagingService&#39; à l&#39;emplacement `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` comme suit :

         ```
         import com.marketo.Marketo;
         
         public class MyFirebaseMessagingService extends FirebaseMessagingService{
         
         @Override
         public void onNewToken(String s){
           super.onNewToken(s);
           MarketoExtension.setPushNotificaitonTokens(s);
           //Add your code here
         }
         
         @Override
         public void onMessageReceived(RemoteMessage remoteMessage) {
           MarketoExtension.showPushNotificaiton(remoteMessage);
           //Add your code here
         }
         }
         ```

         1. Modifiez le fichier « fcm_config_files_process.js » dans location plugins/cordova-plugin-fcm/scripts comme suit :

            ```
            //change
            var strings = fs.readFileSync("platforms/android/res/values/strings.xml").toString();
            //to
            var strings = fs.readFileSync("platforms/android/app/src/main/res/values/strings.xml").toString();
            
            //AND change
            fs.writeFileSync("platforms/android/res/values/strings.xml", strings);
            //to
            fs.writeFileSync("platforms/android/app/src/main/res/values/strings.xml", strings);
            ```

### &#x200B;3. Activer les notifications push dans xCode

Activez la fonctionnalité de notification push dans le projet xCode.

### &#x200B;4. Suivi des notifications push

Collez le code suivant dans la fonction `application:didFinishLaunchingWithOptions:`.

>[!BEGINTABS]

>[!TAB Objectif C]

Mettez à jour la méthode `applicationDidBecomeActive` comme suit.

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

Mettez à jour la méthode `applicationDidBecomeActive` comme suit.

```swift
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotification(launchOptions)
```

>[!ENDTABS]

### &#x200B;5. Initialiser le framework Marketo

Pour initialiser le framework Marketo au démarrage de l’application, ajoutez le code suivant sous la fonction `onDeviceReady` dans le fichier JavaScript principal.

Transmettez `phonegap` comme type de framework pour les applications PhoneGap.

### Syntaxe

```javascript
// This method will Initialize the Marketo Framework using Your MunchkinId and Secret Key
marketo.initialize(
  function() { console.log("MarketoSDK Init done."); },
  function(error) { console.log("an error occurred:" + error); },
  'YOUR_MUNCHKIN_ID',
  'YOUR_SECRET_KEY',
  'FRAMEWORK_TYPE'
);

// For session tracking, add following.
marketo.onStart(
  function(){ console.log("onStart."); },
  function(error){ console.log("Failed to report onStart." + error); }
);
```

### Paramètres

- Rappel de réussite : fonction à exécuter si le framework Marketo s’initialise correctement.
- Rappel d’échec : fonction à exécuter en cas d’échec d’initialisation du framework Marketo.
- MUNCHKIN ID : Munchkin ID reçu de Marketo lors de l’enregistrement.
- CLÉ SECRÈTE : clé secrète reçue de Marketo lors de l’enregistrement.

### &#x200B;6. Initialiser la notification push Marketo

Pour initialiser les notifications push Marketo, ajoutez le code suivant après la fonction initialize dans le fichier JavaScript principal.

### Syntaxe

```javascript
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

### Paramètres

- Rappel de réussite : fonction à exécuter si la notification push Marketo s’initialise correctement.
- Échec du rappel : fonction à exécuter si la notification push Marketo ne parvient pas à s’initialiser.
- GCM_PROJECT_ID : ID de projet GCM trouvé dans [Google Developers Console](https://console.developers.google.com/) après la création de l’application.

Vous pouvez également annuler l’enregistrement du jeton lors de la déconnexion.

```javascript
marketo. uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Association au prospect

Appelez la fonction AssociatedLead pour créer un prospect Marketo.

### Syntaxe

```javascript
marketo.associateLead(
  function(){ console.log("MarketoSDK : Lead Added"); },
  function(error){ console.log("an error occurred:" + error); },
  'Lead_Data_JSON_String'
);
```

### Paramètres

- Rappel de réussite : fonction à exécuter si le framework Marketo associe le prospect avec succès.
- Rappel d’échec : fonction à exécuter si le framework Marketo ne parvient pas à associer le prospect.
- Données de lead : données de lead au format de chaîne JSON.

### Exemple

```javascript
// First create a lead as shown below
var lead = {};
lead[marketo.KEY_FIRST_NAME] = "Phone";
lead[marketo.KEY_LAST_NAME] = "Gap";
lead[marketo.KEY_EMAIL] = email;
lead[marketo.KEY_ADDRESS] = "demo address";
lead[marketo.KEY_CITY] = "city";
lead[marketo.KEY_STATE] = "state";
lead[marketo.KEY_COUNTRY] = "country";
lead[marketo.KEY_POSTAL_CODE] = "postalCode";
lead[marketo.KEY_GENDER] = "gender";
// To use lead custom field, use the REST API NAME as key
lead["REST API NAME"] = "value";

// Use associateLead function to associate it.
marketo.associateLead(
  function() { console.log("MarketoSDK : Lead Associated"); },
  function(error) { console.log("an error occurred:" + error); },
  JSON.stringify(lead)
);
```

## Action de rapport

Appelez la fonction `reportaction` pour signaler une action de l’utilisateur.

### Syntaxe

```javascript
marketo.reportaction(
  function(){ console.log("MarketoSDK : New event sent "); },
  function(error){ console.log("an error occurred:" + error); },
  'Action_Name',
  'Action_Data_JSON_String'
);
```

### Paramètres

- Rappel de réussite : fonction à exécuter si le framework Marketo signale l’action avec succès.
- Rappel d’échec : fonction à exécuter si le framework Marketo ne signale pas l’action.
- Action Name : nom de l’action.
- Données d’action : données d’action au format de chaîne JSON.

### Exemple

```javascript
// First create an event as below
var event = {
    "Action Type":"Add To Cart",
    "Action Details":"Adding Product in cart",
    "Action Metric":"10",
    "Action Length":"1"
}

marketo.reportaction(
    function(){ console.log("Reported action successfully."); },
    function(error){ console.log("Failed to report action." + error); },
    "Add To Cart",
    JSON.stringify(event)
);
```

## Rapports de session

Liez les types d’événements « pause » et « reprise » aux événements de début et d’arrêt du rapport. Ces événements effectuent le suivi du temps passé dans l’application mobile et sont requis sur Android.

```javascript
//Add the following code in your www/js/index.js

bindEvents: function() {
   document.addEventListener('pause', this.onStop, false);
   document.addEventListener('resume', this.onStart, false);
},
onStop: function() {
   marketo.onStop(
       function(){ console.log("onStop"); },
       function(error){ console.log("Failed to report onStop." + error); }
   );
},
onStart: function() {
   marketo.onStart(
       function(){ console.log("onStart."); },
       function(error){console.log( "Failed to report onStart." + error); }
   );
},
```

## Création de leads

Il existe trois façons de créer des prospects à partir d’une application hybride :

1. MARKETO MME SDK
1. API REST MARKETO
1. Envoi du formulaire

Les déclencheurs et filtres qui reconnaissent un nouveau prospect dépendent de la méthode de création :

- Les leads créés avec l’API SDK ou REST MME apparaissent dans les déclencheurs et filtres « Lead créé ».
- Les prospects créés par envoi de formulaire apparaissent dans les déclencheurs et filtres « Remplir le formulaire ».

Utilisez la même méthode de création de prospect dans l’application hybride et l’application web. Si l’application web utilise l’envoi de formulaire ou l’API REST, utilisez cette méthode dans l’application hybride. Si l’application web n’utilise aucune méthode, pensez à utiliser MME SDK pour créer des prospects dans Marketo.
