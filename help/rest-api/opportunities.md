---
title: Opportunités
feature: REST API
description: API REST Marketo pour décrire, interroger, créer et mettre à jour des opportunités, dédupliquer et rechercher des champs, des limites et un comportement en lecture seule avec la synchronisation SFDC ou Dynamics.
exl-id: 46451285-4125-4857-890a-575069a68288
TQID: https://experienceleague.adobe.com/rBDJcXWQrN5qyKRWHyzVC-sc9BH2mQFLm7fKUk-NUn8
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 702
ht-degree: 0%

---

# Opportunités

[Référence du point d’entrée d’opportunité](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities)

Marketo fournit des API pour lire, écrire, créer et mettre à jour des enregistrements d’opportunité. Dans Marketo, l’objet intermédiaire Rôle d’opportunité lie les enregistrements d’opportunité aux enregistrements de prospect et de contact. Une opportunité peut donc être liée à de nombreuses pistes individuelles.

L’API expose les deux types d’objets. Comme pour la plupart des types d’objets Base de données de leads, chacun possède un appel Describe correspondant qui renvoie des métadonnées d’objet.

Les API d’opportunité fournissent un accès en lecture seule aux abonnements pour lesquels la [synchronisation de ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) ou la [synchronisation de Microsoft Dynamics](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) est activée.

## Décrire

Décrivez les enregistrements d’opportunité à l’aide du modèle standard pour les objets de base de données de leads.

```http
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

Les champs de réponse clés sont les suivants :

- `idField` : identifie la clé primaire de l’opportunité, marketoGUID. Cette clé générée par le système prend en charge les opérations de lecture et de mise à jour, mais pas les insertions.
- `dedupeFields` : identifie les clés valides pour les opérations d’insertion. Pour les opportunités, la seule clé est externalOpportunityId.
- `searchableFields` : identifie les champs valides pour les requêtes. Ces champs sont externalOpportunityId et marketoGUID.

## Requête

Le modèle d’[opportunités d’interrogation](https://developer.adobe.com/marketo-apis/api/mapi#operation/getOpportunitiesUsingGET) suit de près l’API Leads. Cependant, le paramètre `filterType` accepte uniquement les champs répertoriés dans le tableau `searchableFields` de la réponse Describe ou dedupeFields correspondante.

Pour les champs d’opportunité personnalisés, seuls les champs de type Chaîne ou Entier apparaissent dans le tableau searchableFields .

```http
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

Vous pouvez inclure les paramètres de requête facultatifs suivants :

- `fields` : renvoie des champs d’opportunité supplémentaires.
- `nextPageToken` : pages dans les jeux de résultats supérieurs à la taille du lot.
- `batchSize` : indique la taille du lot. La valeur par défaut et la valeur maximale sont 300.

Lorsque vous demandez une liste de `fields`, un champ demandé qui n’est pas renvoyé a une valeur implicite null.

## Créer et mettre à jour

Les opportunités suivent le modèle de l’API Leads avec certaines restrictions. Les valeurs `action` sont createOnly, createOrUpdate et updateOnly.

- Pour le mode createOnly ou createOrUpdate , incluez le champ externalOpportunityId dans chaque enregistrement.
- Pour le mode updateOnly, utilisez marketoGUID ou externalOpportunityId.
- Si ce paramètre n’est pas spécifié, le mode par défaut est createOrUpdate.

Le paramètre `lookupField` de l’API Leads n’est pas disponible. Le paramètre dedupeBy le remplace et n’est valide que lorsque l’action est updateOnly.

Les valeurs dedupeBy sont « dedupeFields » et « idField », que la réponse Describe identifie comme externalOpportunityId et marketoGUID, respectivement. Si dedupeBy n&#39;est pas spécifié, il utilise par défaut le mode dedupeFields. Le champ &#39;name&#39; ne doit pas être nul.

Vous pouvez envoyer jusqu’à 300 enregistrements à la fois.

```http
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

La réponse inclut les valeurs suivantes pour chaque enregistrement :

- `marketoGUID` : identifiant de l’enregistrement.
- `status` : succès ou échec de l’enregistrement individuel.
- `seq` : index de l’enregistrement envoyé, qui met en corrélation l’enregistrement de la requête avec l’ordre de réponse.

### Champs

L’objet company contient des champs définis par des attributs tels que le nom d’affichage, le nom de l’API et le dataType. Ensemble, ces attributs sont appelés métadonnées.

Les points d’entrée suivants interrogent les champs sur l’objet société. L’utilisateur de l’API doit disposer d’un rôle avec l’autorisation `Read-Write Schema Standard Field`, l’autorisation `Read-Write Schema Custom Field` ou les deux.

### Champs de requête

Exécutez une requête sur un champ société par nom d’API ou récupérez tous les champs société.

#### Par nom

Le point d’entrée [Get Opportunity Field by Name](https://developer.adobe.com/marketo-apis/api/mapi#operation/getOpportunityFieldByNameUsingGET) récupère les métadonnées d’un champ de l’objet d’entreprise. Le paramètre de chemin d’accès `fieldApiName` obligatoire spécifie le nom de l’API du champ.

La réponse ressemble à la réponse Description de l’opportunité mais inclut des métadonnées supplémentaires. Par exemple, l’attribut `isCustom` indique si le champ est personnalisé.

```http
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

Le point d’entrée [Obtenir les champs d’opportunité](https://developer.adobe.com/marketo-apis/api/mapi#operation/getOpportunityFieldsUsingGET) récupère les métadonnées de tous les champs de l’objet d’entreprise. Par défaut, elle renvoie un maximum de 300 enregistrements. Utilisez le paramètre de requête `batchSize` pour réduire ce nombre.

Si l’attribut `moreResult` est défini sur « true », d’autres résultats sont disponibles. Continuez à appeler le point d’entrée avec la `nextPageToken` renvoyée jusqu’à ce que moreResult ait la valeur false.

```http
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

Supprimer les opportunités par champs de déduplication ou champ d’ID. Définissez le paramètre `deleteBy` sur dedupeFields ou idField. La valeur par défaut est dedupeFields.

Le corps de la requête contient un tableau `input` d’opportunités à supprimer. Chaque appel permet un maximum de 300 opportunités.

```http
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

- Le délai d’expiration des points d’entrée d’opportunité est de 30, sauf indication contraire.
- Le délai d’expiration des opportunités de synchronisation est de 60 ans.
- Le délai d’expiration de la suppression d’opportunités est de 60.
