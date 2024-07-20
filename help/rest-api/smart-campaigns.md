---
title: Campagnes intelligentes
feature: REST API, Smart Campaigns
description: Présentation de la campagne dynamique
exl-id: 540bdf59-b102-4081-a3d7-225494a19fdd
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '989'
ht-degree: 1%

---

# Campagnes intelligentes

[Référence du point de terminaison des campagnes dynamiques (ressource)](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns)

[Référence du point de terminaison des campagnes (Leads)](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

Marketo propose un ensemble d’API REST pour effectuer des opérations sur des campagnes intelligentes. Ces API suivent le modèle d’interface standard pour les API de ressources fournissant des options de requête, de création, de clonage et de suppression. Vous pouvez également gérer l’exécution de campagnes intelligentes en planifiant des campagnes par lots ou en demandant des campagnes de déclenchement.

## Requête

La requête de campagnes intelligentes suit les types de requête standard pour les ressources [by id](#by_id), [by name](#by_name) et [browsing](#browse).

### Par identifiant

Le point d’entrée [Get Smart Campaign by ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) utilise une seule campagne dynamique `id` comme paramètre de chemin d’accès et renvoie un seul enregistrement de campagne dynamique.

```
GET /rest/asset/v1/smartCampaign/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "7883#169838a32f0",
    "warnings": [],
    "result": [
        {
            "id": 1001,
            "name": "Process Bounced Emails",
            "description": "System smart campaign for processing bounced email events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1001,
            "flowId": 1001,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1001A1"
        }
    ]
}
```

Avec ce point de terminaison, il y aura toujours un seul enregistrement à la première position du tableau `result`.

### Par nom

Le point d’entrée [Obtenir une campagne dynamique par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) utilise une seule campagne dynamique `name` comme paramètre et renvoie un seul enregistrement de campagne dynamique.

```
GET /rest/asset/v1/smartCampaign/byName.json?name=Test Trigger Campaign
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "14494#16c886ffa44",
    "warnings": [],
    "result": [
        {
            "id": 1069,
            "name": "Test Trigger Campaign",
            "description": "",
            "createdAt": "2018-02-16T01:34:39Z+0000",
            "updatedAt": "2019-08-13T00:45:21Z+0000",
            "folder": {
                "id": 327,
                "type": "Folder"
            },
            "status": "Inactive",
            "type": "trigger",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 2747,
            "flowId": 1088,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1069A1"
        }
    ]
}
```

Avec ce point de terminaison, il y aura toujours un seul enregistrement à la première position du tableau `result`.

### Parcourir

Le point d’entrée [Get Smart Campaigns](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) fonctionne comme d’autres points d’entrée de navigation de l’API Asset et permet à plusieurs paramètres de requête facultatifs de spécifier des critères de filtrage.

Les paramètres `earliestUpdatedAt` et `latestUpdatedAt` acceptent `datetimes` au format ISO-8601 (sans millisecondes). Si les deux sont définis, le paramètre firstUpdatedAt doit précéder le paramètre latestUpdatedAt.

Le paramètre `folder` spécifie le dossier parent sous lequel naviguer. Le format est un bloc JSON contenant des attributs `id` et `type`.

Le paramètre `maxReturn` est un entier qui spécifie le nombre maximal d’entrées à renvoyer. La valeur par défaut est 20. 200 au maximum.

Le paramètre `offset` est un entier qui spécifie où commencer la récupération des entrées. Peut être utilisé conjointement avec `maxReturn`. La valeur par défaut est 0.

Le paramètre `isActive` est une valeur booléenne qui spécifie de renvoyer uniquement les campagnes Trigger actives.

```
GET /rest/asset/v1/smartCampaigns.json?earliestUpdatedAt=2016-09-10T23:15:00-00:00&latestUpdatedAt=2016-09-10T23:17:00-00:00
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "626#16983a92965",
    "warnings": [],
    "result": [
        {
            "id": 1001,
            "name": "Process Bounced Emails",
            "description": "System smart campaign for processing bounced email events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1001,
            "flowId": 1001,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1001A1"
        },
        {
            "id": 1002,
            "name": "Process Unsubscribes",
            "description": "System smart campaign for processing unsubscribe events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1002,
            "flowId": 1002,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1002A1"
        }
    ]
}
```

Avec ce point de terminaison, il y aura un ou plusieurs enregistrements dans le tableau `result`.

## Créer

Le point d’entrée [ Create Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST) est exécuté avec un POST application/x-www-form-urlencoded avec deux paramètres requis. Le paramètre `name` spécifie le nom de la campagne dynamique à créer. Le paramètre `folder` spécifie le dossier parent dans lequel la campagne dynamique est créée. Le format est un bloc JSON contenant des attributs `id` et `type`.

Vous pouvez éventuellement décrire la campagne dynamique à l’aide du paramètre `description` (2 000 caractères maximum).

```
POST /rest/asset/v1/smartCampaigns.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Smart Campaign 02&folder={"type": "folder","id": 640}&description=This is a smart campaign creation test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "25bc#16c9138f148",
    "warnings": [],
    "result": [
        {
            "id": 1076,
            "name": "Smart Campaign 02",
            "description": "This is a smart campaign creation test.",
            "createdAt": "2019-08-14T17:42:04Z+0000",
            "updatedAt": "2019-08-14T17:42:04Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": true,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5132,
            "flowId": 1095,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1076A1"
        }
    ]
}
```

## Mise à jour

Le point d’entrée [ Update Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/) est exécuté avec un POST application/x-www-form-urlencoded. Une seule campagne dynamique `id` est utilisée comme paramètre de chemin d’accès. Vous pouvez utiliser le paramètre `name` pour mettre à jour le nom de la campagne dynamique ou le paramètre `description` pour mettre à jour la description de la campagne dynamique.

```
POST /rest/asset/v1/smartCampaign/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Smart Campaign 02 Update&description=This is a smart campaign update test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "14b6a#16c924b992f",
    "warnings": [],
    "result": [
        {
            "id": 1076,
            "name": "Smart Campaign 02 Update",
            "description": "This is a smart campaign update test.",
            "createdAt": "2019-08-14T17:42:04Z+0000",
            "updatedAt": "2019-08-14T22:42:04Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": true,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5132,
            "flowId": 1095,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1076A1"
        }
    ]
}
```

## Cloner

Le point d’entrée [Clone Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) est exécuté avec un POST application/x-www-form-urlencoded avec trois paramètres requis. Il faut un paramètre `id` qui spécifie la campagne dynamique à cloner, un paramètre `name` qui spécifie le nom de la nouvelle campagne dynamique et un paramètre `folder` pour spécifier le dossier parent dans lequel la nouvelle campagne dynamique est créée. Le format est un bloc JSON contenant des attributs `id` et `type`.

Vous pouvez éventuellement décrire la campagne dynamique à l’aide du paramètre `description` (2 000 caractères maximum).

```
POST /rest/asset/v1/smartCampaign/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Test Trigger Campaign Clone&folder={"type": "folder","id": 640}&description=This is a smart campaign clone test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "681d#16c9339499b",
    "warnings": [],
    "result": [
        {
            "id": 1077,
            "name": "Test Trigger Campaign Clone",
            "description": "This is a smart campaign clone test.",
            "createdAt": "2019-08-15T03:01:41Z+0000",
            "updatedAt": "2019-08-15T03:01:41Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Inactive",
            "type": "trigger",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5135,
            "flowId": 1096,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1077A1"
        }
    ]
}
```

## Supprimer

Le point d’entrée [Supprimer la campagne dynamique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) utilise une seule campagne dynamique `id` comme paramètre de chemin d’accès.

```
POST /rest/asset/v1/smartCampaign/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d757#16c934216ac",
    "warnings": [],
    "result": [
        {
            "id": 1077
        }
    ]
}
```

## Lot

Les campagnes dynamiques par lots démarrent à un moment spécifique et affectent un ensemble spécifique de pistes toutes à la fois.

## Planning

Utilisez le point d’entrée [Planification de la campagne](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/scheduleCampaignUsingPOST) pour planifier l’exécution d’une campagne par lots immédiatement ou à une date ultérieure. La campagne `id` est un paramètre de chemin d’accès obligatoire. Les paramètres facultatifs sont `tokens`, `runAt` et `cloneToProgram` qui sont transmis dans le corps de la requête sous la forme application/json.

Le paramètre de tableau de jetons est un tableau de Mes jetons qui remplace les jetons de programme existants. Une fois la campagne exécutée, les jetons sont ignorés.  Chaque élément de tableau de jeton contient des paires nom/valeur. Le nom du jeton doit être formaté en tant que &quot;{{my.name}}&quot;.

Le paramètre runAt datetime spécifie le moment auquel exécuter la campagne. Si elle n’est pas spécifiée, la campagne sera exécutée 5 minutes après l’appel du point de fin. La valeur datetime ne peut pas être supérieure à deux ans.

Les campagnes planifiées via cette API attendent toujours au moins cinq minutes avant de s’exécuter.

Le paramètre de chaîne `cloneToProgram` contient le nom d’un programme obtenu.  Lorsque cette option est définie, la campagne, le programme parent et toutes ses ressources sont créés avec le nouveau nom qui en résulte. Le programme parent est cloné et la campagne nouvellement créée sera planifiée. Le programme obtenu est créé sous le parent. Il est possible de ne pas cloner de cette manière les programmes contenant des fragments de code, des notifications push, des messages in-app, des listes statiques, des rapports et des ressources sociales. Lorsqu’il est utilisé, ce point de terminaison est limité à 20 appels par jour. Le point d’entrée [clone program](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) est l’alternative recommandée.

```
POST /rest/v1/campaigns/{id}/schedule.json
```

```json
{
   "input":
      {
         "runAt": "2018-03-28T18:05:00+0000",
         "tokens": [
            {
               "name": "{{my.message}}",
               "value": "Updated message"
            },
            {
               "name": "{{my.other token}}",
               "value": "Value for other token"
            }
          ]
      }
}
```

```json
{
    "requestId": "52b#161d90e1743",
    "result": [
        {
            "id": 3713
        }
    ],
    "success": true
}
```

## Déclencheur

Le déclenchement de campagnes intelligentes affecte une personne à la fois en fonction d’un événement déclenché.

### Demande

Utilisez le point d’entrée [Demander la campagne](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/triggerCampaignUsingPOST) pour transmettre un ensemble de pistes à une campagne de déclenchement afin de l’exécuter dans le flux de la campagne. La campagne doit avoir un déclencheur &quot;Campaign is Requested&quot; (Campaign est demandé) avec comme source &quot;Web Service API&quot; (API de service Web).

Ce point de terminaison nécessite une campagne `id` comme paramètre de chemin d’accès et un paramètre de tableau entier `leads` contenant des ID de piste . Un maximum de 100 pistes est autorisé par appel.

Le paramètre de tableau `tokens` peut éventuellement être utilisé pour remplacer Mon jeton local par le programme parent de la campagne. `tokens` accepte un maximum de 100 jetons. Chaque élément de tableau `tokens` contient une paire nom/valeur. Le nom du jeton doit être formaté en tant que &quot;{{my.name}}&quot;. Si vous utilisez l’approche [Ajouter un jeton système en tant que lien dans un email](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) pour ajouter le jeton système &quot;viewAsWebpageLink&quot;, vous ne pouvez pas le remplacer à l’aide de `tokens`. Au lieu de cela, utilisez l&#39;approche [Ajouter une vue comme lien de page web à un email](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) qui vous permet de remplacer &quot;viewAsWebPageLink&quot; à l&#39;aide de `tokens`.

Les paramètres `leads` et `tokens` sont transmis dans le corps de la requête sous la forme application/json.

```
POST /rest/v1/campaigns/{id}/trigger.json
```

```json
{
   "input":
      {
         "leads" : [
            {
               "id" : 318592
            },
            {
               "id" : 318593
            }
         ],
         "tokens" : [
            {
               "name": "{{my.message}}",
               "value": "Updated message"
            },
            {
               "name": "{{my.other token}}",
               "value": "Value for other token"
            }
         ]
      }
}
```

```json
{
    "requestId": "9e01#161d922f1aa",
    "result": [
        {
            "id": 3712
        }
    ],
    "success": true
}
```

### Activer

Le point d’entrée [Activer la campagne dynamique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST) est simple. Un paramètre de chemin `id` est requis. Pour que l’activation réussisse, les conditions suivantes doivent être vraies pour la campagne :

- Doit être désactivé
- Posséder au moins un déclencheur et une étape de flux
- Doit comporter des déclencheurs, des filtres et des étapes de flux sans erreur

```
POST /rest/asset/v1/smartCampaign/{id}/activate.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "a33a#161d9c0dcf3",
    "result": [
        {
            "id": 1069
        }
    ]
}
```

### Désactiver

L’opération [Désactiver la campagne dynamique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) est simple. Un paramètre de chemin `id` est requis. Pour que la désactivation réussisse, la campagne doit être activée.

```
POST /rest/asset/v1/smartCampaign/{id}/deactivate.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6228#161d9c29fbf",
    "result": [
        {
            "id": 1069
        }
    ]
}
```
