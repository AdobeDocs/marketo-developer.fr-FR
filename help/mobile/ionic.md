---
title: '[!DNL Ionic]'
feature: Mobile Marketing
description: Guide détaillé pour l’intégration du plug-in Marketo Cordova à Ionic, l’activation des notifications push, l’initialisation de SDK, le suivi des sessions et l’association des prospects.
exl-id: 204e5fb4-c9d6-43a6-9d77-0b2a67ddbed3
TQID: https://experienceleague.adobe.com/UTNWd69NliR896RcO-XM2GG35liuLeNNhTXo9GRtB4o
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 581
ht-degree: 2%

---

# Ionique

Intégrez le plug-in Marketo Cordova à une application [!DNL Ionic]. [!DNL Ionic] Le condensateur n&#39;est actuellement pas pris en charge.

## Conditions préalables

1. [Ajoutez une application dans Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) et obtenez la clé secrète de l’application et l’ID Munchkin.
1. Configurez les notifications push pour [](push-notifications.md) ou [Android](push-notifications.md).
1. Installez [[!DNL Ionic]](https://ionicframework.com/getting-started/) et l’interface de ligne de commande [Cordova](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Instructions d’installation

### Configuration du plug-in Marketo [!DNL Ionic]

1. Accédez au répertoire de l’application [!DNL Ionic] et exécutez la commande suivante pour ajouter le module externe Marketo :

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Exécutez la commande suivante pour vérifier que le module externe a été ajouté :

   `$ ionic plugin list com.marketo.plugin 0.X.0 "MarketoPlugin"`

### Migrer vers une version plus récente (facultatif)

1. Pour supprimer un module externe existant, exécutez la commande suivante :

   `$ ionic plugin remove com.marketo.plugin`

1. Pour ajouter à nouveau le module externe, exécutez la commande suivante :

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

### Activer les notifications push dans xCode

1. Activez la fonctionnalité de notification push dans le projet xCode.![Fonction de notification](assets/notification-capability.png)

### Suivi des notifications push

Collez le code suivant dans la fonction `application:didFinishLaunchingWithOptions:`.

>[!BEGINTABS]

>[!TAB Objectif C]

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

```swift
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotfication(launchOptions)
```

>[!ENDTABS]

### Initialiser le framework Marketo

Pour initialiser le framework Marketo au démarrage de l’application, ajoutez le code suivant sous la fonction `onDeviceReady` dans le fichier JavaScript principal.

Transmettez `ionicCordova` comme type de framework pour les applications [!DNL Ionic] Cordova.

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

- Rappel de réussite : fonction à exécuter si le framework Marketo s’initialise correctement.
- Rappel d’échec : fonction à exécuter en cas d’échec d’initialisation du framework Marketo.
- MUNCHKIN ID : Munchkin ID reçu de Marketo lors de l’enregistrement.
- CLÉ SECRÈTE : clé secrète reçue de Marketo lors de l’enregistrement.

### Initialiser la notification push Marketo

Pour initialiser les notifications push Marketo, ajoutez le code suivant après la fonction initialize dans le fichier JavaScript principal.

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

- Rappel de réussite : fonction à exécuter si la notification push Marketo s’initialise correctement.
- Échec du rappel : fonction à exécuter si la notification push Marketo ne parvient pas à s’initialiser.
- GCM_PROJECT_ID : ID de projet GCM trouvé dans [Google Developers Console](https://accounts.google.com/ServiceLogin?service=cloudconsole&passive=1209600&osid=1&continue=https://console.cloud.google.com/apis/dashboard&followup=https://console.cloud.google.com/apis/dashboard) après la création de l’application.

Vous pouvez également annuler l’enregistrement du jeton lors de la déconnexion.

```javascript
marketo.uninitializeMarketoPush(
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
