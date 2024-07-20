---
title: Opportunités
feature: REST API
description: Configurez les opportunités avec l’API Marketo.
exl-id: 46451285-4125-4857-890a-575069a68288
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 0%

---

# Opportunités

[Référence du point de terminaison d’opportunité](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities)

Marketo expose les API pour la lecture, l’écriture, la création et la mise à jour des enregistrements d’opportunité. Dans Marketo, les enregistrements d’opportunité sont liés aux enregistrements de prospect et de contact par le biais de l’objet Rôle d’opportunité intermédiaire, de sorte qu’une opportunité peut être liée à de nombreuses pistes individuelles.  Ces deux types d’objets sont exposés par le biais de l’API. Comme la plupart des types d’objets de la base de données de piste, ils ont tous deux un appel de description correspondant, qui renvoie des métadonnées sur les types d’objets.

Les API d’opportunité sont un accès en lecture seule pour les abonnements pour lesquels la [synchronisation SFDC](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) ou la [synchronisation Microsoft Dynamics](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) sont activées.

## Description

Description Les enregistrements d’opportunité suivent le modèle standard pour les objets de base de données de piste.

```
GET /rest/v1/opportunities/describe.json
```

```json
{  
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[  
      {  
         "name":"opportunity",
         "displayName":"Opportunity",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[  
            "externalOpportunityId"
         ],
         "searchableFields":[  
            [  
               "externalOpportunityId"
            ],
            [  
               "marketoGUID"
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
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            }
         ]
      }
   ]
}
```

Les champs les plus importants pour ce type de réponse sont `idField`, `dedupeFields` et `searchableFields`.  idField indique la clé primaire des opportunités, marketoGUID.  Il s’agit d’une clé unique générée par le système, qui peut être utilisée pour les opérations de lecture et de mise à jour, mais pas pour les insertions, puisqu’il s’agit d’une clé gérée par le système.  Le tableau dedupeFields indique les champs qui sont des clés valides pour les opérations d’insertion. En cas d’opportunités, il s’agit uniquement de externalOpportunityId.  Le tableau searchableFields fournit l’ensemble des champs valides pour interroger, externalOpportunityId et marketoGUID.

## Requête

Le modèle de [ requêtes d’opportunités](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunitiesUsingGET) suit de près celui de l’API Leads avec la restriction ajoutée que le paramètre `filterType` accepte les champs répertoriés dans le tableau `searchableFields` ou de l’appel de description correspondant, ou dedupeFields.  Notez que si vous utilisez des champs d’opportunité personnalisés, seuls les champs d’opportunité personnalisés de type Chaîne ou Entier seront répertoriés dans le tableau searchableFields.

