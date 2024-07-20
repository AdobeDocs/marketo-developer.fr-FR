---
title: Personnes de vente
feature: REST API
description: Lire les données sur les vendeurs.
exl-id: f8ed5aa5-63c1-4c5b-8683-bf47eed1ea18
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# Personnes de vente

[Référence du point de terminaison de la personne commerciale](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)

Les API de personne commerciale sont un accès en lecture seule pour les abonnements qui ont la [synchronisation SFDC](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) ou la [synchronisation Microsoft Dynamics](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) activée. Les vendeurs sont un type d’enregistrement de personne qui est le propriétaire des registres de prospect. Elles sont liées aux enregistrements de piste par le champ externalSalesPersonId sur chaque enregistrement de piste. Lorsqu’une piste est associée à une personne commerciale par un champ externalSalesPersonId renseigné, les champs de recherche correspondants du propriétaire de piste sont renseignés pour cet enregistrement de piste dans Marketo, ce qui permet d’utiliser les filtres et jetons correspondants.

Les personnes chargées des ventes sont liées aux enregistrements de piste à l’aide du point d’entrée [Sync Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) et en transmettant l’attribut externalSalesPersonId.

Les vendeurs sont associés aux enregistrements d’opportunité en utilisant le point d’entrée [Synchroniser les opportunités](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/syncOpportunitiesUsingPOST) et en transmettant l’attribut externalSalesPersonId.

Les personnes chargées des ventes sont liées aux enregistrements de société à l’aide du point d’entrée [Synchroniser les entreprises](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) et en transmettant l’attribut externalSalesPersonId .

Les enregistrements de personne commerciale ne sont modifiables que via l’API.

## Description

La description des enregistrements de personne commerciale suit le modèle standard des objets de base de données de piste.

```
GET /rest/v1/salespersons/describe.json
```

```json
{  
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[  
      {  
         "name":"SalesPerson",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"id",
         "dedupeFields":[  
            "externalSalesPersonId"
         ],
         "searchableFields":[  
            [  
               "email"
            ],
            [  
               "id"
            ],
            [
               "externalSalesPersonId"
            ]
         ],
         "fields":[  
            {  
               "name":"id",
               "displayName":"Marketo Id",
               "dataType":"integer",
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
               "name":"email",
               "displayName":"Email",
               "dataType":"string",
               "length":255,
               "updateable":false
            },
            {  
               "name":"externalSalesPersonId",
               "displayName":"External Sales Person Id",
               "dataType":"string",
               "length":255,
               "updateable":false
            }
         ]
      }
   ]
}
```

Par défaut, le `idField` des vendeurs est &quot;id&quot; et `dedupeFields` est simplement &quot;externalSalesPersonId&quot;.

## Requête

Ventes de personnes utilisant le modèle de requête standard pour les clés simples. Cet exemple illustre l’adresse électronique de l’utilisateur utilisée comme externalSalesPersonId. Par défaut, la requête renvoie tous les champs renseignés pour les enregistrements renvoyés.

```
GET /rest/v1/salespersons.json?filterType=dedupeFields&filterValues=david@test.com,sam@test.com
```

```json
 {  
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[  
      {  
         "seq":0,
         "id":53453,
         "externalSalesPersonId":"sam@test.com",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:23Z"
      },
      {  
         "seq":1,
         "id":53454,
         "externalSalesPersonId":"david@test.com",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:23Z"
      }
   ]
}
```

## Créer et mettre à jour

Le modèle de mises à jour est standard.

```
POST /rest/v1/salespersons.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalSalesPersonId":"sam@test.com",
         "email":"sam@test.com",
         "firstName":"Sam",
         "lastName":"Sanosin"
      },
      {
         "externalSalesPersonId":"david@test.com",
         "email":"david@test.com",
         "firstName":"David",
         "lastName":"Aulassak"
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
         "id":45232
      },
      {
         "seq":1,
         "status": "created",
         "id":45236
      }
   ]
}
```

## Supprimer

Le modèle pour les suppressions est standard.

La suppression des personnes commerciales n’est pas autorisée lorsqu’elles sont &quot;en service&quot;. Dans ce cas, la Personne commerciale est ignorée. Exemples :

- Lorsque la Personne commerciale est associée à des pistes actives
- Lorsque la personne chargée des ventes est associée à une société qui a été supprimée

```
POST /rest/v1/salespersons/delete.json
```

```json
{  
   "deleteBy":"dedupeFields",
   "input":[  
      {  
         "externalSalesPersonId":"sam@test.com"
      },
      {  
         "externalSalesPersonId":"david@test.com"
      },
      {  
         "externalSalesPersonId":"raj@test.com"
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
         "id":56343,
         "status": "deleted"
      },
      {
         "seq":1,
         "id":53453,
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
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

## Délais d’expiration

- Les points de fin de personne commerciale ont un délai d’expiration de 30 s, sauf indication ci-dessous.
   - Personnes des ventes de synchronisation : 60 s
   - Supprimer les personnes commerciales : 60 s
