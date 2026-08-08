---
title: Extraction d’objet personnalisé en bloc
feature: REST API, Custom Objects
description: Guide des API REST d’extraction d’objets personnalisés en bloc Marketo pour l’exportation d’objets personnalisés liés à un prospect avec des filtres de liste et de date de mise à jour, des champs sélectionnés et...
exl-id: 86cf02b0-90a3-4ec6-8abd-b4423cdd94eb
TQID: https://experienceleague.adobe.com/KAT-vab2uZq8FrRbZLy30PCJNfq01znDDuSSWuIu7WE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 1186
ht-degree: 2%

---

# Extraction d’objet personnalisé en bloc

[Référence de point d’entrée d’extraction d’objet personnalisé en bloc](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects)

Les API REST d’extraction d’objets personnalisés en bloc récupèrent de grands ensembles d’enregistrements d’objets personnalisés dans Marketo. Utilisez ces API pour un échange de données continu entre Marketo et des systèmes externes, ETL, Data Warehouse et l’archivage.

L’API exporte les enregistrements d’objets personnalisés Marketo de premier niveau liés directement aux prospects. Spécifiez le nom de l’objet personnalisé et une liste de prospects liés. Pour chaque prospect, l’API écrit les enregistrements d’objet personnalisés liés correspondants sous forme de lignes dans le fichier d’exportation.

