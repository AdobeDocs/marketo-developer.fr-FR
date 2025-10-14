---
title: Actions personnalisées
feature: Mobile Marketing
description: Découvrez comment envoyer et générer des rapports sur les actions personnalisées avec Marketo Mobile SDK pour iOS et Android, mettre les actions en file d’attente hors ligne, déclencher des campagnes intelligentes et ... .
exl-id: 8c2698ce-4e39-4b2b-9d36-0864c55be17a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---

# Actions personnalisées

Vous pouvez suivre l’interaction des utilisateurs et utilisatrices en envoyant des actions personnalisées. Lorsque votre application mobile appelle le SDK Marketo pour envoyer une action personnalisée, celle-ci est initialement enregistrée sur l’appareil. Le SDK Marketo vérifie ensuite s’il existe une connectivité Internet adéquate avant d’envoyer l’action personnalisée. Par conséquent, il peut y avoir un délai entre le moment où l’action personnalisée est envoyée et celui où elle est reçue par Marketo.

Les actions personnalisées peuvent être utilisées comme déclencheurs et filtres dans les campagnes intelligentes. Pour plus d’informations, voir [&#x200B; Activité des applications mobiles &#x200B;](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Envoi d’actions personnalisées sur iOS

Envoyer une action personnalisée.

>[!BEGINTABS]

>[!TAB Objectif C]

```
Marketo *sharedInstance = [Marketo sharedInstance];
[sharedInstance reportAction:@"Login" withMetaData:nil];
```

>[!TAB Swift]

```
sharedInstance.reportAction("Login", withMetaData:nil);
```

>[!ENDTABS]

Envoyer une action personnalisée avec des métadonnées

>[!BEGINTABS]

>[!TAB Objectif C]

```
MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
[meta setType:@"Shopping"];
[meta setDetails:@"RedShirt"];
[meta setLength:20];
[meta setMetric:30];

[sharedInstance reportAction:@"Bought Shirt" withMetaData:meta];
```

>[!TAB Swift]

```
let meta = MarketoActionMetaData()
meta.setType("Shopping");
meta.setDetails("RedShirt");
meta.setLength(20);
meta.setMetric(30);

sharedInstance.reportAction("Bought Shirt", withMetaData:meta);
```

>[!ENDTABS]

Signaler immédiatement toutes les actions (envoyer toutes les actions enregistrées).

>[!BEGINTABS]

>[!TAB Objectif C]

```
[sharedInstance reportAll];
```

>[!TAB Swift]

```
sharedInstance.reportAll();
```

>[!ENDTABS]

## Envoi d’actions personnalisées sur Android

1. Envoyer une action personnalisée.

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

1. Signaler immédiatement toutes les actions personnalisées (envoyer toutes les actions enregistrées).

   ```
   Marketo.reportAll();
   ```

## Résolution des problèmes liés aux actions personnalisées

La configuration des actions personnalisées pour appareils mobiles est simple, mais il existe des restrictions quant au nombre de caractères que vous pouvez envoyer de Mobile SDK vers Marketo. Assurez-vous que toutes vos actions personnalisées qui génèrent des rapports sur Marketo via le SDK mobile comportent moins de 20 caractères.

**Remarque concernant les cas pratiques multi-utilisateurs sur un appareil partagé :** lorsqu’un utilisateur se connecte à une application mobile intégrée à Marketo SDK, le premier appel est effectué pour associer le prospect à l’installation de l’application. Une fois cet appel terminé, d’autres activités utilisateur dans l’application sont visibles dans le journal d’activité du prospect. Notez qu’il s’agit d’un appel asynchrone. En effet, si des actions personnalisées sont consignées immédiatement après la connexion, elles peuvent être associées à l’utilisateur précédemment connecté jusqu’à ce que l’appel associé réussisse.
