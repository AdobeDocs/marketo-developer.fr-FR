---
title: PhoneGap
feature: Mobile Marketing
description: Utilisation de PhoneGap avec Marketo sur les appareils mobiles
exl-id: 99f14c76-9438-4942-9309-643bca434d07
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 1%

---

# PhoneGap

Intégration du module externe Marketo PhoneGap

## Conditions préalables

1. [Ajoutez une application dans l’administrateur Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtenez votre clé secrète et votre identifiant Munchkin).
1. Configurer des notifications push ([iOS](push-notifications.md)) | [Android](push-notifications.md)).
1. [Installez PhoneGap/Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Instructions d’installation

1. Configuration du module externe Marketo PhoneGap

   En supposant que l’interface de ligne de commande Cordova soit installée, accédez au répertoire de votre application PhoneGap et exécutez la commande suivante pour ajouter le module externe Marketo à votre application :

   `$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Installation du module externe FCM

   `$ cordova plugin add cordova-plugin-fcm`

   Pour confirmer que le module externe a été ajouté à l’application, exécutez la commande suivante et vérifiez

   `$ cordova plugin ls com.marketo.plugin 0.X.0 "MarketoPlugin" cordova-plugin-fcm 2.1.2 "FCMPlugin"`

**Migrer vers la version la plus récente (facultatif)**

Pour supprimer un module externe existant, exécutez la commande suivante :

`$ cordova plugin remove com.marketo.plugin`

Pour rajouter le module externe, exécutez la commande suivante :

`$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

**Cordova version 8.0.0 (Cordova@Android7.0.0) et ultérieure**

Une fois la plateforme Cordova Android créée, ouvrez l’application avec Android Studio et mettez à jour la valeur `dirs` du fichier `Marketo.gradle` situé dans le dossier `com.marketo.plugin`.

```
repositories{    
  jcenter()
  flatDir{
      dirs '../app/src/main/aar'
   }
}
```

Ajoutez les plateformes à cibler pour l’application `$cordova platform add android` `$ cordova platform add ios`

Vérifiez la liste des plateformes ajoutées `$cordova platform ls`

1. Prise en charge de Firebase Cloud Messaging

1. Configurez l’application Firebase sur la console Firebase.
   1. Créez/ajoutez un projet sur [&#128279;](https://console.firebase.google.com/)la console Firebase.
      1. Dans la [console Firebase](https://console.firebase.google.com/), sélectionnez **[!UICONTROL Ajouter un projet]**.
      1. Sélectionnez votre projet GCM dans la liste des projets Google Cloud existants, puis sélectionnez **[!UICONTROL Ajouter Firebase]**.
      1. Dans l’écran de bienvenue de Firebase, sélectionnez &quot;Ajouter Firebase à votre application Android&quot;.
      1. Indiquez le nom de votre package et SHA-1, puis sélectionnez **[!UICONTROL Ajouter une application]**. Un nouveau fichier `google-services.json` pour votre application Firebase est téléchargé.
   1. Accédez à **[!UICONTROL Paramètres du projet]** dans [!UICONTROL Aperçu du projet]
      1. Cliquez sur l’onglet **[!UICONTROL Général]** . Téléchargez le fichier &#39;google-services.json&#39;.
      1. Cliquez sur l’onglet **[!UICONTROL Cloud Messaging]** . Copiez [!UICONTROL Server Key] et [!UICONTROL Sender ID]. Fournissez ces [!UICONTROL Clé serveur] et [!UICONTROL ID d’expéditeur] à Marketo.
   1. Configuration des modifications FCM dans l’application PhoneGap
      1. Déplacez le fichier &quot;google-services.json&quot; téléchargé dans le répertoire racine du module d’application PhoneGap.
      1. Supprimez le fichier &quot;MyFirebaseInstanceIDService&quot; de l’emplacement `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` (obsolète)
      1. Modifiez le fichier &quot;MyFirebaseMessagingService&quot; à l’emplacement `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` comme suit :

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

         1. Modifiez le fichier &quot;fcm_config_files_process.js&quot; dans l’emplacement plugins/cordova-plugin-fcm/scripts comme suit :

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


### 3. Activation des notifications push dans xCode

Activez la fonctionnalité de notification push dans le projet xCode.

### 4. Suivi des notifications push

Collez le code suivant dans la fonction `application:didFinishLaunchingWithOptions:`.

>[!BEGINTABS]

>[!TAB Objective C]

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

### 5. Initialisation de la structure Marketo

Pour vous assurer que la structure Marketo est lancée au démarrage de l’application, ajoutez le code suivant sous la fonction `onDeviceReady` dans votre fichier JavaScript principal.

Notez que nous devons transmettre `phonegap` comme type de structure pour les applications PhoneGap.

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

- Rappel de succès : fonction à exécuter si la structure Marketo s’initialise avec succès.
- Rappel d’échec : fonction à exécuter si la structure Marketo ne s’initialise pas.
- ID MUNCHKIN : ID Munchkin reçu de Marketo au moment de l’enregistrement.
- CLÉ SECRETE : clé secrète reçue de Marketo au moment de l’enregistrement.

### 6. Initialisation de la notification push Marketo

Pour vous assurer que la notification push Marketo est lancée, ajoutez le code suivant après la fonction initialize de votre fichier JavaScript principal.

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

- Rappel de succès : fonction à exécuter si la notification push Marketo s’initialise avec succès.
- Rappel d’échec : fonction à exécuter si la notification push Marketo ne s’initialise pas.
- GCM_PROJECT_ID : ID de projet GCM trouvé dans la [console de développeurs Google](https://console.developers.google.com/) après la création de l’application.

Le jeton peut également être désinscrit lors de la déconnexion.

```
marketo. uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Associer le lead

Vous pouvez créer une piste Marketo en appelant la fonction associatedLead .

### Syntaxe

```
marketo.associateLead(
  function(){ console.log("MarketoSDK : Lead Added"); },
  function(error){ console.log("an error occurred:" + error); },
  'Lead_Data_JSON_String'
);
```

### Paramètres

- Rappel de succès : fonction à exécuter si la structure Marketo associe correctement la piste.
- Rappel d’échec : fonction à exécuter si la structure Marketo n’associe pas la piste.
- Données de piste : données de piste au format de chaîne JSON.

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

## Action du rapport

Vous pouvez signaler toute action effectuée par l’utilisateur en appelant la fonction `reportaction` .

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

- Rappel de succès : fonction à exécuter si la structure Marketo signale une action réussie.
- Rappel d’échec : fonction à exécuter si la structure Marketo ne parvient pas à signaler l’action.
- Nom de l’action : nom de l’action.
- Action Data : données d’action au format de chaîne JSON.

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

## Création de rapports de session

Liez les types d’événements &quot;pause&quot; et &quot;resume&quot; comme illustré ci-dessous pour signaler les événements de début et d’arrêt.  Permet d’effectuer le suivi de la durée de la visite dans votre application mobile. Remarque : cela est requis dans Android.

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

## Création de pistes

Il existe trois façons de créer des pistes à partir d’une application hybride :

1. SDK MME Marketo
1. API REST MARKETO
1. Envoi du formulaire

Selon la méthode utilisée, un nouveau prospect sera reconnu par différents déclencheurs et filtres. Les pistes créées à l’aide du SDK MME ou de l’API REST apparaissent dans les déclencheurs et filtres &quot;Lead Created&quot;. Les pistes créées par les envois de formulaire apparaissent dans les déclencheurs et filtres &quot;Remplit le formulaire&quot;.

La bonne pratique consiste à rester cohérente avec la méthode utilisée par l’application web lors de la création de pistes. Si vous disposez déjà d’une application Web qui utilise l’envoi de formulaire comme mécanisme pour créer des pistes, utilisez ce même mécanisme lors de la création de pistes dans votre application hybride. Si vous disposez déjà d’une application Web qui utilise notre API REST comme mécanisme pour créer des pistes, utilisez ce même mécanisme lors de la création de pistes dans votre application hybride. Dans les cas où vous n’utilisez ni envoi de formulaire ni API REST comme mécanisme pour créer des pistes dans votre application web, vous pouvez envisager d’utiliser le SDK MME pour créer des pistes dans Marketo.
