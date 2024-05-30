---
title: "Campagnes intelligentes"
feature: REST API, Smart Campaigns
description: '"Smart campaign overview" (Aperçu des campagnes dynamiques)'
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '989'
ht-degree: 1%

---


# Campagnes intelligentes

[Référence du point de terminaison des campagnes dynamiques (ressource)](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns)

[Référence des points de fin de campagne (Leads)](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

Marketo propose un ensemble d’API REST pour effectuer des opérations sur des campagnes intelligentes. Ces API suivent le modèle d’interface standard pour les API de ressources fournissant des options de requête, de création, de clonage et de suppression. Vous pouvez également gérer l’exécution de campagnes intelligentes en planifiant des campagnes par lots ou en demandant des campagnes de déclenchement.

## Requête

La requête de campagnes intelligentes suit les types de requête standard pour les ressources de [par id](#by_id), [par nom](#by_name), et [navigation](#browse).

### Par identifiant

La variable [Obtention d’une campagne dynamique par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) Le point de terminaison prend une seule campagne dynamique `id` comme paramètre de chemin d’accès et renvoie un seul enregistrement de campagne dynamique.

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

Avec ce point de terminaison, il y aura toujours un seul enregistrement à la première position de la variable `result` tableau.

### Par nom

La variable [Obtention d’une campagne dynamique par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) Le point de terminaison prend une seule campagne dynamique `name` comme paramètre et renvoie un seul enregistrement de campagne dynamique.

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

Avec ce point de terminaison, il y aura toujours un seul enregistrement à la première position de la variable `result` tableau.

### Parcourir

La variable [Obtenir des campagnes dynamiques](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) Le point de terminaison fonctionne comme d’autres points de terminaison de navigation de l’API Asset et permet à plusieurs paramètres de requête facultatifs de spécifier des critères de filtrage.

La variable `earliestUpdatedAt` et `latestUpdatedAt` parameters accept `datetimes` au format ISO-8601 (sans millisecondes). Si les deux sont définis, le paramètre firstUpdatedAt doit précéder le paramètre latestUpdatedAt.

La variable `folder` spécifie le dossier parent sous lequel naviguer. Le format est un bloc JSON contenant `id` et `type` attributs.

La variable `maxReturn` est un entier qui spécifie le nombre maximal d’entrées à renvoyer. La valeur par défaut est 20. 200 au maximum.

La variable `offset` est un entier qui spécifie où commencer la récupération des entrées. Peut être utilisé conjointement avec `maxReturn`. La valeur par défaut est 0.

La variable `isActive` est une valeur booléenne qui spécifie de renvoyer uniquement les campagnes Trigger actives.

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

Avec ce point de terminaison, un ou plusieurs enregistrements seront présents dans la variable `result` tableau.

## Créer

La variable [Créer une campagne dynamique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST) endpoint est exécuté avec un POST application/x-www-form-urlencoded avec deux paramètres requis. La variable `name` spécifie le nom de la campagne dynamique à créer. La variable `folder` spécifie le dossier parent dans lequel la campagne dynamique est créée. Le format est un bloc JSON contenant `id` et `type` attributs.

Vous pouvez éventuellement décrire la campagne dynamique à l’aide de la variable `description` (2 000 caractères maximum).

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

La variable [Mettre à jour la campagne dynamique](https://developer.adobe.com/marketo-apis/api/asset/) endpoint est exécuté avec un POST encodé application/x-www-form-urlencoded. Une seule campagne dynamique est nécessaire `id` comme paramètre de chemin d’accès. Vous pouvez utiliser la variable `name` pour mettre à jour le nom de la campagne dynamique, ou la variable `description` pour mettre à jour la description de la campagne dynamique.

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

La variable [Clonage d’une campagne dynamique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) endpoint est exécuté avec un POST application/x-www-form-urlencoded avec trois paramètres requis. Cela demande une `id` qui spécifie la campagne dynamique à cloner, une `name` qui spécifie le nom de la nouvelle campagne dynamique et un `folder` pour spécifier le dossier parent dans lequel la nouvelle campagne dynamique est créée. Le format est un bloc JSON contenant `id` et `type` attributs.

Vous pouvez éventuellement décrire la campagne dynamique à l’aide de la variable `description` (2 000 caractères maximum).

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

La variable [Supprimer la campagne dynamique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) Le point de terminaison prend une seule campagne dynamique `id` comme paramètre de chemin d’accès.

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

Utilisez la variable [Planification d’une campagne](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/scheduleCampaignUsingPOST) point de fin pour planifier l’exécution immédiate ou ultérieure d’une campagne par lots. La campagne `id` est un paramètre de chemin obligatoire. Les paramètres facultatifs sont `tokens`, `runAt`, et `cloneToProgram` qui sont transmis dans le corps de la requête sous la forme application/json.

Le paramètre de tableau de jetons est un tableau de Mes jetons qui remplace les jetons de programme existants. Une fois la campagne exécutée, les jetons sont ignorés.  Chaque élément de tableau de jeton contient des paires nom/valeur. Le nom du jeton doit être formaté comme &quot;&quot;{{my.name}}&quot;.

Le paramètre runAt datetime spécifie le moment auquel exécuter la campagne. Si elle n’est pas spécifiée, la campagne sera exécutée 5 minutes après l’appel du point de fin. La valeur datetime ne peut pas être supérieure à deux ans.

Les campagnes planifiées via cette API attendent toujours au moins cinq minutes avant de s’exécuter.

La variable `cloneToProgram` Le paramètre string contient le nom d’un programme obtenu.  Lorsque cette option est définie, la campagne, le programme parent et toutes ses ressources sont créés avec le nouveau nom qui en résulte. Le programme parent est cloné et la campagne nouvellement créée sera planifiée. Le programme obtenu est créé sous le parent. Il est possible de ne pas cloner de cette manière les programmes contenant des fragments de code, des notifications push, des messages in-app, des listes statiques, des rapports et des ressources sociales. Lorsqu’il est utilisé, ce point de terminaison est limité à 20 appels par jour. La variable [programme de clonage](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) endpoint est l’alternative recommandée.

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

Utilisez la variable [Demande de campagne](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/triggerCampaignUsingPOST) point d’entrée pour transmettre un ensemble de pistes à une campagne de déclenchement à exécuter dans le flux de la campagne. La campagne doit avoir un déclencheur &quot;Campaign is Requested&quot; (Campaign est demandé) avec comme source &quot;Web Service API&quot; (API de service Web).

Ce point de terminaison nécessite une campagne `id` comme paramètre de chemin d’accès, et une `leads` paramètre de tableau entier contenant les identifiants de piste . Un maximum de 100 pistes est autorisé par appel.

Si vous le souhaitez, la variable `tokens` Le paramètre de tableau peut être utilisé pour remplacer Mon jeton local par le programme parent de la campagne. `tokens` accepte un maximum de 100 jetons. Chaque `tokens` L’élément de tableau contient une paire nom/valeur. Le nom du jeton doit être formaté comme &quot;&quot;{{my.name}}&quot;. Si vous utilisez [Ajout d’un jeton système comme lien dans un courrier électronique](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) Pour ajouter le jeton système &quot;viewAsWebpageLink&quot;, vous ne pouvez pas le remplacer à l’aide de la méthode `tokens`. Utilisez plutôt [Ajout d’une vue comme lien de page web à un courrier électronique](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) qui vous permet de remplacer &quot;viewAsWebPageLink&quot; à l’aide de `tokens`.

La variable `leads` et `tokens` Les paramètres sont transmis dans le corps de la requête sous la forme application/json.

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

La variable [Activer la campagne dynamique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST) est simple. Un `id` le paramètre path est obligatoire. Pour que l’activation réussisse, les conditions suivantes doivent être vraies pour la campagne :

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

La variable [Désactiver la campagne dynamique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) est simple. Un `id` le paramètre path est obligatoire. Pour que la désactivation réussisse, la campagne doit être activée.

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
