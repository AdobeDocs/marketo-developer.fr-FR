---
title: Campagnes intelligentes
feature: REST API, Smart Campaigns
description: Découvrez comment utiliser les API REST Marketo pour les campagnes intelligentes, y compris la requête par identifiant ou nom, parcourir les filtres, créer une suppression de clone et planifier ou demander des déclencheurs
exl-id: 540bdf59-b102-4081-a3d7-225494a19fdd
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1012'
ht-degree: 1%

---

# Campagnes intelligentes

[Référence du point d’entrée des campagnes intelligentes (ressource)](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns)

[Référence des points d’entrée des campagnes (leads)](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

Marketo propose un ensemble d’API REST pour effectuer des opérations sur des campagnes intelligentes. Ces API suivent le modèle d’interface standard des API de ressources en fournissant des options de requête, de création, de clonage et de suppression. Vous pouvez également gérer l’exécution de campagnes intelligentes en planifiant des campagnes par lots ou en demandant des campagnes de déclenchement.

## Requête

Les requêtes de campagnes intelligentes suivent les types de requête standard pour les ressources de [par identifiant](#by_id), [par nom](#by_name) et [navigation](#browse).

### Par Id

Le point d’entrée [Get Smart Campaign by ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) prend un seul `id` de campagne intelligente comme paramètre de chemin d’accès et renvoie un seul enregistrement de campagne intelligente.

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

Avec ce point d’entrée, il y aura toujours un seul enregistrement à la première position du tableau `result`.

### Par nom

Le point d’entrée [Get Smart Campaign by Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) prend un seul `name` de campagne intelligente comme paramètre et renvoie un seul enregistrement de campagne intelligente.

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

Avec ce point d’entrée, il y aura toujours un seul enregistrement à la première position du tableau `result`.

### Parcourir

Le point d’entrée [Obtenir les campagnes intelligentes](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) fonctionne comme d’autres points d’entrée de navigation de l’API Assets et permet à plusieurs paramètres de requête facultatifs de spécifier des critères de filtrage.

Les paramètres `earliestUpdatedAt` et `latestUpdatedAt` acceptent les `datetimes` au format ISO-8601 (sans millisecondes). Si les deux sont définis, la valeur de firstUpdatedAt doit précéder la valeur de latestUpdatedAt.

Le paramètre `folder` spécifie le dossier parent sous lequel naviguer. Le format est le bloc JSON contenant les attributs `id` et `type`.

Le paramètre `maxReturn` est un entier qui spécifie le nombre maximal d’entrées à renvoyer. La valeur par défaut est 20. La valeur maximale est 200.

Le paramètre `offset` est un entier qui spécifie où commencer à récupérer les entrées. Peut être utilisé conjointement avec `maxReturn`. La valeur par défaut est 0.

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

Avec ce point d’entrée, il y aura un ou plusieurs enregistrements dans le tableau `result`.

## Créer

Le point d’entrée [Créer une campagne intelligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST) est exécuté avec une requête POST application/x-www-form-urlencoded comportant deux paramètres obligatoires. Le paramètre `name` spécifie le nom de la campagne intelligente à créer. Le paramètre `folder` spécifie le dossier parent dans lequel la campagne intelligente est créée. Le format est le bloc JSON contenant les attributs `id` et `type`.

Vous pouvez éventuellement décrire la campagne intelligente à l’aide du paramètre `description` (2 000 caractères maximum).

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

Le point d’entrée [Mise à jour de campagne intelligente](https://developer.adobe.com/marketo-apis/api/asset/) est exécuté avec une requête POST application/x-www-form-urlencoded. Un seul `id` de campagne intelligente est utilisé comme paramètre de chemin d’accès. Vous pouvez utiliser le paramètre `name` pour mettre à jour le nom de la campagne intelligente ou le paramètre `description` pour mettre à jour la description de la campagne intelligente.

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

Le point d’entrée [Cloner la campagne intelligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) est exécuté avec une requête POST application/x-www-form-urlencoded comportant trois paramètres obligatoires. Il faut un paramètre `id` qui spécifie la campagne intelligente à cloner, un paramètre `name` qui spécifie le nom de la nouvelle campagne intelligente et un paramètre `folder` pour spécifier le dossier parent dans lequel la nouvelle campagne intelligente est créée. Le format est le bloc JSON contenant les attributs `id` et `type`.

Vous pouvez éventuellement décrire la campagne intelligente à l’aide du paramètre `description` (2 000 caractères maximum).

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

Le point d’entrée [Supprimer la campagne intelligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) utilise une seule `id` de campagne intelligente comme paramètre de chemin d’accès.

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

Les campagnes intelligentes par lots sont lancées à une heure spécifique et affectent simultanément un ensemble spécifique de prospects.

## Planning

Utilisez le point d’entrée [Planifier la campagne](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/scheduleCampaignUsingPOST) pour planifier une campagne par lots à exécuter immédiatement ou à une date ultérieure. Le `id` de campagne est un paramètre de chemin obligatoire. Les paramètres facultatifs sont `tokens`, `runAt` et `cloneToProgram` qui sont transmis dans le corps de la requête en tant qu’application/json.

Le paramètre de tableau de jetons est un tableau de Mes jetons qui remplacent les jetons de programme existants. Une fois la campagne exécutée, les jetons sont ignorés.  Chaque élément de tableau de jetons contient des paires nom/valeur. Le nom du jeton doit être au format « {{my.name}} ».

Le paramètre runAt datetime indique quand exécuter la campagne. Si elle n’est pas spécifiée, la campagne sera exécutée 5 minutes après l’appel du point d’entrée. La valeur datetime ne peut pas être située à plus de deux ans dans le futur.

Les campagnes planifiées via cette API attendent toujours un minimum de cinq minutes avant d’être exécutées.

Le paramètre de chaîne `cloneToProgram` contient le nom d’un programme obtenu.  Lorsqu’elle est définie, la campagne, le programme parent et toutes ses ressources sont créés avec le nouveau nom qui en résulte. Le programme parent est cloné et la campagne qui vient d’être créée est planifiée. Le programme qui en résulte est créé sous le parent. Les programmes contenant des fragments de code, des notifications push, des messages in-app, des listes statiques, des rapports et des ressources sociales ne peuvent pas être clonés de cette manière. Lorsqu’il est utilisé, ce point d’entrée est limité à 20 appels par jour. Le point d’entrée [programme de clonage](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) est l’alternative recommandée.

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

Les campagnes intelligentes de déclenchement affectent une personne à la fois en fonction d’un événement déclenché.

### Requête

Utilisez le point d’entrée [Demande de campagne](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/triggerCampaignUsingPOST) pour transmettre un ensemble de prospects à une campagne de déclenchement afin de l’exécuter dans le flux de la campagne. La campagne doit avoir un déclencheur « La campagne est demandée » avec « API de service web » comme source.

Ce point d’entrée nécessite une campagne `id` comme paramètre de chemin d’accès et un paramètre de tableau d’entiers `leads` contenant les ID de lead . Un maximum de 100 prospects est autorisé par appel.

Le paramètre de tableau `tokens` peut éventuellement être utilisé pour remplacer Mes jetons en local dans le programme parent de la campagne. `tokens` accepte un maximum de 100 jetons. Chaque élément de tableau `tokens` contient une paire nom/valeur. Le nom du jeton doit être au format « {{my.name}} ». Si vous utilisez l’approche [Ajouter un jeton système en tant que lien dans un e-mail](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) pour ajouter le jeton système « viewAsWebpageLink », vous ne pouvez pas le remplacer à l’aide de `tokens`. Utilisez plutôt l’approche [Ajouter un lien Afficher en tant que page Web à un e-mail](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) qui vous permet de remplacer « viewAsWebPageLink » à l’aide de `tokens`.

Les paramètres `leads` et `tokens` sont transmis dans le corps de la requête en tant qu’application/json.

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

Le point d’entrée [Activer la campagne intelligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST) est simple. Un paramètre de chemin d’accès `id` est requis. Pour que l’activation réussisse, ce qui suit doit être vrai pour la campagne :

- Doit être désactivé
- Doit comporter au moins un déclencheur et une étape de flux
- Doit comporter des déclencheurs, des filtres et des étapes de flux sans erreur.

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

La [Désactiver la campagne intelligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) est simple. Un paramètre de chemin d’accès `id` est requis. Pour que la désactivation réussisse, la campagne doit être activée.

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
