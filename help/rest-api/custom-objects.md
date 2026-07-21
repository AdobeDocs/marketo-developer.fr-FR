---
title: Objets personnalisés
feature: REST API, Custom Objects
description: Découvrez comment créer et gérer des objets personnalisés Marketo via l’API REST, y compris comment répertorier et décrire les points d’entrée, les métadonnées, les relations, les champs et les requêtes.
exl-id: 88e8829b-f8f1-46d7-a753-5aa6e20e2c40
TQID: https://experienceleague.adobe.com/NWm9CjFVqQdVDJRrnE4nA299-Lg53-JR7xvY-82dUqY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
subfeature_v2:
  - id: ea4e3ff5-e7b9-4b4c-a5a0-dc27cc3f4275
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 2938
ht-degree: 0%

---

# Objets personnalisés

[**Référence de point d’entrée d’objet personnalisé**](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects)

Les objets personnalisés Marketo peuvent être associés à des objets standard Marketo, tels que des prospects et des sociétés, ou à d’autres objets personnalisés Marketo. Créez des objets personnalisés Marketo dans l’interface utilisateur de [Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects) ou à l’aide de l’API de métadonnées d’objet personnalisé décrite dans ce document.

L’accès à l’API Custom Object Metadata nécessite un type d’abonnement Marketo approprié. Contactez votre CSM pour plus de détails.

## Liste

Outre les appels standard Describe, Query, Update et Delete pour les objets de base de données de lead, les objets personnalisés fournissent un appel [list](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectsUsingGET). Le point d’entrée renvoie les objets personnalisés disponibles dans l’instance de destination et les métadonnées sur chaque objet.

```http
GET /rest/v1/customobjects.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"Car",
         "displayName":"Car",
         "description":"Car owner",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":["vin"],
         "searchableFields":[
            ["vin"],
            ["marketoGUID"],
            ["siebelId"]
         ],
         "relationships":[
            {
               "field":"siebelId",
               "type":"parent",
               "relatedTo":{
                  "name":"Lead",
                  "field":"siebelId"
               }
            }
         ]
      }
   ]
}
```

La réponse répertorie les relations de chaque objet. Chaque relation contient :

- `field` : champ de l’objet contenant la valeur du lien.
- `type` : indique si l&#39;objet associé est un objet parent ou enfant.
- `relatedTo` : nom de l’objet associé et son champ de lien.

## Décrire

L’appel [Décrire](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/describeUsingGET_1) pour les objets personnalisés suit le même modèle que les opportunités et les entreprises, avec deux ajouts :

- Le paramètre `apiName` path spécifie le nom de l’API du type d’objet personnalisé à décrire.
- La réponse inclut un tableau `relationships` qui répertorie les relations disponibles pour le type d’objet personnalisé.

```http
GET /rest/v1/customobjects/{apiName}/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"Car",
         "displayName":"Car",
         "description":"Car owner",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":["vin"],
         "searchableFields":[
            ["vin"],
            ["marketoGUID"],
            ["siebelId"]
         ],
         "relationships":[
            {
               "field":"siebelId",
               "type":"parent",
               "object":{
                  "name":"Lead",
                  "field":"siebelId"
               }
            }
         ],
         "fields":[
            {
               "name":"marketoGUID",
               "displayName":"Marketo GUID",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"vin",
               "displayName":"VIN",
               "description":"Vehicle Identification Number",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"siebelId",
               "displayName":"External Id",
               "description":"External Id",
               "dataType":"string",
               "length":36,
               "updateable":true
            },
            {
               "name":"make",
               "displayName":"Make",
               "dataType":"string",
               "length":36,
               "updateable":true
            },
            {
               "name":"model",
               "displayName":"Model",
               "description":"Vehicle Model",
               "dataType":"string",
               "length":255,
               "updateable":true
            },
            {
               "name":"year",
               "displayName":"Year",
               "dataType":"integer",
               "updateable":true
            },
            {
               "name":"color",
               "displayName":"Color",
               "description":"Vehicle color",
               "dataType":"String",
               "length": 255,
               "updateable":true
            }
         ]
      }
   ]
}
```

## Requête

