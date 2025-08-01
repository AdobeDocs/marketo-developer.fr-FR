---
title: Comptes désignés
feature: REST API
description: Manipuler des comptes nommés via l’API.
exl-id: 2aa1d2a0-9e54-4a9a-abb1-0d0479ed3558
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '679'
ht-degree: 1%

---

# Comptes désignés

[Référence des points d’entrée des comptes nommés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts)

Marketo propose un ensemble d’API pour effectuer des opérations CRUD sur des comptes nommés à utiliser avec Marketo ABM. Ces API suivent le modèle d’interface standard des API de base de données de prospect, en fournissant les options Décrire, Créer/Mettre à jour, Supprimer et Requête .

Actuellement, les seules fonctions liées à ABM disponibles via les API Marketo sont les opérations CRUD pour les comptes nommés ; les leads ne peuvent pas être liés aux comptes nommés via les API.

## Décrire

La description des comptes nommés renvoie des métadonnées relatives à l’utilisation des comptes nommés par le biais des API Marketo, y compris une liste des champs pouvant faire l’objet d’une recherche valide lors de l’interrogation et une liste de tous les champs disponibles pour l’utilisation de l’API. L’`idField` d’un compte nommé est toujours `marketoGUID`. Le seul `dedupeField` disponible et la clé de création est le champ `name` de l’objet .

```
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

La requête de comptes nommés est basée sur l’utilisation d’un filterType et d’un ensemble de valeurs de filtre pouvant aller jusqu’à 300 séparées par des virgules. `filterType` peut s’agir de n’importe quel champ unique renvoyé dans le membre `searchableFields` du résultat de description pour les comptes nommés, tandis que filterValues peut être n’importe quelle entrée valide pour le type de données du champ. Pour renvoyer un ensemble spécifique de champs depuis , un paramètre fields doit être transmis, où la valeur est une liste de champs séparés par des virgules à renvoyer dans la réponse. Comme les autres options de requête, le nombre maximal d’enregistrements pour une page de requête unique est de 300. Les enregistrements supplémentaires dans le jeu doivent être demandés avec l’utilisation du nextPageToken renvoyé par l’appel .

```
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

La création et la mise à jour de comptes nommés suivent le modèle standard de la base de données des prospects. Les enregistrements doivent être transmis dans le membre d’entrée d’un corps JSON dans une requête POST. `input` est le seul membre obligatoire, avec `action` et `dedupeBy` comme membres facultatifs. Jusqu’à 300 enregistrements peuvent être inclus dans l’entrée. L’action peut être createOnly, updateOnly ou createOrUpdate. Si elle n’est pas spécifiée, l’action est createOrUpdate par défaut. dedupeBy ne peut être spécifié que lorsque l’action est updateOnly et n’accepte que les champs dedupeFields ou idField , qui correspondent respectivement au champ name et au champ marketoGUID.

```
POST /rest/v1/namedaccounts.json
```

```
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

L’objet de compte nommé contient un ensemble de champs . Chaque définition de champ est composée d’un ensemble d’attributs qui décrivent le champ. Les exemples d’attributs sont le nom d’affichage, le nom de l’API et dataType. Ces attributs sont collectivement appelés métadonnées.

Les points d’entrée suivants vous permettent d’interroger des champs sur l’objet société. Ces API nécessitent que l&#39;utilisateur propriétaire de l&#39;API dispose d&#39;un rôle avec l&#39;une ou l&#39;autre des autorisations Read-Write Schema Standard Field ou Read-Write Schema Custom Field , ou les deux.

### Champs de requête

L’interrogation de champs de compte nommé est simple. Vous pouvez interroger un seul champ de compte nommé par nom d’API ou interroger l’ensemble de tous les champs d’entreprise.

#### Par nom

Le point d’entrée [Obtenir le champ de compte nommé par nom](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) récupère les métadonnées d’un seul champ sur l’objet de compte nommé. Le paramètre de chemin d’accès fieldApiName obligatoire spécifie le nom d’API du champ. La réponse est similaire au point d’entrée Décrire le compte nommé , mais elle contient des métadonnées supplémentaires telles que l’attribut isCustom qui indique si le champ est un champ personnalisé.

```
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

Le point d’entrée [Obtenir les champs de compte nommés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) récupère les métadonnées de tous les champs de l’objet de compte nommé. Par défaut, un maximum de 300 enregistrements est renvoyé. Vous pouvez utiliser le paramètre de requête batchSize pour réduire ce nombre. Si l’attribut moreResult est défini sur true, cela signifie que davantage de résultats sont disponibles. Continuez à appeler ce point d’entrée jusqu’à ce que l’attribut moreResult renvoie false, ce qui signifie qu’aucun résultat n’est disponible. Le nextPageToken renvoyé par cette API doit toujours être réutilisé pour l’itération suivante de cet appel.

```
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

Les suppressions sont effectuées via une requête JSON POST et possèdent un membre d’entrée obligatoire et un membre deleteBy facultatif. deleteBy peut être l’un des champs « dedupeFields » ou « idField », correspondant respectivement à name ou marketoGUID, et sera défini par défaut sur dedupeFields s’il n’est pas défini. Le membre d’entrée accepte un tableau de 300 enregistrements maximum, contenant un membre chacun, soit name, soit marketoGUID selon le paramètre de deleteBy.

```
POST /rest/v1/namedaccounts/delete.json
```

```
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

- Les points d’entrée de compte nommés ont un délai d’expiration de 30 s, sauf indication ci-dessous
   - Synchroniser les comptes nommés : 120s 
   - Supprimer comptes nommés : 60s
