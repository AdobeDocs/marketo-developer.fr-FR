---
title: Extraction d’objet personnalisé en bloc
feature: REST API, Custom Objects
description: Guide des API REST d’extraction d’objets personnalisés en bloc Marketo pour l’exportation d’objets personnalisés liés à un prospect avec des filtres de liste et de date de mise à jour, des champs sélectionnés et...
exl-id: 86cf02b0-90a3-4ec6-8abd-b4423cdd94eb
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 1%

---

# Extraction d’objet personnalisé en bloc

[Référence de point d’entrée d’extraction d’objet personnalisé en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects)

Le jeu d’API REST d’extraction d’objets personnalisés en bloc fournit une interface de programmation pour récupérer de grands jeux d’enregistrements d’objets personnalisés en dehors de Marketo. Il s’agit de l’interface recommandée pour les cas d’utilisation qui nécessitent un échange continu de données entre Marketo et un ou plusieurs systèmes externes, à des fins d’ETL, d’entreposage de données et d’archivage.

Cette API prend en charge l’exportation d’enregistrements d’objet personnalisés Marketo de premier niveau directement liés à un prospect. Transmettez le nom de l’objet personnalisé et une liste de prospects auxquels l’objet est lié. Pour chaque prospect de la liste, les enregistrements d’objet personnalisé liés qui correspondent au nom d’objet personnalisé spécifié sont écrits en tant que lignes dans le fichier d’exportation. Les données d’objet personnalisées sont visibles dans l’onglet [ Objet personnalisé » de la page de détails du prospect dans l’interface utilisateur de Marketo](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects).

## Autorisations

Les API d’extraction d’objets personnalisés en bloc nécessitent que l’utilisateur de l’API dispose d’un rôle avec l’une ou l’autre des autorisations « Objet personnalisé en lecture seule » ou « Objet personnalisé en lecture-écriture », ou les deux.

## Filtres

L’extraction d’objet personnalisé prend en charge plusieurs options de filtre utilisées pour spécifier une liste de prospects liés à l’objet personnalisé. Si un prospect de la liste est lié à des enregistrements d’objet personnalisés correspondant à un nom d’objet personnalisé donné, les enregistrements sont écrits dans le fichier d’exportation. Un seul type de filtre peut être spécifié par tâche d&#39;exportation.

| Type de filtre | Type de données | Notes |
|---|---|---|
| `updatedAt` | Période | Accepte un objet JSON avec les membres `startAt` et `endAt` &amp;nbsp.;`startAt` accepte une valeur datetime représentant le filigrane bas et `endAt` accepte une valeur datetime représentant le filigrane haut. La plage doit être de 31 jours ou moins. Les traitements avec ce type de filtre renvoient tous les enregistrements accessibles qui ont été mis à jour au cours de la période. Les heures de date doivent être au format ISO-8601, sans millisecondes. |
| `staticListName` | Chaîne | Accepte le nom d’une liste statique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres de la liste statique au moment où la tâche commence le traitement. Récupérez les noms de listes statiques à l’aide du point d’entrée Get Lists. |
| `staticListId` | Nombre entier | Accepte l’identifiant d’une liste statique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres de la liste statique au moment où la tâche commence le traitement. Récupérez les identifiants de liste statiques à l’aide du point d’entrée Get Lists. |
| `smartListName`* | Chaîne | Accepte le nom d’une liste dynamique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres des listes dynamiques au moment où la tâche commence à être traitée. Récupérez les noms des listes dynamiques à l’aide du point d’entrée Get Smart Lists. |
| `smartListId`* | Nombre entier | Accepte l’identifiant d’une liste dynamique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres des listes dynamiques au moment où la tâche commence à être traitée. Récupérez les identifiants de liste dynamique à l’aide du point d’entrée Get Smart Lists. |

Le type de filtre n’est pas disponible pour certains abonnements. Si vous n’êtes pas disponible pour votre abonnement, vous recevez une erreur lors de l’appel du point d’entrée Créer une tâche d’exportation de lead (« 1035, Unsupported filter type for target subscription »). Les clients peuvent contacter l’assistance Marketo pour que cette fonctionnalité soit activée dans leur abonnement.

## Options

Le point d’entrée [Créer une tâche d’exportation d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) fournit plusieurs options de mise en forme. Ces options permettent à l’utilisateur ou à l’utilisatrice de :

- Spécifier les champs à inclure dans le fichier exporté
- Renommer les en-têtes de colonne de ces champs
- Spécifier le format du fichier exporté

| Paramètre | Type de données | Obligatoire | Notes |
|---|---|---|---|
| `fields` | Array[String] | Oui | Tableau de chaînes contenant la valeur du nom de l’attribut d’objet personnalisé tel que renvoyé par le point d’entrée de l’objet personnalisé Describe. Les champs répertoriés sont inclus dans le fichier exporté. |
| `columnHeaderNames` | Objet | Non | Un objet JSON contenant des paires clé-valeur de noms d’en-tête de champ et de colonne. La clé doit être le nom d’un champ inclus dans la tâche d’exportation. La valeur est le nom de l’en-tête de colonne exporté pour ce champ. |
| `format` | Chaîne | Non | Accepte l’un des formats suivants : CSV, TSV, SSV. Le fichier exporté est rendu sous la forme d’un fichier de valeurs séparées par des virgules, des valeurs séparées par des tabulations ou des valeurs séparées par des espaces, respectivement, s’il est défini. La valeur par défaut est CSV si cette valeur n’est pas définie. |