[L&#39;interrogation d&#39;objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectsUsingGET) diffère légèrement de l&#39;interrogation d&#39;autres objets Base de données de leads. Comme pour Describe, la requête utilise un paramètre de chemin d’accès `apiName`.

Pour un filterType normal, envoyez une requête GET avec les paramètres `filterType` et `filterValues` requis. Vous pouvez également inclure les paramètres facultatifs `**fields**`, `batchSize` et `nextPageToken`.

Lorsque vous demandez une liste de champs, un champ demandé qui n’est pas renvoyé a une valeur implicite null.

```http
GET /rest/v1/customobjects/{apiName}.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa",
         "vin":"19UYA31581L000000",
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "vin":"29UYA31581L000000",
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
   ]
}
```

Lors de l’interrogation de clés composites, l’API se comporte comme l’API des rôles d’opportunité et accepte une requête POST avec un corps JSON. Le corps peut contenir les mêmes membres qu’une requête GET, à l’exception de `filterValues`.

Au lieu des valeurs de filtre, fournissez un tableau `input` d’objets . Chaque objet contient un membre pour chaque champ du `dedupeFields` du type d’objet.

```http
POST /rest/v1/customobjects/{apiName}.json?_method=GET
```

```json
{
   "filterType":"dedupeFields",
   "fields":[
      "marketoGuid",
      "Bedrooms",
      "yearBuilt"
   ],
   "input":[
      {
         "mlsNum":"1962352",
         "houseOwnerId":"42645756"
      },
      {
         "mlsNum":"2962352",
         "houseOwnerId":"52645756"
      },
      {
         "mlsNum":"3962352",
         "houseOwnerId":"62645756"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa",
         "Bedrooms":3,
         "yearBuilt":1948,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "Bedrooms":4,
         "yearBuilt":1956,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":2,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc",
         "Bedrooms":3,
         "yearBuilt":2001,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      }
   ]
}
```

## Créer et mettre à jour

Utilisez le point d’entrée [Synchroniser les objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) pour créer ou mettre à jour des objets personnalisés. Spécifiez l’opération avec le paramètre `action` . Chaque appel peut créer ou mettre à jour jusqu’à 300 enregistrements.

Basez les valeurs du tableau `input` sur les informations renvoyées par le point d’entrée [Décrire les objets personnalisés](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/endpoint-reference#!/Custom_Objects/describeUsingGET_1). Dans l’exemple d’objet de carte, le seul champ de déduplication est `vin`. Lorsque vous utilisez le mode dedupeFields pour créer ou mettre à jour des enregistrements, incluez au moins un champ `vin` dans chaque objet du tableau d’entrée.

```http
POST /rest/v1/customobjects/{apiName}.json
```

```json
{
   "action":"updateOnly",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000",
         "siebelId":"f2676861b5fb",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      },
      {
         "vin":"29UYA31581L000000",
         "siebelId":"f2676861b5fc",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      },
      {
         "vin":"39UYA31581L000000",
         "siebelId":"f2676861b5fd",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "status": "updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status": "created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1004",
               "message":"Lead not found"
            }
         ]
      }
   ]
}
```

Lorsque vous mettez à jour des enregistrements en mode `idField`, le `idField` est toujours `marketoGUID`. Incluez un champ `marketoGUID` dans chaque enregistrement.

Ce champ étant géré par le système, `idField` n&#39;est valide que pour le type d&#39;action updateOnly. Le tableau de résultats inclut le **statut** de chaque enregistrement. Il comprend également un `marketoGUID` pour une opération réussie ou un tableau `reasons` pour une opération ayant échoué.

## Supprimer

Pour [supprimer des enregistrements](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST), sélectionnez un mode de `deleteBy` de `idField` ou de `dedupeFields`. Incluez les champs correspondants dans chaque enregistrement du tableau `input`. Chaque appel autorise un maximum de 300 enregistrements.

```http
POST /rest/v1/customobjects/{apiName}/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000"
      },
      {
         "vin":"29UYA31581L000000"
      },
      {
         "vin":"39UYA31581L000000"
      }
   ]
}

{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq":1,
         "marketoGUID":"da42707c-4dc4-4fc1-9fef-f30a3017240a",
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1013",
               "message":"Object not found"
            }
         ]
      }
   ]
}
```

Comme pour les mises à jour, le résultat contient un statut pour chaque enregistrement. Elle comprend également un `marketoGUID` pour une suppression réussie ou un tableau `reasons` pour une suppression ayant échoué.

## Types d’objet personnalisés

L’API de métadonnées d’objet personnalisé vous permet de gérer à distance des schémas d’objet personnalisés. Utilisez-le pour créer un type d’objet personnalisé ou pour modifier un type existant. Après avoir créé ou modifié un type, approuvez-le avant de l’utiliser.

Pour plus d’informations, consultez la [documentation du produit d’objet personnalisé](https://experienceleague.adobe.com/fr/docs/marketo/using/home).

- Vous ne pouvez pas modifier les types d’objets personnalisés créés par l’API dans l’interface utilisateur de Marketo.
- Le nombre maximal de types d&#39;objet personnalisés est de 10.
- Le nombre maximal de champs d’objet personnalisés est de 50 par type.
- Les noms d’API et les noms d’affichage de type d’objet personnalisé peuvent contenir des caractères alphanumériques et le caractère de soulignement « _ ».

### Type de requête

Récupérez les métadonnées de type d’objet personnalisé de l’une des manières suivantes :

- Description Custom Object Type renvoie un enregistrement de type d&#39;objet personnalisé et prend en charge le filtrage par état d&#39;approbation.
- La liste des types d’objet personnalisés renvoie tous les types d’objet personnalisés dans l’abonnement et prend en charge le filtrage par nom et statut d’approbation.

### Type de description

Le point d’entrée [Décrire le type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/describeUsingGET_1) renvoie des métadonnées pour un type d’objet personnalisé. Le paramètre de chemin d’accès `apiName` obligatoire spécifie le nom de l’API du type à décrire.

S’il existe une version approuvée, le point d’entrée la renvoie. Dans le cas contraire, il renvoie le brouillon. Utilisez le paramètre de `state` facultatif pour demander des `draft`, des `approved` ou des `approvedWithDraft`.

```http
GET /rest/v1/customobjects/schema/{apiName}/describe.json?state=approved
```

```json
{
    "requestId": "d9bf#16876fa84b9",
    "result": [
        {
            "state": "approved",
            "version": "approved",
            "displayName": "Car",
            "description": "Automobile owned",
            "apiName": "car",
            "idField": "marketoGUID",
            "createdAt": "2019-01-22T19:12:18Z",
            "updatedAt": "2019-01-22T19:12:18Z",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
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
                        "field": "id"
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
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "year",
                    "displayName": "Year",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

La réponse contient :

- Métadonnées : état, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relations.
- Champs standard : marketoGUID, createdAt, updatedAt.
- Champs personnalisés : leadId, vin, marque, modèle, année.

### Types de liste

Le point d’entrée [Liste des types d’objet personnalisés](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/listCustomObjectTypesUsingGET) renvoie des métadonnées pour tous les types d’objet personnalisés disponibles dans l’instance de destination. Elle est similaire à la [Liste d’objets personnalisés](https://experienceleague.adobe.com/docs/marketo-developer/marketo/soap/custom-objects/custom-objects.html?lang=en), mais comprend des métadonnées supplémentaires telles que l’état, les relations et les champs.

S’il existe une version approuvée, le point d’entrée la renvoie. Dans le cas contraire, il renvoie le brouillon.

Les paramètres facultatifs sont les suivants :

- **state** : indique la version à renvoyer. Les valeurs valides sont **draft**, **Approved** et **ApprovedWithDraft**.
- **names** : indique les types d’objets personnalisés à renvoyer sous la forme d’une liste de noms d’API séparés par des virgules.

```http
GET /rest/v1/customobjects/schema.json?names=purchaseHistory
```

```json
{
    "requestId": "a181#167ebe94703",
    "result": [
        {
            "state": "approved",
            "displayName": "Purchases",
            "description": "Purchase data",
            "apiName": "purchaseHistory",
            "idField": "marketoGUID",
            "createdAt": "2014-09-12T16:13:37Z",
            "updatedAt": "2014-09-12T16:13:42Z",
            "dedupeFields": [
                "lead_id",
                "product_name"
            ],
            "searchableFields": [
                [
                    "lead_id",
                    "product_name"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "lead_id"
                ]
            ],
            "relationships": [
                {
                    "field": "lead_id",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "lead_id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "marketoGUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "amount",
                    "displayName": "Amount",
                    "dataType": "float",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "lead_id",
                    "displayName": "lead_id",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "product_name",
                    "displayName": "Product Name",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "purchase_date",
                    "displayName": "Transaction Date",
                    "dataType": "datetime",
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        },
        {
            "state": "approved",
            "version": "approved",
            "displayName": "Car",
            "description": "No really, it is a car!",
            "apiName": "car_c",
            "idField": "marketoGUID",
            "createdAt": "2017-02-22T19:55:51Z",
            "updatedAt": "2018-12-11T23:52:56Z",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ]
            ],
            "relationships": [],
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
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "year",
                    "displayName": "Year",
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

### Type de création et de mise à jour

#### Type de création

Utilisez le point d’entrée [Synchroniser le type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) pour créer ou mettre à jour un type d’objet personnalisé.

Les attributs sont les suivants :

- **action** : attribut facultatif qui contrôle l’opération d’enregistrement. Les valeurs valides sont **createOnly**, **createOrUpdate** et **updateOnly**. La valeur par défaut est createOrUpdate.
- **displayName** et **apiName** : requis sauf si l’action est updateOnly. Les deux doivent être uniques pour éviter les conflits avec les types configurés par le client. Les partenaires LaunchPoint doivent ajouter un espace de noms représentatif. Pour apiName, utilisez des minuscules ou des majuscules pour le distinguer des autres chaînes de texte.
- **pluralName** : attribut facultatif qui spécifie la forme plurielle de displayName.
- **description** : attribut facultatif qui décrit le type d’objet personnalisé.
- **showInLeadDetail** : attribut booléen facultatif qui active les données d’objet personnalisées dans la page Base de données du lead de l’interface utilisateur de Marketo. La valeur par défaut est false.

Choisissez les noms d’objet personnalisés avec soin. Préfixez chaque nouveau nom d’objet personnalisé avec une chaîne qui identifie votre société. Le préfixe peut contenir des caractères alphanumériques ou des traits de soulignement. Cette convention facilite la recherche de l’objet dans l’interface utilisateur MLM et permet de s’assurer que son nom est unique.

L’exemple suivant crée un type d’objet personnalisé avec le nom d’API « transaction ».

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
  "action":"createOnly",
  "displayName": "Transaction",
  "apiName": "transaction",
  "description": "Commerce happens"
}
```

```json
{
    "requestId": "fb9d#167f2879557",
    "result": [],
    "success": true
}
```

La requête suivante décrit le type nouvellement créé.

```http
GET /rest/v1/customobjects/schema/transaction/describe.json
```

```json
{
    "requestId": "cf9b#167f28db0a9",
    "result": [
        {
            "state": "draft",
            "displayName": "Transaction",
            "description": "Commerce happens",
            "apiName": "transaction",
            "idField": null,
            "createdAt": null,
            "updatedAt": null,
            "dedupeFields": [],
            "searchableFields": [
                []
            ],
            "relationships": [],
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

La réponse contient :

- Métadonnées : état, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relations.
- Champs standard : marketoGUID, createdAt, updatedAt.

#### Type de mise à jour

L’exemple suivant met à jour la Description d’un type existant dont le nom d’API est « transaction ». L’attribut **apiName** est obligatoire. Comme le type existe déjà, la requête utilise updateOnly pour l’attribut facultatif **action**.

Outre **apiName**, vous pouvez mettre à jour les attributs disponibles lors de la création.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
  "action":"updateOnly",
  "apiName": "transaction",
  "description":"No really, commerce happens!"
}
```

```json
{
    "requestId": "103c3#167f2223fd7",
    "result": [],
    "success": true
}
```

## Validation de type

Approuvez les types d’objets personnalisés avant de les utiliser. Lorsque vous créez un type avec le point d’entrée [Synchroniser le type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectTypeUsingPOST), Marketo crée un brouillon. Après avoir ajouté des champs personnalisés, approuvez le brouillon. Approbation crée une version approuvée et supprime le brouillon.

Lorsque vous modifiez un type existant avec un point d’entrée de champ de type d’objet personnalisé Synchroniser le type d’objet personnalisé ou Ajouter/Mettre à jour/Supprimer, Marketo crée un brouillon. Les modifications apportées au type ou à ses champs n’affectent que la version préliminaire. Après avoir apporté des modifications, approuvez le brouillon. L’approbation remplace la version approuvée par le brouillon et supprime le brouillon.

Pour plus d’informations, voir la [documentation sur l’approbation d’objet personnalisé](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

Une fois qu’un type d’objet personnalisé est approuvé, vous ne pouvez pas :

- Mettez à jour le `displayName` ou le `apiName`.
- Ajouter ou supprimer un champ de lien.
- Ajoutez ou supprimez un champ de déduplication.

Planifiez soigneusement le schéma et la convention de nommage avant d’approuver le type.

### Approuver le type

Utilisez le point d’entrée [Approuver le type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/approveCustomObjectTypeUsingPOST) pour publier un brouillon en tant que nouvelle version approuvée. Le seul paramètre obligatoire est le paramètre de chemin d’accès **apiName**.

Vous ne pouvez approuver un type que lorsqu’il est à l’état de brouillon et qu’il satisfait aux [règles de validation](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object) documentées.

```http
POST /rest/v1/customobjects/schema/{apiName}/approve.json
```

```json
{
    "requestId": "11d86#1685304a983",
    "result": [],
    "success": true
}
```

### Type de rejet

Utilisez le point d’entrée [Ignorer le brouillon de type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/discardCustomObjectTypeUsingPOST) pour supprimer un brouillon. Le seul paramètre obligatoire est le paramètre de chemin d’accès `apiName`.

Vous ne pouvez ignorer qu’un type à l’état de brouillon. Vous ne pouvez pas ignorer un type approuvé.

```http
POST /rest/v1/customobjects/schema/{apiName}/discardDraft.json
```

```json
{
    "requestId": "5228#1684edde793",
    "result": [],
    "success": true
}
```

### Supprimer le type

Utilisez le point d’entrée [Supprimer le type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) pour supprimer une version approuvée. Le seul paramètre obligatoire est le paramètre de chemin d’accès `apiName`.

Cette opération est destructive et ne peut pas être annulée. Avant de supprimer un type, supprimez son utilisation des ressources telles que les déclencheurs et les filtres. Utilisez le point d’entrée Assets Get Custom Object Dependent pour récupérer les ressources dépendantes d’un type.

POST /rest/v1/customobjects/schema/{apiName}/delete.json

```json
{
    "requestId": "14e36#1684efc4227",
    "result": [],
    "success": true
}
```

## Champs d’objet personnalisés

Par défaut, tous les types d’objets personnalisés contiennent les champs standard suivants :

- GUID Marketo : identifiant unique du type d’objet personnalisé.
- Créé à : date et heure de création du type d’objet personnalisé.
- Date de mise à jour : date et heure de la dernière mise à jour du type d’objet personnalisé.

Utilisez les points d’entrée suivants pour ajouter, modifier ou supprimer des champs personnalisés.

- Le nombre maximal de champs est de 50.
- Une fois qu’un objet personnalisé est approuvé, vous pouvez y ajouter un maximum de 20 champs supplémentaires.
- Au moins un champ de déduplication est obligatoire. Trois champs de déduplication au maximum sont autorisés.
- Les noms d’API de champ et les noms d’affichage peuvent contenir des caractères alphanumériques et le caractère de soulignement « _ ».

Pour plus d’informations, voir la [documentation sur les champs d’objet personnalisés](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Ajouter des champs

Utilisez le point d’entrée [Ajouter des champs de type d’objet personnalisés](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/addCustomObjectTypeFieldsUsingPOST) pour ajouter un ou plusieurs champs à un objet personnalisé. Le corps de la requête contient un tableau `input` avec un ou plusieurs éléments . Chaque élément est un objet JSON avec des attributs qui décrivent un champ.

Les attributs de champ sont les suivants :

- `name` : obligatoire. Nom de l’API du champ, qui doit être propre à l’objet personnalisé. Utilisez des minuscules ou des majuscules pour distinguer le nom des autres chaînes de texte.
- `displayName` : obligatoire. Nom du champ lisible par l’utilisateur, qui doit être propre à l’objet personnalisé.
- `dataType` : obligatoire. Type de données du champ. Utilisez le point d’entrée [Obtenir les types de données de champ d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) pour récupérer les types de données autorisés.
- `description` : facultatif. Description du champ.
- `isDedupeField` : valeur booléenne facultative qui spécifie si le champ est utilisé pour la déduplication lors des opérations de mise à jour d’objet personnalisé. La valeur par défaut est false. Un champ de déduplication est requis pour les relations de type « un à plusieurs ».
- `relatedTo` : objet facultatif spécifiant un champ de lien. Dans le cas d’une relation un-à-plusieurs, `name` identifie l’« objet de lien » ou l’objet parent, `field` identifie le « champ de lien » ou le champ clé dans l’objet parent.

Les objets personnalisés peuvent contenir des champs avec le type de données « lien ». Les champs de lien établissent des relations entre les objets personnalisés et d’autres types d’objets, tels que le prospect et l’entreprise. Voir la [documentation sur les champs d’objet personnalisés](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields) pour plus d’informations sur les champs de lien. Utilisez le point d’entrée [Obtenir les objets liables d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) pour récupérer les objets de lien autorisés.

Un objet personnalisé ne peut pas être lié à un autre objet personnalisé qui possède un champ de lien existant. Pour plus d’informations, voir la documentation [lier des champs](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Relation De Type « Un À Plusieurs »

Pour une structure d’objet personnalisée un-à-plusieurs, utilisez un champ Lien pour connecter un objet personnalisé à un objet Lead ou Company standard. Le workflow suivant utilise l’exemple [propriétaire de la voiture](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure) pour créer un objet personnalisé qui stocke les informations de la voiture et se connecte aux prospects.

1. Créez un objet **Car**.
1. Ajoutez des champs à l’objet **Car** : dédupliquez sur **VIN** et liez-les à **Lead**&#x200B;**/ID du lead**.
1. Approuvez l’objet **Car**.

Créez tout d’abord le type d’objet personnalisé qui contient des informations spécifiques à la voiture.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action":"createOnly",
    "displayName": "Car",
    "pluralName": "Cars"
    "apiName": "car",
    "description": "Automobile owned",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "cbaa#16876dd3da6",
    "result": [],
    "success": true
}
```

Ajoutez ensuite des champs au type d’objet personnalisé Voiture . Utilisez un champ de lien pour spécifier l’objet et le champ à connecter. Dans cet exemple, l’objet de lien est Lead et le champ du lien est ID.

Utilisez un champ de chaîne pour la déduplication (VIN). Ajoutez trois champs supplémentaires pour stocker les attributs Marque, Modèle et Année.

```http
POST /rest/v1/customobjects/schema/car/addField.json
```

```json
{
  "input": [
    {
      "displayName": "Lead ID",
      "description": "Link field to Lead object",
      "name": "leadID",
      "dataType": "link",
      "relatedTo": {
        "field": "id",
        "name": "lead"
      }
    },
    {
      "displayName": "VIN",
      "description": "Vehicle ID number",
      "name": "vin",
      "dataType": "string",
      "isDedupeField": true
    },
    {
      "displayName": "Make",
      "description": "Vehicle make",
      "name": "make",
      "dataType": "string"
    },
    {
      "displayName": "Model",
      "description": "Vehicle model",
      "name": "model",
      "dataType": "string"
    },
    {
      "displayName": "Year",
      "description": "Vehicle year",
      "name": "year",
      "dataType": "integer"
    }
  ]
}

{
    "requestId": "b359#16876f17996",
    "result": [],
    "success": true
}
```

Enfin, approuvez le type d’objet personnalisé.

```http
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

### Relation Multiple-À-Multiple

Une relation multiple-à-multiple utilise un objet personnalisé « pont » entre un objet standard, tel que Lead ou Company, et un objet personnalisé « edge ». L’objet Edge est l’entité principale et contient des champs descriptifs.

L’objet bridge résout la relation avec deux champs de lien. Un champ pointe vers l’objet standard parent, comme dans une relation un-à-plusieurs. L’autre pointe vers l’objet Edge, qui est un objet personnalisé sans liens. L’objet bridge peut également contenir des champs descriptifs.

Le workflow suivant utilise l’exemple d’inscription à un cours [collégial](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure). Il crée un objet Edge de cours et un objet Bridge d’inscription qui connecte les cours aux prospects.

1. Créez un objet Edge **Cours**.
1. Ajoutez les champs à **Cours :** dédupliquer sur **ID du cours**.
1. Approuver Le **Cours**.
1. Créez un objet bridge **Enrollment**.
1. Ajoutez des champs à **Inscription :** dédupliquer sur **ID d’inscription**, liez-les au champ **Cours**&#x200B;**/ID de cours** et à **Lead**&#x200B;**/ID de lead**.
1. Valider **Inscription**.

Créez tout d’abord le type d’objet Edge contenant des informations spécifiques au cours :

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action":"createOnly",
    "displayName": "Course",
    "pluralName": "Courses",
    "apiName": "course",
    "description": "Modeling a college course, an edge object in Marketo",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "4aec#168879ede00",
    "result": [],
    "success": true
}
```

Ajoutez ensuite quatre champs personnalisés pour modéliser un cours universitaire : ID de cours, Instructeur de cours, Lieu du cours et Nom du cours. Désignez l’ID de cours comme champ de déduplication, car au moins un champ de déduplication est obligatoire.

```http
POST /rest/v1/customobjects/schema/course/addField.json
```

```json
{
    "input": [
        {
            "displayName": "Course ID",
            "name": "courseID",
            "dataType": "string",
            "isDedupeField": true
        },
        {
            "displayName": "Course Instructor",
            "name": "courseInstructor",
            "dataType": "string"
        },
        {
            "displayName": "Course Location",
            "name": "courseLocation",
            "dataType": "string"
        },
        {
            "displayName": "Course Name",
            "name": "courseName",
            "dataType": "string"
        }
    ]
}
```

```json
{
    "requestId": "cc36#16895b82a41",
    "result": [],
    "success": true
}
```

Validez le type d&#39;objet Edge afin de pouvoir le référencer lors de la liaison au type d&#39;objet Bridge. Un type d&#39;objet personnalisé doit être approuvé avant de pouvoir être sélectionné comme objet de lien.

```http
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

Après avoir terminé l’objet Edge, créez le type d’objet Bridge qui contient des informations spécifiques à l’inscription.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action": "createOnly",
    "displayName": "Enrollment",
    "pluralName": "Enrollments",
    "apiName": "enrollment",
    "description": "Bridge object for Course custom object",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "8fbb#168960f671b",
    "result": [],
    "success": true
}
```

Ajoutez deux champs Lier au type d’objet pont : l’un est lié à l’objet Lead et l’autre à l’objet Course. Utilisez le champ ID de lead pour créer un lien vers le lead et le champ ID de cours pour créer un lien vers le cours.

Ajoutez l’ID d’inscription comme champ de déduplication, car au moins un champ de déduplication est obligatoire. Ajoutez ensuite un champ Grade pour suivre les performances de l&#39;élève.

```http
POST /rest/v1/customobjects/schema/enrollment/addField.json
```

```json
{
    "input": [
        {
            "displayName": "Lead ID",
            "description": "Link field to Lead object",
            "name": "leadID",
            "dataType": "link",
            "relatedTo": {
                "field": "id",
                "name": "lead"
            }
        },
        {
            "displayName": "Course ID",
            "description": "Link field to Course object",
            "name": "courseID",
            "dataType": "link",
            "relatedTo": {
                "field": "courseID",
                "name": "course"
            }
        },
        {
            "displayName": "Enrollment ID",
            "description": "Unique ID for deduplication",
            "name": "enrollmentID",
            "dataType": "string",
            "isDedupeField": true
        },
        {
            "displayName": "Grade",
            "description": "Grade for the course",
            "name": "grade",
            "dataType": "string"
        }
    ]
}
```

```json
{
    "requestId": "7be5#168973f5052",
    "result": [],
    "success": true
}
```

Enfin, approuvez l’objet bridge.

```http
POST /rest/v1/customobjects/schema/enrollment/approve.json
```

```json
{
    "requestId": "9a76#16897b0e84b",
    "result": [],
    "success": true
}
```

Renseignez les enregistrements d’objets personnalisés par programmation en utilisant [Synchroniser l’objet personnalisé](#create_and_update) ou [Importer un objet personnalisé en bloc](https://experienceleague.adobe.com/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import.html?lang=en). Vous pouvez également utiliser [Importer des données d’objet personnalisé](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/import-custom-object-data) dans l’interface utilisateur de Marketo.

## Mettre à jour le champ

Utilisez le point d’entrée [Mettre à jour le champ de type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/updateCustomObjectTypeFieldUsingPOST) pour mettre à jour un champ dans un brouillon d’objet personnalisé.

Les paramètres de chemin requis sont les suivants :

- `apiName` : nom de l’API du type d’objet personnalisé.
- `fieldAPIName` : nom d’API du champ de type d’objet personnalisé.

Le corps de la requête contient un objet JSON avec des paires clé/valeur qui spécifient les attributs de champ à mettre à jour.

```http
POST /rest/v1/customobjects/schema/{apiName}/{fieldApiName}/updateField.json
```

```json
{
  "displayName": "Very Long Title",
  "dataType": "text"
}
```

```json
{
    "requestId": "d523#1684f355db9",
    "result": [],
    "success": true
}
```

## Supprimer les champs

Utilisez le point d’entrée [Supprimer les champs de type d’objet personnalisés](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectTypeFieldsUsingPOST) pour supprimer un ou plusieurs champs d’un objet personnalisé. Le paramètre de chemin d’accès `apiName` obligatoire spécifie le nom de l’API du type d’objet personnalisé.

Le corps de la requête contient un objet JSON avec un tableau `input` d’un ou de plusieurs éléments . Chaque élément est un objet JSON dont l’attribut `name` spécifie le nom d’API d’un champ à supprimer.

```http
POST /rest/v1/customobjects/schema/{apiName}/deleteField.json
```

```json
{
    "input":
    [
        {
            "name": "title"
        },
        {
            "name": "author"
        }
    ]
}
```

```json
{
"requestId": "b359#19934f17996",
"result": [],
"success": true
}
```

## Types de données des champs de liste

Le point d’entrée [Get Custom Object Type Data Types](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) renvoie tous les types de données de champ autorisés. Utilisez ce point d’entrée pour identifier les types de données de champ personnalisé disponibles lors de la modélisation d’un type d’objet personnalisé.

```http
GET /rest/v1/customobjects/schema/fieldDataTypes.json
```

```json
{
    "requestId": "c405#167ed49e866",
    "result": [
        "string",
        "boolean",
        "integer",
        "float",
        "link",
        "email",
        "currency",
        "date",
        "datetime",
        "phone",
        "text"
    ],
    "success": true
}
```

## Liste des objets personnalisés pouvant être liés

Le point d’entrée [Get Custom Object Linkable Objects](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) renvoie tous les objets de lien autorisés et leurs champs de lien. La réponse inclut des objets standard, tels que le prospect et l’entreprise, ainsi que tout objet personnalisé créé dans l’instance.

```http
GET /rest/v1/customobjects/schema/linkableObjects.json
```

```json
{
    "requestId": "11e62#167f1160e4e",
    "result": [
        {
            "name": "lead",
            "displayName": "Lead",
            "fields": [
                {
                    "name": "Account Balance",
                    "displayName": "Account Balance",
                    "dataType": "integer"
                },
                {
                    "name": "Email Address",
                    "displayName": "Email Address",
                    "dataType": "email"
                },
                {
                    "name": "Id",
                    "displayName": "Id",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Display Name",
                    "displayName": "Marketo Social Facebook Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Id",
                    "displayName": "Marketo Social Facebook Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Photo URL",
                    "displayName": "Marketo Social Facebook Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Profile URL",
                    "displayName": "Marketo Social Facebook Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Reach",
                    "displayName": "Marketo Social Facebook Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Referred Enrollments",
                    "displayName": "Marketo Social Facebook Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Referred Visits",
                    "displayName": "Marketo Social Facebook Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Gender",
                    "displayName": "Marketo Social Gender",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Display Name",
                    "displayName": "Marketo Social LinkedIn Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Id",
                    "displayName": "Marketo Social LinkedIn Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Photo URL",
                    "displayName": "Marketo Social LinkedIn Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Profile URL",
                    "displayName": "Marketo Social LinkedIn Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Reach",
                    "displayName": "Marketo Social LinkedIn Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social LinkedIn Referred Enrollments",
                    "displayName": "Marketo Social LinkedIn Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social LinkedIn Referred Visits",
                    "displayName": "Marketo Social LinkedIn Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Syndication Id",
                    "displayName": "Marketo Social Syndication Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Total Referred Enrollments",
                    "displayName": "Marketo Social Total Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Total Referred Visits",
                    "displayName": "Marketo Social Total Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Display Name",
                    "displayName": "Marketo Social Twitter Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Id",
                    "displayName": "Marketo Social Twitter Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Photo URL",
                    "displayName": "Marketo Social Twitter Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Profile URL",
                    "displayName": "Marketo Social Twitter Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Reach",
                    "displayName": "Marketo Social Twitter Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Referred Enrollments",
                    "displayName": "Marketo Social Twitter Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Referred Visits",
                    "displayName": "Marketo Social Twitter Referred Visits",
                    "dataType": "integer"
                }
            ]
        },
        {
            "name": "company",
            "displayName": "Company",
            "fields": [
                {
                    "name": "Id",
                    "displayName": "Id",
                    "dataType": "integer"
                }
            ]
        },
        {
            "name": "car_c",
            "displayName": "Car",
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string"
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string"
                }
            ]
        }
    ],
    "success": true
}
```

## Obtenir l’Assets dépendante de l’objet personnalisé

Le point d’entrée [Get Custom Object Dependent Assets](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeDependentAssetsUsingGET) renvoie les ressources dépendantes d’un type d’objet personnalisé et leur emplacement dans l’instance. Utilisez-la lors de la suppression d’une intégration pour identifier partout où un type d’objet personnalisé est utilisé.

```http
GET /rest/v1/customobjects/schema/{apiName}/dependentAssets.json
```

```json
{
    "requestId": "71cf#16a21f30ed6",
    "result": [
        {
            "assetType": "Smart Campaign",
            "assetId": 3773,
            "assetName": "CarTest.HasCar (Smart List)"
        },
        {
            "assetType": "Smart Campaign",
            "assetId": 3773,
            "assetName": "CarTest.HasCar (Smart List)",
            "usedFields": [
                "leadID",
                "make",
                "model",
                "vin",
                "year"
            ]
        }
    ],
    "success": true
}
```

## Délais dépassés

- Le délai d’expiration des points d’entrée d’objets personnalisés est de 30 s, sauf indication contraire.
- Le délai d’expiration de la synchronisation des objets personnalisés est de 120 s.
- Le délai d’expiration de la suppression des objets personnalisés est de 60 s.
