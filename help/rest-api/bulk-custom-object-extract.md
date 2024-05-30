---
title: "Extraction d’objets personnalisés en bloc"
feature: REST API, Custom Objects
description: "Traitement par lot d’objets Marketo personnalisés."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1300'
ht-degree: 1%

---


# Extraction d’objets personnalisés en bloc

[Référence du point de terminaison d’extraction d’objets personnalisés en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects)

L’ensemble d’extraction d’objets personnalisés en bloc des API REST fournit une interface programmatique pour récupérer de grands ensembles d’enregistrements d’objets personnalisés en dehors de Marketo. Il s’agit de l’interface recommandée pour les cas d’utilisation qui nécessitent un échange continu de données entre Marketo et un ou plusieurs systèmes externes, à des fins d’ETL, d’entrepôt de données et d’archivage.

Cette API prend en charge l’exportation d’enregistrements d’objets personnalisés Marketo de premier niveau liés directement à une piste. Transmettez le nom de l’objet personnalisé et une liste de pistes auxquelles l’objet est lié. Pour chaque piste de la liste, les enregistrements d’objet personnalisé liés correspondant au nom d’objet personnalisé spécifié sont écrits en tant que lignes dans le fichier d’exportation. Les données d’objet personnalisées sont visibles dans la variable [Onglet Objet personnalisé de la page des détails du prospect dans l’interface utilisateur de Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects).

## Permissions

Les API Bulk Custom Object Extract exigent que l’utilisateur de l’API dispose d’un rôle avec l’une des autorisations &quot;Objet personnalisé en lecture seule&quot; ou &quot;Objet personnalisé en lecture-écriture&quot;, ou les deux.

## Filtres

L’extraction d’objet personnalisé prend en charge plusieurs options de filtre utilisées pour spécifier une liste de pistes liées à l’objet personnalisé. Si une piste de la liste est liée à des enregistrements d’objets personnalisés correspondant à un nom d’objet personnalisé donné, les enregistrements sont écrits dans le fichier d’exportation. Un seul type de filtre peut être spécifié par tâche d’exportation.

| Type de filtre | Type de données | Notes |
|---|---|---|
| `updatedAt` | Plage de dates | Accepte un objet JSON avec les membres `startAt` et `endAt` &amp;nbsp.;`startAt` accepte une date et une heure représentant le filigrane bas, et `endAt` accepte une date et une heure représentant le filigrane élevé. La période doit être de 31 jours ou moins. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui ont été mis à jour au cours de la période. Datetimes doit être au format ISO-8601, sans millisecondes. |
| `staticListName` | Chaîne | Accepte le nom d’une liste statique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres de la liste statique au moment où le traitement de la tâche commence. Récupérez les noms de liste statiques à l’aide du point de terminaison Get List. |
| `staticListId` | Entier | Accepte l’identifiant d’une liste statique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres de la liste statique au moment où le traitement de la tâche commence. Récupérez les identifiants de liste statique à l’aide du point de terminaison Get Lists (Obtenir les listes). |
| `smartListName`* | Chaîne | Accepte le nom d’une liste dynamique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres des listes dynamiques au moment où le traitement de la tâche commence. Récupérez les noms de liste dynamique à l’aide du point de terminaison Get Smart Lists. |
| `smartListId`* | Entier | Accepte l’identifiant d’une liste dynamique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres des listes dynamiques au moment où le traitement de la tâche commence. Récupérez les identifiants de liste dynamique à l’aide du point de terminaison Get Smart Lists. |

Le type de filtre n’est pas disponible pour certains abonnements. En cas d’indisponibilité de votre abonnement, vous recevez une erreur lors de l’appel du point de terminaison Create Export Lead Job (&quot;1035, Unsupported filter type for target subscription&quot;). Les clients peuvent contacter le support Marketo pour que cette fonctionnalité soit activée dans leur abonnement.

## Options

La variable [Création d’une tâche d’objet personnalisée](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) Le point de fin propose plusieurs options de mise en forme. Ces options permettent à l’utilisateur de :

- Spécifier les champs à inclure dans le fichier exporté
- Renommer les en-têtes de colonne de ces champs
- Définition du format du fichier exporté

| Paramètre | Type de données | Requis | Notes |
|---|---|---|---|
| `fields` | Tableau[Chaîne] | Oui | Tableau de chaînes contenant la valeur du nom d’attribut d’objet personnalisé comme renvoyé par le point de terminaison Décrire l’objet personnalisé. Les champs répertoriés sont inclus dans le fichier exporté. |
| `columnHeaderNames` | Objet | Non | Objet JSON contenant des paires clé-valeur de noms d’en-tête de champ et de colonne. La clé doit être le nom d’un champ inclus dans la tâche d’exportation. La valeur est le nom de l’en-tête de colonne exporté pour ce champ. |
| `format` | Chaîne | Non | Accepte l’un des paramètres suivants : CSV, TSV, SSV. Le fichier exporté est rendu sous la forme d’un fichier de valeurs séparées par des virgules, de valeurs séparées par des tabulations ou de valeurs séparées par des espaces, respectivement, s’il est défini. La valeur par défaut est CSV si elle n’est pas définie. |


## Création d’une tâche

