---
title: Activités
feature: REST API
description: Une API pour la gestion des activités de Marketo Engage.
exl-id: 1e69af23-2b0c-467a-897c-1dcf81343e73
source-git-commit: 6baf62bc8881470eca597899e3228c377fb597d0
workflow-type: tm+mt
source-wordcount: '2029'
ht-degree: 0%

---

# Activités

Marketo permet un large éventail d’activités liées aux enregistrements de piste.  Presque chaque modification, action ou étape de flux est enregistrée par rapport au journal d’activité d’une piste et peut être récupérée via l’API ou exploitée dans les filtres et déclencheurs de liste dynamique et de campagne dynamique.  Les activités sont toujours liées à l’enregistrement de piste via le leadId, qui correspond au champ Identifiant de l’enregistrement, et a également un identifiant unique qui lui est propre.

Il existe un très grand nombre de types d’activité potentiels, qui peuvent varier d’un abonnement à l’autre et avoir des définitions uniques pour chacun d’eux. Bien que chaque activité ait ses propres `id`, `leadId` et `activityDate` uniques, les valeurs `primaryAttributeValueId` et `primaryAttributeValue` varient dans leur signification.

Marketo permet également la création de types d’activité personnalisés par le biais de l’API de métadonnées des activités personnalisées. L’ajout d’activités personnalisées est effectué par le biais de l’API Ajouter des activités personnalisées .

La plupart des activités seront purgées après un certain temps.

## Description

Pour récupérer une liste des types disponibles et leurs définitions pour une instance, vous pouvez utiliser le point de terminaison [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET) .

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

Les réponses du monde réel incluent bien plus de définitions. Dans cet exemple, le type affiché est &quot;Remplir le formulaire&quot;, qui possède un attribut principal &quot;Identifiant de formulaire web&quot;, qui fait référence à l’identifiant Marketo du formulaire qui a été rempli et peut être utilisé pour établir une relation avec cette ressource particulière dans Marketo. En outre, il existe des définitions pour chacun des attributs possibles d’un enregistrement d’activité particulier de ce type et de leurs types de données. Notez que si le champ est vide, cet attribut particulier est omis d’un enregistrement d’activité individuel.

## Requête

Pour récupérer les activités de Marketo, appelez le point de terminaison [Get Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET). Vous devez d’abord récupérer un jeton de pagination pour la date et l’heure à laquelle vous souhaitez commencer à récupérer les activités. Vous transmettez ensuite le jeton de pagination dans le paramètre de requête `nextPageToken`. En outre, vous transmettez jusqu’à dix identifiants de type d’activité dans le paramètre de requête `activityTypeIds` sous la forme d’une liste séparée par des virgules.

Vous pouvez éventuellement inclure un paramètre de requête listId pour limiter votre recherche aux seuls enregistrements inclus dans une liste statique spécifique, ou un paramètre de requête leadIds et rechercher des activités provenant d’un jeu de pistes spécifique uniquement. Vous pouvez transmettre jusqu’à 30 leadIds sous forme de liste séparée par des virgules.

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

Pour le premier appel, utilisez l’API Get Paging Token pour obtenir `nextPageToken`. Pour les appels suivants vers ce point de terminaison, utilisez le `nextPageToken returned` de la réponse. Ce point de terminaison renvoie toujours `the nextPageToken`.

Si l’attribut `moreResult` est défini sur true, cela signifie que d’autres résultats sont disponibles. Continuez à appeler ce point de terminaison jusqu’à ce que l’attribut `moreResult` renvoie false, ce qui signifie qu’il n’y a aucun résultat disponible. Les `nextPageToken` renvoyés par cette API doivent toujours être réutilisés pour la prochaine itération de cet appel.

Dans certains cas, cette API peut répondre avec moins de 300 éléments d’activité, mais l’attribut `moreResult` est également défini sur true.  Cela indique qu’il existe d’autres activités qui peuvent être renvoyées et que le point de terminaison peut être interrogé pour des activités plus récentes en incluant le `nextPageToken` renvoyé dans un appel ultérieur.

