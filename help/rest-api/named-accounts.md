---
title: Comptes désignés
feature: REST API
description: Guide REST Marketo sur le CRUD sur les comptes nommés pour ABM, avec description, requête, création d’exemples de mise à jour, champs consultables, règles de déduplication et aucun lien de prospect.
exl-id: 2aa1d2a0-9e54-4a9a-abb1-0d0479ed3558
TQID: https://experienceleague.adobe.com/iY3UYVelm3aKuuDBCTxaVCbkXfwnJzDjV3Kvn9rcNbA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 590
ht-degree: 1%

---

# Comptes désignés

[Référence des points d’entrée des comptes nommés](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Accounts)

Marketo fournit des API pour effectuer des opérations CRUD sur des comptes nommés à utiliser avec Marketo ABM. Ces API suivent le modèle standard de l’interface de la base de données de leads et fournissent les options Décrire, Créer/Mettre à jour, Supprimer et Requête .

Actuellement, les API Marketo ne prennent en charge que les opérations CRUD pour les comptes nommés. Vous ne pouvez pas lier des prospects à des comptes nommés par le biais des API.

## Décrire

Décrire les comptes nommés renvoie des métadonnées pour l’utilisation de comptes nommés via les API Marketo. La réponse inclut des champs pouvant faire l’objet d’une recherche valides et tous les champs disponibles pour l’API.

La `idField` d’un compte nommé est toujours `marketoGUID`. Le champ `name` de l’objet est la seule `dedupeField` et clé de création disponible.

```http
GET /rest/v1/namedaccounts/describe.json
```

```json
{
   "requestId":"d65e#156c27ac57d",
   "result":[
      {
         "name":"Named Account",
         "description":"Marketo standard account attribute map",
         "createdAt":"2016-08-18T20:16:41Z",
         "updatedAt":"2016-08-18T20:16:41Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "name"
         ],
         "searchableFields":[
            [
               "marketoGUID",
            ],
            [
               "annualRevenue"
            ],
            [
               "city"
            ],
            [
               "country"
            ],
            [
               "domainName"
            ],
            [
               "industry"
            ],
            [
               "logoUrl"
            ],
            [
               "membershipCount"
            ],
            [
               "name"
            ],
            [
               "numberOfEmployees"
            ],
            [
               "opptyAmount"
            ],
            [
               "opptyCount"
            ],
            [
               "score1"
            ],
            [
               "score2"
            ],
            [
               "score3"
            ],
            [
               "score4"
            ],
            [
               "score5"
            ],
            [
               "sicCode"
            ],
            [
               "state"
            ]
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
               "name":"annualRevenue",
               "displayName":"annualRevenue",
               "dataType":"currency",
               "updateable":true
            },
            {
               "name":"city",
               "displayName":"city",
               "dataType":"string",
               "length":255,
               "updateable":true
            },
            {
               "name":"country",
               "displayName":"country",
               "dataType":"string",
               "length":255,
               "updateable":true
            }
         ]
      }
   ],
   "success":true
}
```

### Requête

Interroger des comptes nommés à l’aide d’un filterType et de valeurs filterValues séparées par des virgules jusqu’à 300. FilterType peut être n’importe quel champ unique renvoyé dans le membre `searchableFields` de la réponse Describe. Chaque entrée filterValues doit être une valeur valide pour le type de données du champ.

Pour renvoyer des champs spécifiques, transmettez un paramètre fields avec une liste de champs séparés par des virgules. Une page de requête contient un maximum de 300 enregistrements. Pour récupérer des enregistrements supplémentaires, utilisez le nextPageToken renvoyé par l’appel .

```http
GET /rest/v1/namedaccounts.json?filterType=name&filterValues=Google,Yahoo
```

```json
{
    "requestId": "6dac#157d4ddc9d7",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "16efafdd-0148-4ea7-8782-f451d7c6345d",
            "createdAt": "2016-10-17T22:49:04Z",
            "name": "Google",
            "updatedAt": "2016-10-17T22:49:04Z"
        },
        {
            "seq": 1,
            "marketoGUID": "44d62353-7f9d-4d43-b9cc-7ef0f7a09137",
            "createdAt": "2016-10-17T22:49:04Z",
            "name": "Yahoo",
            "updatedAt": "2016-10-17T22:49:04Z"
        }
    ],
    "success": true
}
```

### Créer et mettre à jour

Créez et mettez à jour des comptes nommés à l’aide du modèle de base de données de leads standard. Transmettez des enregistrements dans le membre d’entrée du corps JSON d’une requête POST. Vous pouvez inclure jusqu’à 300 enregistrements.

