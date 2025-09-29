---
title: Membres du programme
feature: REST API
description: Utilisez l’API REST Marketo pour lire, créer, mettre à jour et supprimer des membres de programme, gérer des champs standard et personnalisés et effectuer des requêtes à l’aide de champs pouvant faire l’objet de recherches.
exl-id: 22f29a42-2a30-4dce-a571-d7776374cf43
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 2%

---

# Membres du programme

[Référence des points d’entrée des membres du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members)

Marketo propose des API pour lire, créer, mettre à jour et supprimer des enregistrements de membre de programme. Les enregistrements des membres du programme sont associés aux enregistrements de lead via le champ ID de lead. Les enregistrements sont composés d’un ensemble de champs standard et éventuellement de 20 champs personnalisés supplémentaires. Les champs contiennent des données spécifiques au programme pour chaque membre et peuvent être utilisés dans des formulaires, des filtres, des déclencheurs et des actions de flux. Ces données sont visibles dans l’onglet [ Membres ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members) du programme dans l’interface utilisateur de Marketo Engage.

## Décrire

Le point d’entrée [Décrire le membre de programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) suit le modèle standard pour les objets de base de données de prospect. Le tableau `searchableFields` vous donne l’ensemble des champs valides pour l’interrogation. Le tableau `fields` contient des métadonnées de champ, notamment le nom de l’API REST, le nom d’affichage et la possibilité de mise à jour des champs.

```
GET /rest/v1/programs/members/describe.json
```

