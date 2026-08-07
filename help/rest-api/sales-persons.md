---
title: Vendeurs
feature: REST API
description: Guide de l’API REST Marketo pour les enregistrements de commercial avec synchronisation SFDC ou Dynamics, à l’aide de externalSalesPersonId pour mettre en relation les prospects et effectuer des requêtes, des upserts et des suppressions.
exl-id: f8ed5aa5-63c1-4c5b-8683-bf47eed1ea18
TQID: https://experienceleague.adobe.com/JwLNgM0zgztyoYJotCiSdGxMixnzA0kvkFbvq8kEkzE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 363
ht-degree: 0%

---

# Vendeurs

[Référence du point d’entrée du commercial](https://developer.adobe.com/marketo-apis/api/mapi#tag/Sales-Persons)

Les API Sales Person fournissent un accès en lecture seule aux abonnements pour lesquels [SFDC Sync](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) ou [Microsoft Dynamics Sync](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) est activé.

Les vendeurs sont des enregistrements de personne qui représentent les vendeurs des enregistrements de prospect. Le champ externalSalesPersonId de chaque enregistrement Lead associe un Lead à un Commercial. Lorsque ce champ est renseigné, Marketo renseigne les champs de recherche de Propriétaire de lead correspondants dans l’enregistrement de lead. Vous pouvez ensuite utiliser les filtres et jetons associés.

Associez les commerciaux à d&#39;autres enregistrements en transmettant l&#39;attribut externalSalesPersonId au point d&#39;entrée correspondant :

- Enregistrements de leads : [Synchroniser les leads](https://developer.adobe.com/marketo-apis/api/mapi#operation/syncLeadUsingPOST).
- Enregistrements des opportunités : [Synchroniser les opportunités](https://developer.adobe.com/marketo-apis/api/mapi#operation/syncOpportunitiesUsingPOST).
- Enregistrements d’entreprise : [Synchroniser les entreprises](https://developer.adobe.com/marketo-apis/api/mapi#operation/syncCompaniesUsingPOST).

Les enregistrements de commercial ne peuvent être modifiés que par le biais de l’API.

## Décrire

Décrivez les enregistrements de commercial à l&#39;aide du modèle standard pour les objets de base de données de leads.

```http
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

Par défaut, l’`idField` du commercial est « id » et l’`dedupeFields` est « externalSalesPersonId ».

## Requête

Interrogez les vendeurs à l&#39;aide du modèle de requête standard pour les clés simples. L’exemple suivant utilise l’adresse e-mail de l’utilisateur comme externalSalesPersonId.

Par défaut, la requête renvoie tous les champs renseignés pour les enregistrements correspondants.

```http
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

Créez ou mettez à jour des vendeurs en utilisant le modèle de mise à jour standard.

```http
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

Supprimez les vendeurs à l&#39;aide du modèle de suppression standard.

Vous ne pouvez pas supprimer un vendeur « en cours d&#39;utilisation ». La demande ignore le vendeur dans les cas suivants :

- Le vendeur est associé aux prospects actifs.
- Le vendeur est associé à une entreprise qui a été supprimée.

```http
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

- Le délai d’expiration des points d’entrée commerciaux est de 30 s, sauf indication contraire.
- Le délai d’expiration de Sync Sales Persons est de 60 ans.
- Le délai d&#39;expiration de la fonction Supprimer les vendeurs est de 60 s.
