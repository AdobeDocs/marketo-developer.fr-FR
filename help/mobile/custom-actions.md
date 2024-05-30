---
title: "Actions personnalisées"
feature: "Mobile Marketing"
description: "Aperçu des actions personnalisées"
source-git-commit: c51e1b84efdf444c13714c1a08ecc4cac677f483
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---


# Actions personnalisées

Vous pouvez effectuer le suivi des interactions utilisateur en envoyant des actions personnalisées. Lorsque votre application mobile appelle le SDK Marketo pour envoyer une action personnalisée, l’action personnalisée est initialement enregistrée sur l’appareil. Le SDK Marketo vérifie ensuite s’il existe une connectivité Internet adéquate avant d’envoyer l’action personnalisée. Par conséquent, il peut y avoir un délai entre le moment où l’action personnalisée est envoyée et celui où elle est reçue par Marketo.

Les actions personnalisées peuvent être utilisées comme déclencheurs et filtres dans les campagnes dynamiques. Pour plus d’informations, voir [Activité de l’application mobile](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

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

Envoyez une action personnalisée avec des métadonnées.

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

1. Envoyez une action personnalisée avec des métadonnées.

   ```
   MarketoActionMetaData meta = new MarketoActionMetaData();
   meta.setActionType("Shopping");
   meta.setActionDetails("RedShirt");
   meta.setActionLength("20");
   meta.setActionMetric("30");
   
   Marketo.reportAction("Bought Shirt", meta);
   ```

1. Signalez immédiatement toutes les actions personnalisées (envoyez toutes les actions enregistrées).

   ```
   Marketo.reportAll();
   ```

## Dépannage des actions personnalisées

La configuration d’actions personnalisées mobiles est simple, mais il existe des restrictions quant au nombre de caractères que vous pouvez envoyer du SDK Mobile à Marketo. Assurez-vous que toutes les actions personnalisées qui signalent des informations à Marketo par le biais du SDK mobile comportent moins de 20 caractères.

**Remarque sur les cas d’utilisation multi-utilisateurs sur un appareil partagé :** Lorsqu’un utilisateur se connecte à une application mobile intégrée au SDK Marketo, le premier appel est effectué pour associer le prospect à l’installation de l’application. Une fois cet appel terminé, d’autres activités utilisateur de l’application sont visibles dans le journal des activités du prospect. Remarque : comme il s’agit d’un appel asynchrone, si des actions personnalisées sont consignées immédiatement après la connexion, elles peuvent être associées à l’utilisateur qui était précédemment connecté jusqu’à ce que l’appel associé réussisse.