Les paramètres de la tâche sont définis avant le démarrage de l’exportation à l’aide de la fonction [Création d’une tâche d’objet personnalisée](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) point de terminaison .

La variable `apiName` Le paramètre path est le nom d’objet personnalisé renvoyé par la variable [Description d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) point de terminaison . Cela spécifie l’objet personnalisé Marketo à exporter. Les objets personnalisés CRM ne sont pas autorisés. La variable `filter` contient la liste des pistes liées à l’objet personnalisé. Cela peut faire référence à une liste statique ou à une liste dynamique. La variable `fields` contient les noms d’API des attributs d’objet personnalisés à inclure dans le fichier d’exportation. En option, nous pouvons définir la variable `format` du fichier, et l’événement `columnHeaderNames`.

Par exemple, supposons que nous ayons créé un objet personnalisé nommé &quot;Car&quot; avec les champs suivants : Couleur, Couleur, Créer, Modèle, VIN. Le champ de lien est Identifiant de piste et le champ de déduplication est VIN.

Définition d’objet personnalisé

![Objet personnalisé](assets/custom-object-car.png)


Champs d’objet personnalisés

![Champs d’objet personnalisés](assets/custom-object-car-fields.png)

Nous pouvons appeler [Description d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) pour inspecter par programmation les attributs d’objet personnalisés qui apparaissent dans la variable `fields` dans la réponse.

```
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

Créez plusieurs enregistrements d’objet personnalisés et liez-les à un autre prospect à l’aide de la fonction [Synchroniser les objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) point de terminaison . Une piste peut être liée à de nombreux enregistrements d’objets personnalisés. On parle alors de relation &quot;un à plusieurs&quot;.

```
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

Chacune des trois pistes référencées ci-dessus appartient à une liste statique nommée &quot;Acheteurs de voitures&quot; dont la `id` est 1081, comme vous pouvez le voir ci-dessous en appelant la fonction [Obtenir des pistes par identifiant de liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET_1) point de terminaison .

```
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

Maintenant, créons une tâche d’exportation pour récupérer ces enregistrements. En utilisant la variable [Création d’une tâche d’objet personnalisée](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) point de fin, nous spécifions des attributs d’objet personnalisés dans la variable `fields` et un identifiant de liste statique dans la variable `filter` .

```
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

Cela renvoie un état dans la réponse indiquant que la tâche a été créée. La tâche a été définie et créée, mais elle n’a pas encore été lancée. Pour ce faire, la variable [Enqueue Exporter une tâche d’objet personnalisée](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/enqueueExportCustomObjectsUsingPOST) endpoint doit être appelé à l’aide de la fonction `apiName`, et la variable `exportId` à partir de la réponse d’état de création.

```
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

Cela répond par un premier `status` de &quot;En file d’attente&quot; après lequel est défini sur &quot;Traitement&quot; lorsqu’un emplacement d’exportation est disponible.

## État de la tâche d’interrogation

L’état ne peut être récupéré que pour les tâches qui ont été créées par le même utilisateur de l’API.

Puisqu’il s’agit d’un point de terminaison asynchrone, après la création de la tâche, nous devons interroger son état pour déterminer sa progression. Sondage à l’aide du [Obtenir l’état de la tâche d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsStatusUsingGET) point de terminaison . Le statut n’est mis à jour qu’une fois toutes les 60 secondes, donc une fréquence d’interrogation inférieure à celle-ci n’est pas recommandée, et dans presque tous les cas est toujours excessive. Le champ d’état peut répondre à l’un des types suivants : Créé, En file d’attente, En cours de traitement, Annulé, Terminé ou Échec.

```
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

Le point de terminaison d’état répond indiquant que la tâche est toujours en cours de traitement. Le fichier n’est donc pas encore disponible pour la récupération. Une fois la tâche `status` Les modifications apportées à &quot;Terminé&quot; peuvent être téléchargées.

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

Pour récupérer le fichier d’une exportation d’objet personnalisé terminée, appelez simplement le [Obtenir un fichier d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingGET) Point de terminaison avec votre `apiName` et `exportId`.

La réponse contient un fichier formaté selon la configuration de la tâche. Le point de terminaison répond avec le contenu du fichier. Si un attribut d’objet personnalisé demandé est vide (ne contient aucune donnée), `null` est placé dans le champ correspondant du fichier d&#39;export.

```
GET /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/file.json
```

```csv
leadId,color,make,model,vIN
11,Pearl White,Tesla,Model S,5YJSA1E41FF156789
12,Midnight Silver Metallic,Tesla,Model X,LRWXB2B41FF198765
13,Fusion Red,Tesla,Roadster,SFGRC3C41FF154321
```

Pour prendre en charge la récupération partielle et conviviale en cas de reprise des données extraites, le point de fin de fichier prend éventuellement en charge la plage d’en-tête HTTP du type bytes. Si l’en-tête n’est pas défini, l’ensemble du contenu est renvoyé. Vous pouvez en savoir plus sur l’utilisation de l’en-tête de plage dans Marketo [Extraction en bloc](bulk-extract.md).

## Annulation d’une tâche

Si une tâche a été mal configurée ou devient inutile, elle peut être facilement annulée à l’aide de la fonction [Annuler l’exportation d’objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingPOST) point de terminaison . Cette fonction répond par une `status` indiquant que la tâche a été annulée.

```
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
