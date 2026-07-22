---
title: Membres du programme
feature: REST API
description: Utilisez l’API REST Marketo pour lire, créer, mettre à jour et supprimer des membres de programme, gérer des champs standard et personnalisés et effectuer des requêtes à l’aide de champs pouvant faire l’objet de recherches.
exl-id: 22f29a42-2a30-4dce-a571-d7776374cf43
TQID: https://experienceleague.adobe.com/scEHyXYq9C7cCS1kIX810wG7ahT9fsa448NwIfBmzQM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1670
ht-degree: 2%

---

# Membres du programme

[Référence du point d’entrée des membres du programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members)

Marketo fournit des API pour lire, créer, mettre à jour et supprimer des enregistrements de membre de programme. Le champ ID lead associe les enregistrements de membre de programme aux enregistrements de lead.

Chaque enregistrement contient des champs standard et peut contenir jusqu’à 20 champs personnalisés. Ces champs stockent des données de membre spécifiques au programme à utiliser dans des formulaires, des filtres, des déclencheurs et des actions de flux. Vous pouvez afficher ces données dans l’onglet [Membres](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members) du programme dans l’interface utilisateur de Marketo Engage.

## Décrire

Le point d’entrée [Décrire le membre de programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2) suit le modèle standard pour les objets de base de données de leads.

- Le tableau `searchableFields` identifie les champs valides pour les requêtes.
- Le tableau `fields` contient des métadonnées telles que le nom de l’API REST, le nom d’affichage et la possibilité de mise à jour du champ.

```http
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

Utilisez le point d’entrée [Obtenir les membres du programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/getProgramMembersUsingGET) pour récupérer les membres d’un programme. La requête nécessite un paramètre de chemin d’accès `programId` et des paramètres de requête `filterType` et `filterValues`.

`programId` indique le programme à rechercher.

`filterType` indique le champ à utiliser comme filtre de recherche. Il accepte n’importe quel champ de la liste « searchableFields » renvoyée par le point d’entrée [Décrire le membre de programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2). Pour un champ personnalisé, le type de données doit être « string » ou « integer ».

Lorsque filterType n’est pas « leadId », la requête peut traiter un maximum de 100 000 enregistrements de membre de programme. Selon la configuration de votre instance Marketo, vous recevez l’une des erreurs suivantes :

- Si le nombre total de membres du programme dépasse 100 000, une erreur est renvoyée : « 1003, Taille totale des membres : 100 001 dépasse la limite autorisée de 100 000 pour le filtre ».
- Si le nombre total de membres du programme _qui correspondent au filtre_ dépasse 100 000, une erreur est renvoyée : « 1003, Taille d’abonnement correspondante : 100 001 dépasse la limite autorisée (100 000) pour cette api ».

Pour interroger un programme dont le nombre d’adhésions dépasse la limite, utilisez plutôt l’API [Bulk Program Member Extract](bulk-program-member-extract.md).

`filterValues` spécifie les valeurs à rechercher et accepte jusqu’à 300 valeurs séparées par des virgules. L’appel recherche les enregistrements pour lesquels le champ de membre de programme correspond à l’une des filterValues incluses.

Vous pouvez également filtrer par période en spécifiant `updatedAt` comme filterType et en fournissant les paramètres datetime `startAt` et `endAt`. La plage doit être de sept jours ou moins. Utilisez le format ISO-8601 sans millisecondes pour les valeurs de date et d’heure.

Le paramètre de requête `fields` facultatif accepte une liste de noms d’API de champ séparés par des virgules renvoyée par le point d’entrée [Décrire le membre de programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2). Lorsqu’il est inclus, chaque enregistrement de réponse contient les champs spécifiés. Lorsqu’elle est omise, la réponse renvoie `acquiredBy`, `leadId`, `membershipDate`, `programId` et `reachedSuccess` par défaut.

Par défaut, le point d’entrée renvoie un maximum de 300 enregistrements. Utilisez le paramètre de requête `batchSize` pour réduire ce nombre.

Si l’attribut **moreResult** est défini sur true, d’autres résultats sont disponibles. Continuez à appeler le point d’entrée avec la `nextPageToken` renvoyée jusqu’à ce que moreResult ait la valeur false.

Si la longueur totale de la requête GET dépasse 8 Ko, le point d’entrée renvoie l’erreur HTTP « 414, URI trop long ». Pour contourner cette limite, modifiez la requête de GET en POST, ajoutez le paramètre `_method=GET` et placez la chaîne de requête dans le corps de la requête.

```http
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