Notez que dans chaque élément du tableau de résultats, l’attribut `id` entier est remplacé par l’attribut de chaîne `marketoGUID` en tant qu’identifiant unique. 

### Modifications valeur des données

Pour les activités Data Value Change, une version spécialisée de l’API d’activités est fournie. Le point de terminaison [Get Lead Changes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) renvoie uniquement les activités des enregistrements Data Value Change vers les champs de piste. L’interface est identique à l’API Get Lead Activities avec deux différences :

* Il n’existe pas de paramètre `activityTypeIds`, puisque le point de terminaison renvoie uniquement les activités Data Value Change et New Lead .
* Le paramètre de requête `fields` est requis, où vous pouvez transmettre une liste de champs séparés par des virgules pour indiquer les champs pour lesquels vous souhaitez récupérer les modifications.

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

Chaque activité de la réponse comporte un tableau de champs, y compris une liste des modifications apportées à l’activité, qui spécifiera les `id` et `name` du champ modifiés, ainsi que les nouvelles et les anciennes valeurs relatives à la modification.

Notez que dans chaque élément du tableau de résultats, l’attribut `id` entier est remplacé par l’attribut de chaîne `marketoGUID` en tant qu’identifiant unique.

### Leçons supprimées

Il existe également un point de terminaison spécial [Get Deleted Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) pour récupérer les activités supprimées de Marketo.

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

Notez que dans chaque élément du tableau de résultats, l’attribut `id` entier est remplacé par l’attribut de chaîne `marketoGUID` en tant qu’identifiant unique.

### Résultats de la page

Par défaut, les points de terminaison mentionnés dans cette section renvoient 300 éléments d’activité à la fois.  Si l’attribut `moreResult` est défini sur true, d’autres résultats sont disponibles. Appelez le point de terminaison jusqu’à ce que l’attribut `moreResult` renvoie false, ce qui signifie qu’il n’y a plus de résultats disponibles. L’ `nextPageToken` renvoyé par ce point de terminaison doit toujours être réutilisé pour la prochaine itération de cet appel.

Dans certains cas, ce point de terminaison peut répondre avec moins de 300 éléments d’activité, mais l’attribut `moreResult` est également défini sur true.  Cela indique qu’il existe d’autres activités qui peuvent être renvoyées et que le point de terminaison peut être interrogé pour des activités plus récentes en incluant le `nextPageToken` renvoyé dans un appel ultérieur. Notez que l’élément `nextPageToken` doit être codé URL dans la requête.

## Types d’activité personnalisés

Les activités personnalisées fonctionnent de la même manière que les activités standard, à la différence que le schéma est géré par des tiers et non par Marketo. Les instances des activités personnalisées sont liées aux enregistrements de piste par le biais de `leadId` de la même manière que les activités standard, mais les attributs principal et secondaire sont définis arbitrairement. Lorsqu’un type d’activité personnalisé est approuvé, un déclencheur et un filtre de liste dynamique correspondants sont créés, de sorte que les pistes puissent être traitées en fonction des données d’activité personnalisées actuelles ou historiques.

* Nombre maximal d’activités personnalisées : 10
* Nombre maximal d’attributs par activité personnalisée : 20

La récupération des données d’activité personnalisées s’effectue de la même manière que les activités standard, par le biais de l’API [Get Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET).

## Types de requête

Outre le point d’entrée Get Activity Types standard, les points d’entrée [Get Custom Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getCustomActivityTypeUsingGET) et [ Description Custom Activity Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/describeCustomActivityTypeUsingGET) renvoient des détails sur les types d’activité configurés dans l’instance Marketo et les métadonnées concernant les attributs d’un type donné. L’activité normale [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET) renvoie toujours des métadonnées concernant les activités personnalisées, mais n’indique pas si un type donné est personnalisé.

### Obtention de types

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

### Description des types

