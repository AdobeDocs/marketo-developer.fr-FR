---
title: Activités
feature: REST API
description: Une API pour la gestion des activités Marketo Engage.
exl-id: 1e69af23-2b0c-467a-897c-1dcf81343e73
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '2029'
ht-degree: 0%

---

# Activités

Marketo permet une grande variété de types d’activités liés aux enregistrements de prospect.  Presque chaque modification, action ou étape de flux est enregistrée par rapport au journal d’activité d’un prospect et peut être récupérée via l’API ou exploitée dans les filtres et triggers de liste dynamique et de campagne intelligente.  Les activités sont toujours liées à l’enregistrement du prospect via l’ID de prospect, correspondant au champ d’ID de l’enregistrement, et possède également un ID unique qui lui est propre.

Il existe un très grand nombre de types d’activité potentiels, qui peuvent varier d’un abonnement à l’autre et avoir des définitions uniques pour chacun. Bien que chaque activité ait ses propres `id`, `leadId` et `activityDate`, les valeurs `primaryAttributeValueId` et `primaryAttributeValue` varient dans leur signification.

Marketo permet également la création de types d’activités personnalisés par le biais de l’API de métadonnées des activités personnalisées. L’ajout d’activités personnalisées est effectué via l’API Ajouter des activités personnalisées.

La plupart des activités seront purgées après un certain temps.

## Décrire

Pour récupérer une liste des types disponibles et leurs définitions pour une instance, vous pouvez utiliser le point d’entrée [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET).

```
GET /rest/v1/activities/types.json
```