Les membres de la requête sont les suivants :

- `input` : le seul membre requis.
- `action` : membre facultatif qui accepte createOnly, updateOnly ou createOrUpdate. La valeur par défaut est createOrUpdate.
- `dedupeBy` : membre facultatif disponible uniquement lorsque l’action est updateOnly. Elle accepte les champs dedupeFields ou idField , qui correspondent respectivement au nom et au marketoGUID .

```http
POST /rest/v1/namedaccounts.json
```

```text
Content-Type: application/json
```

```json
{
   "action":"updateOnly",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "name":"Google",
         "domainName":"www.google.com"
      },
      {
         "name":"Yahoo",
         "domainName":"www.yahoo.com"
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
         "status":"updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status":"created",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc"
      }
   ]
}
```

### Champs

L’objet de compte nommé contient des champs définis par des attributs tels que le nom d’affichage, le nom de l’API et le dataType. Ensemble, ces attributs sont appelés métadonnées.

Les points d’entrée suivants interrogent les champs sur l’objet société. L’utilisateur de l’API doit disposer d’un rôle avec l’autorisation Champ standard du schéma en lecture-écriture, l’autorisation Champ personnalisé du schéma en lecture-écriture, ou les deux.

### Champs de requête

Exécutez une requête dans un champ de compte nommé par nom d’API ou récupérez tous les champs d’entreprise.

#### Par nom

Le point d’entrée [Obtenir le champ de compte nommé par nom](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) récupère les métadonnées d’un champ sur l’objet de compte nommé. Le paramètre de chemin d’accès fieldApiName obligatoire spécifie le nom de l’API du champ.

La réponse ressemble à la réponse Décrire le compte nommé , mais elle comprend des métadonnées supplémentaires. Par exemple, l’attribut isCustom indique si le champ est personnalisé.

```http
GET /rest/v1/namedaccounts/schema/fields/annualRevenue.json
```

```json
{
    "requestId": "371c#17e979c5d1f",
    "result": [
        {
            "displayName": "Annual Revenue",
            "name": "annualRevenue",
            "description": null,
            "dataType": "currency",
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

Le point d’entrée [Obtenir les champs de compte nommés](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) récupère les métadonnées de tous les champs de l’objet de compte nommé. Par défaut, elle renvoie un maximum de 300 enregistrements. Utilisez le paramètre de requête batchSize pour réduire ce nombre.

Si l’attribut moreResult est défini sur true, d’autres résultats sont disponibles. Continuez à appeler le point d’entrée avec le nextPageToken renvoyé jusqu’à ce que moreResult ait la valeur false.

```http
GET /rest/v1/namedaccounts/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "f287#17e995bd0c5",
    "result": [
        {
            "displayName": "Name",
            "name": "name",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Domain Name",
            "name": "domainName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Industry",
            "name": "industry",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "SIC Code",
            "name": "sicCode",
            "description": null,
            "dataType": "string",
            "length": 40,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "City",
            "name": "city",
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
    "nextPageToken": "N42LHXWEULHZ3N2I77DKOJUVOY======",
    "moreResult": true
}
```

### Supprimer

Supprimez les comptes nommés en envoyant une requête POST avec un corps JSON. La requête comprend un membre d’entrée obligatoire et un membre deleteBy facultatif.

Le membre deleteBy accepte « dedupeFields » ou « idField », qui correspondent respectivement au nom et au marketoGUID. Si cette option n’est pas définie, les champs dédupliqués sont utilisés par défaut. Le membre d’entrée accepte jusqu’à 300 enregistrements. Chaque enregistrement contient soit name, soit marketoGUID, selon le paramètre deleteBy.

```http
POST /rest/v1/namedaccounts/delete.json
```

```text
Content-Type: application/json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "name":"Google"
      },
      {
         "name":"Yahoo"
      },
      {
         "name":"Marketo"
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      },
      {
         "seq":1,
         "id":"dff23271-f996-47d7-984f-f2676861b5fc",
         "status":"deleted"
      },
      {
         "seq":2,
         "status":"skipped",
         "reasons":[
            {
               "code":"1013",
               "message":"Record not found"
            }
         ]
      }
   ]
}
```

## Délais dépassés

- Le délai d’expiration des points d’entrée de comptes nommés est de 30, sauf indication contraire.
- Le délai d’expiration de la synchronisation des comptes nommés est de 120.
- Le délai d’expiration de la suppression des comptes nommés est de 60 s.
