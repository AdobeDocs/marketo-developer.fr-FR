---
title: Base de données des leads
feature: REST API, Database
description: Manipuler la base de données principale de piste.
exl-id: e62e381f-916b-4d56-bc3d-0046219b68d3
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1345'
ht-degree: 1%

---

# Base de données des leads

Les API de la base de données de prospect Marketo sont les API les plus fréquemment utilisées fournies par Marketo, car elles permettent l’échange de données entre personnes et données liées aux personnes à partir de Marketo, telles que les activités, opportunités et entreprises.

## Objets

Les objets de base de données de piste sont les suivants :

- Prospects
- Sociétés/Comptes
- Comptes nommés
- Opportunités
- OpportunityRoles
- Salesjeudi
- Objets personnalisés
- Activités
- Liste et appartenance à un programme

La plupart de ces objets incluent au moins les méthodes Create, Read, Update et Delete. Elle comprend également une méthode &quot;Description&quot; qui fournit une liste des champs disponibles pour chaque type, ainsi qu’une liste des champs utilisés pour le dédoublonnage (pour les objets non liés au plomb) et les champs pouvant faire l’objet de recherches pour récupérer les enregistrements. L’ensemble le plus riche est fourni pour les prospects, car ils disposent de la plus grande variété de fonctionnalités des applications Marketo.

## API

