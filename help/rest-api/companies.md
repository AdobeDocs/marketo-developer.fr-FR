---
title: Sociétés
feature: REST API
description: Utilisez l’API REST Entreprises Marketo pour décrire, interroger et synchroniser les enregistrements d’entreprise, gérer les champs et effectuer une déduplication par externalCompanyId, et notez que la synchronisation CRM est en lecture seule.
exl-id: 80e514a2-1c86-46a7-82bc-e4db702189b0
TQID: https://experienceleague.adobe.com/LdJYN4lx9JfcE-02zTz8ktfYXm4EdPtxMYOx9gGR0sg
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 572
ht-degree: 1%

---

# Sociétés

[Référence des points d’entrée de sociétés](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies)

Les sociétés représentent les organisations auxquelles appartiennent les enregistrements de prospect. Pour ajouter un prospect à une entreprise, renseignez son champ de `externalCompanyId` à l’aide des points d’entrée [Prospects de synchronisation](https://developer.adobe.com/marketo-apis/api/mapi#operation/syncLeadUsingPOST) ou [Importation de prospects en bloc](bulk-lead-import.md).

Vous ne pouvez pas supprimer un prospect d&#39;une entreprise à moins de l&#39;ajouter à une autre entreprise. Les leads liés à un enregistrement d’entreprise héritent des valeurs de cet enregistrement comme si les valeurs existaient sur l’enregistrement de lead.

Les API d’entreprise fournissent un accès en lecture seule aux abonnements pour lesquels [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) ou [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) est activé.

## Décrire

Décrivez l&#39;objet company permettant de récupérer les informations nécessaires pour interagir avec les enregistrements company.

```http
GET /rest/v1/companies/describe.json
```

```json
{
   "success":true,
   "requestId":"5847#14d44113ad7",
   "result":[
      {
         "name":"Company",
         "description":"Company object",
         "createdAt":"2015-05-11T17:11:32Z",
         "updatedAt":"2015-05-11T17:11:32Z",
         "idField":"id",
         "dedupeFields":[
            "externalCompanyId"
         ],
         "searchableFields":[
            [
               "externalCompanyId"
            ],
            [
               "id"
            ],
            [
               "company"
            ]
         ],
         "fields":[
            {
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"externalCompanyId",
               "displayName":"External Company Id",
               "dataType":"string",
               "length":100,
               "updateable":false
            },
            {
               "name":"id",
               "displayName":"Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"annualRevenue",
               "displayName":"Annual Revenue",
               "dataType":"currency",
               "updateable":true
            }
            {
               "name":"company",
               "displayName":"Company Name",
               "dataType":"string",
               "length":255,
               "updateable":true
            }
         ]
      }
   ]
}
```

## Requête

Le modèle de [sociétés d’interrogation](https://developer.adobe.com/marketo-apis/api/mapi#operation/getCompaniesUsingGET) suit de près l’API Leads. Cependant, le paramètre `filterType` accepte uniquement les champs répertoriés dans le tableau searchableFields de la réponse Describe Companies ou dedupeFields.

Les paramètres de requête sont les suivants :

- `filterType` et `filterValues` : paramètres requis.
- `fields`, `nextPageToken` et `batchSize` : paramètres facultatifs qui fonctionnent comme les paramètres correspondants dans les API Leads et Opportunités.

Lorsque vous demandez une liste de `fields`, un champ demandé qui n’est pas renvoyé a une valeur implicite null.

Si vous omettez le paramètre fields , la réponse renvoie ces champs par défaut :

- identifiant
- dedupeFields
- updatedAt
- createdAt

```http
GET /rest/v1/companies.json?filterType=id&filterValues=3433,5345
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "id":3433,
         "externalCompanyId":"19UYA31581L000000",
         "company":"Google"
      },
      {
         "seq":1,
         "id":5345,
         "externalCompanyId":"29UYA31581L000000",
         "company":"Yahoo"
      }
   ]
}
```

## Créer et mettre à jour

Le point d’entrée [Sync Companies](https://developer.adobe.com/marketo-apis/api/mapi#operation/syncCompaniesUsingPOST) accepte un paramètre `input` obligatoire contenant un tableau d’objets d’entreprise.

Comme pour les opportunités, le point d’entrée prend en charge trois modes de création et de mise à jour : createOnly, updateOnly et createOrUpdate. Spécifiez le mode dans le paramètre `action` de la requête.

Les paramètres `dedupeBy` et `action` sont facultatifs. Ils utilisent par défaut respectivement dedupeFields et createOrUpdate.

```http
POST /rest/v1/companies.json
```

```text
Content-Type: application/json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalCompanyId":"19UYA31581L000000",
         "company":"Google"
      },
      {
         "externalCompanyId":"29UYA31581L000000",
         "company":"Yahoo"
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
         "id":1232
      },
      {
         "seq":1,
         "status":"created",
         "id":1323
      }
   ]
}
```

### Champs

L’objet company contient des champs définis par des attributs tels que le nom d’affichage, le nom de l’API et le dataType. Ensemble, ces attributs sont appelés métadonnées.

Les points d’entrée suivants interrogent les champs sur l’objet société. L’utilisateur de l’API doit disposer d’un rôle avec l’autorisation `Read-Write Schema Standard Field`, l’autorisation `Read-Write Schema Custom Field` ou les deux.

### Champs de requête

Exécutez une requête sur un champ société par nom d’API ou récupérez tous les champs société.

#### Par nom

Le point d’entrée [Obtenir le champ de l’entreprise par nom](https://developer.adobe.com/marketo-apis/api/mapi#operation/getCompanyFieldByNameUsingGET) récupère les métadonnées d’un champ de l’objet d’entreprise. Le paramètre de chemin d’accès `fieldApiName` obligatoire spécifie le nom de l’API du champ.

La réponse ressemble à la réponse Décrire la société, mais elle comprend des métadonnées supplémentaires. Par exemple, l’attribut `isCustom` indique si le champ est personnalisé.

```http
GET /rest/v1/companies/schema/fields/industry.json
```

```json
{
    "requestId": "88f6#17e976d6ab4",
    "result": [
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
        }
    ],
    "success": true
}
```

#### Parcourir

Le point d’entrée [Obtenir les champs d’entreprise](https://developer.adobe.com/marketo-apis/api/mapi#operation/getCompanyFieldsUsingGET) récupère les métadonnées de tous les champs de l’objet d’entreprise. Par défaut, elle renvoie un maximum de 300 enregistrements. Utilisez le paramètre de requête `batchSize` pour réduire ce nombre.

Si l’attribut `moreResult` est défini sur « true », d’autres résultats sont disponibles. Continuez à appeler le point d’entrée avec la `nextPageToken` renvoyée jusqu’à ce que `moreResult` soit false.

```http
GET /rest/v1/companies/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "b50e#17e995c2d35",
    "result": [
        {
            "displayName": "Company Name",
            "name": "company",
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
            "displayName": "Site",
            "name": "site",
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
            "displayName": "Website",
            "name": "website",
            "description": null,
            "dataType": "url",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Main Phone",
            "name": "mainPhone",
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
            "displayName": "Annual Revenue",
            "name": "annualRevenue",
            "description": null,
            "dataType": "currency",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "L7XD3EFJ3OLFZKXKJBWYULOTRA======",
    "moreResult": true
}
```

### Supprimer

Spécifiez les critères de suppression sous la forme d’une liste de valeurs de recherche dans le tableau `input`. Spécifiez la méthode de suppression dans le paramètre `deleteBy` .

Les valeurs autorisées sont dedupeFields et idField. La valeur par défaut est dedupeFields.

```text
Content-Type: application/json
```

```http
POST /rest/v1/companies/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalCompanyId":"19UYA31581L000000"
      },
      {
         "externalCompanyId":"29UYA31581L000000"
      },
      {
         "externalCompanyId":"39UYA31581L000000"
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
         "id":1234,
         "status":"deleted"
      },
      {
         "seq":1,
         "id":56456,
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

- Sauf indication contraire, les points d’entrée d’entreprise ont un délai d’expiration de 30 s.
- Le délai d’expiration de Sync Companies est de 60 ans.
- Le délai d’expiration de la suppression des entreprises est de 60.
