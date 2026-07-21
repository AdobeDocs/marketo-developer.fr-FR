---
title: Actions personnalisées
feature: Mobile Marketing
description: Découvrez comment envoyer et générer des rapports sur les actions personnalisées avec Marketo Mobile SDK pour iOS et Android, mettre les actions en file d’attente hors ligne, déclencher des campagnes intelligentes et ... .
exl-id: 8c2698ce-4e39-4b2b-9d36-0864c55be17a
TQID: https://experienceleague.adobe.com/yZKzdm-dH0cYPGGKE-Z-4KcbhGIwyFl0Z9vEqcv1QXI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 259
ht-degree: 2%

---

# Actions personnalisées

Les actions personnalisées effectuent le suivi des interactions utilisateur dans votre application mobile. Lorsque l’application appelle le SDK Marketo pour envoyer une action personnalisée, le SDK enregistre d’abord l’action sur l’appareil. Le SDK envoie l’action lorsqu’il détecte une connectivité Internet adéquate. Il se peut donc que Marketo reçoive l’action après un délai.

Les actions personnalisées peuvent être utilisées comme déclencheurs et filtres dans les campagnes intelligentes. Pour plus d’informations, voir [&#x200B; Activité des applications mobiles &#x200B;](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Envoi d’actions personnalisées sur iOS

Envoyez une action personnalisée.

>[!BEGINTABS]

>[!TAB Objectif C]

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];
[sharedInstance reportAction:@"Login" withMetaData:nil];
```

>[!TAB Swift]

```swift
sharedInstance.reportAction("Login", withMetaData:nil);
```

>[!ENDTABS]

Envoyer une action personnalisée avec des métadonnées

>[!BEGINTABS]

>[!TAB Objectif C]

```objectivec
MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
[meta setType:@"Shopping"];
[meta setDetails:@"RedShirt"];
[meta setLength:20];
[meta setMetric:30];

[sharedInstance reportAction:@"Bought Shirt" withMetaData:meta];
```

>[!TAB Swift]

```swift
let meta = MarketoActionMetaData()
meta.setType("Shopping");
meta.setDetails("RedShirt");
meta.setLength(20);
meta.setMetric(30);

sharedInstance.reportAction("Bought Shirt", withMetaData:meta);
```

>[!ENDTABS]

Signaler immédiatement toutes les actions enregistrées.

>[!BEGINTABS]

>[!TAB Objectif C]

```objectivec
[sharedInstance reportAll];
```

>[!TAB Swift]

```swift
sharedInstance.reportAll();
```

>[!ENDTABS]

## Envoi d’actions personnalisées sur Android

1. Envoyez une action personnalisée.

   ```
   Marketo.reportAction("Login", null);
   ```

1. Envoyer une action personnalisée avec des métadonnées

   ```
   MarketoActionMetaData meta = new MarketoActionMetaData();
   meta.setActionType("Shopping");
   meta.setActionDetails("RedShirt");
   meta.setActionLength("20");
   meta.setActionMetric("30");
   
   Marketo.reportAction("Bought Shirt", meta);
   ```

1. Signalez immédiatement toutes les actions personnalisées enregistrées.

   ```
   Marketo.reportAll();
   ```

## Résoudre les problèmes liés aux actions personnalisées

Les noms des actions personnalisées envoyés de Mobile SDK vers Marketo doivent comporter moins de 20 caractères.

**Cas pratiques multi-utilisateurs sur un appareil partagé :** lorsqu’un utilisateur se connecte à une application mobile qui utilise Marketo SDK, le premier appel associe le prospect à l’installation de l’application. Une fois l’appel réussi, les activités utilisateur suivantes apparaissent dans le journal d’activité du prospect.

L’appel d’association est asynchrone. Les actions personnalisées consignées immédiatement après la connexion peuvent être associées à l’utilisateur précédemment connecté jusqu’à ce que l’appel réussisse.