Vous pouvez afficher les données d’objet personnalisées dans l’onglet [&#x200B; Objet personnalisé » de la page des détails du prospect dans l’interface utilisateur de Marketo](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects).

## Autorisations

L’utilisateur de l’API doit disposer d’un rôle avec l’autorisation Objet personnalisé en lecture seule, l’autorisation Objet personnalisé en lecture-écriture, ou les deux.

## Filtres

Les filtres d’extraction d’objet personnalisé spécifient une liste de prospects liés à l’objet personnalisé. Si un prospect répertorié est lié à des enregistrements correspondant au nom d’objet personnalisé spécifié, l’API écrit ces enregistrements dans le fichier d’exportation.

Spécifiez un seul type de filtre par tâche d’exportation.

| Type de filtre | Type de données | Notes |
| --- | --- | --- |
| `updatedAt` | Période | Accepte un objet JSON avec les membres `startAt` et `endAt` &amp;nbsp.;`startAt` accepte une valeur datetime représentant le filigrane bas et `endAt` accepte une valeur datetime représentant le filigrane haut. La plage doit être de 31 jours ou moins. Les traitements avec ce type de filtre renvoient tous les enregistrements accessibles qui ont été mis à jour au cours de la période. Les heures de date doivent être au format ISO-8601, sans millisecondes. |
| `staticListName` | Chaîne | Accepte le nom d’une liste statique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres de la liste statique au moment où la tâche commence le traitement. Récupérez les noms de listes statiques à l’aide du point d’entrée Get Lists. |
| `staticListId` | Nombre entier | Accepte l’identifiant d’une liste statique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres de la liste statique au moment où la tâche commence le traitement. Récupérez les identifiants de liste statiques à l’aide du point d’entrée Get Lists. |
| `smartListName`* | Chaîne | Accepte le nom d’une liste dynamique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres des listes dynamiques au moment où la tâche commence à être traitée. Récupérez les noms des listes dynamiques à l’aide du point d’entrée Get Smart Lists. |
| `smartListId`* | Nombre entier | Accepte l’identifiant d’une liste dynamique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres des listes dynamiques au moment où la tâche commence à être traitée. Récupérez les identifiants de liste dynamique à l’aide du point d’entrée Get Smart Lists. |

Certains abonnements ne prennent pas en charge ce type de filtre. S’il n’est pas disponible, le point d’entrée Créer une tâche d’exportation principale renvoie `1035, Unsupported filter type for target subscription`. Contactez l’assistance Marketo pour demander cette fonctionnalité pour votre abonnement.

## Options

Le point d’entrée [Créer une tâche d’exportation d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#operation/createExportCustomObjectsUsingPOST) offre les options suivantes :

- Spécifiez les champs à inclure dans le fichier d’exportation.
- Renommez les en-têtes de colonne exportés.
- Spécifiez le format du fichier d’exportation.

| Paramètre | Type de données | Obligatoire | Notes |
| --- | --- | --- | --- |
| `fields` | Array[String] | Oui | Tableau de chaînes contenant la valeur du nom de l’attribut d’objet personnalisé tel que renvoyé par le point d’entrée de l’objet personnalisé Describe. Les champs répertoriés sont inclus dans le fichier exporté. |
| `columnHeaderNames` | Objet | Non | Un objet JSON contenant des paires clé-valeur de noms d’en-tête de champ et de colonne. La clé doit être le nom d’un champ inclus dans la tâche d’exportation. La valeur est le nom de l’en-tête de colonne exporté pour ce champ. |
| `format` | Chaîne | Non | Accepte l’un des formats suivants : CSV, TSV, SSV. Le fichier exporté est rendu sous la forme d’un fichier de valeurs séparées par des virgules, des valeurs séparées par des tabulations ou des valeurs séparées par des espaces, respectivement, s’il est défini. La valeur par défaut est CSV si cette valeur n’est pas définie. |

## Création d’un traitement

Utilisez le point d’entrée [Créer une tâche d’exportation d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#operation/createExportCustomObjectsUsingPOST) pour définir la tâche d’exportation.

La requête utilise les paramètres suivants :

- `apiName` : paramètre de chemin d’accès obligatoire. Spécifie l’objet personnalisé Marketo à exporter, en utilisant le nom renvoyé par le point d’entrée [Décrire l’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#operation/describeUsingGET_1). Les objets personnalisés CRM ne sont pas autorisés.
- `filter` : obligatoire. Spécifie les leads liés en référençant une liste statique ou une liste dynamique.
- `fields` : obligatoire. Indique les noms d’API des attributs d’objet personnalisés à inclure dans le fichier d’exportation.
- `format` : facultatif. Indique le format du fichier d’exportation.
- `columnHeaderNames` : facultatif. Spécifie les noms d&#39;en-tête de colonne de remplacement.

Cet exemple utilise un objet personnalisé `Car` avec des champs `Color`, `Make`, `Model` et `VIN`. Le champ de lien est l’ID de lead et le champ de déduplication est VIN.

Définition d’objet personnalisé

![Objet personnalisé](assets/custom-object-car.png)

Champs d’objet personnalisés

![Champs d’objet personnalisés](assets/custom-object-car-fields.png)

Appelez [Description de l’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#operation/describeUsingGET_1) pour inspecter les attributs d’objet personnalisés par programmation. La réponse renvoie les attributs en `fields`.

```http
GET /rest/v1/customobjects/car_c/describe.json
```

```json
{
    "requestId": "148ef#1793e00f64f",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It's a car.",
            "createdAt": "2021-05-05T16:14:41Z",
            "updatedAt": "2021-05-05T16:14:42Z",
            "idField": "marketoGUID",
            "dedupeFields": [
                "vIN"
            ],
            "searchableFields": [
                [
                    "vIN"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "leadID"
                ]
            ],
            "relationships": [
                {
                    "field": "leadID",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "Id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "leadID",
                    "displayName": "Lead ID",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "vIN",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

Utilisez le point d’entrée [Synchroniser les objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi#operation/syncCustomObjectsUsingPOST) pour créer des enregistrements d’objets personnalisés et les lier à un prospect. Un prospect peut être lié à plusieurs enregistrements d’objets personnalisés, créant ainsi une relation un-à-plusieurs.

```http
POST /rest/v1/customobjects/car_c.json
```

```json
{
   "action":"createOrUpdate",
   "input":[
       {
           "leadId": 11,
           "color": "Pearl White",
           "make": "Tesla",
           "model": "Model S",
           "vIN": "5YJSA1E41FF156789"
       },
       {
           "leadId": 12,
           "color": "Midnight Silver Metallic",
           "make": "Tesla",
           "model": "Model X",
           "vIN": "LRWXB2B41FF198765"
       },
       {
           "leadId": 13,
           "color": "Fusion Red",
           "make": "Tesla",
           "model": "Roadster",
           "vIN": "SFGRC3C41FF154321"
       }
    ]
}
```

```json
{
    "requestId": "50d9#1793e066088",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "d911eaa1-fd0b-4a99-9b71-c6a7233c782c",
            "status": "created"
        },
        {
            "seq": 1,
            "marketoGUID": "20d04ffb-51f0-4336-924c-c783b9bb4215",
            "status": "created"
        },
        {
            "seq": 2,
            "marketoGUID": "e7da4331-8e7a-473b-85c8-047638eb6c7f",
            "status": "created"
        }
    ],
    "success": true
}
```

Les trois prospects de cet exemple appartiennent à la liste statique `Car Buyers`, qui a une `id` de 1 081. Appelez le point d’entrée [Get Leads by List Id](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByListIdUsingGET_1) pour récupérer les membres de la liste.

```http
GET /rest/v1/lists/1081/leads.json
```

```json
{
    "requestId": "d023#1793e1e982b",
    "result": [
        {
            "id": 11,
            "firstName": "Hanna",
            "lastName": "Crawford",
            "email": "208161Hanna.Crawford@pookmail.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        },
        {
            "id": 12,
            "firstName": "Bertha",
            "lastName": "Fulton",
            "email": "208160Bertha.Fulton@trashymail.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        },
        {
            "id": 13,
            "firstName": "Faith",
            "lastName": "England",
            "email": "208159Faith.England@dodgit.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        }
    ],
    "success": true
}
```

Pour récupérer ces enregistrements, appelez le point d’entrée [Créer une tâche d’exportation d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#operation/createExportCustomObjectsUsingPOST). Spécifiez les attributs d’objet personnalisés dans `fields` et l’identifiant de liste statique dans `filter`.

```http
POST /bulk/v1/customobjects/car_c/export/create.json
```

```json
{
    "fields": [
        "leadId",
        "color",
        "make",
        "model",
        "vIN"
    ],
    "filter": {
        "staticListId": 1081
    }
}
```

```json
{
    "requestId": "8d2f#1793e289e87",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Created",
            "createdAt": "2021-05-05T20:12:01Z"
        }
    ],
    "success": true
}
```

La réponse confirme la création du traitement, mais le démarrage de l’exportation n’est pas automatique. Transmettez `apiName` et le `exportId` renvoyé au point d’entrée [Mettre en file d’attente la tâche d’exportation d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#operation/enqueueExportCustomObjectsUsingPOST) pour démarrer la tâche.

```http
POST /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/enqueue.json
```

```json
{
    "requestId": "cfaf#1793e2a0762",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z"
        }
    ],
    "success": true
}
```

La réponse mise en file d&#39;attente renvoie initialement un statut `Queued`. Lorsqu’un emplacement d’exportation devient disponible, le statut passe à `Processing`.

## Interroger le statut de la tâche

Vous ne pouvez récupérer le statut que pour les tâches créées par le même utilisateur de l’API.

Comme l’exportation s’exécute de manière asynchrone, utilisez le point d’entrée [Obtenir le statut de la tâche d’exportation de l’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#operation/getExportCustomObjectsStatusUsingGET) pour interroger sa progression. Le statut n’est mis à jour qu’une fois toutes les 60 secondes. N’effectuez donc pas d’interrogations plus fréquentes.

Le statut peut être `Created`, `Queued`, `Processing`, `Canceled`, `Completed` ou `Failed`.

```http
GET /bulk/v1/customobjects/{apiName}/export/{exportId}/status.json
```

```json
{
    "requestId": "14daa#1793e2cf9de",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z",
            "startedAt": "2021-05-05T20:14:15Z"
        }
    ],
    "success": true
}
```

Cette réponse indique que la tâche est toujours en cours de traitement et que le fichier n’est donc pas disponible. Lorsque le statut de la tâche passe à `Completed`, le fichier est prêt à être téléchargé.

```json
{
    "requestId": "14daa#1793e2cf9de",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Completed",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z",
            "startedAt": "2021-05-05T20:14:15Z",
            "finishedAt": "2021-05-05T20:14:28Z",
            "numberOfRecords": 3,
            "fileSize": 182,
            "fileChecksum": "sha256:fac0cabc2352229c12e18b2fde03d1f24178bc71e9e926f520ae8d61bbe98c01"
        }
    ],
    "success": true
}
```

## Récupération de vos données

Pour récupérer une exportation d’objet personnalisé terminée, transmettez `apiName` et `exportId` au point d’entrée [Obtenir l’exportation du fichier d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#operation/getExportCustomObjectsFileUsingGET).

Le point d’entrée renvoie le fichier au format configuré pour la tâche. Si un attribut d’objet personnalisé demandé ne contient aucune donnée, le champ d’exportation correspondant contient `null`.

```http
GET /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/file.json
```

```csv
leadId,color,make,model,vIN
11,Pearl White,Tesla,Model S,5YJSA1E41FF156789
12,Midnight Silver Metallic,Tesla,Model X,LRWXB2B41FF198765
13,Fusion Red,Tesla,Roadster,SFGRC3C41FF154321
```

Pour une récupération partielle ou pouvant être reprise, le point d’entrée du fichier prend en charge l’en-tête `Range` HTTP facultatif avec un type de plage de `bytes`. Si vous ne définissez pas l’en-tête , le point d’entrée renvoie l’intégralité du fichier. Pour plus d’informations, voir [&#x200B; Extraction en bloc &#x200B;](bulk-extract.md).

## Annulation d’un traitement

Pour annuler une tâche mal configurée ou qui n’est plus nécessaire, appelez le point d’entrée [&#x200B; Annuler la tâche d’exportation d’objet personnalisé &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#operation/cancelExportCustomObjectsUsingPOST). Le statut de la réponse indique que le traitement est annulé.

```http
POST /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/cancel.json
```

```json
{
    "requestId": "e5f9#179391286a7",
    "result": [
        {
            "exportId": "4a8cdd80-0d16-4dd6-9923-6ec97e30e91b",
            "format": "CSV",
            "status": "Cancelled",
            "createdAt": "2021-05-04T20:24:33Z"
        }
    ],
    "success": true
}
```