## Création d’un traitement

Les paramètres de la tâche sont définis avant le lancement de l’exportation à l’aide du point d’entrée [Créer une tâche d’exportation d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST).

Le paramètre de chemin d’accès `apiName` obligatoire est le nom d’objet personnalisé renvoyé par le point d’entrée [Décrire l’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1). Indique l’objet personnalisé Marketo à exporter. Les objets personnalisés CRM ne sont pas autorisés. Le paramètre `filter` obligatoire contient la liste des prospects liés à l’objet personnalisé. Il peut s’agir d’une liste statique ou d’une liste dynamique. Le paramètre `fields` obligatoire contient les noms d’API des attributs d’objet personnalisés à inclure dans le fichier d’exportation. En option, nous pouvons définir le `format` du fichier et le `columnHeaderNames`.

Par exemple, supposons que nous ayons créé un objet personnalisé appelé « Voiture » avec les champs suivants : Couleur, Marque, Modèle, VIN. Le champ de lien est l’ID de lead et le champ de déduplication est VIN.

Définition d’objet personnalisé

![Objet personnalisé](assets/custom-object-car.png)

Champs d’objet personnalisés

![Champs d’objet personnalisés](assets/custom-object-car-fields.png)

Nous pouvons appeler [Description de l’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) pour inspecter par programmation les attributs d’objet personnalisés qui apparaissent dans l’attribut `fields` dans la réponse.

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

Créez plusieurs enregistrements d’objet personnalisés et liez chacun d’eux à un prospect différent à l’aide du point d’entrée [Synchroniser les objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST). Un prospect peut être lié à de nombreux enregistrements d’objets personnalisés. C’est ce qu’on appelle une relation « un à plusieurs ».

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

Chacun des trois prospects référencés ci-dessus appartient à une liste statique nommée « Acheteurs de voitures » dont la `id` est 1 081, comme vous pouvez le voir ci-dessous en appelant le point d’entrée [Obtenir les prospects par ID de liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET_1).

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

Créons maintenant une tâche d’exportation pour récupérer ces enregistrements. À l’aide du point d’entrée [Créer une tâche d’exportation d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST), nous spécifions des attributs d’objet personnalisés dans le paramètre `fields` et un identifiant de liste statique dans le paramètre `filter`.

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

Cette opération renvoie un statut dans la réponse indiquant que la tâche a été créée. La tâche a été définie et créée, mais elle n&#39;a pas encore été lancée. Pour ce faire, le point d’entrée [Mettre en file d’attente la tâche d’exportation d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/enqueueExportCustomObjectsUsingPOST) doit être appelé à l’aide du `apiName` et du `exportId` de la réponse du statut de création.

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

Cela répond par un `status` initial de « Mise en file d’attente », après quoi est défini sur « Traitement » lorsqu’un emplacement d’exportation est disponible.

## Interroger le statut de la tâche

Le statut ne peut être récupéré que pour les tâches créées par le même utilisateur de l’API.

Puisqu’il s’agit d’un point d’entrée asynchrone, après avoir créé la tâche, nous devons interroger son statut pour déterminer sa progression. Interroger à l’aide du point d’entrée [Obtenir le statut de la tâche d’exportation d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsStatusUsingGET). Le statut n’est mis à jour qu’une fois toutes les 60 secondes. Il est donc déconseillé d’utiliser une fréquence d’interrogation inférieure à cette fréquence, qui reste excessive dans presque tous les cas. Le champ de statut peut répondre par l’une des options suivantes : Créé, En file d’attente, Traitement, Annulé, Terminé ou En échec.

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

Le point d’entrée de statut répond indiquant que la tâche est toujours en cours de traitement et que le fichier n’est donc pas encore disponible pour récupération. Une fois que le `status` de traitement est remplacé par « Terminé », il peut être téléchargé.

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

Pour récupérer le fichier d’une exportation d’objet personnalisé terminée, appelez simplement le point d’entrée [Obtenir l’exportation du fichier d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingGET) avec vos `apiName` et `exportId`.

La réponse contient un fichier formaté selon la configuration de la tâche. Le point d’entrée répond avec le contenu du fichier . Si un attribut d’objet personnalisé demandé est vide (ne contient aucune donnée), `null` est placé dans le champ correspondant dans le fichier d’exportation.

```
GET /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/file.json
```

```csv
leadId,color,make,model,vIN
11,Pearl White,Tesla,Model S,5YJSA1E41FF156789
12,Midnight Silver Metallic,Tesla,Model X,LRWXB2B41FF198765
13,Fusion Red,Tesla,Roadster,SFGRC3C41FF154321
```

Pour prendre en charge la récupération partielle et conviviale des données extraites, le point d’entrée du fichier prend éventuellement en charge le `Range` d’en-tête HTTP de type `bytes`. Si l’en-tête n’est pas défini, l’intégralité du contenu est renvoyée. Vous pouvez en savoir plus sur l’utilisation de l’en-tête Plage dans Marketo [Extraction en bloc](bulk-extract.md).

## Annulation d’un traitement

Si une tâche n’a pas été configurée correctement ou devient inutile, elle peut facilement être annulée à l’aide du point d’entrée [Annuler la tâche d’exportation d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingPOST). Cette opération répond avec un `status` indiquant que la tâche a été annulée.

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