Deux points d’entrée prennent en charge les opérations de création et de mise à jour sur les membres du programme :

- Un point d’entrée ne met à jour que le statut de membre du programme.
- Un point d’entrée met à jour les champs des membres du programme marqués comme « modifiables ».

Chaque point d’entrée peut modifier jusqu’à 300 enregistrements de membre de programme par appel.

### Statut de membre du programme

Utilisez le point d’entrée [Synchroniser le statut du membre du programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/syncProgramMemberStatusUsingPOST) pour créer ou mettre à jour le statut du programme pour un ou plusieurs membres.

Les paramètres requis sont les suivants :

- `programId` : paramètre de chemin d’accès qui spécifie le programme contenant les membres à créer ou à mettre à jour.
- `statusName` : indique le statut du programme à appliquer à une liste de prospects. Le statusName doit correspondre à un statut disponible pour le canal du programme. Récupérez les statuts valides avec le point d’entrée [Obtenir les canaux](https://developer.adobe.com/marketo-apis/api/asset#tag/Channels/operation/getAllChannelsUsingGET). Si le statut d’un prospect a une valeur d’étape supérieure à la valeur statusName désignée, la requête ignore ce prospect.
- `input` : tableau de valeurs `leadId` qui correspondent aux membres du programme. Vous pouvez soumettre jusqu’à 300 leadId par appel.

Le point d’entrée effectue un upsert sur chaque enregistrement. Si l’ID de lead est associé à un membre du programme, le point d’entrée met à jour son statut d’abonnement. Dans le cas contraire, il crée un enregistrement de membre du programme, associe l’enregistrement à l’ID de prospect et attribue le statut d’abonnement.

La réponse inclut une `status` de « mis à jour », « créé » ou « ignoré ». Un résultat ignoré comprend également un tableau `reasons`. Le champ `seq` est un index qui met en corrélation chaque enregistrement envoyé avec l’ordre de réponse.

Si l’appel aboutit, une activité « Modifier le statut du programme » est consignée dans le journal d’activité du prospect.

```http
POST /rest/v1/programs/{programId}/members/status.json
```

```text
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

Utilisez le point d’entrée [Synchroniser les données de membre de programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/syncProgramMemberDataUsingPOST) pour mettre à jour les données de champ de membre de programme pour un ou plusieurs membres. Vous pouvez modifier n’importe quel champ personnalisé ou champ standard marqué comme « modifiable » par le point d’entrée [Décrire le membre du programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2).

Les paramètres requis sont les suivants :

- `programId` : un paramètre de chemin d’accès qui spécifie le programme contenant les membres à mettre à jour.
- `input` : tableau dont les éléments contiennent un `leadId` et un ou plusieurs champs à mettre à jour par nom d’API. Vous pouvez soumettre jusqu’à 300 leadId par appel.

Le point d’entrée met à jour chaque enregistrement. L’ID de prospect doit être associé à un membre du programme et chaque champ doit pouvoir être mis à jour.

La réponse inclut une `status` de « mise à jour » ou « ignorée ». Un résultat ignoré comprend également un tableau `reasons`. Le champ `seq` est un index qui met en corrélation chaque enregistrement envoyé avec l’ordre de réponse.

Si l’appel aboutit, une activité « Modifier les données du membre du programme » est consignée dans le journal d’activité du prospect.

```http
POST /rest/v1/programs/{programId}/members.json
```

```text
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

L’objet membre de programme contient des champs standard et des champs personnalisés facultatifs. Des champs standard sont présents dans chaque abonnement Marketo Engage, tandis que les utilisateurs créent des champs personnalisés selon leurs besoins.

Chaque champ est défini par des attributs tels que le nom d’affichage, le nom de l’API et le dataType. Ensemble, ces attributs sont appelés métadonnées.

Les points d’entrée suivants interrogent, créent et mettent à jour des champs sur l’objet membre de programme. L’utilisateur de l’API doit disposer d’un rôle avec l’autorisation **Champ standard de schéma en lecture-écriture**, l’autorisation **Champ personnalisé de schéma en lecture-écriture** ou les deux.

### Champs de requête

Exécutez une requête sur un champ de membre de programme par nom d’API ou récupérez tous les champs de membre de programme. Les autorisations des rôles déterminent si la réponse peut inclure des champs standard, des champs personnalisés ou les deux. La réponse inclut également des champs masqués.

#### Par nom

Le point d’entrée [Obtenir le champ de membre de programme par nom](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/getProgramMemberFieldByNameUsingGET) récupère les métadonnées d’un champ sur l’objet de membre de programme. Le paramètre de chemin d’accès `fieldApiName` obligatoire spécifie le nom de l’API du champ.

La réponse ressemble à la réponse Describe Program Member , mais elle comprend des métadonnées supplémentaires. Par exemple, l’attribut `isCustom` indique si le champ est personnalisé.

```http
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

Le point d’entrée [Obtenir les champs de membre de programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/getProgramMemberFieldsUsingGET) récupère les métadonnées de tous les champs de l’objet de membre de programme. Par défaut, elle renvoie un maximum de 300 enregistrements. Utilisez le paramètre de requête `batchSize` pour réduire ce nombre.

Si l’attribut `moreResult` est défini sur « true », d’autres résultats sont disponibles. Continuez à appeler le point d’entrée avec la `nextPageToken` renvoyée jusqu’à ce que moreResult ait la valeur false.

```http
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

Le point d’entrée [Créer des champs de membre de programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/createProgramMemberFieldUsingPOST) crée des champs personnalisés sur l’objet de membre de programme. Il offre des fonctionnalités comparables à l’interface utilisateur de [&#128279;](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields). Vous pouvez créer jusqu’à 20 champs personnalisés avec ce point d’entrée.

Examinez attentivement chaque champ avant de le créer dans une instance Marketo Engage de production. Une fois un champ créé, vous ne pouvez pas le supprimer ; [vous pouvez uniquement le masquer](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo). Les champs non utilisés encombrent l’instance.

Le paramètre `input` requis est un tableau d’objets de champ de membre de programme. Chaque objet contient un ou plusieurs attributs.

- Les attributs requis sont `displayName`, `name` et `dataType`. Ils correspondent respectivement au nom d’affichage de l’interface utilisateur, au nom de l’API et au type de champ.
- Les attributs facultatifs sont `description`, `isHidden`, `isHtmlEncodingInEmail` et `isSensitive`.

Les attributs `name` et `displayName` comportent les règles de dénomination suivantes :

- L’attribut `name` doit être unique, commencer par une lettre et contenir uniquement des lettres, des chiffres ou des traits de soulignement.
- Le *`isplayName` doit être unique et ne peut pas contenir de caractères spéciaux.

Une convention commune est d&#39;appliquer [cas du chameau](https://en.wikipedia.org/wiki/Camel_case#) aux `displayName` pour produire des `name`. Par exemple, un `displayName` de « Mon champ personnalisé » génère un `name` de « myCustomField ».

```http
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

Le point d’entrée [Mettre à jour le champ de membre de programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/updateProgramMemberFieldUsingPOST) met à jour un champ personnalisé sur l’objet de membre de programme. La plupart des mises à jour des champs disponibles dans l’interface utilisateur de Marketo Engage le sont également via l’API. Le tableau suivant résume les différences.

| Attribut | Mis à jour par l’API ? | Peut-on les mettre à jour par l’interface utilisateur ? | Mis à jour par l’API ? | Peut-on les mettre à jour par l’interface utilisateur ? |
| --- | --- | --- | --- | --- |
| dataType | non | non | non | oui |
| description | oui | oui | oui | oui |
| displayName | non | non | oui | oui |
| isCustom | non | non | non | non |
| isHidden | non | oui | oui (si créé par l’API) | oui |
| isHtmlEncodingInEmail | oui | oui | oui | oui |
| isSensible | oui | oui | oui | oui |
| length | non | non | non | non |
| name | non | non | non | non |

La requête requiert les paramètres suivants :

- `fieldApiName` : paramètre de chemin d’accès qui spécifie le nom d’API du champ à mettre à jour.
- `input` : tableau contenant un objet de champ de prospect avec un ou plusieurs attributs.

```http
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

Utilisez le point d’entrée [Supprimer les membres du programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/deleteProgramMemberUsingPOST) pour supprimer les enregistrements de membre du programme. Le paramètre de chemin d’accès `programId` obligatoire spécifie le programme contenant les membres à supprimer.

Le corps de la requête contient un tableau `input` d’ID de prospect. Chaque appel autorise un maximum de 300 ID de prospect.

La réponse inclut une `status` de « supprimé » ou « ignoré ». Un résultat ignoré comprend également un tableau `reasons`. Le champ `seq` est un index qui met en corrélation chaque enregistrement envoyé avec l’ordre de réponse.

```http
POST /rest/v1/programs/{programId}/members/delete.json
```

```text
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
