---
title: '[!DNL Ionic]'
feature: Mobile Marketing
description: Guide détaillé pour l’intégration du plug-in Marketo Cordova à Ionic, l’activation des notifications push, l’initialisation de SDK, le suivi des sessions et l’association des prospects.
exl-id: 204e5fb4-c9d6-43a6-9d77-0b2a67ddbed3
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '620'
ht-degree: 2%

---

# Ionique

Cette rubrique décrit comment intégrer le plug-in Marketo Cordova. [!DNL Ionic] condensateur n&#39;est actuellement pas pris en charge.

## Conditions préalables

1. [Ajoutez une application dans Marketo Admin](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtenez la clé secrète de l’application et l’ID Munchkin).
1. Configurer Des Notifications Push ([iOS](push-notifications.md) | [Android](push-notifications.md) ).
1. Installez [[!DNL Ionic]](https://ionicframework.com/getting-started/) et [Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Instructions d’installation

### Configuration du plug-in Marketo [!DNL Ionic]

1. En supposant que l’interface de ligne de commande Cordova soit installée, accédez à votre répertoire d’applications [!DNL Ionic] et exécutez la commande suivante pour ajouter le module externe Marketo à votre application :

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Pour confirmer que le module externe a été ajouté à l’application, exécutez la commande suivante :

   `$ ionic plugin list com.marketo.plugin 0.X.0 "MarketoPlugin"`

### Migrer vers une version plus récente (facultatif)

1. Pour supprimer un module externe existant, exécutez la commande suivante :

   `$ ionic plugin remove com.marketo.plugin`

1. Pour lire le module externe, exécutez la commande suivante :

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

### Activer les notifications push dans xCode

1. Activez la fonctionnalité de notification push dans le projet xCode.![Fonction de notification](assets/notification-capability.png)

### Suivi des notifications push

Collez le code suivant dans la fonction `application:didFinishLaunchingWithOptions:`.

>[!BEGINTABS]

>[!TAB Objectif C]

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

### Initialiser le framework Marketo

Pour vous assurer que le framework Marketo est lancé au démarrage de l’application, ajoutez le code suivant sous la fonction `onDeviceReady` dans votre fichier JavaScript principal.

Vous devez transmettre `ionicCordova` comme type de framework pour les applications [!DNL Ionic] Cordova.

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

- Rappel de succès : fonction à exécuter si le framework Marketo s’initialise correctement.
- Rappel d’échec : fonction à exécuter si l’initialisation du framework Marketo échoue.
- MUNCHKIN ID : Munchkin ID reçu de Marketo au moment de l’enregistrement.
- CLÉ SECRÈTE : clé secrète reçue de Marketo au moment de l’enregistrement.

### Initialiser la notification push Marketo

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

- Rappel de réussite : fonction à exécuter si la notification push Marketo s’initialise avec succès.
- Rappel d’échec : fonction à exécuter si l’initialisation de la notification push Marketo échoue.
- GCM_PROJECT_ID : ID de projet GCM trouvé dans [Google Developers Console](https://accounts.google.com/ServiceLogin?service=cloudconsole&passive=1209600&osid=1&continue=https://console.cloud.google.com/apis/dashboard&followup=https://console.cloud.google.com/apis/dashboard) après la création de l’application.

Le jeton peut également être désenregistré lors de la déconnexion.

```javascript
marketo.uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Association au prospect

Vous pouvez créer un lead Marketo en appelant la fonction AssociatedLead.

### Syntaxe

```javascript
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

## Action de rapport

Vous pouvez signaler toute action effectuée par un utilisateur en appelant la fonction `reportaction`.

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

- Rappel de succès : fonction à exécuter si le framework Marketo signale une action avec succès.
- Rappel d’échec : fonction à exécuter en cas d’échec de l’action de rapport du framework Marketo.
- Nom de l’action : nom de l’action.
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

Liez les types d’événements « pause » et « reprise » comme illustré ci-dessous pour générer des rapports sur les événements Démarrer et Arrêter. Permet d’effectuer le suivi de la durée de consultation de votre application mobile. Remarque : cela est obligatoire dans Android.

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

Selon la méthode utilisée, un prospect nouvellement créé est reconnu par différents déclencheurs et filtres. Les prospects créés à l’aide de l’API REST ou de MME SDK apparaissent dans les déclencheurs et filtres « Lead créé ». Les leads créés par des envois de formulaire apparaissent dans les déclencheurs et filtres « Remplir le formulaire ».

La bonne pratique consiste à rester cohérent avec la méthode utilisée par l’application web lors de la création de prospects. Si vous disposez déjà d’une application web qui utilise l’envoi de formulaire comme mécanisme de création de prospects, utilisez le même mécanisme lors de la création de prospects dans votre application hybride. Si vous disposez déjà d’une application web qui utilise notre API REST comme mécanisme de création de prospects, utilisez ce même mécanisme lors de la création de prospects dans votre application hybride. Dans les cas où vous n’utilisez ni l’envoi de formulaire ni l’API REST comme mécanisme de création de prospects dans votre application web, vous pouvez envisager d’utiliser MME SDK pour créer des prospects dans Marketo.
