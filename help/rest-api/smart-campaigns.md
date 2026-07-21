---
title: Campagnes intelligentes
feature: REST API, Smart Campaigns
description: Découvrez comment utiliser les API REST Marketo pour les campagnes intelligentes, y compris la requête par identifiant ou nom, parcourir les filtres, créer une suppression de clone et planifier ou demander des déclencheurs
exl-id: 540bdf59-b102-4081-a3d7-225494a19fdd
TQID: https://experienceleague.adobe.com/iysRjtqd9plkreyIMuNjAF3YVFHtDUIrc-GInB4V8mg
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
subfeature_v2:
  - id: ad89fb33-8541-4339-afe7-bb13d1633714
  - id: d0251300-e25f-466f-9856-7e11ce8fa7aa
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1009
ht-degree: 1%

---

# Campagnes intelligentes

[Référence Des Points D’Entrée Des Campagnes Intelligentes (Ressource)](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns)

[Référence des points d’entrée des campagnes (leads)](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns)

Utilisez les API REST de campagne intelligente pour interroger, créer, cloner et supprimer des campagnes intelligentes. Vous pouvez également planifier des campagnes par lots, demander des campagnes de déclenchement et gérer l’activation des campagnes.

## Requête

Exécutez des requêtes sur les campagnes intelligentes [par identifiant](#by_id), [par nom](#by_name) ou par [navigation](#browse).

### Par Id

Le point d’entrée [Get Smart Campaign by ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) prend un seul `id` de campagne intelligente comme paramètre de chemin d’accès et renvoie un seul enregistrement de campagne intelligente.

```http
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

Le point d’entrée renvoie un enregistrement à la première position du tableau de `result`.

### Par nom

Le point d’entrée [Get Smart Campaign by Name](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) prend un seul `name` de campagne intelligente comme paramètre et renvoie un seul enregistrement de campagne intelligente.

```http
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

Le point d’entrée renvoie un enregistrement à la première position du tableau de `result`.

### Parcourir

Le point d’entrée [Obtenir les campagnes intelligentes](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) prend en charge les paramètres de requête facultatifs pour le filtrage et la pagination.

Les paramètres `earliestUpdatedAt` et `latestUpdatedAt` acceptent les `datetimes` au format ISO-8601 (sans millisecondes). Si les deux sont définis, la valeur de firstUpdatedAt doit précéder la valeur de latestUpdatedAt.

Le paramètre `folder` spécifie le dossier parent à parcourir. Transmettez-le en tant qu’objet JSON contenant `id` et `type`.

L’entier `maxReturn` spécifie le nombre maximal d’entrées. La valeur par défaut est 20 et la valeur maximale est 200.

L’entier `offset` spécifie où commencer à récupérer les entrées. Utilisez-le avec `maxReturn`. La valeur par défaut est 0.

Définissez le paramètre booléen `isActive` pour renvoyer uniquement les campagnes de déclenchement actives.

```http
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

Le point d’entrée renvoie un ou plusieurs enregistrements dans le tableau `result`.

## Créer

Envoyez une requête `application/x-www-form-urlencoded` POST au point d’entrée [Créer une campagne intelligente](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST). Les paramètres `name` et `folder` sont requis. Transmettez `folder` en tant qu’objet JSON contenant des `id` et des `type`.

Vous pouvez éventuellement décrire la campagne intelligente à l’aide du paramètre `description` (2 000 caractères maximum).

```http
POST /rest/asset/v1/smartCampaigns.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Envoyez une requête POST `application/x-www-form-urlencoded` au point d’entrée [Mise à jour de la campagne intelligente](https://developer.adobe.com/marketo-apis/api/asset). Le paramètre de chemin d’accès à la `id` Smart-Campaign est obligatoire. Utilisez `name` pour modifier le nom ou `description` pour modifier la description.

```http
POST /rest/asset/v1/smartCampaign/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
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

Envoyez une requête `application/x-www-form-urlencoded` POST au point d’entrée [Cloner une campagne intelligente](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5). Les paramètres `id`, `name` et `folder` sont requis. Ils spécifient la campagne source, le nouveau nom de campagne et le dossier parent. Transmettez `folder` en tant qu’objet JSON contenant des `id` et des `type`.

Vous pouvez éventuellement décrire la campagne intelligente à l’aide du paramètre `description` (2 000 caractères maximum).

```http
POST /rest/asset/v1/smartCampaign/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Le point d’entrée [Supprimer la campagne intelligente](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) utilise une seule `id` de campagne intelligente comme paramètre de chemin d’accès.

```http
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

Les campagnes intelligentes par lots s’exécutent à une heure spécifiée et traitent ensemble un ensemble défini de prospects.

## Programmation

Utilisez [Planifier une campagne](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns/operation/scheduleCampaignUsingPOST) pour planifier une campagne par lots. Le paramètre de chemin de `id` de la campagne est obligatoire. Transmettez les paramètres facultatifs `tokens`, `runAt` et `cloneToProgram` dans le corps de la requête JSON.

Le tableau `tokens` remplace les jetons My Tokens du programme existant pour cette exécution. Marketo ignore les remplacements après l’exécution de la campagne. Chaque élément contient une paire nom/valeur et le nom du jeton doit utiliser le format `{{my.name}}`.

Le paramètre date-heure `runAt` indique quand exécuter la campagne. Si cet attribut est omis, la campagne s’exécute cinq minutes après la requête. La valeur ne peut pas être supérieure à deux ans dans le futur.

Les campagnes planifiées via cette API attendent toujours un minimum de cinq minutes avant d’être exécutées.

Le paramètre de chaîne `cloneToProgram` contient le nom d’un programme obtenu.  Lorsqu’elle est définie, la campagne, le programme parent et toutes ses ressources sont créés avec le nouveau nom qui en résulte. Le programme parent est cloné et la campagne qui vient d’être créée est planifiée. Le programme qui en résulte est créé sous le parent. Les programmes contenant des fragments de code, des notifications push, des messages in-app, des listes statiques, des rapports et des ressources sociales ne peuvent pas être clonés de cette manière. Lorsqu’il est utilisé, ce point d’entrée est limité à 20 appels par jour. Le point d’entrée [programme de clonage](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) est l’alternative recommandée.

```http
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

Les campagnes intelligentes Trigger traitent une personne à la fois en réponse à un événement.

### Requête

Utilisez [Demande de campagne](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns/operation/triggerCampaignUsingPOST) pour transmettre des prospects par le biais du flux d’une campagne de déclenchement. La campagne doit utiliser un déclencheur Campaign est demandé avec l’API de service web comme source.

Le paramètre de chemin d’`id` de la campagne et un tableau entier `leads` d’identifiants de prospect sont requis. Chaque appel accepte un maximum de 100 prospects.

Le paramètre de tableau `tokens` peut éventuellement être utilisé pour remplacer Mes jetons en local dans le programme parent de la campagne. `tokens` accepte un maximum de 100 jetons. Chaque élément de tableau `tokens` contient une paire nom/valeur. Le nom du jeton doit être au format « `{{my.name}}` ». Si vous utilisez l’approche [Ajouter un jeton système en tant que lien dans un e-mail](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) pour ajouter le jeton système « viewAsWebpageLink », vous ne pouvez pas le remplacer à l’aide de `tokens`. Utilisez plutôt l’approche [Ajouter un lien Afficher en tant que page Web à un e-mail](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) qui vous permet de remplacer « viewAsWebPageLink » à l’aide de `tokens`.

Transmettez les paramètres `leads` et `tokens` dans le corps de la requête JSON.

```http
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

Le point d’entrée [Activer la campagne intelligente](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST) est simple. Un paramètre de chemin d’accès `id` est requis. Pour que l’activation réussisse, ce qui suit doit être vrai pour la campagne :

- La campagne est désactivée.
- La campagne comporte au moins un déclencheur et une étape de flux.
- La campagne comporte des déclencheurs, des filtres et des étapes de flux sans erreur.

```http
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

La [Désactiver la campagne intelligente](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) est simple. Un paramètre de chemin d’accès `id` est requis. Pour que la désactivation réussisse, la campagne doit être activée.

```http
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