```
GET /rest/v1/opportunities.json?filterType=marketoGUID&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{  
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[  
      {  
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa ",
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {  
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc ",
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

Vous pouvez également inclure les paramètres de requête facultatifs `fields`, pour renvoyer des champs d’opportunité supplémentaires, `nextPageToken`, pour la pagination via des ensembles plus grands que la taille du lot, `batchSize`, qui est définie par défaut sur et dont le nombre maximal est de 300.  Lorsque vous demandez une liste de `fields`, si un champ particulier est demandé, mais n’est pas renvoyé, la valeur est implicitement nulle.

## Créer et mettre à jour

Les opportunités suivent de près le modèle d’API de prospect, avec certaines restrictions.  Les valeurs disponibles pour `action` sont : createOnly, createOrUpdate et updateOnly.  Lors de l’utilisation du mode createOnly ou createOrUpdate, le champ externalOpportunityId doit être inclus dans chaque enregistrement.  Pour le mode updateOnly, vous pouvez utiliser marketoGUID ou externalOpportunityId .  Le mode est défini par défaut sur createOrUpdate s’il n’est pas spécifié.

Le paramètre `lookupField` de l’API Leads n’est pas disponible et est remplacé par le paramètre dedupeBy , qui n’est valide que si l’action est updateOnly.  Les valeurs disponibles pour dedupeBy sont &quot;dedupeFields&quot; ou &quot;idField&quot;, qui sont spécifiées par l’appel de description en tant que externalOpportunityId et marketoGUID respectivement.  Si dedupeBy n’est pas spécifié, le mode dedupeFields est défini par défaut.  Le champ &#39;nom&#39; ne doit pas être nul.

Vous pouvez envoyer jusqu’à 300 enregistrements à la fois.

```
POST /rest/v1/opportunities.json
```

```json
{  
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[  
      {  
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {  
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
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
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

L’API répondra avec le `marketoGUID` pour chaque enregistrement, ainsi qu’un champ `status`, indiquant le succès ou l’échec individuel de chaque enregistrement, et un champ `seq` utilisé pour corréler les enregistrements envoyés, à l’ordre de la réponse.  Le nombre dans le champ est l’index de l’enregistrement envoyé dans la requête.

### Champs

L’objet company contient un ensemble de champs.  Chaque définition de champ comprend un ensemble d’attributs qui décrivent le champ.  Les attributs sont par exemple le nom d’affichage, le nom de l’API et dataType.  Ces attributs sont connus collectivement sous le nom de métadonnées.

Les points de terminaison suivants vous permettent d’interroger des champs sur l’objet company. Ces API requièrent que l’utilisateur de l’API propriétaire ait un rôle avec l’une ou les deux autorisations `Read-Write Schema Standard Field` ou `Read-Write Schema Custom Field`.

### Champs de requête

La requête sur les champs d’opportunité est simple.  Vous pouvez interroger un champ de société unique par nom d’API ou interroger l’ensemble de tous les champs de société.

#### Par nom

Le point de terminaison [Obtenir le champ d’opportunité par nom](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldByNameUsingGET) récupère les métadonnées d’un champ unique sur l’objet de la société.  Le paramètre de chemin d’accès `fieldApiName` requis spécifie le nom d’API du champ.  La réponse est similaire au point de terminaison Décrire l’opportunité , mais contient des métadonnées supplémentaires telles que l’attribut `isCustom` qui indique si le champ est un champ personnalisé.

```
GET /rest/v1/opportunities/schema/fields/externalOpportunityId.json
```

```json
{
    "requestId": "12331#17e9779cb4b",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
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

Le point d’entrée [Obtenir les champs d’opportunité](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldsUsingGET) récupère les métadonnées de tous les champs de l’objet d’entreprise.  Par défaut, un maximum de 300 enregistrements est renvoyé.  Vous pouvez utiliser le paramètre de requête `batchSize` pour réduire ce nombre.  Si l’attribut `moreResult` est défini sur true, cela signifie que d’autres résultats sont disponibles.  Continuez à appeler ce point de terminaison jusqu’à ce que l’attribut moreResult renvoie false, ce qui signifie qu’il n’y a aucun résultat disponible.  Les `nextPageToken` renvoyés par cette API doivent toujours être réutilisés pour la prochaine itération de cet appel.

```
GET /rest/v1/opportunities/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "b4a#17e995b31da",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
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
            "displayName": "Description",
            "name": "description",
            "description": null,
            "dataType": "string",
            "length": 2000,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Type",
            "name": "type",
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
            "displayName": "Stage",
            "name": "stage",
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
    "nextPageToken": "E5ZONGE4SAHALYYW6FS25KB5BM======",
    "moreResult": true
}
```

#### Supprimer

Vous pouvez supprimer des opportunités par champs de déduplication ou champ d’identifiant. Spécifiez en utilisant le paramètre `deleteBy` avec une valeur dedupeFields ou idField. Si elle n’est pas spécifiée, la valeur par défaut est dedupeFields. Le corps de la requête contient un tableau `input` d’opportunités de suppression. Un maximum de 300 opportunités par appel est autorisé.

```
POST /rest/v1/opportunities/delete.json
```

```json
{ 
   "deleteBy":"dedupeFields",
   "input":[ 
      { 
         "externalOpportunityId":"19UYA31581L000000"
      },
      { 
         "externalOpportunityId":"29UYA31581L000000"
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
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      }
   ]
}
```

## Délais d’expiration

- Les points de terminaison d’opportunité ont un délai d’expiration de 30 s, sauf indication ci-dessous.
   - Opportunités de synchronisation : 60 s 
   - Suppression d’opportunités : 60 s
