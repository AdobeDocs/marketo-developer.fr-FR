---
title: "Membres du programme"
feature: REST API
description: "Créez et gérez les membres du programme."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1712'
ht-degree: 0%

---


# Membres du programme

[Référence du point de terminaison des membres du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members)

Marketo expose les API pour la lecture, la création, la mise à jour et la suppression des enregistrements de membres du programme. Les enregistrements de membre du programme sont associés aux enregistrements de piste via le champ ID de piste . Les enregistrements sont composés d’un ensemble de champs standard et éventuellement de 20 champs personnalisés supplémentaires. Les champs contiennent des données spécifiques au programme pour chaque membre et peuvent être utilisés dans des formulaires, des filtres, des déclencheurs et des actions de flux. Ces données sont consultables dans la [Onglet Membres](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members) dans l’interface utilisateur de Marketo Engage.

## Description

La variable [Description du membre du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) endpoint suit le modèle standard pour les objets de base de données de piste. La variable `searchableFields` tableau vous donne l’ensemble des champs valides pour l’interrogation. La variable `fields` contient des métadonnées de champ, notamment le nom de l’API REST, le nom d’affichage et la capacité de mise à jour des champs.

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

La variable [Obtention des membres du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMembersUsingGET) endpoint vous permet de récupérer les membres d’un programme. Cela nécessite une `programId` paramètre path et `filterType` et `filterValues` paramètres de requête.

`programId` est utilisé pour spécifier le programme à rechercher.

`filterType` sert à spécifier le champ à utiliser comme filtre de recherche. Il accepte tous les champs de la liste &quot;searchableFields&quot; renvoyés par la variable [Description du membre du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) point de terminaison . Si vous spécifiez un filterType qui est un champ personnalisé, le dataType du champ personnalisé doit être &quot;string&quot; ou &quot;integer&quot;. Si vous spécifiez un filterType autre que &quot;leadId&quot;, un maximum de 100 000 enregistrements de membres du programme peut être traité par la requête. Selon la configuration de votre instance Marketo, vous recevez l’une des erreurs suivantes :

- Si le nombre total de membres du programme dépasse 100 000, une erreur est renvoyée : &quot;1003, Nombre total d&#39;inscrits : 100 001 au-delà de la limite autorisée de 100 000 pour le filtre&quot;.
- Si le nombre total de membres du programme _qui correspondent au filtre_ supérieur à 100 000, une erreur est renvoyée : &quot;1003, taille d’adhésion correspondante : 100 001 dépasse la limite autorisée (100 000) pour cette api&quot;.

Pour interroger un programme dont le nombre d’adhésions dépasse la limite autorisée, utilisez la variable [API Bulk Program Member Extract](bulk-program-member-extract.md) au lieu de .

`filterValues` sert à spécifier les valeurs à rechercher et accepte jusqu’à 300 valeurs dans un format séparé par des virgules. L’appel recherche des enregistrements dont le champ du membre du programme correspond à l’une des valeurs filterValues incluses.

Vous pouvez également filtrer par période en spécifiant `updatedAt` as filterType with `startAt` et `endAt` paramètres datetime. La période doit être de sept jours ou moins. Datetimes doit être au format ISO-8601, sans millisecondes.

Le paramètre facultatif `fields` le paramètre de requête accepte une liste de noms d’API de champ séparés par des virgules qui sont renvoyés par la variable [Description du membre du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) point de terminaison . Lorsqu’il est inclus, chaque enregistrement de la réponse inclut les champs spécifiés. Lorsque cette valeur est omise, le jeu de champs par défaut renvoyé est `acquiredBy`, `leadId`, `membershipDate`, `programId`, et `reachedSuccess`.

Par défaut, un maximum de 300 enregistrements est renvoyé. Vous pouvez utiliser la variable `batchSize` paramètre de requête pour réduire ce nombre. Si la variable **moreResult** est true, cela signifie que plus de résultats sont disponibles. Continuez à appeler ce point de terminaison jusqu’à ce que l’attribut moreResult renvoie false, ce qui signifie qu’il n’y a aucun résultat disponible. La variable `nextPageToken` La valeur renvoyée par cette API doit toujours être réutilisée pour la prochaine itération de cet appel.

