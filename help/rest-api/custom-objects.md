---
title: Objets personnalisés
feature: REST API, Custom Objects
description: Créer et manipuler des objets Marketo personnalisés.
exl-id: 88e8829b-f8f1-46d7-a753-5aa6e20e2c40
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '2909'
ht-degree: 0%

---

# Objets personnalisés

[**Référence de point d’entrée d’objet personnalisé**](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects) Marketo permet aux utilisateurs de définir des objets personnalisés Marketo associés à des objets standard Marketo (prospects, sociétés) ou d’autres objets personnalisés Marketo.  Les objets personnalisés Marketo peuvent être créés à l’aide de l’interface utilisateur de Marketo, comme décrit [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects) ou à l’aide de l’API de métadonnées d’objet personnalisé, comme décrit ci-dessous.

Un type d’abonnement Marketo approprié est requis pour accéder à l’API de métadonnées d’objet personnalisé.  Veuillez consulter votre CSM pour plus de détails.

## Liste

Outre les appels standard de description, de requête, de mise à jour et de suppression disponibles pour les objets de base de données de prospect, les objets personnalisés disposent d’un [appel de liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET).  L’appel de ce point d’entrée renvoie une réponse avec une liste d’objets personnalisés disponibles dans l’instance de destination, ainsi que des métadonnées supplémentaires sur les objets.

```
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

La réponse fournit une liste des relations présentes sur chaque objet.  Une relation comporte un membre de `field` qui indique le champ de l&#39;objet contenant la valeur du lien, un membre de `type` qui indique si la relation est avec un objet de type parent ou enfant, et un objet de `relatedTo` indiquant le nom de l&#39;objet associé, ainsi que le champ de lien de cet objet.

## Décrire

L’[appel de description](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) pour les objets personnalisés suit le même modèle que celui d’Opportunités et entreprises, avec l’ajout du tableau `relationships` dans la réponse et d’un paramètre de chemin d’accès `apiName` dans l’URI qui prend le nom de l’API du type d’objet personnalisé à décrire.  Comme l’appel de liste, toutes les relations disponibles pour ce type d’objet personnalisé sont répertoriées.

```
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

[L’interrogation d’objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET) est légèrement différente des autres API de la base de données de leads et utilise `apiName` paramètre de chemin d’accès tel que descriptionr.  Pour les paramètres filterType classiques, la requête est un GET simple comme les requêtes pour d’autres types d’enregistrements, et nécessite un `filterType` et un `filterValues`.  Il accepte éventuellement les paramètres `**fields**`, `batchSize` et `nextPageToken`.  Lors de la demande d’une liste de champs, si un champ particulier est demandé, mais n’est pas renvoyé, la valeur est implicitement nulle.

```
GET /rest/v1/customobjects/{apiName}.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa,dff23271-f996-47d7-984f-f2676861b5fb
```

```
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

Lorsque vous interrogez des clés composites, l’API se comporte comme l’API des rôles d’opportunité, en acceptant une requête POST avec un corps JSON.  Le corps JSON peut comporter les mêmes membres qu’une requête GET, à l’exception de `filterValues`.  Au lieu des valeurs de filtre, il existe un tableau `input` qui prend les objets contenant un membre nommé pour chacun des `dedupeFields` du type d’objet.

```
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

