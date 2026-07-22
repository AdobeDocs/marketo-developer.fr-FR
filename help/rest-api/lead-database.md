---
title: Base de données des leads
feature: REST API, Database
description: Guide des API de base de données de lead Marketo couvrant les objets, CRUD et décrire les méthodes, les modèles de requête, les limites de lot et les restrictions d’intégration dans CRM.
exl-id: e62e381f-916b-4d56-bc3d-0046219b68d3
TQID: https://experienceleague.adobe.com/7lGbhE92lvIE-XkMyUIaK9GrreZVRdM-WVZTpHARhxE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1058
ht-degree: 1%

---

# Base de données des leads

Les API de la base de données des prospects Marketo échangent des données personnelles et des données relatives aux personnes avec Marketo. Ces données incluent les activités, les opportunités et les entreprises.

## Objets

La base de données de leads comprend les objets suivants :

- Prospects
- Sociétés/comptes
- Comptes désignés
- Opportunités
- OpportunityRoles
- SalesPersons
- Objets personnalisés
- Activités
- Liste et appartenance à un programme

La plupart des objets de base de données de leads prennent en charge les méthodes Create, Read, Update et Delete. La méthode Describe fournit les champs disponibles pour chaque type d’objet. Pour les objets autres que les leads, il identifie également les champs utilisés pour la déduplication et les champs qui peuvent faire l’objet de recherches lors de la récupération d’enregistrements.

Les objets de lead prennent en charge le plus grand nombre de fonctionnalités possible, car ils ont le plus grand nombre d’utilisations dans les applications Marketo.

## API

