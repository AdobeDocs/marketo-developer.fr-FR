---
title: Objets personnalisés
feature: REST API, Custom Objects
description: Créez et manipulez des objets Marketo personnalisés.
source-git-commit: f1c9c5ba07013c2fdccb9f799b5c0077eb789a4b
workflow-type: tm+mt
source-wordcount: '2910'
ht-degree: 0%

---


# Objets personnalisés

[**Référence de point d’entrée d’objet personnalisé**](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects) Marketo permet aux utilisateurs de définir des objets personnalisés Marketo liés à des objets standard Marketo (Leads, Entreprises) ou d’autres objets personnalisés Marketo.  Les objets personnalisés Marketo peuvent être créés à l’aide de l’interface utilisateur de Marketo comme décrit [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects) ou à l’aide de l’API de métadonnées d’objet personnalisé comme décrit ci-dessous.

Un type d’abonnement Marketo approprié est requis pour accéder à l’API de métadonnées d’objet personnalisé.  Pour plus d’informations, consultez votre CSM .

## Liste

Outre les appels standard de description, de requête, de mise à jour et de suppression disponibles pour les objets de base de données de piste, les objets personnalisés ont un [appel de liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET) disponible.  L’appel de ce point de terminaison renvoie une réponse avec une liste d’objets personnalisés disponibles dans l’instance de destination, ainsi que des métadonnées supplémentaires sur les objets.

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

La réponse donne une liste des relations présentes sur chaque objet.  Une relation aura un membre `field` qui indique le champ sur l’objet qui contient la valeur de lien, un membre `type` qui indique si la relation est à un objet de type parent ou enfant, et un objet `relatedTo` qui indique le nom de l’objet associé, et le champ de lien sur cet objet.

## Description

L’ [ appel de description](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) pour les objets personnalisés suit le même modèle que celui des opportunités et des entreprises, avec l’ajout du tableau `relationships` dans la réponse et un paramètre de chemin d’accès `apiName` dans l’URI qui prend le nom d’API du type d’objet personnalisé à décrire.  Tout comme l’appel de liste, cette option répertorie toutes les relations disponibles pour ce type d’objet personnalisé.

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

[La requête d’objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET) diffère légèrement des autres API de base de données de piste et utilise le paramètre de chemin `apiName` comme décrit.  Pour un paramètre filterType normal, la requête est une requête simple de type GET pour d’autres types d’enregistrements, et requiert un `filterType` et un `filterValues`.  Il accepte éventuellement les paramètres `**fields**`, `batchSize` et `nextPageToken`.  Lorsque vous demandez une liste de champs, si un champ particulier est demandé, mais n’est pas renvoyé, la valeur est implicitement nulle.

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

Par ailleurs, lorsque vous interrogez des clés composites, l’API se comporte comme l’API Rôles d’opportunité, en acceptant un POST avec un corps JSON.  Le corps JSON peut avoir les mêmes membres qu’une requête de GET, à l’exception de `filterValues`.  Au lieu des valeurs de filtre, il existe un tableau `input` qui prend les objets qui contiennent un membre nommé pour chaque `dedupeFields` du type d’objet.

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

