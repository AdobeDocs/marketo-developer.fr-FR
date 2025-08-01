---
title: Rôles d’opportunité
feature: REST API
description: Gestion des rôles d’opportunité dans Marketo.
exl-id: 2ba84f4d-82d0-4368-94e8-1fc6d17b69ed
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Rôles d’opportunité

[Référence du point d’entrée des rôles d’opportunité](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityRolesUsingGET)

Les leads sont liés aux opportunités via l’objet de `opportunityRole` intermédiaire.

Les API de rôle d’opportunité ne sont exposées que pour les abonnements pour lesquels la synchronisation CRM native n’est pas activée.

## Décrire

Tout comme les opportunités, une description des opérations d’appel et de CRUD est exposée pour les rôles d’opportunité.

```
GET /rest/v1/opportunities/roles/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"opportunityRole",
         "displayName":"Opportunity Role",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "externalOpportunityId",
            "leadId",
            "role"
         ],
         "searchableFields":[
            [
               "externalOpportunityId",
               "leadId",
               "role"
            ],
            [
               "marketoGUID"
            ],
            [
               "leadId"
            ],
            [
               "externalOpportunityId"
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
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"leadId",
               "displayName":"Lead Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"role",
               "displayName":"Role",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"isPrimary",
               "displayName":"Is Primary",
               "dataType":"boolean",
               "updateable":true
            },
            {
               "name":"externalCreatedDate",
               "displayName":"External Created Date",
               "dataType":"datetime",
               "updateable":true
            }
         ]
      }
   ]
}
```

## Requête

Notez que les `dedupeFields` et les `searchableFields` sont un peu différents des opportunités. `dedupeFields` fournit en fait une clé composite, où les trois éléments `externalOpportunityId`, `leadId` et `role` sont requis. Pour que la création d’enregistrements réussisse, les champs d’ID doivent exister dans l’instance de destination pour que l’opportunité et le lien de prospect existent. Par `searchableFields`, `marketoGUID`, `leadId` et `externalOpportunityId` sont tous valides pour les requêtes seules et utilisent un modèle identique à Opportunités, mais il existe une option supplémentaire consistant à utiliser la clé composée pour la requête, ce qui nécessite l’envoi d’un objet JSON via POST, avec le paramètre de requête supplémentaire `_method=GET`.

```
POST /rest/v1/opportunities/roles.json?_method=GET
```

```json
{
   "filterType": "dedupeFields",
   "fields": [
      "marketoGuid",
      "externalOpportunityId",
      "leadId",
      "role"
   ],
   "input": [
      {
        "externalOpportunityId": "Opportunity1",
        "leadId": 1,
        "role": "Captain"
      },
      {
        "externalOpportunityId": "Opportunity2",
        "leadId": 1872,
        "role": "Commander"
      },
      {
        "externalOpportunityId": "Opportunity3",
        "leadId": 273891,
        "role": "Lieutenant Commander"
      }
   ]
}
```

Cela produit le même type de réponse qu’une requête GET standard, mais avec une interface différente pour effectuer la requête.

## Créer et mettre à jour

Les rôles d’opportunité ont la même interface pour créer et mettre à jour des enregistrements que les opportunités.

```
POST /rest/v1/opportunities/roles.json
```

```json
{
   "action": "createOrUpdate",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "externalOpportunityId": "19UYA31581L000000",
         "leadId": 456783,
         "role": "Technical Buyer",
         "isPrimary": false
      },
      {
         "externalOpportunityId": "19UYA31581L000000",
         "leadId": 456784,
         "role": "Technical Buyer",
         "isPrimary": false
      }
   ]
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result":[
      {
         "seq": 0,
         "status": "updated",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq": 1,
         "status": "created",
         "marketoGUID": "cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

## Supprimer

Vous pouvez supprimer des rôles d’opportunité en dédupliquant des champs ou des champs d’ID. Spécifiez à l’aide du paramètre deleteBy avec une valeur de dedupeFields ou idField. S’il n’est pas spécifié, la valeur par défaut est dedupeFields. Le corps de la requête contient un tableau d’entrée des rôles d’opportunité à supprimer. Un maximum de 300 rôles d’opportunité par appel est autorisé.

```
POST /rest/v1/opportunities/roles/delete.json
```

```json
{
   "deleteBy": "dedupeFields",
   "input": [
      {
        "externalOpportunityId": "19UYA31581L000000",
        "leadId": 456783,
        "role": "Technical Buyer"
      }
   ]
}
```

```json
{
    "requestId": "10f7c#173264db42d",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
            "status": "deleted"
        }
    ]
    "success": true
}
```

## Délais dépassés

- Le délai d’expiration des points d’entrée du rôle d’opportunité est de 30, sauf indication ci-dessous
   - Rôles de l’opportunité de synchronisation : 60 s 
   - Supprimer rôles d’opportunité : 60 s