```json
{
    "requestId": "f813#1791563c7cc",
    "result": [
        {
            "name": "API Program Membership",
            "description": "Map for API program membership fields",
            "createdAt": "2021-03-20T01:30:05Z",
            "updatedAt": "2021-03-20T01:30:05Z",
            "dedupeFields": [
                "leadId",
                "programId"
            ],
            "searchableFields": [
                [
                    "leadId"
                ],
                [
                    "myCustomField"
                ],
                [
                    "reachedSuccess"
                ],
                [
                    "statusName"
                ]
            ],
            "fields": [
                {
                    "name": "acquiredBy",
                    "displayName": "acquiredBy",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "attendanceLikelihood",
                    "displayName": "attendanceLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "createdAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "isExhausted",
                    "displayName": "isExhausted",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "leadId",
                    "displayName": "leadId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "membershipDate",
                    "displayName": "membershipDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "nurtureCadence",
                    "displayName": "nurtureCadence",
                    "dataType": "string",
                    "length": 4,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "program",
                    "displayName": "program",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "programId",
                    "displayName": "programId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccess",
                    "displayName": "reachedSuccess",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccessDate",
                    "displayName": "reachedSuccessDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "registrationLikelihood",
                    "displayName": "registrationLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusName",
                    "displayName": "statusName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusReason",
                    "displayName": "statusReason",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "trackName",
                    "displayName": "trackName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "updatedAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "waitlistPriority",
                    "displayName": "waitlistPriority",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "myCustomField",
                    "displayName": "myCustomField",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "registrationCode",
                    "displayName": "registrationCode",
                    "dataType": "string",
                    "length": 100,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "webinarUrl",
                    "displayName": "webinarUrl",
                    "dataType": "string",
                    "length": 2000,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

## Requête

Le point d’entrée [Obtenir les membres du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMembersUsingGET) vous permet de récupérer les membres d’un programme. Elle nécessite un paramètre de chemin d’accès `programId`, ainsi que des paramètres de requête `filterType` et `filterValues`.

`programId` est utilisé pour spécifier le programme à rechercher.

`filterType` est utilisé pour spécifier le champ à utiliser comme filtre de recherche. Il accepte n’importe quel champ de la liste « searchableFields » renvoyée par le point d’entrée [Décrire le membre de programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2). Si vous spécifiez un filterType qui est un champ personnalisé, le dataType du champ personnalisé doit être « string » ou « integer ». Si vous spécifiez un filterType autre que « leadId », 100 000 enregistrements de membre de programme au maximum peuvent être traités par la requête. Selon la configuration de votre instance Marketo, vous recevez l’une des erreurs suivantes :

- Si le nombre total de membres du programme dépasse 100 000, une erreur est renvoyée : « 1003, Taille totale des membres : 100 001 dépasse la limite autorisée de 100 000 pour le filtre ».
- Si le nombre total de membres du programme _qui correspondent au filtre_ dépasse 100 000, une erreur est renvoyée : « 1003, Taille d’abonnement correspondante : 100 001 dépasse la limite autorisée (100 000) pour cette api ».

Pour interroger un programme dont le nombre d’adhésions dépasse la limite, utilisez plutôt l’API [Bulk Program Member Extract](bulk-program-member-extract.md).

`filterValues` est utilisé pour spécifier les valeurs à rechercher et accepte jusqu’à 300 valeurs dans un format séparé par des virgules. L’appel recherche les enregistrements pour lesquels le champ du membre du programme correspond à l’une des filterValues incluses.

Vous pouvez également filtrer par période en spécifiant `updatedAt` comme filterType avec les paramètres datetime `startAt` et `endAt`. La plage doit être de sept jours ou moins. Les heures de date doivent être au format ISO-8601, sans millisecondes.

Le paramètre de requête `fields` facultatif accepte une liste séparée par des virgules de noms d’API de champ renvoyés par le point d’entrée [Décrire le membre de programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2). Lorsqu’il est inclus, chaque enregistrement de la réponse inclut les champs spécifiés. Si cet attribut est omis, l’ensemble par défaut des champs renvoyés est `acquiredBy`, `leadId`, `membershipDate`, `programId` et `reachedSuccess`.

Par défaut, un maximum de 300 enregistrements est renvoyé. Vous pouvez utiliser le paramètre de requête `batchSize` pour réduire ce nombre. Si l’attribut **moreResult** est défini sur true, cela signifie que davantage de résultats sont disponibles. Continuez à appeler ce point d’entrée jusqu’à ce que l’attribut moreResult renvoie false, ce qui signifie qu’aucun résultat n’est disponible. Les `nextPageToken` renvoyés par cette API doivent toujours être réutilisés pour l’itération suivante de cet appel.

Si la longueur totale de votre requête GET dépasse 8 Ko, une erreur HTTP est renvoyée : « 414, URI trop long ». Pour pallier ce problème, vous pouvez remplacer votre GET par POST, ajouter `_method=GET` paramètre et placer une chaîne de requête dans le corps de la requête.

```
GET /rest/v1/programs/{programId}/members.json?filterType=statusName&filterValues=Influenced
```

```json
{
    "requestId": "109da#17915eec072",
    "result": [
        {
            "seq": 0,
            "leadId": 1789,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 1,
            "leadId": 1790,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 2,
            "leadId": 1791,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 3,
            "leadId": 1792,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 4,
            "leadId": 1793,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 5,
            "leadId": 1794,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 6,
            "leadId": 1795,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 7,
            "leadId": 1796,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 8,
            "leadId": 1797,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 9,
            "leadId": 1798,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 10,
            "leadId": 1799,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 11,
            "leadId": 1800,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        }
    ],
    "success": true,
    "moreResult": false
}
```

## Créer et mettre à jour

Deux points d’entrée prennent en charge l’opération de création/mise à jour sur les membres du programme. L’un d’eux vous permet de mettre à jour uniquement le statut de membre du programme. L’autre vous permet de mettre à jour l’ensemble des champs des membres du programme marqués comme « modifiables ». Les deux points d’entrée vous permettent de modifier jusqu’à 300 enregistrements de membre de programme par appel.

### Statut de membre du programme

Le point d’entrée [ Synchroniser le statut des membres du programme ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberStatusUsingPOST) permet de créer ou de mettre à jour le statut du programme pour un ou plusieurs membres.

Le paramètre de chemin d’accès `programId` obligatoire spécifie le programme contenant les membres à créer ou à mettre à jour.

Le paramètre `statusName` requis spécifie le statut du programme à appliquer à une liste de prospects. Le statusName doit correspondre à un statut disponible pour le canal du programme. Les statuts valides peuvent être récupérés à l’aide du point d’entrée [Obtenir les canaux](https://developer.adobe.com/marketo-apis/api/asset/#tag/Channels/operation/getAllChannelsUsingGET). Si le statut d’un prospect a une valeur d’étape supérieure à la valeur statusName désignée, ce prospect est ignoré.

Le paramètre `input` requis est un tableau de `leadId` qui correspondent aux membres du programme. Vous pouvez soumettre jusqu’à 300 leadId par appel. Une opération d’upsert est effectuée sur chaque enregistrement. Si l’ID de prospect est associé à un membre du programme, son statut d’abonnement est mis à jour. Dans le cas contraire, un nouvel enregistrement de membre du programme est créé, l’enregistrement est associé à l’ID de lead et le statut d’abonnement est attribué.

Le point d’entrée répond avec une `status` de « mis à jour », « créé » ou « ignoré ». Si cette opération est ignorée, un tableau `reasons` est également inclus. Le point d’entrée répond également avec un champ `seq` qui est un index qui peut être utilisé pour mettre en corrélation les enregistrements envoyés avec l’ordre de la réponse.

Si l’appel aboutit, une activité « Modifier le statut du programme » est consignée dans le journal d’activité du prospect.

```
POST /rest/v1/programs/{programId}/members/status.json
```

```
Content-Type: application/json
```

```json
{
    "statusName":"Influenced",
    "input":[
        {
            "leadId": 1800
        },
        {
            "leadId": 1801
        },
        {
            "leadId": 1235
        }
    ]
}
```

```json
{
    "requestId": "14b2d#17916378ec5",
    "result": [
        {
            "seq": 0,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1037",
                    "message": "Lead skipped because it is already in or past this status"
                }
            ]
        },
        {
            "seq": 1,
            "status": "updated",
            "leadId": 1801
        },
        {
            "seq": 2,
            "status": "created",
            "leadId": 1235
        }
    ],
    "success": true
}
```

### Données des membres du programme

Le point d’entrée [Synchroniser les données des membres du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberDataUsingPOST) est utilisé pour mettre à jour les données de champ des membres du programme pour un ou plusieurs membres. Vous pouvez modifier n’importe quel champ personnalisé ou champ standard « modifiable » (voir [Description du membre de programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) point d’entrée).

Le paramètre de chemin d’accès `programId` obligatoire spécifie le programme contenant les membres à mettre à jour.

Le paramètre `input` requis est un tableau . Chaque élément de tableau contient un `leadId` et un ou plusieurs champs à mettre à jour (à l’aide du nom de l’API). Une opération de mise à jour est effectuée sur chaque enregistrement. L’ID de lead doit être associé à un membre du programme. Les champs doivent être modifiables. Vous pouvez soumettre jusqu’à 300 leadId par appel.

Le point d’entrée répond avec une `status` de « mise à jour » ou « ignoré ». Si cette opération est ignorée, un tableau `reasons` est également inclus. Le point d’entrée répond également avec un champ `seq` qui est un index qui peut être utilisé pour mettre en corrélation les enregistrements envoyés avec l’ordre de la réponse.

Si l’appel aboutit, une activité « Modifier les données du membre du programme » est consignée dans le journal d’activité du prospect.

```
POST /rest/v1/programs/{programId}/members.json
```

```
Content-Type: application/json
```

```json
{
    "input":[
        {
            "leadId": 1789,
            "registrationCode": "dcff5f12-a7c7-11eb-bcbc-0242ac130002"
        },
        {
            "leadId": 1790,
            "registrationCode": "c0404b78-d3fd-47bf-82c4-d16f3852ab3a"
        },
        {
            "leadId": 1003,
            "registrationCode": "aa880c57-75b8-426b-a33a-fbf6302d7cb4"
        }
    ]
}
```

```json
{
    "requestId": "edc3#1791659b8d2",
    "result": [
        {
            "seq": 0,
            "status": "updated",
            "leadId": 1789
        },
        {
            "seq": 1,
            "status": "updated",
            "leadId": 1790
        },
        {
            "seq": 2,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1013",
                    "message": "Membership not found"
                }
            ]
        }
    ],
    "success": true
}
```

## Champs

L’objet membre de programme contient des champs standard et des champs personnalisés facultatifs. Des champs standard sont présents dans chaque abonnement Marketo Engage, tandis que des champs personnalisés sont créés par l’utilisateur selon les besoins. Chaque définition de champ est composée d’un ensemble d’attributs qui décrivent le champ. Les exemples d’attributs sont le nom d’affichage, le nom de l’API et dataType. Ces attributs sont collectivement appelés métadonnées.

Les points d’entrée suivants vous permettent d’interroger, de créer et de mettre à jour des champs sur l’objet membre de programme. Ces API nécessitent que l&#39;utilisateur propriétaire de l&#39;API dispose d&#39;un rôle avec l&#39;une ou l&#39;autre des autorisations **Champ standard de schéma en lecture-écriture** ou **Champ personnalisé de schéma en lecture-écriture**.

### Champs de requête

L’interrogation des champs de membre de programme est simple. Vous pouvez interroger un seul champ de membre de programme par nom d’API ou interroger l’ensemble de tous les champs de membre de programme. Les champs standard et personnalisés peuvent être récupérés, selon les autorisations de rôle utilisées. Les champs masqués sont également récupérés.

#### Par nom

Le point d’entrée [Obtenir le champ de membre de programme par nom](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldByNameUsingGET) récupère les métadonnées d’un seul champ sur l’objet de membre de programme. Le paramètre de chemin d’accès `fieldApiName` obligatoire spécifie le nom d’API du champ. La réponse est similaire au point d’entrée Describe Program Member , mais elle contient des métadonnées supplémentaires telles que l’attribut `isCustom` qui indique si le champ est un champ personnalisé.

```
GET /rest/v1/programs/members/schema/fields/{fieldApiName}.json
```

```json
{
    "requestId": "15416#17e955554de",
    "result": [
        {
            "displayName": "Status",
            "name": "statusName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true
}
```

#### Parcourir

Le point d’entrée [Obtenir les champs de membre de programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldsUsingGET) récupère les métadonnées de tous les champs de l’objet de membre de programme. Par défaut, un maximum de 300 enregistrements est renvoyé. Vous pouvez utiliser le paramètre de requête `batchSize` pour réduire ce nombre. Si l’attribut `moreResult` est défini sur « true », cela signifie que d’autres résultats sont disponibles. Continuez à appeler ce point d’entrée jusqu’à ce que l’attribut moreResult renvoie false, ce qui signifie qu’aucun résultat n’est disponible. Les `nextPageToken` renvoyés par cette API doivent toujours être réutilisés pour l’itération suivante de cet appel.

```
GET /rest/v1/programs/members/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "102f6#17e9557f123",
    "result": [
        {
            "displayName": "Acquired By",
            "name": "acquiredBy",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Nurture Cadence",
            "name": "nurtureCadence",
            "description": null,
            "dataType": "string",
            "length": 4,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Nurture Exhausted",
            "name": "isExhausted",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Member Date",
            "name": "membershipDate",
            "description": null,
            "dataType": "datetime",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Program",
            "name": "program",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "BC7J6EPVLT6T4B5FKUU3APCYN4======",
    "moreResult": true
}
```

### Créer des champs

Le point d’entrée [Créer des champs de membre de programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/createProgramMemberFieldUsingPOST) crée un ou plusieurs champs personnalisés sur l’objet de membre de programme. Ce point d’entrée fournit une fonctionnalité comparable à ce qui est [disponible dans l’interface utilisateur de Marketo Engage](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields). Vous pouvez créer jusqu’à 20 champs personnalisés à l’aide de ce point d’entrée.

Examinez attentivement chaque champ que vous créez dans votre instance de production de Marketo Engage à l’aide de l’API. Une fois un champ créé, vous ne pouvez pas le supprimer ([vous pouvez uniquement le masquer](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo)). La prolifération des champs inutilisés est une mauvaise pratique qui encombrera votre instance.

Le paramètre `input` requis est un tableau d’objets de champ de membre de programme. Chaque objet contient un ou plusieurs attributs. Les attributs obligatoires sont les `displayName`, `name` et `dataType` qui correspondent respectivement au nom d’affichage de l’interface utilisateur du champ, au nom d’API du champ et au type de champ. Vous pouvez éventuellement spécifier `description`, `isHidden`, `isHtmlEncodingInEmail` et `isSensitive`.

Quelques règles sont associées à la dénomination des `name` et des `displayName`. L’attribut `name` doit être unique, commencer par une lettre et ne contenir que des lettres, des chiffres ou des traits de soulignement. Le *`isplayName` doit être unique et ne peut pas contenir de caractères spéciaux. Une convention de nommage courante consiste à appliquer [casse mixte](https://en.wikipedia.org/wiki/Camel_case#) aux `displayName` pour produire des `name`. Par exemple, un `displayName` de « Mon champ personnalisé » génère un `name` de « myCustomField ».

```
POST /rest/v1/programs/members/schema/fields.json
```

```json
{
  "input": [
    {
        "displayName": "PMCF Custom Field 03",
        "name": "pMCFCustomField03",
        "description": "My third custom field",
        "dataType": "string"
    }
  ]
}
```

```json
{
    "requestId": "13a7#17e955fcb44",
    "result": [
        {
            "name": "pMCFCustomField03",
            "status": "created"
        }
    ],
    "success": true
}
```

### Mettre à jour le champ

Le point d’entrée [Mettre à jour le champ de membre de programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/updateProgramMemberFieldUsingPOST) met à jour un seul champ personnalisé sur l’objet de membre de programme. En règle générale, les opérations de mise à jour des champs effectuées à l’aide de l’interface utilisateur Marketo Engage sont réalisables avec l’API . Quelques différences sont résumées dans le tableau ci-dessous.

| Attribut | Mis à jour par l’API ? | Peut-on les mettre à jour par l’interface utilisateur ? | Mis à jour par l’API ? | Peut-on les mettre à jour par l’interface utilisateur ? |
|---|---|---|---|---|
| dataType | non | non | non | oui |
| description | oui | oui | oui | oui |
| displayName | non | non | oui | oui |
| isCustom | non | non | non | non |
| isHidden | non | oui | oui (si créé par l’API) | oui |
| isHtmlEncodingInEmail | oui | oui | oui | oui |
| isSensible | oui | oui | oui | oui |
| length | non | non | non | non |
| name | non | non | non | non |

Le paramètre de chemin d’accès `fieldApiName` obligatoire spécifie le nom d’API du champ à mettre à jour. Le paramètre `input` requis est un tableau contenant un seul objet de champ de prospect. L’objet de champ contient un ou plusieurs attributs.

```
POST /rest/v1/programs/members/schema/fields/pMCFCustomField03.json
```

```json
{
  "input": [
      {
        "displayName": "Lunch Preference",
        "description": "Attendee food preference",
        "isHtmlEncodingInEmail": true
      }
  ]
}
```

```json
{
    "requestId": "215f#17e95663955",
    "result": [
        {
            "name": "pMCFCustomField03",
            "status": "updated"
        }
    ],
    "success": true
}
```

## Supprimer

Le point d’entrée [Supprimer les membres du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/deleteProgramMemberUsingPOST) est utilisé pour supprimer les enregistrements de membre de programme. Le paramètre de chemin d’accès `programId` obligatoire spécifie le programme contenant les membres à supprimer. Le corps de la requête contient un tableau `input` d’ID de prospect. 300 ID de lead au maximum  sont autorisés par appel.

Le point d’entrée répond avec une `status` de « supprimé » ou « ignoré ». Si cette opération est ignorée, un tableau `reasons` est également inclus. Le point d’entrée répond également avec un champ `seq` qui est un index qui peut être utilisé pour mettre en corrélation les enregistrements envoyés avec l’ordre de la réponse.

```
POST /rest/v1/programs/{programId}/members/delete.json
```

```
Content-Type: application/json
```

```json
{
    "input":[
        {
            "leadId": 1235
        },
        {
            "leadId": 77
        }
    ]
}
```

```json
{
    "requestId": "302a#17916619417",
    "result": [
        {
            "seq": 0,
            "status": "deleted",
            "leadId": 1235
        },
        {
            "seq": 1,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1037",
                    "message": "Lead not in program"
                }
            ]
        }
    ],
    "success": true
}
```
