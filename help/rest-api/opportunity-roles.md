---
title: Rôles d’opportunité
feature: REST API
description: Gérez les rôles d’opportunité Marketo via l’API REST, notamment la description, les requêtes avec des champs de déduplication composés, la création d’une suppression de mise à jour, les dépassements de délai et aucune synchronisation CRM.
exl-id: 2ba84f4d-82d0-4368-94e8-1fc6d17b69ed
TQID: https://experienceleague.adobe.com/aE27mBhsrn-0SO41M-pV5NFjoMq--1Lp-L2TQGL7-8Y
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 252
ht-degree: 0%

---

# Rôles d’opportunité

[Référence du point d’entrée des rôles d’opportunité](https://developer.adobe.com/marketo-apis/api/mapi#operation/getOpportunityRolesUsingGET)

Les liens d’objet `opportunityRole` intermédiaires mènent à des opportunités.

Les API de rôle d’opportunité sont disponibles uniquement pour les abonnements pour lesquels la synchronisation CRM native n’est pas activée.

## Décrire

Comme pour les opportunités, l’API fournit un appel de description et des opérations CRUD pour les rôles d’opportunité.

```http
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

Les valeurs `dedupeFields` et `searchableFields` diffèrent des opportunités. `dedupeFields` fournit une clé composée qui nécessite `externalOpportunityId`, `leadId` et `role`. Pour que la création d’enregistrements réussisse, l’opportunité et le prospect référencés par les champs d’identifiant doivent exister dans l’instance de destination.

Les valeurs `searchableFields` `marketoGUID`, `leadId` et `externalOpportunityId` sont valides pour les requêtes individuelles qui utilisent le même modèle que les opportunités. Vous pouvez également effectuer une requête à l’aide de la clé composée . Cette requête nécessite un objet JSON envoyé via POST avec le paramètre de requête `_method=GET`.

```http
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

Cette requête produit le même type de réponse qu’une requête GET standard, mais utilise une interface de requête différente.

## Créer et mettre à jour

Créez et mettez à jour les rôles d’opportunité à l’aide de la même interface que les opportunités.

```http
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

Supprimer les rôles d’opportunité par champs de déduplication ou champ d’ID. Définissez le paramètre deleteBy sur dedupeFields ou idField. La valeur par défaut est dedupeFields.

Le corps de la requête contient un tableau d’entrée des rôles d’opportunité à supprimer. Chaque appel autorise un maximum de 300 rôles d’opportunité.

```http
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

- Le délai d’expiration des points d’entrée du rôle d’opportunité est de 30, sauf indication contraire.
- Le délai d’expiration des rôles d’opportunité de synchronisation est de 60.
- Le délai d’expiration de la suppression des rôles d’opportunité est de 60.
