---
title: PhoneGap
feature: Mobile Marketing
description: Utilisation de PhoneGap avec Marketo sur les appareils mobiles
exl-id: 99f14c76-9438-4942-9309-643bca434d07
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 1%

---

# PhoneGap

Intégration du plug-in Marketo PhoneGap

## Conditions préalables

1. [Ajoutez une application dans Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtenez la clé secrète de l’application et l’ID Munchkin).
1. Configurer Des Notifications Push ([iOS](push-notifications.md) | [Android](push-notifications.md)).
1. [Installez PhoneGap/Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Instructions d’installation

1. Configuration du plug-in Marketo PhoneGap

   En supposant que l’interface de ligne de commande Cordova soit installée, accédez à votre répertoire d’applications PhoneGap et exécutez la commande suivante pour ajouter le module externe Marketo à votre application :

   `$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Installation du plug-in FCM

   `$ cordova plugin add cordova-plugin-fcm`

   Pour confirmer que le module externe a été ajouté à l’application, exécutez la commande suivante et vérifiez

   `$ cordova plugin ls com.marketo.plugin 0.X.0 "MarketoPlugin" cordova-plugin-fcm 2.1.2 "FCMPlugin"`

**Migration vers une version plus récente (facultatif)**

Pour supprimer un module externe existant, exécutez la commande suivante :

`$ cordova plugin remove com.marketo.plugin`

Pour ajouter à nouveau le module externe, exécutez la commande suivante :

`$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

**Cordova version 8.0.0 (Cordova@Android7.0.0) et ultérieures**

Une fois la plateforme Cordova Android créée, ouvrez l’application avec Android Studio et mettez à jour la valeur `dirs` du fichier `Marketo.gradle` situé dans le dossier `com.marketo.plugin`.

```
repositories{
  jcenter()
  flatDir{
      dirs '../app/src/main/aar'
   }
}
```

Ajouter les plateformes à cibler pour l&#39;application `$cordova platform add android` `$ cordova platform add ios`

Vérifier la liste des plateformes ajoutées `$cordova platform ls`

1. Prise en charge de Firebase Cloud Messaging

1. Configurez l’application Firebase sur la console Firebase.
   1. Créez/ajoutez un projet sur la console [&#128279;](https://console.firebase.google.com/)Firebase).
      1. Dans la [console Firebase](https://console.firebase.google.com/), sélectionnez **[!UICONTROL Ajouter un projet]**.
      1. Sélectionnez votre projet GCM dans la liste des projets Google Cloud existants, puis sélectionnez **[!UICONTROL Ajouter Firebase]**.
      1. Dans l’écran d’accueil de Firebase, sélectionnez Ajouter Firebase à l’application Android.
      1. Indiquez le nom de votre package et SHA-1, puis sélectionnez **[!UICONTROL Ajouter une application]**. Un nouveau fichier `google-services.json` pour votre application Firebase est téléchargé.
   1. Accédez à **[!UICONTROL Paramètres du projet]** dans [!UICONTROL Présentation du projet]
      1. Cliquez sur l’onglet **[!UICONTROL Général]**. Téléchargez le fichier « google-services.json ».
      1. Cliquez sur l’onglet **[!UICONTROL Cloud Messaging]**. Copiez [!UICONTROL clé du serveur] et [!UICONTROL ID de l’expéditeur]. Fournissez ces [!UICONTROL clé de serveur] et [!UICONTROL ID d’expéditeur] à Marketo.
   1. Configuration des modifications FCM dans l’application Phonegap
      1. Déplacez le fichier « google-services.json » téléchargé vers le répertoire racine du module d’application Phonegap.
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

Mettez à jour la méthode `applicationDidBecomeActive` comme ci-dessous

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

Mettez à jour la méthode `applicationDidBecomeActive` comme ci-dessous

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotification(launchOptions)
```

>[!ENDTABS]

### &#x200B;5. Initialiser le framework Marketo

Pour vous assurer que le framework Marketo est lancé au démarrage de l’application, ajoutez le code suivant sous la fonction `onDeviceReady` dans votre fichier JavaScript principal.

Notez que nous devons transmettre `phonegap` comme type de framework pour les applications PhoneGap.

### Syntaxe

```
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

- Rappel de succès : fonction à exécuter si le framework Marketo s’initialise correctement.
- Rappel d’échec : fonction à exécuter si l’initialisation du framework Marketo échoue.
- MUNCHKIN ID : Munchkin ID reçu de Marketo au moment de l’enregistrement.
- CLÉ SECRÈTE : clé secrète reçue de Marketo au moment de l’enregistrement.

### &#x200B;6. Initialiser La Notification Push Marketo

Pour vous assurer que la notification push Marketo est lancée, ajoutez le code suivant après la fonction initialize dans votre fichier JavaScript principal.

### Syntaxe

```
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

### Paramètres

- Rappel de réussite : fonction à exécuter si la notification push Marketo s’initialise avec succès.
- Rappel d’échec : fonction à exécuter si l’initialisation de la notification push Marketo échoue.
- GCM_PROJECT_ID : ID de projet GCM trouvé dans [Google Developers Console](https://console.developers.google.com/) après la création de l’application.

Le jeton peut également être désenregistré lors de la déconnexion.

```
marketo. uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Association au prospect

Vous pouvez créer un lead Marketo en appelant la fonction AssociatedLead.

### Syntaxe

```
marketo.associateLead(
  function(){ console.log("MarketoSDK : Lead Added"); },
  function(error){ console.log("an error occurred:" + error); },
  'Lead_Data_JSON_String'
);
```

### Paramètres

- Rappel de succès : fonction à exécuter si le framework Marketo associe le prospect avec succès.
- Rappel d’échec : fonction à exécuter si le framework Marketo ne parvient pas à associer le prospect.
- Données de lead : données de lead au format de chaîne JSON.

### Exemple

```
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

Vous pouvez signaler toute action effectuée par un utilisateur en appelant la fonction `reportaction`.

### Syntaxe

```
marketo.reportaction(
  function(){ console.log("MarketoSDK : New event sent "); },
  function(error){ console.log("an error occurred:" + error); },
  'Action_Name',
  'Action_Data_JSON_String'
);
```

### Paramètres

- Rappel de succès : fonction à exécuter si le framework Marketo signale une action avec succès.
- Rappel d’échec : fonction à exécuter en cas d’échec de l’action de rapport du framework Marketo.
- Nom de l’action : nom de l’action.
- Données d’action : données d’action au format de chaîne JSON.

### Exemple

```
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

Liez les types d’événements « pause » et « reprise » comme illustré ci-dessous pour générer des rapports sur les événements Démarrer et Arrêter.  Permet d’effectuer le suivi de la durée de consultation de votre application mobile. Remarque : cela est obligatoire dans Android.

```
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

Selon la méthode utilisée, un prospect nouvellement créé sera reconnu par différents déclencheurs et filtres. Les prospects créés à l’aide de l’API REST ou de MME SDK apparaissent dans les déclencheurs et filtres « Lead créé ». Les leads créés par des envois de formulaire apparaissent dans les déclencheurs et filtres « Remplir le formulaire ».

La bonne pratique consiste à rester cohérent avec la méthode utilisée par l’application web lors de la création de prospects. Si vous disposez déjà d’une application web qui utilise l’envoi de formulaire comme mécanisme de création de prospects, utilisez le même mécanisme lors de la création de prospects dans votre application hybride. Si vous disposez déjà d’une application web qui utilise notre API REST comme mécanisme de création de prospects, utilisez ce même mécanisme lors de la création de prospects dans votre application hybride. Dans les cas où vous n’utilisez ni l’envoi de formulaire ni l’API REST comme mécanisme de création de prospects dans votre application web, vous pouvez envisager d’utiliser MME SDK pour créer des prospects dans Marketo.