Pour les descriptions de type, vous devez transmettre `apiName` comme paramètre de chemin d’accès. Par défaut, vous obtenez la version approuvée de l’activité. Vous pouvez éventuellement transmettre le paramètre `draft=true` pour récupérer la version préliminaire de l’activité.

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

## Créer un type

Chaque type d’activité personnalisé nécessite un nom d’affichage, un nom d’API, un nom de déclencheur, un nom de filtre et un attribut principal.

Pour garantir la cohérence de vos types avec les conventions Marketo et éviter les collisions, il est important de suivre quelques instructions lors de la création de vos types :

**Nom d’affichage :** Le nom d’affichage du type d’activité doit décrire brièvement ce qu’un enregistrement d’activité représente, comme &quot;Envoyer un courrier électronique&quot; ou &quot;Modifier la valeur des données&quot;. Ces noms doivent généralement se présenter sous la forme infinitive, c’est-à-dire &quot;Participer à l’événement&quot;.  Les noms d’affichage acceptent les caractères alphanumériques, les espaces et les traits de soulignement. Les noms d’affichage doivent contenir au moins une lettre.

**Nom de l’API :** Le nom de l’API est composé de caractères alphanumériques (longueur maximale de 255). Si vous êtes un partenaire LaunchPoint, vous devez ajouter un espace de noms représentatif aux noms de vos API de type d’activité. Cela permet d’éviter les collisions avec des types configurés par le client.  La convention consiste à utiliser toutes les minuscules ou CamelCase pour aider à distinguer les autres chaînes de texte.

**Description :** Pour les activités qui peuvent avoir un comportement non évident, doit inclure une description de ce que le type d’activité représente par rapport à la piste.

**Nom du déclencheur :** Chaque type d’activité doit avoir un nom de déclencheur unique, lisible par l’utilisateur. Les noms des déclencheurs doivent se trouver dans le contexte de la troisième personne, par exemple &quot;Assister à un événement&quot;. Les partenaires LaunchPoint doivent inclure leur nom de société dans l’activité, par exemple &quot;Assister au webinaire - Société Acme&quot;.

**Nom du filtre :**  Chaque type d’activité doit avoir un nom de filtre unique, lisible par l’utilisateur. Les noms des filtres doivent se trouver dans la tension passée de la troisième personne, telle que &quot;Participé à un événement&quot;. Les partenaires LaunchPoint doivent inclure leur nom de société dans l’activité, à savoir &quot;Webinaire assisté - Société Acme&quot;.

**Attribut de Principal :** L’attribut principal d’une activité personnalisée doit être le champ le plus significatif pour le type d’activité. Par exemple, pour une activité &quot;Événement assisté&quot;, il s’agit du nom de l’événement. Les attributs de Principal sont inclus en tant que paramètres par défaut dans chaque instance d’un déclencheur ou d’un filtre pour ce type d’activité, et la valeur est affichée dans le journal d’activité d’un enregistrement de personne sans qu’il faille descendre dans l’activité.

Lorsqu’une activité personnalisée est créée, elle est créée en tant que brouillon et doit être approuvée avant de pouvoir être utilisée pour ajouter des enregistrements d’activité de ce type. Toutes les mises à jour sont implicitement appliquées à la version préliminaire du type . Pour refléter les modifications apportées à la version active du type, il doit être approuvé. Lorsqu’un type d’activité personnalisé est approuvé et en cours d’utilisation, aucune modification ne peut être apportée aux champs ci-dessus.

Lors de la création d’un type, le paramètre de description est facultatif, tandis que tous les paramètres suivants sont requis : `apiName`, `name`, `triggerName`, `filterName`, `primaryAttribute`.

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

La mise à jour d’un type est très similaire, sauf que apiName est le seul paramètre requis comme paramètre de chemin d’accès.

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

## Type d’approbation

Les types peuvent être gérés à l’aide des options Approuver le type d’activité personnalisé, Ignorer le brouillon du type d’activité personnalisé et Supprimer le type d’activité personnalisé, tout comme les ressources Marketo standard.


## Attributs de type d’activité personnalisés