Pour obtenir la liste complète des points de terminaison de l’API de base de données de piste, y compris les paramètres, et les informations de modélisation, consultez la [Référence du point de terminaison de l’API de base de données de piste](https://developer.adobe.com/marketo-apis/api/mapi/).

Pour les instances avec une intégration CRM native activée (Microsoft Dynamics ou Salesforce.com), les API Société, Opportunité, Rôle d’opportunité et Personne commerciale sont désactivées. Les enregistrements sont gérés par le CRM lorsqu&#39;ils sont activés et ne peuvent pas être consultés ni mis à jour via les API Marketo.

- Taille max. du lot (standard) : 300 enregistrements
- Taille max. du lot (en bloc) : fichier de 10 Mo
- Taille de lot par défaut : 300 enregistrements
- En-tête de type contenu (standard) : application/json
- En-tête de type contenu (en masse) : multipart/form-data

## Description

Pour les pistes, les entreprises, les opportunités, les rôles, les personnes de vente et les objets personnalisés, une API de description est fournie. L’appel de cette méthode récupère les métadonnées de l’objet, ainsi qu’une liste des champs disponibles pour la mise à jour et l’interrogation. La description est un élément essentiel de la conception d’une intégration correcte avec Marketo. Il fournit des métadonnées enrichies sur la manière dont les objets peuvent et ne peuvent pas être interactifs, ainsi que sur la manière dont ils peuvent être créés, mis à jour et interrogés. Outre les pistes descriptives, chacune de ces méthodes renvoie une liste de clés disponibles pour `deduplication` dans le paramètre de réponse `dedupeFields`. Une liste de champs est disponible en tant que clés pour l’interrogation du paramètre de réponse `searchableFields`.

```
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

Dans cet exemple, `dedupeFields` est en fait une clé composite. Cela signifie que lors des futures mises à jour et création, lorsque vous utiliserez le mode `dedupeFields`, vous devrez inclure les trois de `externalOpportunityId`, `leadId` et `role` pour chaque rôle. Le tableau `searchableFields` fournit également la liste des champs disponibles pour interroger les enregistrements de rôle. Cela inclut également la clé composite `externalOpportunityId`, `leadId` et `role`.

Il existe également un paramètre de réponse de champ qui fournira le nom de chaque champ, le `displayName` tel qu’il apparaît dans l’interface utilisateur de Marketo, le type de données du champ, s’il peut être mis à jour après sa création et la longueur du champ, le cas échéant.

## Requête

Les objets de base de données de piste partagent tous un schéma de base pour effectuer une requête sur des clés simples, où un seul champ est référencé.

```
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Pour tous les objets, à l’exception des pistes, vous pouvez sélectionner votre {champ à interroger} dans la propriété searchableFields de l’appel de description correspondant, puis composer une liste de valeurs séparées par des virgules allant jusqu’à 300. Il existe également ces paramètres de requête facultatifs :

- `batchSize` - Nombre entier du nombre de résultats à renvoyer. La valeur par défaut et la valeur maximale sont 300.
- `nextPageToken` - Jeton renvoyé par un précédent appel de pagination. Pour plus d’informations, voir [Jetons de pagination](paging-tokens.md) .
- `fields` - Liste de noms de champ séparés par des virgules à renvoyer pour chaque enregistrement. Voir la description correspondante pour une liste de champs valides. Si un champ particulier est demandé, mais n’est pas renvoyé, la valeur est implicitement nulle.
- `_method` - Utilisé pour envoyer des requêtes à l’aide de la méthode HTTP du POST. Voir la section _method=GET ci-dessous pour en savoir plus sur l’utilisation.

Pour un exemple rapide, examinons les opportunités de requêtage :

```
GET /rest/v1/opportunities.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb
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

`filterType` spécifié dans cet appel est &quot;idField&quot; et non &quot;marketoGUID&quot;. Ce et &quot;dedupeFields&quot; sont deux cas spéciaux, où le champ correspondant à idField ou dedupeFields peut être alias de cette manière. &quot;marketoGUID&quot; est toujours le champ de recherche qui en résulte dans l’appel, mais il n’est pas défini explicitement dans l’appel . Les champs et/ou les ensembles de champs indiqués par les `idField` et `dedupeFields` d’une description d’objet seront toujours valides `filterTypes` pour une requête. Cet appel recherche des enregistrements correspondant aux GUID inclus dans filterValues et renvoie les enregistrements correspondants. Si aucun enregistrement n’est trouvé utilisant cette méthode, la réponse indique toujours la réussite. Toutefois, le tableau de résultat sera vide, car la recherche a été exécutée avec succès, mais aucun enregistrement n’a été renvoyé.

Si l’ensemble d’enregistrements de la requête dépasse 300 ou le `batchSize` spécifié, selon le plus petit, alors la réponse comporte un membre `moreResult` avec la valeur true et un `nextPageToken`, qui peut être inclus dans un appel ultérieur pour récupérer davantage de l’ensemble. Pour plus d’informations, voir [Jetons de pagination](paging-tokens.md) .

### URI longs

Parfois, par exemple, lors de l’interrogation par des GUID, votre URI peut être long et dépasser les 8 Ko autorisés par le service REST. Dans ce cas, vous devez utiliser la méthode de POST HTTP au lieu de GET, et ajouter un paramètre de requête `_method=GET`. En outre, le reste des paramètres de requête doit être transmis dans le corps du POST sous la forme d’une chaîne &quot;application/x-www-form-urlencoded&quot; et transmettre l’en-tête de type Contenu associé.

```
POST /rest/v1/opportunities.json?_method=GET
```

```
Content-Type: application/x-www-form-urlencoded
```

```
filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb,544fb7f5-2ddf-4fca-ae32-7e6ef1415e9f,f1ba41a2-69d1-4a35-9807-0e159d66f2c9,f7521272-3331-4a89-a768-222baff2f894
```

Outre les URI longs, ce paramètre est également requis lors de l’interrogation de clés composites.

### Clés composées

Le modèle d’interrogation des clés composites diffère des clés simples, car il nécessite l’envoi d’un POST avec un corps JSON. Cela n’est pas nécessaire dans tous les cas, uniquement dans les cas où une option `dedupeFields` avec plusieurs champs est utilisée comme `filterType`. Actuellement, les clés composites ne sont utilisées que par les rôles d’opportunité et certains objets personnalisés. Examinons un exemple de requête pour les rôles d’opportunité avec la clé composite de `dedupeFields` :

```
POST /rest/v1/opportunities/roles.json?_method=GET
```

```json
{  
   "filterType":"dedupeFields",
   "fields":[  
      "marketoGuid",
      "externalOpportunityId",
      "leadId",
      "role"
   ],
   "input":[  
      {  
        "externalOpportunityId":"Opportunity1",
        "leadId": 1,
        "role": "Captain"
      },
      {  
        "externalOpportunityId":"Opportunity2",
        "leadId": 1872,
        "role": "Commander"
      },
      {  
        "externalOpportunityId":"Opportunity3",
        "leadId": 273891,
        "role": "Lieutenant Commander"
      }
   ]
}
```

La structure de l’objet JSON est principalement plate, et tous les paramètres de requête des requêtes avec des clés simples sont des membres valides, à l’exception de `filterValues`. À la place d’une valeur de filtre, il existe un tableau &quot;input&quot; d’objets JSON, dont chacun doit avoir un membre pour chacun des champs de votre clé composite ; dans ce cas, ils sont `externalOpportunityId`, `leadId` et `role`. Cette opération exécute une requête pour `roles`, en fonction des entrées fournies et renvoie les résultats correspondants. Si la réponse renvoie un paramètre avec `moreResult=true` et un `nextPageToken`, vous devez inclure toutes les entrées d’origine et le `nextPageToken` pour que la requête s’exécute correctement.

## Créer et mettre à jour

Les créations et mises à jour des enregistrements de base de données de piste sont toutes effectuées par le biais de POST avec des corps JSON. L’interface pour les opportunités, les rôles, les objets personnalisés, les entreprises et les personnes chargées de la vente sont identiques. L&#39;interface du prospect est un peu différente, et vous pouvez en lire plus à ce sujet en particulier.

Le seul paramètre requis est un tableau appelé `input` contenant jusqu’à 300 objets, chacun avec les champs que vous souhaitez insérer/mettre à jour en tant que membres. Vous pouvez également éventuellement inclure un paramètre `action` qui peut être l’un des paramètres suivants : `createOnly`, `updateOnly` ou `createOrUpdate`. Si l’action est omise, le mode par défaut est `createOrUpdate`. `dedupeBy` est un autre paramètre facultatif qui peut être utilisé lorsque l’action est définie sur createOnly ou `createOrUpdate`. ` dedupeBy` peut être `idField` ou `dedupeFields`. Si `idField` est sélectionné, le `idField` répertorié dans la description est utilisé pour le dédoublonnage et doit être inclus dans chaque enregistrement. Le mode `idField` n’est pas compatible avec le mode `createOnly`. Si `dedupeFields` est sélectionné, alors le `dedupeFields` répertorié dans la description de l’objet utilisé et chacun d’eux doit être inclus dans chaque enregistrement. Si le paramètre `dedupeBy` est omis, le mode est `dedupeFields` par défaut.

Lors de la transmission d’une liste de valeurs de champ, une valeur `null`, ou une chaîne vide, est écrite dans la base de données sous la forme `null`.

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

Outre l’API de pistes, les appels pour créer ou mettre à jour des objets de base de données de pistes renvoient un champ `seq` dans chaque objet du tableau `result`. Le numéro répertorié correspond à l’ordre de l’enregistrement mis à jour dans la requête effectuée. Chaque élément renvoie la valeur de `idField` pour le type d’objet et un `status`. Le champ d’état indique l’un des éléments &quot;créé&quot;, &quot;mis à jour&quot; ou &quot;ignoré&quot;.  Si l’état est ignoré, un tableau &quot;raisons&quot; correspondant s’affiche avec un ou plusieurs objets de raison comprenant un code et un message, indiquant pourquoi un enregistrement a été ignoré. Voir [codes d’erreur](error-codes.md) pour plus de détails.

### Supprimer

L’interface de suppression est standard pour les objets de la base de données de piste autres que les pistes. Outre l’entrée, il n’existe qu’un seul paramètre requis `deleteBy,` qui peut avoir une valeur idField ou dedupeFields. Examinons la suppression de certains objets personnalisés.

```
POST /rest/v1/customobjects/{name}/delete.json
```

```json
{  
   "deleteBy":"dedupeFields",
   "input":[  
      {  
         "vin":"19UYA31581L000000"
      },
      {  
         "vin":"29UYA31581L000000"
      },
      {  
         "vin":"39UYA31581L000000"
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
         "status": "deleted"
      },
      {
         "seq":1,
         "marketoGUID":"da42707c-4dc4-4fc1-9fef-f30a3017240a",
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1013",
               "message":"Object not found"
            }
         ]
      }
   ]
}
```

Les `seq`, `status`, `marketoGUID` et `reasons` doivent vous être familiers à l&#39;heure actuelle.

Pour plus d’informations sur l’utilisation des opérations CRUD pour chaque type d’objet, consultez leurs pages respectives.
