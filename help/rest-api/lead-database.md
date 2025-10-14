---
title: Base de données des leads
feature: REST API, Database
description: Guide des API de base de données de lead Marketo couvrant les objets, CRUD et décrire les méthodes, les modèles de requête, les limites de lot et les restrictions d’intégration dans CRM.
exl-id: e62e381f-916b-4d56-bc3d-0046219b68d3
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1357'
ht-degree: 1%

---

# Base de données des leads

Les API de la base de données des prospects Marketo sont les API les plus fréquemment utilisées par Marketo. Elles permettent en effet l’échange de données relatives aux personnes à partir de Marketo, notamment les activités, les opportunités et les entreprises.

## Objets

Les objets de la base de données de lead sont les suivants :

- Prospects
- Sociétés/comptes
- Comptes désignés
- Opportunités
- OpportunityRoles
- SalesPersons
- Objets personnalisés
- Activités
- Liste et appartenance à un programme

La plupart de ces objets incluent au moins les méthodes Create, Read, Update et Delete. Elle comprend également une méthode de description qui fournit la liste des champs disponibles pour chaque type, ainsi qu&#39;une liste des champs utilisés pour la déduplication (pour les objets autres que les objets de lead) et des champs dans lesquels des recherches peuvent être effectuées pour récupérer des enregistrements. L’ensemble le plus riche est fourni pour les prospects, car ils disposent de la plus grande variété de fonctionnalités dans les applications Marketo.

## API