```json
  "requestId": "6e78#148ad3b76f1",
  "success": true,
  "result": [
    {
      "id": 2,
      "name": "Fill Out Form",
      "description": "User fills out and submits form on web page",
      "primaryAttribute": {
        "name": "Webform ID",
        "dataType": "integer"
      },
      "attributes": [
        {
          "name": "Client IP Address",
          "dataType": "string"
        },
        {
          "name": "Form Fields",
          "dataType": "text"
        },
        {
          "name": "Query Parameters",
          "dataType": "string"
        },
        {
          "name": "Referrer URL",
          "dataType": "string"
        },
        {
          "name": "User Agent",
          "dataType": "string"
        },
        {
          "name": "Webpage ID",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

Les réponses du monde réel comprennent beaucoup plus de définitions. Dans cet exemple, le type affiché est un « Remplir le formulaire », qui possède un attribut principal « Identifiant du formulaire web », qui fait référence à l’identifiant Marketo du formulaire rempli et peut être utilisé pour établir une relation avec cette ressource particulière dans Marketo. En outre, il existe des définitions pour chacun des attributs possibles d’un enregistrement d’activité particulier de ce type et leurs types de données. Notez que si le champ est vide, cet attribut particulier est omis d’un enregistrement d’activité individuel.

## Requête

Pour récupérer les activités à partir de Marketo, appelez le point d’entrée [Obtenir les activités de lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET). Vous devez d’abord récupérer un jeton de pagination pour la date et l’heure à partir desquelles vous souhaitez commencer à récupérer les activités. Vous transmettez ensuite le jeton de pagination dans le paramètre de requête `nextPageToken`. En outre, vous transmettez jusqu’à dix identifiants de type d’activité dans le paramètre de requête `activityTypeIds` sous la forme d’une liste séparée par des virgules.

Vous pouvez éventuellement inclure un paramètre de requête listId pour limiter votre recherche aux seuls enregistrements inclus dans une liste statique spécifique ou un paramètre de requête leadIds et rechercher des activités à partir d’un ensemble de prospects spécifié uniquement. Vous pouvez transmettre jusqu’à 30 leadId sous forme de liste séparée par des virgules.

```
GET /rest/v1/activities.json?activityTypeIds=1&nextPageToken=WQV2VQVPPCKHC6AQYVK7JDSA3I3LCWXH3Y6IIZ7YSGQLXHCPVE5Q====
```

```json
{
  "requestId": "24fd#15188a88d7f",
  "result": [
    {
      "id": 102988,
      "marketoGUID": "102988",
      "leadId": 1,
      "activityDate": "2023-01-16T23:32:19Z",
      "activityTypeId": 1,
      "primaryAttributeValueId": 71,
      "primaryAttributeValue": "localhost/munchkintest2.html",
      "attributes": [
        {
          "name": "Client IP Address",
          "value": "10.0.19.252"
        },
        {
          "name": "Query Parameters",
          "value": ""
        },
        {
          "name": "Referrer URL",
          "value": ""
        },
        {
          "name": "User Agent",
          "value": "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36"
        },
        {
          "name": "Webpage URL",
          "value": "/munchkintest2.html"
        }
      ]
    }
  ],
  "success": true,
  "nextPageToken": "WQV2VQVPPCKHC6AQYVK7JDSA3J62DUSJ3EXJGDPTKPEBFW3SAVUA====",
  "moreResult": false
}
```

Pour le premier appel, utilisez l’API Get Paging Token pour obtenir des `nextPageToken`. Pour les appels suivants vers ce point d’entrée, utilisez le `nextPageToken returned` de la réponse . Ce point d’entrée renvoie toujours `the nextPageToken`.

Si l’attribut `moreResult` est défini sur « true », cela signifie que d’autres résultats sont disponibles. Continuez à appeler ce point d’entrée jusqu’à ce que l’attribut `moreResult` renvoie false, ce qui signifie qu’aucun résultat n’est disponible. Les `nextPageToken` renvoyés par cette API doivent toujours être réutilisés pour l’itération suivante de cet appel.

Dans certains cas, cette API peut répondre avec moins de 300 éléments d’activité, mais son attribut `moreResult` est également défini sur true.  Cela indique que d’autres activités peuvent être renvoyées et que le point d’entrée peut être interrogé pour des activités plus récentes en incluant les `nextPageToken` renvoyées dans un appel suivant.

Notez que dans chaque élément du tableau de résultats, l’attribut entier `id` est remplacé par l’attribut de chaîne `marketoGUID` en tant qu’identifiant unique. 

### Modifications de la valeur des données

Pour les activités Changement de valeur des données , une version spécialisée de l’API d’activités est fournie. Le point d’entrée [Obtenir les modifications de lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) renvoie uniquement les activités des enregistrements Modification de la valeur des données aux champs de lead. L’interface est identique à l’API Get Lead Activities avec deux différences :

* Il n’existe aucun paramètre `activityTypeIds`, car le point d’entrée renvoie uniquement les activités Modification de la valeur des données et Nouveau prospect.
* Le paramètre de requête `fields` est obligatoire, où vous pouvez transmettre une liste de champs séparés par des virgules pour indiquer les champs pour lesquels vous souhaitez récupérer les modifications.

```
GET /rest/v1/activities/leadchanges.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&fields=firstName,lastName,department
```

```json
{
  "requestId": "a9ae#148add1e53d",
  "success": true,
  "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBRGA3TQ===",
  "moreResult": true,
  "result": [
    {
      "id": 1078,
      "marketoGUID": "1078",
      "leadId": 775,
      "activityDate": "2014-09-17T22:31:49+0000",
      "activityTypeId": 13,
      "fields": [
        {
          "id": 48,
          "name": "firstName",
          "newValue": "FirstName_6176",
          "oldValue": "FirstName_4914"
        }
      ],
      "attributes": [
        {
          "name": "Reason",
          "value": "Web service API"
        },
        {
          "name": "Source",
          "value": "Web service API"
        },
        {
          "name": "Lead ID",
          "value": 775
        }
      ]
    }
  ]
}
```

Chaque activité de la réponse comporte un tableau de champs , y compris une liste des modifications apportées à l’activité, qui spécifie la `id` et la `name` du champ modifié, ainsi que les nouvelles et anciennes valeurs relatives à la modification.

Notez que dans chaque élément du tableau de résultats, l’attribut entier `id` est remplacé par l’attribut de chaîne `marketoGUID` en tant qu’identifiant unique.

### Leads supprimés

Il existe également un point d’entrée spécial [Obtenir les leads supprimés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) pour récupérer les activités supprimées de Marketo.

```
GET /rest/v1/activities/deletedleads.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ
```

```json
{
  "requestId": "a9ae#148add1e53d",
  "success": true,
  "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBRGA3TQ===",
  "moreResult": true,
  "result": [
    {
      "id": 2,
      "marketoGUID": "2",
      "leadId": 6,
      "activityDate": "2013-09-26T06:56:35+0000",
      "activityTypeId": 37,
      "primaryAttributeValueId": 6,
      "primaryAttributeValue": "Owyliphys Iledil",
      "attributes": []
    },
    {
      "id": 3,
      "marketoGUID": "3",
      "leadId": 9,
      "activityDate": "2013-12-28T00:39:45+0000",
      "activityTypeId": 37,
      "primaryAttributeValueId": 4,
      "primaryAttributeValue": "First Last",
      "attributes": []
    }
  ]
}
```

Notez que dans chaque élément du tableau de résultats, l’attribut entier `id` est remplacé par l’attribut de chaîne `marketoGUID` en tant qu’identifiant unique.

### Résultats de navigation

Par défaut, les points d’entrée mentionnés dans cette section renvoient 300 éléments d’activité à la fois.  Si l’attribut `moreResult` est défini sur « true », d’autres résultats sont disponibles. Appelez le point d’entrée jusqu’à ce que l’attribut `moreResult` renvoie false, ce qui signifie qu’il n’y a plus de résultats disponibles. Les `nextPageToken` renvoyées à partir de ce point d’entrée doivent toujours être réutilisées pour l’itération suivante de cet appel.

Dans certains cas, ce point d’entrée peut répondre avec moins de 300 éléments d’activité, mais son attribut `moreResult` peut également être défini sur true.  Cela indique que d’autres activités peuvent être renvoyées et que le point d’entrée peut être interrogé pour des activités plus récentes en incluant les `nextPageToken` renvoyées dans un appel suivant. Notez que la `nextPageToken` doit être encodée en URL dans la requête.

## Types d’activités personnalisées

Les activités personnalisées fonctionnent comme les activités standard, à la différence que le schéma est géré par des tiers et non par Marketo. Les instances d’activités personnalisées sont liées aux enregistrements de prospect par le biais du `leadId` tout comme les activités standard, mais les attributs principaux et secondaires sont définis de manière arbitraire. Lorsqu’un type d’activité personnalisé est approuvé, un déclencheur et un filtre de liste dynamique correspondants sont créés, de sorte que les prospects puissent être traités en fonction des données d’activité personnalisée actuelles ou historiques.

* Nombre maximal d’activités personnalisées : 10
* Nombre maximal d’attributs par activité personnalisée : 20

La récupération des données d’activité personnalisées est effectuée de la même manière que les activités standard, via l’API [Obtenir les activités du prospect](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET).

## Types de requête

Outre le point d’entrée standard Get Activity Types, les points d’entrée [Get Custom Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getCustomActivityTypeUsingGET) et [Describe Custom Activity Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/describeCustomActivityTypeUsingGET) renvoient des détails sur les types d’activité configurés dans l’instance Marketo, ainsi que des métadonnées concernant les attributs d’un type donné. La méthode normale [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET) renvoie toujours des métadonnées concernant les activités personnalisées, mais n’indique pas si un type donné est personnalisé.

### Obtenir les types

```
GET /rest/v1/activities/external/types.json
```

```json
{
  "requestId": "185d6#14b51985ff0",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved"
    }
  ]
}
```

### Décrire les types

Pour les descriptions de type, vous devez transmettre `apiName` comme paramètre de chemin d’accès. Par défaut, vous obtenez la version approuvée de l’activité. Vous pouvez éventuellement transmettre le paramètre `draft=true` pour récupérer le brouillon de l’activité.

```
GET /rest/v1/activities/external/type/{apiName}/describe.json
```

```json
{
  "requestId": "185d6#14b51985ff0",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendees",
          "name": "Number of Attendees",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

## Type de création

Chaque type d’activité personnalisé nécessite un nom d’affichage, un nom d’API, un nom de déclencheur, un nom de filtre et un attribut principal.

Pour garantir la cohérence de vos types avec les conventions Marketo et éviter les collisions, il est important de suivre quelques instructions lors de la création de vos types :

**Nom d’affichage :** le nom d’affichage du type d’activité doit décrire brièvement ce que représente un enregistrement d’activité, par exemple « Envoyer un e-mail » ou « Modifier la valeur des données ». Ces noms doivent généralement se trouver dans la forme infinie, c’est-à-dire « Assister à l’événement ».  Les noms d’affichage acceptent les caractères alphanumériques, les espaces et les traits de soulignement. Les noms d’affichage doivent contenir au moins une lettre.

**Nom de l’API :** le nom de l’API est composé de caractères alphanumériques (longueur maximale de 255). Si vous êtes un partenaire LaunchPoint, vous devez ajouter un espace de noms représentatif aux noms des API de type d’activité. Cela permet d’éviter les conflits avec les types configurés par le client.  La convention consiste à utiliser uniquement des minuscules ou des majuscules pour faire la distinction entre d’autres chaînes de texte.

**Description :** pour les activités pouvant avoir un comportement non évident, doit inclure une description du type d’activité par rapport au prospect.

**Nom du déclencheur :** chaque type d’activité doit avoir un nom de déclencheur unique et lisible par l’utilisateur. Les noms des déclencheurs doivent être au présent de la troisième personne, par exemple « Participe à un événement ». Les partenaires LaunchPoint doivent inclure le nom de leur société dans l’activité, par exemple « Participe au webinaire - Société Acme ».

**Nom du filtre :**  Chaque type d’activité doit avoir un nom de filtre unique et lisible par l’utilisateur. Les noms des filtres doivent être au passé de la troisième personne, par exemple « A assisté à un événement ». Les partenaires LaunchPoint doivent inclure le nom de leur société dans l’activité, à savoir « Webinaire suivi - Société Acme ».

**Attribut de Principal :** l’attribut principal d’une activité personnalisée doit être le champ le plus significatif pour le type d’activité. Par exemple, pour une activité « Événement auquel vous avez participé », il s’agit du nom de l’événement. Les attributs de Principal sont inclus par défaut en tant que paramètres dans chaque instance d’un déclencheur ou d’un filtre pour ce type d’activité, et la valeur s’affiche dans le journal d’activité d’un enregistrement de personne sans qu’il soit nécessaire d’effectuer une analyse approfondie de l’activité.

Lorsqu’une activité personnalisée est créée, elle est créée en tant que brouillon et doit être approuvée avant de pouvoir être utilisée pour ajouter des enregistrements d’activité de ce type. Toutes les mises à jour sont implicitement appliquées au brouillon du type . Pour refléter les modifications apportées à la version active du type, elle doit être approuvée. Lorsqu’un type d’activité personnalisé est approuvé et utilisé, aucune modification ne peut être apportée aux champs ci-dessus.

Lors de la création d&#39;un type, le paramètre de description est optionnel, alors que tous les paramètres suivants sont requis : `apiName`, `name`, `triggerName`, `filterName`, `primaryAttribute`.

```
POST /rest/v1/activities/external/type.json
```

```json
{
  "apiName": "attendConference",
  "name": "Attend Conference",
  "description": "Attend the conference",
  "triggerName": "Attends Conference",
  "filterName": "Attended Conference",
  "primaryAttribute": {
    "apiName": "conferenceName",
    "name": "Conference Name",
    "description": "Name of the conference"
  }
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "status": "draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Type de mise à jour

La mise à jour d’un type est très similaire, sauf que l’apiName est le seul paramètre obligatoire en tant que paramètre de chemin d’accès.

```
POST /rest/v1/activities/external/type/{apiName}.json
```

```json
{
  "name": "Attend Conference",
  "description": "Attend the conference",
  "triggerName": "Attend Conference",
  "filterName": "Attended Conference",
  "primaryAttribute": {
    "apiName": "conferenceName",
    "name": "Conference Name",
    "description": "Name of the conference"
  }
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "status": "draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Approuver le type

Les types peuvent être gérés avec le type d’activité personnalisé Approuver , Ignorer le brouillon de type d’activité personnalisé et Supprimer le type d’activité personnalisé , tout comme les ressources Marketo standard.


## Attributs de type d’activité personnalisés

Chaque type d’activité personnalisé peut avoir de 0 à 20 attributs secondaires. Les attributs Secondaire peuvent avoir n’importe quel type de champ valide pour un champ Marketo. Ils sont ajoutés, mis à jour et supprimés séparément du type parent, mais peuvent être modifiés lorsqu’un type d’activité est en cours d’utilisation, puis approuvé. Lorsque des champs sont modifiés sur un type actif, le nouvel attribut secondaire est défini pour toutes les activités de ce type créées après approbation. Les modifications ne seront pas appliquées rétroactivement aux activités existantes partageant ce type.

Soyez prudent lors de la suppression des attributs, car cela affectera leur disponibilité pour une utilisation dans les filtres correspondants.

Les mises à jour apportées à la liste des attributs secondaires utilisent le nom d’API de chaque attribut comme clé primaire. Le nom d’API d’un attribut ne peut pas être modifié, il doit être supprimé et ajouté à nouveau avec le nom d’API souhaité.

Les types de données valides pour les attributs sont les suivants : chaîne, booléen, entier, flottant, lien, e-mail, devise, date, date et heure, téléphone, texte.

Lors de la modification de l’attribut principal d’un type d’activité, tout attribut principal existant doit d’abord être rétrogradé en définissant `isPrimary` sur false.

### Créer des attributs

La création d’un attribut nécessite un paramètre de chemin d’accès `apiName` obligatoire. Les paramètres `name` et `dataType` sont également requis.` The description and` paramètres `isPrimary` sont facultatifs.

```
POST /rest/v1/activities/external/type/{apiName}/attributes/create.json
```

```json
{
  "attributes": [
    {
      "apiName": "conferenceDate",
      "name": "Conference Date",
      "description": "Date of the conference",
      "dataType": "datetime"
    },
    {
      "apiName": "numberOfAttendees",
      "name": "Number of Attendees",
      "description": "Number of people attending conference",
      "dataType": "integer"
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendees",
          "name": "Number of Attendees",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

### Mettre à jour les attributs

Lors de la mise à jour des attributs, la `apiName` de l’attribut est la clé primaire. Le paramètre `apiName` doit exister pour que la mise à jour réussisse (en d’autres termes, vous ne pouvez pas modifier le paramètre `apiName` à l’aide de la mise à jour).

```
POST /rest/v1/activities/external/type/{apiName}/attributes/update.json
```

```json
{
  "attributes": [
    {
      "apiName": "conferenceDate",
      "name": "Conference Date",
      "description": "Date of the conference",
      "dataType": "datetime"
    },
    {
      "apiName": "numberOfAttendee",
      "name": "Number of Attendee",
      "description": "Number of people attending conference",
      "dataType": "integer"
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendee",
          "name": "Number of Attendee",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

### Supprimer les attributs

La suppression d’un attribut utilise un paramètre de chemin d’accès `apiName` obligatoire qui est le nom de l’API d’activité personnalisée.  Un paramètre d’attribut qui est un tableau d’objets d’attribut est également requis.  Chaque objet doit contenir un paramètre `apiName` qui est le nom de l’API de type d’activité personnalisée.

```
POST /rest/v1/activities/external/type/{apiName}/attributes/delete.json
```

```json
{ "attributes":[ { "apiName":"conferenceDate" }, { "apiName":"numberOfAttendees" } ] }
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Ajouter des activités personnalisées

Les activités personnalisées sont des enregistrements en écriture unique d’activités historiques liées aux enregistrements de personnes individuelles dans Marketo. Ces activités possèdent un schéma géré par les administrateurs Marketo ou à distance au moyen d’une intégration d’API. Les activités personnalisées sont ajoutées aux enregistrements de prospect via le point d’entrée [Ajouter des activités personnalisées](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/addCustomActivityUsingPOST) et associées à chaque enregistrement de prospect via son champ `leadId`. Les activités personnalisées peuvent être affichées dans l’interface utilisateur via le journal d’activité du prospect ou récupérées via le point d’entrée Get Lead Activities en spécifiant l’identifiant de type de l’activité personnalisée.

Les activités personnalisées sont appropriées pour l’enregistrement de données liées à un enregistrement de personne unique et qui n’ont pas besoin d’être mises à jour ou remplacées. Par exemple, l’enregistrement d’une personne assistant à un événement en tant qu’activité « Événement auquel vous avez participé ». Pour les enregistrements liés à une personne qui peuvent changer, comme l’inscription des étudiants, il convient plutôt d’utiliser des objets personnalisés, car ils peuvent être mis à jour, contrairement aux activités personnalisées.

Le membre d’entrée est un tableau d’objets d’activité. 300 enregistrements d’activité au maximum peuvent être envoyés à la fois.

Les membres `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue` et attributs sont requis. Le tableau d’attributs doit contenir l’attribut non principal. Elle peut être spécifiée à l’aide du nom (nom du champ) ou de l’apiName (nom de l’API), et d’une valeur correspondant à la valeur que vous définissez.

```
POST /rest/v1/activities/external.json
```

```json
{
  "input": [
    {
      "leadId": 1001,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Game Giveaway",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    },
    {
      "leadId": 1200,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Game Giveaway",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    },
    {
      "leadId": 3000,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Contest Form",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 50,
      "marketoGUID": "50",
      "status": "added"
    },
    {
      "id": 51,
      "marketoGUID": "51",
      "status": "added"
    },
    {
      "status": "skipped",
      "errors": [
        {
          "code": "1004",
          "message": "Lead not found"
        }
      ]
    }
  ]
}
```

## Délais dépassés

Les points d’entrée des activités ont un délai d’expiration de 30 s, sauf indication ci-dessous.

* Obtenir le jeton de pagination : 300s 
* Ajouter une activité personnalisée : 90s