Chaque type d’activité personnalisé peut comporter entre 0 et 20 attributs secondaires. Les attributs Secondaires peuvent avoir n’importe quel type de champ valide pour un champ Marketo. Elles sont ajoutées, mises à jour et supprimées séparément du type parent, mais peuvent être modifiées lorsqu’un type d’activité est en cours d’utilisation, puis approuvées. Lorsque des champs sont édités sur un type en direct, alors toutes les activités de ce type créées après validation disposent du nouvel attribut secondaire. Les modifications ne seront pas appliquées rétroactivement aux activités existantes partageant ce type.

Soyez attentif à la suppression des attributs, car cela affectera leur disponibilité à utiliser dans les filtres correspondants.

Les mises à jour apportées à la liste des attributs secondaires utilisent le nom de l’API de chaque attribut comme clé primaire. Le nom de l’API d’un attribut ne peut pas être modifié. Il doit être supprimé et ajouté à nouveau avec le nom de l’API souhaité.

Les types de données valides pour les attributs sont les suivants : chaîne, booléen, entier, réel, lien, courrier électronique, devise, date, date, heure, téléphone, texte.

Lors de la modification de l’attribut principal d’un type d’activité, tout attribut principal existant doit être rétrogradé en définissant `isPrimary` sur false en premier.

### Création d’attributs

La création d’un attribut nécessite un paramètre de chemin `apiName`. Les paramètres `name` et `dataType` sont également requis.Les paramètres ` The description and` `isPrimary` sont facultatifs.

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

### Mise à jour des attributs

Lors de l’exécution de mises à jour des attributs, l’ `apiName` de l’attribut est la clé primaire. Le paramètre `apiName` doit exister pour que la mise à jour réussisse (c’est-à-dire que vous ne pouvez pas modifier le paramètre `apiName` en utilisant update).

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

### Suppression d’attributs

La suppression d’un attribut prend un paramètre de chemin `apiName` obligatoire qui est le nom de l’API d’activité personnalisée.  Un paramètre d’attribut qui est un tableau d’objets d’attribut est également requis.  Chaque objet doit contenir un paramètre `apiName` qui est le nom de l’API de type d’activité personnalisé.

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

## Ajout d’activités personnalisées

Les activités personnalisées sont des enregistrements d’écriture unique des activités historiques liées à des enregistrements de personne individuels dans Marketo. Ces activités comportent un schéma géré par les administrateurs Marketo ou à distance via une intégration API. Les activités personnalisées sont ajoutées aux enregistrements de piste via le point de terminaison [Ajouter des activités personnalisées](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/addCustomActivityUsingPOST) et associées à chaque enregistrement de piste via son champ `leadId` . Les activités personnalisées peuvent être visualisées dans l’interface utilisateur via le journal des activités du prospect ou récupérées via le point de terminaison Get Lead Activities en spécifiant l’ID de type de l’activité personnalisée.

Les activités personnalisées sont adaptées à l’enregistrement de données liées à un enregistrement d’une seule personne et qui n’ont pas besoin d’être mises à jour ou écrasées. Par exemple, vous pouvez enregistrer une personne participant à un événement comme activité &quot;Événement de participation&quot;. Pour les enregistrements liés à une personne qui peuvent changer, tels que l’inscription d’étudiants, les objets personnalisés doivent être utilisés à la place, car ils peuvent être mis à jour, dans le cas où les activités personnalisées ne le sont pas.

Le membre d’entrée est un tableau d’objets d’activité. Au maximum, 300 enregistrements d’activité peuvent être envoyés simultanément.

Les membres `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue` et attributs sont requis. Le tableau d’attributs doit contenir l’attribut non principal. Vous pouvez le spécifier à l’aide de name (nom du champ) ou apiName (nom de l’API), et value correspondant à la valeur que vous définissez.

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

## Délais d’expiration

Les points de terminaison des activités ont un délai d’expiration de 30 s, sauf indication contraire ci-dessous.

* Obtenir le jeton de pagination : 300 s 
* Ajouter une activité personnalisée : 90 s
