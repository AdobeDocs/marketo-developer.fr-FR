---
title: Opportunités
feature: REST API
description: ' Configurez les opportunités avec l’API Marketo.'
exl-id: 46451285-4125-4857-890a-575069a68288
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 0%

---

# Opportunités

[Référence du point d’entrée d’opportunité](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities)

Marketo propose des API pour la lecture, l’écriture, la création et la mise à jour des enregistrements d’opportunité. Dans Marketo, les enregistrements d’opportunité sont liés aux enregistrements de prospect et de contact par le biais de l’objet Rôle d’opportunité intermédiaire. Une opportunité peut donc être liée à de nombreux prospects individuels.  Ces deux types d’objets sont exposés par le biais de l’API et, comme la plupart des types d’objets de la base de données de leads, ils possèdent tous deux un appel Describe correspondant qui renvoie des métadonnées sur les types d’objets.

Les API d’opportunité sont en lecture seule pour les abonnements pour lesquels la [synchronisation de SFDC](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) ou la [synchronisation de Microsoft Dynamics](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) est activée.

## Décrire

La description des enregistrements d’opportunité suit le modèle standard pour les objets de base de données de prospect.

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

Les champs les plus importants pour ce type de réponse sont `idField`, `dedupeFields` et `searchableFields`.  idField indique la clé primaire des opportunités, marketoGUID.  Il s’agit d’une clé unique générée par le système, qui peut être utilisée pour les opérations de lecture et de mise à jour, mais pas pour les insertions, car elle est gérée par le système.  Le tableau dedupeFields indique quels champs sont des clés valides pour les opérations d’insertion ; dans le cas d’opportunités, il s’agit uniquement de externalOpportunityId.  Le tableau searchableFields vous donne l’ensemble des champs valides pour l’interrogation, externalOpportunityId et marketoGUID.

## Requête

Le modèle d’[opportunités d’interrogation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunitiesUsingGET) suit de près celui de l’API de leads avec la restriction ajoutée que le paramètre `filterType` accepte les champs répertoriés dans le tableau `searchableFields` ou de l’appel de description correspondant, ou dedupeFields.  Notez que si vous utilisez des champs d’opportunité personnalisés, seuls les champs d’opportunité personnalisés de type Chaîne ou Entier seront répertoriés dans le tableau searchableFields .

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

Vous pouvez également inclure les paramètres de requête facultatifs `fields`, pour renvoyer des champs d’opportunité supplémentaires, `nextPageToken`, pour paginer dans des ensembles supérieurs à la taille du lot, `batchSize`, qui est définie par défaut sur et a un maximum de 300.  Lors de la demande d’une liste de `fields`, si un champ particulier est demandé, mais n’est pas renvoyé, la valeur est implicitement nulle.

## Créer et mettre à jour

Les opportunités suivent de près le modèle de l’API des prospects, avec certaines restrictions.  Les valeurs disponibles pour `action` sont les suivantes : createOnly, createOrUpdate et updateOnly.  En mode createOnly ou createOrUpdate, le champ externalOpportunityId doit être inclus dans chaque enregistrement.  Pour le mode updateOnly, marketoGUID ou externalOpportunityId peut être utilisé.  Le mode par défaut est createOrUpdate s’il n’est pas spécifié.

Le paramètre `lookupField` de l’API de leads n’est pas disponible et est remplacé par le paramètre dedupeBy, qui n’est valide que si l’action est updateOnly.  Les valeurs disponibles pour dedupeBy sont « dedupeFields » ou « idField » qui sont spécifiées par l’appel de description comme externalOpportunityId et marketoGUID, respectivement.  Si dedupeBy n&#39;est pas spécifié, il utilise par défaut le mode dedupeFields.  Le champ &#39;name&#39; ne doit pas être nul.

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

L’API répond avec la `marketoGUID` de chaque enregistrement, ainsi qu’un champ `status`, indiquant le succès ou l’échec individuel de chaque enregistrement, et un champ `seq` qui est utilisé pour mettre en corrélation les enregistrements envoyés, avec l’ordre de la réponse.  Le nombre dans le champ est l’index de l’enregistrement envoyé dans la requête.

### Champs

L’objet company contient un ensemble de champs.  Chaque définition de champ se compose d’un ensemble d’attributs qui décrivent le champ.  Les exemples d’attributs sont le nom d’affichage, le nom de l’API et dataType.  Ces attributs sont collectivement appelés métadonnées.

Les points d’entrée suivants vous permettent d’interroger des champs sur l’objet société. Ces API nécessitent que l’utilisateur de l’API propriétaire dispose d’un rôle avec l’une ou l’autre des autorisations `Read-Write Schema Standard Field` ou `Read-Write Schema Custom Field`, ou les deux.

### Champs de requête

L’interrogation des champs d’opportunité est simple.  Vous pouvez interroger un seul champ société par nom d’API ou interroger l’ensemble de tous les champs société.

#### Par nom

Le point d’entrée [Obtenir le champ de l’opportunité par nom](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldByNameUsingGET) récupère les métadonnées d’un seul champ sur l’objet d’entreprise.  Le paramètre de chemin d’accès `fieldApiName` obligatoire spécifie le nom d’API du champ.  La réponse est similaire au point d’entrée Décrire l’opportunité, mais contient des métadonnées supplémentaires telles que l’attribut `isCustom` qui indique si le champ est un champ personnalisé.

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

Le point d’entrée [Obtenir les champs d’opportunité](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldsUsingGET) récupère les métadonnées de tous les champs de l’objet d’entreprise.  Par défaut, un maximum de 300 enregistrements est renvoyé.  Vous pouvez utiliser le paramètre de requête `batchSize` pour réduire ce nombre.  Si l’attribut `moreResult` est défini sur « true », cela signifie que d’autres résultats sont disponibles.  Continuez à appeler ce point d’entrée jusqu’à ce que l’attribut moreResult renvoie false, ce qui signifie qu’aucun résultat n’est disponible.  Les `nextPageToken` renvoyés par cette API doivent toujours être réutilisés pour l’itération suivante de cet appel.

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

Vous pouvez supprimer des opportunités en dédupliquant des champs ou des champs d’ID. Spécifiez à l’aide du paramètre `deleteBy` avec une valeur de dedupeFields ou idField. S’il n’est pas spécifié, la valeur par défaut est dedupeFields. Le corps de la requête contient un tableau `input` d’opportunités à supprimer. Un maximum de 300 opportunités par appel est autorisé.

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

## Délais dépassés

- Le délai d’expiration des points d’entrée d’opportunité est de 30, sauf indication ci-dessous
   - Opportunités de synchronisation : années 60 
   - Supprimer les opportunités : années 60
