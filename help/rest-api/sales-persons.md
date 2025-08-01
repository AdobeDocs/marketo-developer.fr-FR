---
title: Vendeurs
feature: REST API
description: Lire les données sur les vendeurs.
exl-id: f8ed5aa5-63c1-4c5b-8683-bf47eed1ea18
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# Vendeurs

[Référence du point d’entrée du commercial](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)

Les API Sales Person sont en lecture seule pour les abonnements pour lesquels la [Synchronisation de SFDC](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) ou la [Synchronisation de Microsoft Dynamics](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) est activée. Les vendeurs sont un type d&#39;enregistrement de personne qui sont les propriétaires de ventes des enregistrements de prospect. Ils sont associés aux enregistrements de lead par le champ externalSalesPersonId sur chaque enregistrement de lead. Lorsqu’un prospect est associé à un commercial par un champ externalSalesPersonId renseigné, les champs de recherche de Propriétaire de prospect correspondants sont renseignés pour cet enregistrement de prospect dans Marketo, ce qui permet d’utiliser les filtres et jetons correspondants.

Les commerciaux sont associés aux enregistrements de lead à l’aide du point d’entrée [Synchroniser les leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) et en transmettant l’attribut externalSalesPersonId.

Les commerciaux sont associés aux enregistrements d’opportunité à l’aide du point d’entrée [Synchroniser les opportunités](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/syncOpportunitiesUsingPOST) et en transmettant l’attribut externalSalesPersonId.

Les commerciaux sont associés aux enregistrements d’entreprise à l’aide du point d’entrée [Synchroniser les entreprises](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) et en transmettant l’attribut externalSalesPersonId.

Les enregistrements de commercial ne peuvent être modifiés que par le biais de l’API.

## Décrire

La description des enregistrements de commercial suit le modèle standard pour les objets de base de données de prospect.

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

Par défaut, le `idField` des commerciaux est « id » et le `dedupeFields` est simplement « externalSalesPersonId ».

## Requête

Vendeurs utilisant le modèle de requête standard pour les clés simples. Cet exemple montre comment l’adresse e-mail de l’utilisateur est utilisée comme externalSalesPersonId. Par défaut, la requête renvoie tous les champs qui sont renseignés pour les enregistrements renvoyés.

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

Le modèle des mises à jour est standard.

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

Le modèle des suppressions est standard.

La suppression de vendeurs n&#39;est pas autorisée lorsqu&#39;ils sont « utilisés ». Dans ce cas, le vendeur est ignoré. Exemples :

- Lorsque le vendeur est associé à des leads actifs
- Lorsque le vendeur est associé à une entreprise qui a été supprimée

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

## Délais dépassés

- Le délai d’expiration des points d’entrée commerciaux est de 30 s, sauf indication ci-dessous
   - Synchroniser les commerciaux : 60s
   - Supprimer vendeurs : 60s