Pour obtenir une liste complète des points d’entrée, paramètres et informations de modélisation de l’API de base de données de lead, consultez la [Référence des points d’entrée de l’API de base de données de lead](https://developer.adobe.com/marketo-apis/api/mapi).

Lorsqu’une instance dispose d’une intégration native de Microsoft Dynamics ou de CRM Salesforce.com, les API Société, Opportunité, Rôle d’opportunité et Commercial sont désactivées. Le CRM gère ces enregistrements. Vous ne pouvez donc pas y accéder ni les mettre à jour via les API Marketo.

- Taille de lot maximale (standard) : 300 enregistrements
- Taille de lot maximale (en masse) : fichier de 10 Mo
- Taille de lot par défaut : 300 enregistrements
- En-tête de type de contenu (standard) : application/json
- En-tête de type contenu (en bloc) : multipart/form-data

## Décrire

L’API Describe est disponible pour les prospects, les entreprises, les opportunités, les rôles, les commerciaux et les objets personnalisés. Utilisez-la pour récupérer les métadonnées de l’objet et les champs disponibles pour les mises à jour et les requêtes.

À l’exception des leads Describe, chaque point d’entrée Describe renvoie :

- `dedupeFields` : clés disponibles pour la déduplication.
- `searchableFields` : clés disponibles pour les requêtes.

```http
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

Dans cet exemple, `dedupeFields` est une clé composée. Lorsque vous utilisez `dedupeFields` mode pour les futures créations et mises à jour, incluez `externalOpportunityId`, `leadId` et `role` pour chaque rôle.

Le tableau `searchableFields` répertorie les champs disponibles pour interroger les enregistrements de rôle. Cette liste comprend la clé composée de `externalOpportunityId`, `leadId` et `role`.

Le paramètre de réponse `fields` fournit les informations suivantes pour chaque champ :

- Nom.
- `displayName` comme indiqué dans l’interface utilisateur de Marketo.
- Type de données.
- Indique si le champ peut être mis à jour après la création.
- Longueur du champ, le cas échéant.

## Requête

Les objets Base de données de lead partagent un modèle de requête de base pour les clés simples qui font référence à un champ.

```http
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Pour tous les objets, à l’exception des prospects, sélectionnez `{field to query}` dans `searchableFields` dans la réponse Describe correspondante. Fournissez une liste séparée par des virgules contenant jusqu’à 300 valeurs.

Vous pouvez également inclure les paramètres de requête facultatifs suivants :

- `batchSize` : un entier qui spécifie le nombre de résultats à renvoyer. La valeur par défaut et la valeur maximale sont 300.
- `nextPageToken` : jeton renvoyé par un appel précédent pour la pagination. Voir [Jetons de pagination](paging-tokens.md) pour plus d’informations.
- `fields` : liste séparée par des virgules de noms de champ à renvoyer pour chaque enregistrement. Voir la description correspondante pour les champs valides. Si vous demandez un champ qui n’est pas renvoyé, sa valeur est implicitement nulle.
- `_method` : envoie les requêtes à l’aide de la méthode HTTP POST. Voir la section _method=GET pour plus d&#39;informations.

L’exemple suivant interroge les opportunités :

```http
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

Le `filterType` de cet appel est « idField » et non « marketoGUID ». « idField » et « dedupeFields » sont des cas spéciaux qui vous permettent d’utiliser un alias pour le ou les champs correspondants. Bien que l’appel ne définisse pas explicitement « marketoGUID », il reste le champ de recherche.

Les champs ou ensembles de champs identifiés par `idField` et `dedupeFields` dans une description d’objet sont toujours valides `filterTypes` une requête. Cet appel renvoie des enregistrements qui correspondent aux GUID dans filterValues. Si aucun enregistrement ne correspond, la réponse indique la réussite et renvoie un tableau de résultats vide.

Si le jeu d&#39;enregistrements correspondant dépasse 300 ou la `batchSize` spécifiée, la valeur la plus petite étant retenue, la réponse inclut `moreResult` avec une valeur true et une `nextPageToken`. Incluez le jeton dans un appel suivant pour récupérer d’autres enregistrements. Voir [Jetons de pagination](paging-tokens.md) pour plus d’informations.

### URI longs

Un URI peut dépasser la limite de 8 Ko du service REST, par exemple lorsque vous effectuez une requête à l’aide de GUID. Dans ce cas, utilisez la méthode HTTP POST au lieu de GET et ajoutez le paramètre de requête `_method=GET`.

Transmettez les paramètres de requête restants dans le corps POST sous la forme d’une chaîne « application/x-www-form-urlencoded ». Transmettez également l’en-tête Type de contenu associé.

```http
POST /rest/v1/opportunities.json?_method=GET
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb,544fb7f5-2ddf-4fca-ae32-7e6ef1415e9f,f1ba41a2-69d1-4a35-9807-0e159d66f2c9,f7521272-3331-4a89-a768-222baff2f894
```

Le paramètre `_method=GET` est également requis lors de l’interrogation de clés composites.

### Clés composites

Pour interroger une clé composite, envoyez une requête POST avec un corps JSON. N’utilisez ce modèle que lorsque la `filterType` est une option `dedupeFields` comportant plusieurs champs.

Actuellement, les clés composées ne sont utilisées que par les rôles d’opportunité et certains objets personnalisés. L’exemple suivant interroge les rôles d’opportunité avec la clé composée de `dedupeFields` :

```http
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

L’objet JSON accepte tous les paramètres de requête utilisés pour les requêtes à clé simple, à l’exception de `filterValues`. Au lieu de `filterValues`, fournissez un tableau « d’entrée » d’objets JSON. Chaque objet doit inclure chaque champ de la clé composée. Dans cet exemple, les champs sont `externalOpportunityId`, `leadId` et `role`.

La requête `roles` les entrées fournies et renvoie les résultats correspondants. Si la réponse inclut `moreResult=true` et un `nextPageToken`, incluez toutes les entrées d’origine et le `nextPageToken` dans la requête suivante.

## Créer et mettre à jour

Créez et mettez à jour des enregistrements de base de données de leads en envoyant des requêtes POST avec des corps JSON. Les opportunités, les rôles, les objets personnalisés, les entreprises et les commerciaux utilisent la même interface. Les prospects utilisent une autre interface, qui est décrite dans la documentation des prospects.

Le seul paramètre requis est `input`, un tableau contenant jusqu’à 300 objets. Chaque objet contient les champs à insérer ou à mettre à jour.

Vous pouvez également inclure les paramètres facultatifs suivants :

- `action` : accepte les `createOnly`, les `updateOnly` ou les `createOrUpdate`. S’il est omis, le mode est défini par défaut sur `createOrUpdate`.
- `dedupeBy` : accepte `idField` ou `dedupeFields` lorsque l’action est définie sur createOnly ou `createOrUpdate`. S’il est omis, le mode est défini par défaut sur `dedupeFields`.

Lorsque `dedupeBy` est `idField`, les `idField` répertoriées dans la description sont utilisées à des fins de déduplication et doivent être incluses dans chaque enregistrement. Le mode `idField` n’est pas compatible avec le mode `createOnly`.

Lorsque `dedupeBy` est `dedupeFields`, incluez chaque champ `dedupeFields` répertorié dans la description de l’objet dans chaque enregistrement.

Lorsque vous transmettez des valeurs de champ, la base de données écrit une valeur de `null` ou une chaîne vide comme `null`.

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

À l’exception de l’API Leads, les appels de création et de mise à jour renvoient un champ `seq` dans chaque objet du tableau `result`. Le nombre correspond à la position de l’enregistrement mis à jour dans la requête.

Chaque résultat renvoie également la valeur de `idField` du type d’objet et une `status` de « créé », « mis à jour » ou « ignoré ». Si le statut est ignoré, le résultat inclut un tableau « raisons ». Chaque objet Raison contient un code et un message expliquant pourquoi l’enregistrement a été ignoré. Voir [codes d’erreur](error-codes.md) pour plus d’informations.

### Supprimer

À l’exception des prospects, les objets de la base de données des leads utilisent une interface de suppression standard. En plus de l’entrée, le seul paramètre requis est `deleteBy,` qui accepte idField ou dedupeFields.

L’exemple suivant supprime des objets personnalisés :

```http
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

La réponse inclut `seq`, `status` et `marketoGUID`. Pour les enregistrements ignorés, cela inclut également les `reasons`.

Pour plus d’informations sur les opérations CRUD pour un type d’objet spécifique, consultez la documentation relative à cet objet.