Si la longueur totale de votre requête de GET dépasse 8 Ko, une erreur HTTP est renvoyée : &quot;414, URI trop long&quot; (par [RFC 7231](https://datatracker.ietf.org/doc/html/rfc72316.5.12)). Pour pallier ce problème, vous pouvez remplacer votre GET par POST et ajouter `_method=GET` et placez la chaîne de requête dans le corps de la requête.

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

Deux points de terminaison prennent en charge l’opération de création/mise à jour sur les membres du programme. L’un vous permet de mettre à jour uniquement le statut des membres du programme. L’autre permet de mettre à jour l’ensemble des champs des membres du programme marqués comme &quot;modifiables&quot;. Les deux points de terminaison vous permettent de modifier jusqu’à 300 enregistrements de membres de programme par appel.

### État du membre du programme

La variable [État membre du programme de synchronisation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberStatusUsingPOST) endpoint sert à créer ou à mettre à jour l’état du programme pour un ou plusieurs membres.

La variable `programId` path spécifie le programme contenant les membres à créer ou à mettre à jour.

La variable `statusName` spécifie l’état du programme à appliquer à une liste de pistes. statusName doit correspondre à un état disponible pour le canal du programme. Les états valides peuvent être récupérés à l’aide de la variable [Obtention de canaux](https://developer.adobe.com/marketo-apis/api/asset/#tag/Channels/operation/getAllChannelsUsingGET) point de terminaison . Si l’état d’une piste a une valeur d’étape supérieure à celle du statusName désigné, cette piste sera ignorée.

La variable `input` est un tableau de `leadId` qui correspondent aux membres du programme. Vous pouvez envoyer jusqu’à 300 LeadIds par appel. Une opération upsert est effectuée sur chaque enregistrement. Si le leadId est associé à un membre du programme, son statut d’adhésion est mis à jour. Si ce n’est pas le cas, un nouvel enregistrement de membre de programme est créé, l’enregistrement est associé au leadId et l’état d’adhésion est attribué.

Le point de terminaison répond par une `status` de &quot;mis à jour&quot;, &quot;créé&quot; ou &quot;sauté&quot;. Si vous ignorez, une `reasons` est également inclus. Le point de terminaison répond également avec une `seq` champ qui est un index qui peut être utilisé pour mettre en corrélation les enregistrements envoyés avec l’ordre de la réponse.

Si l’appel aboutit, une activité &quot;Modifier l’état du programme&quot; est écrite dans le journal des activités du prospect.

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

### Données du membre du programme

La variable [Synchroniser les données des membres du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberDataUsingPOST) endpoint permet de mettre à jour les données de champ des membres du programme pour un ou plusieurs membres. Vous pouvez modifier n’importe quel champ personnalisé ou champ standard &quot;modifiable&quot; (voir [Description du membre du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) point de terminaison).

La variable `programId` path spécifie le programme contenant les membres à mettre à jour.

La variable `input` est un tableau. Chaque élément de tableau contient une `leadId` et un ou plusieurs champs à mettre à jour (à l’aide du nom de l’API). Une opération de mise à jour est effectuée sur chaque enregistrement. Le leadId doit être associé à un membre du programme. Les champs doivent pouvoir être mis à jour. Vous pouvez envoyer jusqu’à 300 LeadIds par appel.

Le point de terminaison répond par une `status` de &quot;mise à jour&quot; ou &quot;sauté&quot;. Si vous ignorez, une `reasons` est également inclus. Le point de terminaison répond également avec une `seq` champ qui est un index qui peut être utilisé pour mettre en corrélation les enregistrements envoyés avec l’ordre de la réponse.

Si l’appel aboutit, une activité &quot;Modifier les données du membre du programme&quot; est écrite dans le journal des activités du prospect.

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

L’objet program member contient des champs standard et des champs personnalisés facultatifs. Les champs standard sont présents dans chaque abonnement de Marketo Engage, tandis que les champs personnalisés sont créés par l’utilisateur selon les besoins. Chaque définition de champ est composée d’un ensemble d’attributs qui décrivent le champ. Les attributs sont par exemple le nom d’affichage, le nom de l’API et dataType. Ces attributs sont connus collectivement sous le nom de métadonnées.

Les points de terminaison suivants vous permettent d’interroger, de créer et de mettre à jour des champs sur l’objet membre du programme. Ces API requièrent que l’utilisateur de l’API propriétaire ait un rôle avec l’une des API ou les deux. **Champ standard de schéma en lecture-écriture** ou **Champ personnalisé du schéma en lecture-écriture** autorisations.

### Champs de requête

La requête sur les champs des membres du programme est simple. Vous pouvez interroger un seul champ de membre de programme par nom d’API ou interroger l’ensemble de tous les champs de membre de programme. Il est possible de récupérer les champs standard et personnalisés en fonction des autorisations de rôle utilisées. Les champs masqués sont également récupérés.

#### Par nom

La variable [Obtenir le champ du membre du programme par nom](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldByNameUsingGET) endpoint récupère les métadonnées d’un champ unique sur l’objet de membre du programme. La variable `fieldApiName` Le paramètre path spécifie le nom de l’API du champ. La réponse est semblable au point d’entrée Description du membre du programme , mais contient des métadonnées supplémentaires telles que la variable `isCustom` qui indique si le champ est un champ personnalisé.

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

La variable [Obtention des champs des membres du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldsUsingGET) endpoint récupère les métadonnées de tous les champs de l’objet de membre du programme. Par défaut, un maximum de 300 enregistrements est renvoyé. Vous pouvez utiliser la variable `batchSize` paramètre de requête pour réduire ce nombre. Si la variable `moreResult` est true, cela signifie que plus de résultats sont disponibles. Continuez à appeler ce point de terminaison jusqu’à ce que l’attribut moreResult renvoie false, ce qui signifie qu’il n’y a aucun résultat disponible. La variable `nextPageToken` La valeur renvoyée par cette API doit toujours être réutilisée pour la prochaine itération de cet appel.

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

La variable [Création de champs de membre de programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/createProgramMemberFieldUsingPOST) endpoint crée un ou plusieurs champs personnalisés sur l’objet program member. Ce point de terminaison fournit des fonctionnalités comparables à ce qui est [disponible dans l’interface utilisateur du Marketo Engage](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields). Vous pouvez créer jusqu’à 20 champs personnalisés à l’aide de ce point de terminaison.

Examinez attentivement chaque champ que vous créez dans votre instance de production de Marketo Engage à l’aide de l’API. Une fois qu’un champ a été créé, vous ne pouvez pas le supprimer ([vous pouvez uniquement le masquer.](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo)). La prolifération des champs inutilisés est une mauvaise pratique qui encombrera votre instance.

La variable `input` est un tableau d’objets de champ de membre de programme. Chaque objet contient un ou plusieurs attributs. Les attributs requis sont : `displayName`, `name`, et `dataType` qui correspondent respectivement au nom d’affichage de l’interface utilisateur du champ, au nom de l’API du champ et au type de champ. Vous pouvez éventuellement spécifier `description`, `isHidden`, `isHtmlEncodingInEmail`,et `isSensitive`.

Il existe quelques règles associées à `name` et `displayName` attribution de noms. La variable `name` doit être unique, commencer par une lettre et contenir uniquement des lettres, des chiffres ou des traits de soulignement. Le *`isplayName` doit être unique et ne peut pas contenir de caractères spéciaux. Une convention de dénomination commune doit s’appliquer [casse du chameau](https://en.wikipedia.org/wiki/Camel_case#) to `displayName` pour produire `name`. Par exemple, un `displayName` de &quot;Mon champ personnalisé&quot; générerait une `name` de &quot;myCustomField&quot;.

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

La variable [Mettre à jour le champ membre du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/updateProgramMemberFieldUsingPOST) endpoint met à jour un champ personnalisé unique sur l’objet program member. En règle générale, les opérations de mise à jour de champ effectuées à l’aide de l’interface utilisateur du Marketo Engage sont réalisables à l’aide de l’API. Quelques différences sont résumées dans le tableau ci-dessous.

| Attribut | Mise à jour par API ? | Mise à jour par l’interface utilisateur ? | Mise à jour par API ? | Mise à jour par l’interface utilisateur ? |
|---|---|---|---|---|
| dataType | non | non | non | oui |
| description | oui | oui | oui | oui |
| displayName | non | non | oui | oui |
| isCustom | non | non | non | non |
| isHidden | non | oui | oui (si créé par l’API) | oui |
| isHtmlEncodingInEmail | oui | oui | oui | oui |
| isSensitive | oui | oui | oui | oui |
| length | non | non | non | non |
| name | non | non | non | non |

La variable `fieldApiName` path spécifie le nom de l’API du champ à mettre à jour. La variable `input` est un tableau contenant un seul objet de champ de piste. L’objet de champ contient un ou plusieurs attributs.

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

La variable [Supprimer des membres de programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/deleteProgramMemberUsingPOST) endpoint est utilisé pour supprimer les enregistrements de membre du programme. La variable `programId` path spécifie le programme contenant les membres à supprimer. Le corps de la requête contient une `input` tableau d’identifiants de piste. Un maximum de 300 ID de piste par appel est autorisé.

Le point de terminaison répond par une `status` de &quot;supprimé&quot; ou &quot;sauté&quot;. Si vous ignorez, une `reasons` est également inclus. Le point de terminaison répond également avec une `seq` champ qui est un index qui peut être utilisé pour mettre en corrélation les enregistrements envoyés avec l’ordre de la réponse.

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