Utilisez le point d’entrée [Synchroniser les objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) pour créer ou mettre à jour des objets personnalisés. Vous pouvez spécifier l’opération à l’aide du paramètre `action`.  Jusqu’à 300 enregistrements peuvent être créés ou mis à jour en un seul appel.  Les valeurs utilisées dans le tableau `input` sont largement basées sur les informations renvoyées par le point d’entrée [Décrire les objets personnalisés](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/endpoint-reference#!/Custom_Objects/descriptionusingGET_1). Dans un exemple d’objet de carte, il n’existe qu’un seul champ de déduplication, `vin`.  Pour mettre à jour ou créer des enregistrements lors de l’utilisation du mode dedupeFields , chaque enregistrement de notre tableau d’entrée doit inclure au moins un champ `vin`.

```
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

Lors de l’exécution de mises à jour via `idField` mode, la `idField` est toujours `marketoGUID`. Par conséquent, chaque enregistrement nécessite toujours un champ de `marketoGUID`.  N’oubliez pas que `idField` n’est valide que pour le type d’action updateOnly, car ce champ est toujours géré par le système.  Votre réponse inclut le **statut** de chaque enregistrement individuel dans le tableau de résultats, ainsi qu’un tableau `marketoGUID` ou `reasons` selon que l’opération a réussi ou non pour un enregistrement individuel.

## Supprimer

La [suppression des enregistrements](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) est très simple.  Sélectionnez simplement le mode de `deleteBy`, `idField` ou `dedupeFields`, et incluez les champs correspondants dans chacun des enregistrements de votre tableau de `input`. Un maximum de 300 enregistrements par appel est autorisé.

```
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

Comme la mise à jour, votre résultat contient un statut pour chaque enregistrement individuel, et soit un tableau `marketoGUID`, soit un tableau `reasons`, selon que la suppression a réussi ou non.

## Types d’objet personnalisés

L’API de métadonnées d’objet personnalisé vous permet de gérer à distance des schémas d’objet personnalisés.  L’API vous permet de créer un type d’objet personnalisé ou de modifier un type existant.  Une fois le type d&#39;objet personnalisé créé ou modifié, il doit être approuvé pour utilisation.  Pour plus d’informations sur les objets personnalisés, consultez la documentation du produit [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/home).

* Les types d’objets personnalisés créés par l’API ne peuvent pas être modifiés à l’aide de l’interface utilisateur de Marketo
* Le nombre maximal de types d’objets personnalisés autorisés est de 10
* Le nombre maximal de champs d’objet personnalisés autorisés est de 50 par type
* Les noms d’API et les noms d’affichage de type d’objet personnalisé peuvent contenir des caractères alphanumériques et un trait de soulignement « _ »

### Type de requête

Il existe deux manières de récupérer des métadonnées de type d’objet personnalisé : Décrire le type d’objet personnalisé, qui renvoie  l’enregistrement d’un seul type d’objet personnalisé, et peut être filtré par état d’approbation, et Liste des types d’objet personnalisés, qui renvoie une liste de tous les types d’objet personnalisés de l’abonnement, et peut être filtré par nom et par état d’approbation.

### Type de description

Le point d’entrée [Décrire le type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) renvoie des métadonnées pour un seul type d’objet personnalisé. Le paramètre de chemin d’accès `apiName` obligatoire est le nom de l’API du type d’objet personnalisé décrit.  Si une version approuvée existe, elle est renvoyée.  Dans le cas contraire, le brouillon est renvoyé.  Le paramètre de `state` facultatif est utilisé pour spécifier la version à renvoyer : `draft`, `approved` ou `approvedWithDraft`.

```
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

Voici les attributs qui s’affichent :

* Métadonnées : état, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relations
* Champs standard : marketoGUID, createdAt, updatedAt
* Champs personnalisés leadId, vin, marque,  modèle, année

### Types de liste

Le point d’entrée [Liste des types d’objet personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/listCustomObjectTypesUsingGET) renvoie des métadonnées pour tous les types d’objet personnalisés disponibles dans l’instance de destination.  Notez que ce point d’entrée est similaire à [Répertorier les objets personnalisés](https://experienceleague.adobe.com/docs/marketo-developer/marketo/soap/custom-objects/custom-objects.html?lang=en), mais il est plus complet et inclut des métadonnées supplémentaires telles que l’état, les relations et les champs. Si une version approuvée existe, elle est renvoyée.  Dans le cas contraire, le brouillon est renvoyé.  Le paramètre facultatif **state** est utilisé pour spécifier la version du type d’objet personnalisé à renvoyer : **draft**, **Approved** ou **ApprovedWithDraft**.  Le paramètre facultatif **names** est utilisé pour spécifier les noms spécifiques des types d’objets personnalisés à renvoyer ; il est structuré sous la forme d’une liste de noms d’API séparés par des virgules.

```
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
            "description": "No really, it's a car!",
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

Le point d’entrée [Synchroniser le type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) est utilisé pour créer ou mettre à jour un type d’objet personnalisé.  L’opération d’enregistrement à effectuer est contrôlée par l’attribut facultatif **action** qui peut être **createOnly**, **createOrUpdate** ou **updateOnly**.  Le paramètre par défaut est createOrUpdate. Les attributs **displayName** et **apiName** sont requis, sauf si vous utilisez updateOnly comme action.   Les deux doivent être uniques pour éviter les conflits avec les types configurés par le client.  Si vous êtes un partenaire LaunchPoint, vous devez ajouter un espace de noms représentatif à ces noms.  Pour apiName, la convention est d’utiliser des minuscules ou des majuscules pour aider à distinguer les autres chaînes de texte. L’attribut facultatif **pluralName** spécifie la forme plurielle de displayName.  L’attribut facultatif **description** est utilisé pour décrire le type d’objet personnalisé.  L’attribut booléen facultatif **showInLeadDetail** est utilisé pour permettre l’affichage des données d’objet personnalisées dans la page Base de données du prospect de l’interface utilisateur de Marketo.  Le paramètre par défaut est false.

Soyez prudent lorsque vous nommez des objets personnalisés. Lors de la création d’un objet personnalisé, il est recommandé de faire précéder le nom d’une chaîne qui indique le nom de votre société (caractères alphanumériques ou traits de soulignement autorisés). Cela permet de rechercher facilement l’objet personnalisé dans l’interface utilisateur MLM et permet également de s’assurer que le nom est unique.

Voici un exemple de création d’un type d’objet personnalisé avec l’API  Nommez « transaction ».

```
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

Voici un appel suivant pour décrire le type nouvellement créé.

```
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

Nous pouvons voir ici les données d’objet personnalisées suivantes :

* Métadonnées : état, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relations
* Champs standard : marketoGUID, createdAt, updatedAt

#### Type de mise à jour

Voici un exemple de mise à jour de la description d’un type existant dont le nom d’API est « transaction ».  L’attribut **apiName** est obligatoire.  Ici, nous supposons que le type existe déjà et nous utilisons updateOnly pour l’attribut facultatif **action**.  Outre **apiName**, les attributs disponibles pour la création peuvent être mis à jour.

```
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

Les types d&#39;objet personnalisés doivent être approuvés avant de pouvoir être utilisés. Lorsqu’un nouveau type d’objet personnalisé est créé à l’aide du point d’entrée [Synchroniser le type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectTypeUsingPOST), il est créé en tant que version préliminaire. Lorsque vous avez terminé d’ajouter des champs personnalisés, vous devez approuver la version préliminaire. Cela crée une version approuvée et supprime le brouillon. Lorsqu’un type d’objet personnalisé existant est modifié à l’aide du point d’entrée Synchroniser le type d’objet personnalisé ou de l’un des points d’entrée Ajouter/Mettre à jour/Supprimer le type d’objet personnalisé Champ, un brouillon est créé. Toutes les modifications apportées au type ou à ses champs n’ont un impact que sur le brouillon. Une fois la modification terminée, vous devez approuver le brouillon. Cette opération remplace la version approuvée par la version préliminaire et supprime la version préliminaire. Pour plus d’informations sur l’approbation d’objet personnalisé, consultez la documentation du produit [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

Une fois qu’un type d’objet personnalisé est approuvé, vous ne pouvez pas :

* Mettre à jour le `displayName` ou le `apiName`
* Ajouter ou supprimer un champ de lien
* Ajouter ou supprimer un champ de déduplication

Pour ces raisons, il est important de réfléchir soigneusement au schéma et à la convention de nommage que vous prévoyez d’utiliser.

### Approuver le type

Utilisez le point d’entrée [Approuver le type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/approveCustomObjectTypeUsingPOST) pour publier un brouillon en tant que nouvelle version approuvée.  **apiName** est le seul paramètre obligatoire en tant que paramètre de chemin d’accès.  Un type ne peut pas être approuvé à moins qu&#39;il ne soit à l&#39;état de brouillon et qu&#39;il satisfasse à un ensemble de règles de validation décrites [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

```
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

Utilisez le point d’entrée [Ignorer le brouillon de type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/discardCustomObjectTypeUsingPOST) pour supprimer un brouillon. `apiName` est le seul paramètre obligatoire en tant que paramètre de chemin d’accès. Un type doit être à l’état de brouillon pour être ignoré, c’est-à-dire qu’un type approuvé ne peut pas être ignoré.

```
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

Utilisez le point d’entrée [Supprimer le type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) pour supprimer une version approuvée.  `apiName` est le seul paramètre obligatoire en tant que paramètre de chemin d’accès.  Notez qu’il s’agit d’une opération destructrice qui ne peut pas être annulée.  Un type ne peut pas être supprimé à moins qu’il n’ait été supprimé de l’utilisation par une ressource, comme des déclencheurs ou des filtres.  Le point d’entrée Assets Get Custom Object Dependent peut être utilisé pour récupérer une liste de ressources dépendantes pour un type donné.

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

* GUID Marketo - Identifiant unique pour le type d’objet personnalisé
* Créé à - Date et heure de création du type d’objet personnalisé
* Mise à jour le - Date et heure de la dernière mise à jour du type d’objet personnalisé

Vous pouvez ajouter, modifier ou supprimer des champs personnalisés à l’aide des points d’entrée décrits ci-dessous.

* Le nombre maximal de champs autorisés est de 50
* Une fois qu’un objet personnalisé a été approuvé, vous pouvez y ajouter un maximum de 20 champs supplémentaires
* Au moins 1 champ de déduplication est requis, 3 au maximum sont autorisés.
* Les noms d’API de champ et d’affichage peuvent contenir des caractères alphanumériques et le trait de soulignement « _ »

Pour plus d’informations sur les champs d’objet personnalisés, consultez la documentation du produit [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Ajouter des champs

Le point d’entrée [Ajouter des champs de type d’objet personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/addCustomObjectTypeFieldsUsingPOST) vous permet d’ajouter un ou plusieurs champs à votre objet personnalisé.  Le corps de la requête contient un tableau `input` avec un ou plusieurs éléments .  Chaque élément est un objet JSON avec des attributs qui décrivent un champ. L’attribut `name` obligatoire est le nom d’API du champ et doit être propre à l’objet personnalisé.   La convention consiste à utiliser des minuscules ou des majuscules pour faire la distinction entre d’autres chaînes de texte. L’attribut `displayName` obligatoire est le nom lisible par l’homme du champ et doit être propre à l’objet personnalisé. L’attribut `dataType` obligatoire est le type de données du champ.  A  liste des types de données autorisés pouvant être obtenus en appelant le point d’entrée [Obtenir les types de données de champ de type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET).  Les objets personnalisés peuvent contenir des champs avec le type de données « lien ».  Les champs de lien sont utilisés pour établir des relations entre les objets personnalisés et d&#39;autres types d&#39;objets du système, par exemple Lead, Société.  Vous trouverez plus d’informations sur les champs de lien [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). L’attribut `description` facultatif est la description du champ. L’attribut booléen `isDedupeField` facultatif indique si le champ est utilisé à des fins de déduplication lors des opérations de mise à jour d’objet personnalisé.  Le paramètre par défaut est false.  Pour les relations de type « un à plusieurs », un champ de déduplication est requis. L’attribut d’objet `relatedTo` facultatif spécifie un champ de lien.  Pour les relations un-à-plusieurs, cet objet contient un attribut `name` qui est l’« objet de lien » ou l’objet parent à lier, et un attribut `field` qui est le « champ de lien »,  ou le champ de l’objet parent à utiliser comme attribut de clé.  Appelez le point d’entrée [Get Custom Object Linkable Objects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) pour récupérer une liste d’objets de lien autorisés.  Pour plus d’informations sur les champs de lien, consultez la documentation du produit [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). Un objet personnalisé ne peut pas être lié à un autre objet personnalisé qui possède un champ de lien existant.

### Relation De Type « Un À Plusieurs »

Pour une structure d’objet personnalisée un-à-plusieurs, utilisez un champ de lien dans un objet personnalisé pour le connecter à un objet standard : prospect ou entreprise. En utilisant l’exemple de propriétaire de voiture de la documentation du produit Marketo [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure), nous créons un objet personnalisé qui contient des informations relatives à la voiture pour établir une connexion avec les prospects.

1. Créez un objet **Car**
1. Ajouter des champs à l’objet **Car** : déduplication sur **VIN**, lien vers **Lead****/ID du lead**
1. Approuver l’objet **Car**

Créez tout d’abord le type d’objet personnalisé pour contenir des informations spécifiques à la voiture.

```
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

Ajoutez à présent des champs au type d’objet personnalisé Voiture . Nous utilisons un champ de lien pour spécifier à la fois l’objet et le champ auquel se connecter. Dans ce cas, l’objet du lien est Lead et le champ du lien est ID. Utilisez un champ de chaîne pour la déduplication (VIN).  Nous ajouterons trois champs supplémentaires pour stocker les attributs de voiture supplémentaires (marque, modèle, année).

```
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

```
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

Les relations multiples-à-multiples sont représentées à l’aide d’un objet personnalisé « pont », ou intermédiaire, entre un objet personnalisé standard, tel que Lead ou Company, et un objet personnalisé « edge ». L’objet Edge est l’entité principale contenant des attributs descriptifs (champs). L’objet bridge contient les données permettant de résoudre les relations d’objet à l’aide de 2 champs de lien.  Un champ de lien renvoie à l’objet standard parent comme dans un  configuration de relation un-à-plusieurs.  L’autre champ de lien pointe vers l’objet Edge , qui est un objet personnalisé sans liens.  L’objet bridge peut également contenir des attributs descriptifs (champs). En utilisant l’exemple d’inscription à un cours universitaire de la documentation du produit Marketo [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure), nous créons un objet personnalisé Edge pour contenir des informations relatives au cours, ainsi qu’un objet de passerelle d’inscription utilisé pour connecter les cours aux prospects. Les étapes sont les suivantes :

1. Créer un objet Edge **Course**
1. Ajouter des champs au **Cours :** dédupliquer sur **ID du cours**
1. Approve **Course**
1. Créer un objet bridge **Enrollment**
1. Ajoutez des champs à **Inscription :** dédupliquer sur **ID d’inscription**, lien vers le champ **Cours****/ID de cours** et lien vers **Lead****/ID de lead**.
1. Approve **Enrollment**

Créez tout d’abord le type d’objet Edge pour contenir des informations spécifiques au cours :

```
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

Ajoutons ensuite des champs personnalisés au type d’objet Edge.  Dans cet exemple, nous allons ajouter les quatre champs personnalisés suivants pour modéliser un cours universitaire : ID du cours, Instructeur du cours, Lieu du cours, Nom du cours.  Notez que nous désignons l’ID de cours comme champ de déduplication, puisqu’au moins un champ de déduplication est obligatoire.

```
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

```
{
    "requestId": "cc36#16895b82a41",
    "result": [],
    "success": true
}
```

Nous devons maintenant approuver le type d&#39;objet Edge afin de pouvoir le référencer ultérieurement lors de la liaison au type d&#39;objet Bridge.  Notez que les types d’objet personnalisés doivent être approuvés pour pouvoir être sélectionnés en tant qu’objet de lien.

```
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

L&#39;objet Edge est terminé.  Passons maintenant à la création du type d’objet de pont pour contenir des informations spécifiques à l’inscription.

```
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

Pour ajouter des champs personnalisés au type d’objet Bridge, ajoutez deux champs de lien : l’un lié à l’objet Lead et l’autre à l’objet Course que nous venons de créer. Pour créer un lien vers l’objet Lead, utilisez le champ ID du lead . Pour créer un lien vers l’objet de cours, utilisez le champ ID du cours .  Ajoutez ensuite un identifiant unique d’ID d’inscription comme champ de déduplication, car au moins un champ de déduplication est obligatoire. Enfin, ajoutez un champ Grade pour suivre les résultats de l’élève.

```
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

```
POST /rest/v1/customobjects/schema/enrollment/approve.json
```

```json
{
    "requestId": "9a76#16897b0e84b",
    "result": [],
    "success": true
}
```

Vous pouvez renseigner les enregistrements d&#39;objets personnalisés par programmation en utilisant [Synchroniser l&#39;objet personnalisé](#create_and_update) ou [Importer un objet personnalisé en bloc](https://experienceleague.adobe.com/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import.html?lang=en). Vous pouvez également utiliser la fonctionnalité d’interface utilisateur de Marketo [Importer des données d’objet personnalisées](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/import-custom-object-data).

## Mettre à jour le champ

Le point d’entrée [Mettre à jour le champ de type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/updateCustomObjectTypeFieldUsingPOST) vous permet de mettre à jour un champ dans votre brouillon d’objet personnalisé.  Le paramètre de chemin d’accès obligatoire `apiName` est le nom de l’API du type d’objet personnalisé.  Le paramètre de chemin d’accès obligatoire `fieldAPIName` est le nom d’API du champ de type d’objet personnalisé.  Le corps de la requête contient un objet JSON contenant des paires clé/valeur qui spécifient les attributs de champ à mettre à jour.

```
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

Le point d’entrée [Supprimer les champs de type d’objet personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectTypeFieldsUsingPOST) vous permet de supprimer un ou plusieurs champs de votre objet personnalisé.  Le paramètre de chemin d’accès obligatoire `apiName` est le nom de l’API du type d’objet personnalisé.  Le corps de la requête contient un objet JSON avec un tableau `input` comportant un ou plusieurs éléments .  Chaque élément est un objet JSON avec un attribut `name` qui spécifie le nom de l’API du champ à supprimer.

```
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

Le point d’entrée [Obtenir les types de données de champ d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) renvoie la liste de tous les types de données de champ autorisés. Cela s’avère utile lors de la modélisation de votre type d’objet personnalisé pour identifier les types de données de champ personnalisé pris en charge.

```
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

Le point d’entrée [Get Custom Object Linkable Objects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) renvoie une liste de tous les objets de lien autorisés et de leurs champs de lien.  La liste contient les objets standard (lead, entreprise) et les objets personnalisés qui ont été créés dans l&#39;instance.

```
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

Le point d’entrée [Get Custom Object Dependent Assets](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeDependentAssetsUsingGET) renvoie une liste des ressources dépendantes d’un type d’objet personnalisé, y compris leur emplacement dans l’instance.  Cela s’avère utile lors de la suppression d’une intégration et vous devez identifier partout où un type d’objet personnalisé est utilisé.

```
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

* Les points d’entrée d’objets personnalisés ont un délai d’expiration de 30 s, sauf indication ci-dessous
   * Synchroniser les objets personnalisés : 120s 
   * Supprimer objets personnalisés : 60s