Pour obtenir la liste complète des points d’entrée de l’API de base de données de leads, y compris les paramètres et les informations de modélisation, consultez la [&#x200B; Référence des points d’entrée de l’API de base de données de leads &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/).

Pour les instances dont l’intégration CRM native est activée (Microsoft Dynamics ou Salesforce.com), les API Société, Opportunité, Rôle de l’opportunité et Commercial sont désactivées. Lorsqu’ils sont activés, les enregistrements sont gérés via le CRM et ne sont pas accessibles ni mis à jour via les API Marketo.

- Taille de lot maximale (standard) : 300 enregistrements
- Taille de lot maximale (en masse) : fichier de 10 Mo
- Taille de lot par défaut : 300 enregistrements
- En-tête de type de contenu (standard) : application/json
- En-tête de type contenu (en bloc) : multipart/form-data

## Décrire

Pour les prospects, les entreprises, les opportunités, les rôles, les commerciaux et les objets personnalisés, une description de l’API est fournie. L’appel à cette méthode récupère les métadonnées de l’objet et une liste des champs disponibles pour la mise à jour et l’interrogation. La description est une partie essentielle de la conception d’une intégration appropriée à Marketo. Il fournit des métadonnées riches sur la manière dont les objets peuvent et ne peuvent pas être interagissent, ainsi que sur la manière dont ils peuvent être créés, mis à jour et interrogés. Outre les leads de description, chacun d’eux renvoie une liste de clés disponibles pour `deduplication` dans le paramètre de réponse `dedupeFields`. Une liste de champs est disponible en tant que clés pour l’interrogation dans le paramètre de réponse `searchableFields`.

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

Dans cet exemple, `dedupeFields` est en fait une clé composée. Cela signifie que dans les futures mises à jour et créations, lors de l’utilisation du mode `dedupeFields` , vous devez inclure les trois `externalOpportunityId`, `leadId` et `role` pour chaque rôle. Le tableau `searchableFields` fournit également la liste des champs disponibles pour l’interrogation des enregistrements de rôle. Cela inclut également la clé composite `externalOpportunityId`, `leadId` et `role`.

Il existe également un paramètre de réponse aux champs qui fournit le nom de chaque champ, la `displayName` telle qu’elle apparaît dans l’interface utilisateur de Marketo, le type de données du champ, s’il peut être mis à jour après la création et la longueur du champ, le cas échéant.

## Requête

Les objets de base de données de lead partagent tous le même modèle de base pour interroger des clés simples, où un seul champ est référencé.

```
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Pour tous les objets, à l’exception des prospects, vous pouvez sélectionner votre {field to query} dans les searchableFields de l’appel de description correspondant et composer une liste séparée par des virgules contenant jusqu’à 300 valeurs. Il existe également ces paramètres de requête facultatifs :

- `batchSize` - Nombre entier du nombre de résultats à renvoyer. La valeur par défaut et la valeur maximale sont 300.
- `nextPageToken` - Jeton renvoyé par un appel précédent pour la pagination. Voir [Jetons de pagination](paging-tokens.md) pour plus d’informations.
- `fields` - Liste de noms de champs séparés par des virgules à renvoyer pour chaque enregistrement. Voir la description correspondante pour obtenir la liste des champs valides. Si un champ particulier est demandé, mais n’est pas renvoyé, la valeur est implicitement nulle.
- `_method` - Utilisé pour envoyer des requêtes à l’aide de la méthode HTTP POST. Pour plus d&#39;informations, consultez la section _method=GET ci-dessous.

Pour un exemple rapide, examinons les opportunités d’interrogation :

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

Le `filterType` spécifié dans cet appel est « idField » et non « marketoGUID ». Ces champs et « dedupeFields » sont tous deux des cas spéciaux, où le champ correspondant à idField ou dedupeFields peut être alias de cette manière. Le « marketoGUID » est toujours le champ de recherche obtenu dans l’appel, mais il n’est pas explicitement défini dans l’appel. Les champs et/ou ensembles de champs indiqués par le `idField` et le `dedupeFields` d&#39;une description d&#39;objet seront toujours valides `filterTypes` une requête. Cet appel recherche les enregistrements correspondant aux GUID inclus dans filterValues et renvoie tous les enregistrements correspondants. Si aucun enregistrement n’est trouvé à l’aide de cette méthode, la réponse indique toujours la réussite, mais le tableau de résultats sera vide, car la recherche a été exécutée avec succès, mais il n’y avait aucun enregistrement à renvoyer.

Si le jeu d&#39;enregistrements dans la requête dépasse 300 ou la `batchSize` spécifiée, la plus petite de ces valeurs étant retenue, la réponse comporte un `moreResult` de membres avec une valeur true et une `nextPageToken`, qui peuvent être inclus dans un appel suivant pour récupérer davantage de jeux. Voir [Jetons de pagination](paging-tokens.md) pour plus d’informations.

### URI longs

Parfois, par exemple lors de l’interrogation par des GUID, votre URI peut être long et dépasser les 8 Ko autorisés par le service REST. Dans ce cas, vous devez utiliser la méthode HTTP POST au lieu de GET et ajouter un paramètre de requête `_method=GET`. En outre, le reste des paramètres de requête doit être transmis dans le corps POST sous la forme d’une chaîne « application/x-www-form-urlencoded » et transmettez l’en-tête Content-Type associé.

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

### Clés composites

Le modèle d’interrogation des clés composites est différent des clés simples, dans la mesure où il nécessite l’envoi d’un POST avec un corps JSON. Cela n’est pas nécessaire dans tous les cas, uniquement dans ceux où une option `dedupeFields` avec plusieurs champs est utilisée comme `filterType`. Actuellement, les clés composées ne sont utilisées que par les rôles d’opportunité, ainsi que par certains objets personnalisés. Prenons un exemple de requête pour les rôles d’opportunité avec la clé composée de `dedupeFields` :

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

La structure de l’objet JSON est principalement plate, et tous les paramètres de requête pour les requêtes avec des clés simples sont des membres valides, à l’exception de `filterValues`. Au lieu d’une valeur de filtre, il existe un tableau « d’entrée » d’objets JSON, qui doivent chacun avoir un membre pour chacun des champs de votre clé composée ; dans ce cas, ils sont `externalOpportunityId`, `leadId` et `role`. Cette opération exécute une requête de `roles` sur les entrées fournies et renvoie les résultats correspondants. Si la réponse renvoie un paramètre avec `moreResult=true` et une `nextPageToken`, vous devez inclure toutes les entrées d’origine et la `nextPageToken` pour que la requête s’exécute correctement.

## Créer et mettre à jour

Les créations et les mises à jour des enregistrements de la base de données de prospect sont toutes effectuées par le biais de POST avec des corps JSON. Les interfaces pour Opportunités, Rôles, Objets personnalisés, Entreprises et Vendeurs sont toutes identiques. L’interface du prospect est un peu différente, et vous pouvez en savoir plus à ce sujet.

Le seul paramètre obligatoire est un tableau appelé `input` contenant jusqu’à 300 objets, chacun avec les champs que vous souhaitez insérer/mettre à jour en tant que membres. Vous pouvez également inclure de manière facultative un paramètre `action` qui peut être l’un des suivants : `createOnly`, `updateOnly` ou `createOrUpdate`. Si l’action est omise, le mode par défaut est `createOrUpdate`. `dedupeBy` est un autre paramètre facultatif qui peut être utilisé lorsque l’action est définie sur createOnly ou `createOrUpdate`. `dedupeBy` peut être `idField` ou `dedupeFields`. Si `idField` est sélectionné, les `idField` répertoriées dans la description sont utilisées à des fins de déduplication et doivent être incluses dans chaque enregistrement. Le mode `idField` n’est pas compatible avec le mode `createOnly`. Si `dedupeFields` sont sélectionnés , les `dedupeFields` répertoriés dans la description de l’objet utilisé et chacun d’eux doit être inclus dans chaque enregistrement. Si le paramètre `dedupeBy` est omis, le mode est défini par défaut sur `dedupeFields`.

Lors de la transmission d’une liste de valeurs de champ, une valeur de `null`, ou une chaîne vide, est écrite dans la base de données comme `null`.

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

Outre l’API de leads, les appels pour créer ou mettre à jour des objets de base de données de leads renvoient un champ `seq` dans chaque objet du tableau de `result`. Le nombre listé correspond à l&#39;ordre de l&#39;enregistrement mis à jour dans la requête effectuée. Chaque élément renvoie la valeur du `idField` pour le type d’objet et un `status`. Le champ de statut indique s’il s’agit d’un statut « créé », « mis à jour » ou « ignoré ».  Si l’état est ignoré, il y aura également un tableau « raisons » correspondant avec un ou plusieurs objets raison qui incluent un code et un message, indiquant pourquoi un enregistrement a été ignoré. Voir [codes d’erreur](error-codes.md) pour plus d’informations.

### Supprimer

L’interface pour les suppressions est standard pour les objets de base de données de leads autres que les leads. Outre l’entrée, il n’existe qu’un seul paramètre obligatoire `deleteBy,` qui peut avoir la valeur idField ou dedupeFields. Examinons la possibilité de supprimer certains objets personnalisés.

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

Les `seq`, `status`, `marketoGUID` et `reasons` devraient tous vous être déjà familiers.

Pour plus d’informations sur l’utilisation des opérations CRUD pour chaque type d’objet individuel, consultez leurs pages respectives.
