---
title: Sociétés
feature: REST API
description: Utilisez l’API REST Entreprises Marketo pour décrire, interroger et synchroniser les enregistrements d’entreprise, gérer les champs et effectuer une déduplication par externalCompanyId, et notez que la synchronisation CRM est en lecture seule.
exl-id: 80e514a2-1c86-46a7-82bc-e4db702189b0
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 1%

---

# Sociétés

[Référence des points d’entrée d’entreprises](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

Les sociétés représentent l&#39;organisation à laquelle appartiennent les enregistrements de leads. Les leads sont ajoutés à une entreprise en renseignant leur champ de `externalCompanyId` correspondant à l’aide des points d’entrée [Synchroniser les leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) ou [Importation de leads en bloc](bulk-lead-import.md). Une fois qu’un prospect a été ajouté à une entreprise, vous ne pouvez pas le supprimer de cette entreprise (à moins d’ajouter le prospect à une autre entreprise). Les leads liés à un enregistrement d’entreprise héritent directement des valeurs d’un enregistrement d’entreprise comme si les valeurs existaient sur le propre enregistrement du lead.

Les API d’entreprise sont en lecture seule pour les abonnements pour lesquels la [synchronisation de SFDC](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) ou la [synchronisation de Microsoft Dynamics](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) est activée.

## Décrire

La description de l&#39;objet société vous donne toutes les informations nécessaires pour interagir avec lui.

```
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

Le modèle pour [interroger des sociétés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompaniesUsingGET) suit de près celui de l’API des prospects avec la restriction ajoutée que le paramètre `filterType` accepte les champs répertoriés dans le tableau searchableFields de l’appel Describe Companies ou dedupeFields.

`filterType` et `filterValues` sont des paramètres de requête obligatoires.  `fields`, `nextPageToken` et `batchSize` sont des paramètres facultatifs.  Les paramètres fonctionnent comme les paramètres correspondants dans les API Leads et Opportunités. Lors de la demande d’une liste de `fields`, si un champ particulier est demandé, mais n’est pas renvoyé, la valeur est implicitement nulle.

Si le paramètre fields est omis, l’ensemble par défaut de champs renvoyés est :

- identifiant
- dedupeFields
- updatedAt
- createdAt

```
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

Le point d’entrée [Sync Companies](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) accepte le paramètre `input` obligatoire contenant un tableau d’objets d’entreprise. Tout comme les opportunités, il existe trois modes de création et de mise à jour d’entreprises : createOnly, updateOnly et createOrUpdate.  Les modes sont spécifiés dans le paramètre `action` de la requête. Les paramètres `dedupeBy` et `action` sont facultatifs et utilisent par défaut les modes dedupeFields et createOrUpdate respectivement.

```
POST /rest/v1/companies.json
```

```
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

L’objet company contient un ensemble de champs. Chaque définition de champ est composée d’un ensemble d’attributs qui décrivent le champ. Les exemples d’attributs sont le nom d’affichage, le nom de l’API et dataType. Ces attributs sont collectivement appelés métadonnées.

Les points d’entrée suivants vous permettent d’interroger des champs sur l’objet société. Ces API nécessitent que l’utilisateur de l’API propriétaire dispose d’un rôle avec l’une ou l’autre des autorisations `Read-Write Schema Standard Field` ou `Read-Write Schema Custom Field`, ou les deux.

### Champs de requête

Interroger les champs d’entreprise est simple. Vous pouvez interroger un seul champ société par nom d’API ou interroger l’ensemble de tous les champs société.

#### Par nom

Le point d’entrée [Obtenir le champ d’entreprise par nom](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldByNameUsingGET) récupère les métadonnées d’un seul champ sur l’objet d’entreprise. Le paramètre de chemin d’accès `fieldApiName` obligatoire spécifie le nom d’API du champ. La réponse est similaire au point d’entrée Décrire la société, mais elle contient des métadonnées supplémentaires telles que l’attribut `isCustom` qui indique si le champ est un champ personnalisé.

```
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

Le point d’entrée [Obtenir les champs d’entreprise](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldsUsingGET) récupère les métadonnées de tous les champs de l’objet d’entreprise. Par défaut, un maximum de 300 enregistrements est renvoyé. Vous pouvez utiliser le paramètre de requête `batchSize` pour réduire ce nombre. Si l’attribut `moreResult` est défini sur « true », cela signifie que d’autres résultats sont disponibles. Continuez à appeler ce point d’entrée jusqu’à ce que l’attribut moreResult renvoie false, ce qui signifie qu’aucun résultat n’est disponible. Les `nextPageToken` renvoyés par cette API doivent toujours être réutilisés pour l’itération suivante de cet appel.

```
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

Les critères de suppression sont spécifiés dans le tableau `input` , qui contient une liste de valeurs de recherche.  La méthode de suppression est spécifiée dans le paramètre `deleteBy` .  Les valeurs autorisées sont les suivantes : dedupeFields, idField.  La valeur par défaut est dedupeFields.

```
Content-Type: application/json
```

```
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

- Les points d’entrée d’entreprise ont un délai d’expiration de 30 s, sauf indication ci-dessous
   - Entreprises de synchronisation : années 60
   - Supprimer entreprises : 60s
