---
title: '[!DNL Ionic]'
feature: Mobile Marketing
description: Utilisation de  [!DNL Ionic]  avec Marketo pour les appareils mobiles
exl-id: 204e5fb4-c9d6-43a6-9d77-0b2a67ddbed3
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 2%

---

# Ionique

Cette rubrique décrit comment intégrer le module externe Marketo Cordova . Le condensateur [!DNL Ionic] n’est actuellement pas pris en charge.

## Conditions préalables

1. [Ajoutez une application dans l’administrateur Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtenez votre clé secrète et votre identifiant Munchkin).
1. Configurer des notifications push ([iOS](push-notifications.md)) | [Android](push-notifications.md) ).
1. Installez [[!DNL Ionic]](https://ionicframework.com/getting-started/) et [l’interface de ligne de commande Cordova](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Instructions d’installation

### Configuration du module externe Marketo [!DNL Ionic]

1. En supposant que l’interface de ligne de commande Cordova soit installée, accédez au répertoire de votre application [!DNL Ionic] et exécutez la commande suivante pour ajouter le module externe Marketo à votre application :

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Pour confirmer que le module externe a été ajouté à l’application, exécutez la commande suivante :

   `$ ionic plugin list com.marketo.plugin 0.X.0 "MarketoPlugin"`

### Migrer vers la version la plus récente (facultatif)

1. Pour supprimer un module externe existant, exécutez la commande suivante :

   `$ ionic plugin remove com.marketo.plugin`

1. Pour lire le module externe, exécutez la commande suivante :

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

### Activation des notifications push dans xCode

1. Activez la fonctionnalité de notification push dans le projet xCode.![Fonctionnalité de notification](assets/notification-capability.png)

### Suivi des notifications push

Collez le code suivant dans la fonction `application:didFinishLaunchingWithOptions:`.

>[!BEGINTABS]

>[!TAB Objective C]

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotfication(launchOptions)
```

>[!ENDTABS]

### Initialisation de la structure Marketo

Pour vous assurer que la structure Marketo est lancée au démarrage de l’application, ajoutez le code suivant sous la fonction `onDeviceReady` dans votre fichier JavaScript principal.

Vous devez transmettre `ionicCordova` comme type de structure pour les applications Cordova [!DNL Ionic].

#### Syntaxe

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

#### Paramètres

- Rappel de succès : fonction à exécuter si la structure Marketo s’initialise avec succès.
- Rappel d’échec : fonction à exécuter si la structure Marketo ne s’initialise pas.
- ID MUNCHKIN : ID Munchkin reçu de Marketo au moment de l’enregistrement.
- CLÉ SECRETE : clé secrète reçue de Marketo au moment de l’enregistrement.

### Initialisation de la notification push Marketo

Pour vous assurer que la notification push Marketo est lancée, ajoutez le code suivant après la fonction initialisée dans votre fichier JavaScript principal.

#### Syntaxe

```javascript
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

#### Paramètres

- Rappel de succès : fonction à exécuter si la notification push Marketo s’initialise avec succès.
- Rappel d’échec : fonction à exécuter si la notification push Marketo ne s’initialise pas.
- GCM_PROJECT_ID : ID de projet GCM trouvé dans la [console de développeurs Google](https://accounts.google.com/ServiceLogin?service=cloudconsole&amp;passive=1209600&amp;osid=1&amp;continue=https://console.cloud.google.com/apis/dashboard&amp;followup=https://console.cloud.google.com/apis/dashboard) après la création de l’application.

Le jeton peut également être désinscrit lors de la déconnexion.

```javascript
marketo.uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Associer le lead

Vous pouvez créer une piste Marketo en appelant la fonction associatedLead .

### Syntaxe

```javascript
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

```javascript
// First create a lead as shown below
var lead = {};
lead[marketo.KEY_FIRST_NAME] = "Ionic";
lead[marketo.KEY_LAST_NAME] = "App";
lead[marketo.KEY_EMAIL] = email;
lead[marketo.KEY_ADDRESS] = "demo address";
lead[marketo.KEY_CITY] = "city";
lead[marketo.KEY_STATE] = "state";
lead[marketo.KEY_COUNTRY] = "country";
lead[marketo.KEY_POSTAL_CODE] = "postalCode";
lead[marketo.KEY_GENDER] = "gender";

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

```javascript
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

## Création de rapports de session

Liez les types d’événements &quot;pause&quot; et &quot;resume&quot; comme illustré ci-dessous pour signaler les événements de début et d’arrêt. Permet d’effectuer le suivi de la durée de la visite dans votre application mobile. Remarque : cela est requis dans Android.

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

## Création de pistes

Il existe trois façons de créer des pistes à partir d’une application hybride :

1. SDK MME Marketo
1. API REST MARKETO
1. Envoi du formulaire

Selon la méthode utilisée, un nouveau prospect est reconnu par différents déclencheurs et filtres. Les pistes créées à l’aide du SDK MME ou de l’API REST apparaissent dans les déclencheurs et filtres &quot;Lead Created&quot;. Les pistes créées par les envois de formulaire apparaissent dans les déclencheurs et filtres &quot;Remplit le formulaire&quot;.

La bonne pratique consiste à rester cohérente avec la méthode utilisée par l’application web lors de la création de pistes. Si vous disposez déjà d’une application Web qui utilise l’envoi de formulaire comme mécanisme pour créer des pistes, utilisez ce même mécanisme lors de la création de pistes dans votre application hybride. Si vous disposez déjà d’une application Web qui utilise notre API REST comme mécanisme pour créer des pistes, utilisez ce même mécanisme lors de la création de pistes dans votre application hybride. Dans les cas où vous n’utilisez ni envoi de formulaire ni API REST comme mécanisme pour créer des pistes dans votre application web, vous pouvez envisager d’utiliser le SDK MME pour créer des pistes dans Marketo.
