---
title: "Entreprises"
feature: REST API
description: "Configurez les données de l’entreprise avec les API Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 1%

---


# Entreprises

[Référence des points de terminaison des entreprises](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

Les entreprises représentent l’organisation à laquelle appartiennent les enregistrements de piste. Les pistes sont ajoutées à une entreprise en renseignant les `externalCompanyId` champ utilisant [Pistes de synchronisation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) ou [Importation de pistes en bloc](bulk-lead-import.md) points de fin. Une fois qu’une piste a été ajoutée à une société, vous ne pouvez pas la supprimer (à moins que vous n’ajoutiez la piste à une autre société). Les pistes liées à un enregistrement de société héritent directement des valeurs d’un enregistrement de société comme si les valeurs existaient sur leur propre enregistrement.

Les API d’entreprise sont un accès en lecture seule pour les abonnements qui ont [Synchronisation SFDC](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) ou [Synchronisation Microsoft Dynamics](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) sont activées.

## Description

La description de l’objet d’entreprise vous donne toutes les informations que vous devez interagir avec celui-ci.

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

Le modèle de [requêter des sociétés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompaniesUsingGET) suit de près celle de l’API de pistes avec la restriction ajoutée que la variable `filterType` accepte les champs répertoriés dans le tableau searchableFields de l’appel Description des sociétés ou dedupeFields.

`filterType` et `filterValues` sont des paramètres de requête requis.  `fields`, `nextPageToken`, et `batchSize` sont des paramètres facultatifs.  Les paramètres fonctionnent de la même manière que les paramètres correspondants dans les API Leads and Opportunities. Lorsque vous demandez une liste de `fields`, si un champ particulier est demandé, mais n’est pas renvoyé, la valeur est implicite comme étant nulle.

Si le paramètre fields est omis, le jeu de champs renvoyé par défaut est :

- id
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

La variable [Entreprises de synchronisation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) endpoint accepte les `input` qui contient un tableau d’objets de société. Comme les opportunités, il existe trois modes de création et de mise à jour des entreprises : createOnly, updateOnly et createOrUpdate.  Les modes sont spécifiés dans la variable `action` du paramètre de la requête. Les deux `dedupeBy` et `action` Les paramètres sont facultatifs et par défaut, les modes dedupeFields et createOrUpdate sont respectivement définis.

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

L’objet company contient un ensemble de champs. Chaque définition de champ est composée d’un ensemble d’attributs qui décrivent le champ. Les attributs sont par exemple le nom d’affichage, le nom de l’API et dataType. Ces attributs sont connus collectivement sous le nom de métadonnées.

Les points de terminaison suivants vous permettent d’interroger des champs sur l’objet company. Ces API requièrent que l’utilisateur de l’API propriétaire ait un rôle avec l’une des API ou les deux. `Read-Write Schema Standard Field` ou `Read-Write Schema Custom Field` autorisations.

### Champs de requête

La requête sur les champs de l’entreprise est simple. Vous pouvez interroger un champ de société unique par nom d’API ou interroger l’ensemble de tous les champs de société.

#### Par nom

La variable [Obtention du champ de la société par nom](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldByNameUsingGET) Le point de terminaison récupère les métadonnées d’un champ unique sur l’objet de la société. La variable `fieldApiName` Le paramètre path spécifie le nom de l’API du champ. La réponse est semblable au point de terminaison Décrire la société , mais contient des métadonnées supplémentaires telles que la variable `isCustom` qui indique si le champ est un champ personnalisé.

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

La variable [Obtention des champs de l’entreprise](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldsUsingGET) endpoint récupère les métadonnées de tous les champs de l’objet de société. Par défaut, un maximum de 300 enregistrements est renvoyé. Vous pouvez utiliser la variable `batchSize` paramètre de requête pour réduire ce nombre. Si la variable `moreResult` est true, cela signifie que plus de résultats sont disponibles. Continuez à appeler ce point de terminaison jusqu’à ce que l’attribut moreResult renvoie false, ce qui signifie qu’il n’y a aucun résultat disponible. La variable `nextPageToken` La valeur renvoyée par cette API doit toujours être réutilisée pour la prochaine itération de cet appel.

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

Les critères de suppression sont spécifiés dans la variable `input` qui contient une liste de valeurs de recherche.  La méthode de suppression est spécifiée dans la variable `deleteBy` .  Les valeurs possibles sont : dedupeFields, idField.  La valeur par défaut est dedupeFields.

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

## Délais d’expiration

- Les points de terminaison d’entreprise ont un délai d’expiration de 30 s, sauf indication ci-dessous.
   - Entreprises de synchronisation : 60 s 
   - Suppression d’entreprises : 60 s