Utilisez le point d’entrée [Synchroniser les objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) pour créer ou mettre à jour des objets personnalisés. Vous pouvez spécifier l’opération à l’aide du paramètre `action` .  Vous pouvez créer ou mettre à jour jusqu’à 300 enregistrements en un seul appel.  Les valeurs utilisées dans le tableau `input` sont largement basées sur les informations renvoyées par le point de terminaison [Description des objets personnalisés](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/endpoint-reference#!/Custom_Objects/descriptionUsingGET_1). Dans un exemple d’objet de voiture, il n’y a qu’un seul champ de déduplication, `vin`.  Pour mettre à jour ou créer des enregistrements lors de l’utilisation du mode dedupeFields , chaque enregistrement de notre tableau d’entrée doit inclure au moins un champ `vin`.

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

Lors de l’exécution de mises à jour via le mode `idField`, `idField` sera toujours `marketoGUID`, de sorte que chaque enregistrement nécessitera toujours un champ `marketoGUID`.  N’oubliez pas que `idField` n’est valide que pour le type d’action updateOnly, car ce champ est toujours géré par le système.  Votre réponse inclura le **statut** de chaque enregistrement individuel dans le tableau de résultats, et soit un tableau `marketoGUID` ou `reasons` selon que l’opération a réussi ou non pour un enregistrement individuel.

## Supprimer

[La suppression d&#39;enregistrements](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) est très simple.  Sélectionnez simplement votre mode `deleteBy`, `idField` ou `dedupeFields`, et incluez les champs correspondants dans chacun des enregistrements de votre tableau `input`. Un maximum de 300 enregistrements par appel est autorisé.

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

Comme pour la mise à jour, votre résultat contiendra un état pour chaque enregistrement individuel, et soit un tableau `marketoGUID` ou `reasons` selon si la suppression a réussi.

## Types d’objet personnalisés

L’API de métadonnées d’objet personnalisé vous permet de gérer à distance les schémas d’objet personnalisés.  L’API vous permet de créer un type d’objet personnalisé ou d’en modifier un existant.  Une fois le type d’objet personnalisé créé ou modifié, il doit être approuvé pour utilisation.  Pour plus d’informations sur les objets personnalisés, consultez la documentation du produit [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/home).

* Les types d’objets personnalisés créés par l’API ne peuvent pas être modifiés à l’aide de l’interface utilisateur de Marketo
* Le nombre maximal de types d’objets personnalisés est de 10.
* Le nombre maximal de champs d’objet personnalisés autorisé est de 50 par type.
* Les noms d’API et les noms d’affichage de type d’objet personnalisé peuvent contenir des caractères alphanumériques et un caractère de soulignement &quot;_&quot;.

### Type de requête

Il existe deux façons de récupérer les métadonnées de type d’objet personnalisé : Description du type d’objet personnalisé, qui renvoie  l’enregistrement d’un seul type d’objet personnalisé est filtrable par état d’approbation et par type d’objet personnalisé de liste , qui renvoie une liste de tous les types d’objets personnalisés dans l’abonnement et peut être filtré par nom et par état d’approbation.

### Type de description

Le point d’entrée [Description du type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) renvoie des métadonnées pour un seul type d’objet personnalisé. Le paramètre de chemin d’accès `apiName` requis est le nom de l’API du type d’objet personnalisé décrit.  Si une version approuvée existe, elle est renvoyée.  Dans le cas contraire, la version préliminaire est renvoyée.  Le paramètre facultatif `state` est utilisé pour spécifier la version à renvoyer : `draft`, `approved` ou `approvedWithDraft`.

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

Ici, nous pouvons voir les attributs suivants :

* Métadonnées : état, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relations
* Champs standard : marketoGUID, createdAt, updatedAt
* Champs personnalisés leadId, vin, make,  modèle, année

### Types de liste

Le point d’entrée [List Custom Object Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/listCustomObjectTypesUsingGET) renvoie des métadonnées pour tous les types d’objets personnalisés disponibles dans l’instance de destination.  Notez que ce point de terminaison est similaire à [Liste des objets personnalisés](https://experienceleague.adobe.com/docs/marketo-developer/marketo/soap/custom-objects/custom-objects.html?lang=fr), mais il est plus complet et inclut des métadonnées supplémentaires telles que l’état, les relations et les champs. Si une version approuvée existe, elle est renvoyée.  Dans le cas contraire, la version préliminaire est renvoyée.  Le paramètre facultatif **state** est utilisé pour spécifier la version du type d’objet personnalisé à renvoyer : **draft**, **approved** ou **approvedWithDraft**.  Le paramètre facultatif **names** est utilisé pour spécifier des noms spécifiques de types d’objets personnalisés à renvoyer. Il est structuré comme une liste de noms d’API séparés par des virgules.

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

#### Créer un type

Le point d’entrée [Sync Custom Object Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) est utilisé pour créer ou mettre à jour un type d’objet personnalisé.  L’opération d’enregistrement à effectuer est contrôlée par l’attribut facultatif **action** qui peut être **createOnly**, **createOrUpdate** ou **updateOnly**.  Le paramètre par défaut est createOrUpdate. Les attributs **displayName** et **apiName** sont requis, sauf lors de l’utilisation de updateOnly comme action.   Les deux doivent être uniques pour éviter les collisions avec des types configurés par le client.  Si vous êtes un partenaire LaunchPoint, vous devez ajouter un espace de noms représentatif à ces noms.  Pour apiName, la convention consiste à utiliser des minuscules ou des majuscules pour aider à distinguer les autres chaînes de texte. L’attribut facultatif **pluralName** spécifie la forme plurielle de displayName.  L’attribut facultatif **description** est utilisé pour décrire le type d’objet personnalisé.  L’attribut booléen facultatif **showInLeadDetail** est utilisé pour activer l’affichage de données d’objet personnalisées dans la page Base de données de pistes de l’interface utilisateur de Marketo.  Le paramètre par défaut est false.

Soyez prudent lorsque vous nommez des objets personnalisés. Lors de la création d’un objet personnalisé, il est recommandé de faire précéder le nom d’une chaîne indiquant le nom de votre société (caractères alphanumériques ou traits de soulignement autorisés). Cela permet de faciliter la recherche de l’objet personnalisé dans l’interface utilisateur MLM et de ne pas s’assurer que le nom est unique.

Voici un exemple de création d’un type d’objet personnalisé avec l’API  Nom &quot;transaction&quot;.

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

Voici un appel suivant pour décrire le type que vous venez de créer.

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

Ici, nous pouvons voir les données personnalisées suivantes liées à l’objet :

* Métadonnées : état, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relations
* Champs standard : marketoGUID, createdAt, updatedAt

#### Type de mise à jour

Voici un exemple de mise à jour de la description pour un type existant dont le nom d’API est &quot;transaction&quot;.  L’attribut **apiName** est requis.  Nous supposerons ici que le type existe déjà et utiliserons updateOnly pour l&#39;attribut facultatif **action**.  Outre **apiName**, les attributs disponibles pour la création peuvent être mis à jour.

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

## Validation du type

Les types d’objets personnalisés doivent être approuvés avant d’être utilisés. Lorsqu’un nouveau type d’objet personnalisé est créé à l’aide du point de terminaison [Sync Custom Object Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectTypeUsingPOST) , il est créé en tant que version préliminaire. Lorsque vous avez terminé d’ajouter des champs personnalisés, vous devez approuver la version préliminaire. Cette opération crée une version approuvée et supprime le brouillon de version. Lorsqu’un type d’objet personnalisé existant est modifié à l’aide du point de terminaison Synchroniser le type d’objet personnalisé ou en utilisant l’un des points de terminaison Ajouter/Mettre à jour/Supprimer un type d’objet personnalisé de type , une version préliminaire est créée. Toutes les modifications apportées au type ou à ses champs n’affectent que la version préliminaire. Lorsque vous avez terminé de modifier, vous devez approuver la version préliminaire. Cette opération remplace la version approuvée par la version préliminaire et supprime la version préliminaire. Pour plus d’informations sur l’approbation d’objet personnalisé, consultez la documentation du produit [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

Une fois qu’un type d’objet personnalisé est approuvé, vous ne pouvez pas :

* Mettez à jour `displayName` ou `apiName`
* Ajout ou suppression d’un champ de lien
* Ajouter ou supprimer un champ de déduplication

Pour ces raisons, il est important de bien réfléchir à la convention de dénomination et au schéma que vous prévoyez d’utiliser.

### Type d’approbation

Utilisez le point de terminaison [Approve Custom Object Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/approveCustomObjectTypeUsingPOST) pour publier une version préliminaire en tant que nouvelle version approuvée.  **apiName** est le seul paramètre requis en tant que paramètre de chemin d’accès.  Un type ne peut pas être approuvé s’il n’est pas à l’état de brouillon et satisfait un ensemble de règles de validation décrites [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

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

### Type d’abandon

Utilisez le point de terminaison [Ignorer le type d’objet personnalisé Brouillon](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/discardCustomObjectTypeUsingPOST) pour supprimer une version préliminaire. `apiName` est le seul paramètre requis en tant que paramètre de chemin d’accès. Un type doit être en état de brouillon pour être ignoré, c’est-à-dire qu’un type approuvé ne peut pas être ignoré.

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

### Type de suppression

Utilisez le point d’entrée [Supprimer le type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) pour supprimer une version approuvée.  `apiName` est le seul paramètre requis en tant que paramètre de chemin d’accès.  Notez qu’il s’agit d’une opération destructrice ; elle ne peut pas être annulée.  Un type ne peut pas être supprimé s’il n’a pas été supprimé de l’utilisation par des ressources, telles que des déclencheurs ou des filtres.  Le point d’entrée Get Custom Object Dependent Assets peut être utilisé pour récupérer une liste des ressources dépendantes pour un type donné.

POST /rest/v1/customObjects/schema/{apiName}/delete.json

```json
{
    "requestId": "14e36#1684efc4227",
    "result": [],
    "success": true
}
```

## Champs d’objet personnalisés

Par défaut, tous les types d’objet personnalisés contiennent les champs standard suivants :

* GUID Marketo - Identifiant unique pour le type d’objet personnalisé
* Créé à : date et heure de création du type d’objet personnalisé
* Mise à jour : At - Date et heure de la dernière mise à jour du type d’objet personnalisé

Vous pouvez ajouter/modifier/supprimer des champs personnalisés à l’aide des points de fin décrits ci-dessous.

* Le nombre maximal de champs autorisés est de 50
* Une fois qu’un objet personnalisé a été approuvé, vous pouvez ajouter 20 champs supplémentaires au maximum à l’objet personnalisé.
* Au moins 1 champ de déduplication est requis ; 3 au maximum sont autorisés
* Les noms d’API de champ et les noms d’affichage peuvent contenir des caractères alphanumériques et un caractère de soulignement &quot;_&quot;.

Pour plus d’informations sur les champs d’objet personnalisés, consultez la documentation du produit [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Ajouter des champs

Le point d’entrée [Ajouter des champs de type d’objet personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/addCustomObjectTypeFieldsUsingPOST) vous permet d’ajouter un ou plusieurs champs à votre objet personnalisé.  Le corps de la requête contient un tableau `input` avec un ou plusieurs éléments.  Chaque élément est un objet JSON avec des attributs qui décrivent un champ. L’attribut `name` requis est le nom de l’API du champ et doit être unique à l’objet personnalisé.   La convention consiste à utiliser des minuscules ou des majuscules pour aider à distinguer les autres chaînes de texte. L’attribut `displayName` requis est le nom lisible par l’utilisateur du champ et doit être unique à l’objet personnalisé. L’attribut `dataType` requis est le type de données du champ.  A  Vous pouvez obtenir la liste des types de données autorisés en appelant le point de terminaison [Get Custom Object Type Field Data Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) .  Les objets personnalisés peuvent contenir des champs avec le type de données &quot;link&quot;.  Les champs de lien sont utilisés pour établir des relations entre des objets personnalisés et d’autres types d’objets dans le système, par exemple prospect, société.  Vous trouverez plus d&#39;informations sur les champs de lien [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). L’attribut facultatif `description` est la description du champ. L’attribut booléen facultatif `isDedupeField` spécifie si le champ est utilisé pour le dédoublonnage lors des opérations de mise à jour d’objet personnalisé.  Le paramètre par défaut est false.  Pour les relations de type &quot;un à plusieurs&quot;, un champ de déduplication est requis. L’attribut d’objet facultatif `relatedTo` spécifie un champ de lien.  Pour les relations de type &quot;un à plusieurs&quot;, cet objet contient un attribut `name` qui est &quot;l’objet de lien&quot; ou l’objet parent auquel créer un lien, ainsi qu’un attribut `field` qui est le &quot;champ de lien&quot;,  ou le champ dans l’objet parent à utiliser comme attribut clé.  Appelez le point d’entrée [Obtenir des objets liés personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) pour récupérer une liste d’objets de lien autorisés.  Pour plus d’informations sur les champs de lien, consultez la documentation du produit [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). Un objet personnalisé ne peut pas être lié à un autre objet personnalisé qui comporte un champ de lien existant.

### Relation Un-À-Plusieurs

Pour une structure d’objet personnalisé de type &quot;un à plusieurs&quot;, utilisez un champ de lien dans un objet personnalisé pour le connecter à un objet standard : prospect ou société. En reprenant l’exemple du propriétaire de la voiture de la documentation du produit Marketo [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure), nous créons un objet personnalisé qui contient des informations relatives à la voiture pour se connecter à Leads.

1. Créer un objet **Car**
1. Ajoutez des champs à l’objet **Car** : dédupliquez sur **VIN**, liez à **Lead**&#x200B;**/Lead ID**
1. Approuver l’objet **Car**

Créez tout d’abord le type d’objet personnalisé pour contenir les informations spécifiques à la voiture.

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

Maintenant, ajoutez des champs au type d’objet personnalisé Voiture . Nous utilisons un champ de lien pour spécifier l’objet et le champ auxquels se connecter. Dans ce cas, l’objet de lien est &quot;Lead&quot; et le champ de lien est &quot;identifiant&quot;. Utilisez un champ de chaîne pour la déduplication (VIN).  Nous ajouterons trois champs supplémentaires pour stocker les attributs de voiture supplémentaires (marque, modèle, année).

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

### Relation multiple-à-multiple

Les relations de type &quot;plusieurs à plusieurs&quot; sont représentées à l’aide d’un objet personnalisé &quot;pont&quot; ou intermédiaire entre un objet personnalisé standard, tel qu’une piste ou une entreprise, et d’un objet personnalisé &quot;périphérie&quot;. L’objet edge est l’entité principale contenant des attributs descriptifs (champs). L’objet bridge contient les données permettant de résoudre les relations de l’objet à l’aide de 2 champs de lien.  Un champ de lien renvoie à l’objet standard parent comme dans un objet  configuration de relation un-à-multiple.  L’autre champ de lien pointe vers l’objet Edge, qui est un objet personnalisé sans lien.  L’objet bridge peut également contenir des attributs descriptifs (champs). À l’aide de l’exemple d’inscription aux cours universitaires de la documentation du produit Marketo [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure), nous créons un objet personnalisé de périphérie qui contient des informations relatives aux cours, ainsi qu’un objet de pont d’inscription utilisé pour connecter les cours aux pistes. Les étapes sont les suivantes :

1. Créez un objet Edge **Course**
1. Ajoutez des champs à la déduplication **Cours :** sur **ID de cours**
1. Approuver **Cours**
1. Créer un objet de pont **Enrollment**
1. Ajoutez des champs à la déduplication **Enrollment:** sur **ID d&#39;inscription**, liez au champ **Parcours**&#x200B;**/ID de cours** et liez-le à **Lead**&#x200B;**/ID de piste**
1. Approuver **Enrollment**

Créez tout d’abord le type d’objet Edge pour contenir les informations spécifiques au cours :

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

Ajoutons ensuite des champs personnalisés au type d’objet Edge.  Dans cet exemple, nous allons ajouter les quatre champs personnalisés suivants pour modéliser un cours universitaire : ID de cours, instructeur, Emplacement du cours, Nom du cours.  Notez que nous désignons l’ID de cours comme champ de déduplication, puisqu’au moins un champ de déduplication est requis.

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

Nous devons maintenant approuver le type d’objet Edge afin de pouvoir le référencer ultérieurement lors de la liaison au type d’objet bridge.  Notez que les types d’objets personnalisés doivent être approuvés pour pouvoir être sélectionnés comme objet de lien.

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

L’objet edge est terminé.  Maintenant, passons à la création du type d’objet de pont pour contenir les informations spécifiques à l’inscription.

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

Pour ajouter des champs personnalisés au type d’objet de pont, ajoutez deux champs de lien : un lien vers l’objet Lead et un autre lien vers l’objet course que nous venons de créer. Pour créer un lien vers l’objet Lead, utilisez le champ Lead Id. Pour créer un lien vers l’objet Cours, utilisez le champ ID du cours .  Ajoutez ensuite un identifiant unique d’identifiant d’inscription comme champ de déduplication, car au moins un champ de déduplication est requis. Enfin, ajoutez un champ Note pour suivre comment l’étudiant s’est comporté.

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

Enfin, approuvez l’objet de pont.

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

Vous pouvez renseigner les enregistrements d’objet personnalisé par programmation à l’aide de l’option [Synchroniser l’objet personnalisé](#create_and_update) ou de l’option [Importation d’objet personnalisé en bloc](https://experienceleague.adobe.com/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import.html?lang=fr). Vous pouvez également utiliser la fonctionnalité d’interface utilisateur de Marketo [Importer des données d’objet personnalisées](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-custom-objects/import-custom-object-data).

## Mettre à jour le champ

Le point d’entrée [Mettre à jour le champ de type d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/updateCustomObjectTypeFieldUsingPOST) vous permet de mettre à jour un champ dans votre brouillon d’objet personnalisé.  Le paramètre de chemin d’accès requis `apiName` est le nom de l’API du type d’objet personnalisé.  Le paramètre de chemin d’accès requis `fieldAPIName` est le nom de l’API du champ de type d’objet personnalisé.  Le corps de la requête contient un objet JSON contenant des paires clé/valeur qui spécifient les attributs de champ à mettre à jour.

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

## Supprimer des champs

Le point d’entrée [Supprimer les champs de type d’objet personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectTypeFieldsUsingPOST) vous permet de supprimer un ou plusieurs champs de votre objet personnalisé.  Le paramètre de chemin d’accès requis `apiName` est le nom de l’API du type d’objet personnalisé.  Le corps de la requête contient un objet JSON avec un tableau `input` avec un ou plusieurs éléments.  Chaque élément est un objet JSON avec un attribut `name` qui spécifie le nom de l’API du champ à supprimer.

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

## Types de données de champ de liste

Le point de terminaison [Get Custom Object Type Field Data Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) renvoie la liste de tous les types de données de champ autorisés. Cela s’avère utile lors de la modélisation de votre type d’objet personnalisé pour identifier les types de données de champ personnalisé pris en charge.

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

## Liste d’objets personnalisés liés

Le point d’entrée [Obtenir des objets interconnectés personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) renvoie une liste de tous les objets de lien autorisés et leurs champs de lien.  La liste contient les objets standard (prospect, entreprise) et les objets personnalisés créés dans l’instance.

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

## Obtention d’une Assets dépendante d’objet personnalisé

Le point de terminaison [Get Custom Object Dependent Assets](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeDependentAssetsUsingGET) renvoie une liste des ressources dépendantes d’un type d’objet personnalisé, y compris leur emplacement dans l’instance.  Cela s’avère utile lors de la suppression d’une intégration et vous devez identifier tous les emplacements où un type d’objet personnalisé est utilisé.

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

## Délais d’expiration

* Les points de terminaison d’objets personnalisés ont un délai d’expiration de 30 s, sauf indication ci-dessous.
   * Synchroniser les objets personnalisés : 120 s 
   * Suppression d’objets personnalisés : 60 s
