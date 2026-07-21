---
title: Activités
feature: REST API
description: Utilisez l’API REST Activités Marketo Engage pour répertorier les types d’activité, récupérer les activités de prospect avec des jetons de pagination et gérer les modifications personnalisées et de valeur de données.
exl-id: 1e69af23-2b0c-467a-897c-1dcf81343e73
TQID: https://experienceleague.adobe.com/62keaj4uNoxIPCzr9AQzKrIsfuHBvC25knYisZRUvF4
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1758
ht-degree: 0%

---

# Activités

Marketo prend en charge de nombreux types d’activités liés aux enregistrements de prospect. Presque chaque modification, action ou étape de flux est enregistrée dans le journal d’activité d’un prospect. Vous pouvez récupérer ces activités par le biais de l’API ou les utiliser dans les filtres et triggers de liste dynamique et de campagne intelligente.

Chaque activité possède un `id` unique et se connecte à un enregistrement de prospect via `leadId`, qui correspond au champ d’ID de l’enregistrement. Chaque activité possède également un `activityDate`.

Les types d’activité disponibles varient selon l’abonnement. Chaque type possède sa propre définition. La signification de `primaryAttributeValueId` et `primaryAttributeValue` dépend du type d’activité.

Utilisez l’API de métadonnées d’activités personnalisées pour créer des types d’activités personnalisés. Utilisez l’API Ajouter des activités personnalisées pour ajouter des enregistrements d’activités personnalisées.

La plupart des activités seront purgées après un certain temps.

## Décrire

Utilisez le point d’entrée [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET) pour récupérer les types d’activité disponibles et leurs définitions pour une instance.

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

Les réponses réelles incluent d’autres définitions. Cet exemple illustre le type d&#39;activité « Remplir le formulaire ». Son attribut principal, « ID du formulaire web », fait référence à l’ID Marketo du formulaire envoyé et lie l’activité à cette ressource.

La réponse définit également chaque attribut possible pour le type d’activité et son type de données. Si un champ est vide, cet attribut est omis dans l’enregistrement d’activité individuel.

## Requête

Utilisez le point d’entrée [Obtenir les activités de prospect](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET) pour récupérer les activités. Tout d’abord, récupérez un jeton de pagination pour la date et l’heure auxquelles la récupération de l’activité doit commencer. Transmettez ce jeton dans le paramètre de requête `nextPageToken`.

Transmettez jusqu’à dix identifiants de type d’activité sous la forme d’une liste séparée par des virgules dans le paramètre de requête `activityTypeIds`.

Vous pouvez éventuellement affiner la requête avec l’un des paramètres suivants :

- `listId` limite les résultats aux enregistrements d’une liste statique spécifique.
- `leadIds` limite les résultats aux activités pour un maximum de 30 prospects, fournis sous forme de liste séparée par des virgules.

>[!CAUTION]
>
>À compter du 30/12/2026, les appels aux points d’entrée `Get Lead Activities` et `Get Lead Changes` qui incluent le paramètre `listId` échoueront (code d’erreur 1003) si les listes cibles contiennent 10 000 prospects ou plus. Pour éviter toute interruption de service, assurez-vous que la portée des appels est correctement définie pour éviter cette limite. Voir le [ Guide de migration ](migration.md).

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

Pour le premier appel, utilisez l’API Get Paging Token pour obtenir des `nextPageToken`. Pour chaque appel suivant, transmettez le `nextPageToken` renvoyé par la réponse précédente. Ce point d’entrée renvoie toujours `nextPageToken`.

Si `moreResult` est vrai, d’autres résultats sont disponibles. Continuez à appeler le point d’entrée avec la `nextPageToken` renvoyée jusqu’à ce que `moreResult` soit false.

L’API peut renvoyer moins de 300 éléments d’activité tout en définissant `moreResult` sur true. Dans ce cas, incluez le `nextPageToken` renvoyé dans un autre appel pour récupérer des activités plus récentes.

Dans chaque élément du tableau de résultats, l’attribut de chaîne `marketoGUID` remplace l’attribut entier `id` en tant qu’identifiant unique.

### Modifications de la valeur des données

Utilisez le point d’entrée [Obtenir les modifications du prospect](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadChangesUsingGET) pour récupérer les enregistrements de modification de la valeur des données pour les champs de prospect. Son interface diffère de l’API Get Lead Activities de deux façons :

- Le point d’entrée ne comporte aucun paramètre `activityTypeIds`, car il renvoie uniquement les activités Modification de la valeur des données et Nouveau prospect.
- Le paramètre de requête `fields` obligatoire accepte une liste de champs séparés par des virgules dont vous souhaitez récupérer les modifications.

>[!CAUTION]
>
>À compter du 30/12/2026, les appels aux points d’entrée `Get Lead Activities` et `Get Lead Changes` qui incluent le paramètre `listId` échoueront (code d’erreur 1003) si les listes cibles contiennent 10 000 prospects ou plus. Pour éviter toute interruption de service, assurez-vous que la portée des appels est correctement définie pour éviter cette limite. Voir le [ Guide de migration ](migration.md).

```http
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

Chaque activité de la réponse comporte un tableau de champs qui répertorie ses modifications. Chaque modification spécifie la `id` et la `name` du champ, ainsi que les nouvelles et anciennes valeurs.

Dans chaque élément du tableau de résultats, l’attribut de chaîne `marketoGUID` remplace l’attribut entier `id` en tant qu’identifiant unique.

### Leads supprimés

Utilisez le point d’entrée [Obtenir les leads supprimés](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET) pour récupérer les activités de lead supprimées dans Marketo.

```http
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

Dans chaque élément du tableau de résultats, l’attribut de chaîne `marketoGUID` remplace l’attribut entier `id` en tant qu’identifiant unique.

### Résultats de navigation

Par défaut, les points d’entrée de cette section renvoient 300 éléments d’activité à la fois. Si `moreResult` est vrai, d’autres résultats sont disponibles. Transmettez le `nextPageToken` renvoyé à chaque appel suivant jusqu’à ce que `moreResult` ait la valeur false.

Un point d’entrée peut renvoyer moins de 300 éléments d’activité tout en définissant `moreResult` sur true. Dans ce cas, incluez le `nextPageToken` renvoyé dans un autre appel pour récupérer des activités plus récentes. Code URL `nextPageToken` dans la requête.

## Types d’activités personnalisées

Les activités personnalisées fonctionnent comme des activités standard, mais des tiers gèrent leurs schémas. Les enregistrements d’activité personnalisés renvoient vers les enregistrements de prospect via `leadId` et leurs attributs principaux et secondaires sont définis par l’utilisateur.

Lorsqu’un type d’activité personnalisé est approuvé, Marketo crée un déclencheur et un filtre de liste dynamique correspondants. Vous pouvez ensuite traiter les prospects en fonction des données d’activité personnalisée actuelles ou historiques.

- Activités Personnalisées Maximales : 10
- Nombre maximal d’attributs par activité personnalisée : 20

Récupérez les données d’activité personnalisées par le biais de l’API [Obtenir les activités de lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET), de la même manière que vous récupérez des activités standard.

## Types de requête

Utilisez [Get Custom Activity Types](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getCustomActivityTypeUsingGET) pour récupérer des détails sur les types configurés dans une instance Marketo. Utilisez [Décrire le type d’activité personnalisé](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/describeCustomActivityTypeUsingGET) pour récupérer les métadonnées d’attribut pour un type spécifique.

Le point d’entrée standard [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET) renvoie également des métadonnées d’activité personnalisées, mais il n’identifie pas si un type est personnalisé.

### Obtenir les types

```http
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

Pour décrire un type, transmettez `apiName` comme paramètre de chemin d’accès. Par défaut, le point d’entrée renvoie la version approuvée de l’activité. Pour récupérer le brouillon, transmettez le paramètre facultatif `draft=true`.

```http
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

Chaque type d’activité personnalisé nécessite un nom d’affichage, un nom d’API, un nom de déclencheur, un nom de filtre et un attribut principal. Suivez les instructions ci-dessous pour garantir la cohérence des types avec les conventions Marketo et éviter les collisions de noms :

- **Nom d’affichage :** décrivez brièvement ce que représente un enregistrement d’activité, par exemple « Envoyer un e-mail » ou « Modifier la valeur des données ». Utilisez le formulaire infini, tel que « Assister à l’événement ». Les noms d’affichage acceptent les caractères alphanumériques, les espaces et les traits de soulignement et doivent contenir au moins une lettre.

- **Nom de l’API :** utilisez des caractères alphanumériques, avec une longueur maximale de 255. Si vous êtes un partenaire LaunchPoint, ajoutez un espace de noms représentatif aux noms des API de type d’activité pour éviter les conflits avec les types configurés par le client. Utilisez des minuscules ou des majuscules pour distinguer les noms d’API des autres chaînes.

- **Description :** pour les activités ayant un comportement non évident, expliquez ce que le type d’activité représente par rapport au prospect.

- **Nom du déclencheur :** attribuez un nom unique, lisible par l’utilisateur au présent tiers, tel que « Participe à un événement ». Les partenaires LaunchPoint doivent inclure le nom de leur société, tel que « Participe au webinaire - Société Acme ».

- **Nom du filtre :** saisissez un nom unique, lisible par l’utilisateur au passé pour la troisième personne, tel que « A assisté à un événement ». Les partenaires LaunchPoint doivent inclure le nom de leur société, tel que « Webinaire assisté - Société Acme ».

- **Attribut de Principal :** sélectionnez le champ le plus significatif pour le type d’activité. Pour une activité « Événement assisté », ce champ correspond au nom de l’événement. L’attribut principal apparaît par défaut comme paramètre dans chaque déclencheur ou filtre pour le type d’activité. Sa valeur apparaît également dans le journal d’activité d’une personne sans qu’il soit nécessaire d’effectuer une analyse approfondie de l’activité.

Un nouveau type d’activité personnalisé est créé en tant que brouillon. Validez le type avant d&#39;ajouter les enregistrements d&#39;activité de ce type. Les mises à jour s’appliquent à la version brouillon et doivent être approuvées avant d’apparaître dans la version active. Une fois qu’un type d’activité personnalisée est approuvé et utilisé, les champs précédents ne peuvent pas être modifiés.

Lors de la création d’un type, le paramètre de description est facultatif. Les paramètres requis sont `apiName`, `name`, `triggerName`, `filterName` et `primaryAttribute`.

```http
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

Pour mettre à jour un type, transmettez l’apiName requis comme paramètre de chemin d’accès. D’autres champs peuvent être fournis dans le corps de la requête.

```http
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

Gérez les types avec le type d’activité personnalisé Approuver , Ignorer le brouillon de type d’activité personnalisé et Supprimer le type d’activité personnalisé, comme vous le feriez avec les ressources Marketo standard.

## Attributs de type d’activité personnalisés

Chaque type d’activité personnalisé peut avoir de 0 à 20 attributs secondaires. Un attribut secondaire peut utiliser n’importe quel type de champ Marketo valide. Ajoutez, mettez à jour et supprimez des attributs secondaires séparément du type parent.

Vous pouvez modifier les attributs lorsqu’un type d’activité est en cours d’utilisation, puis approuver les modifications. Les activités créées après approbation utilisent le nouveau jeu d’attributs secondaire. Les modifications ne s’appliquent pas rétroactivement aux activités existantes de ce type.

La suppression des attributs supprime également leur disponibilité dans les filtres correspondants.

Les mises à jour de la liste des attributs secondaires utilisent le nom d’API de chaque attribut comme clé primaire. Pour modifier le nom d’une API, supprimez l’attribut et ajoutez-le à nouveau avec le nom d’API souhaité.

Les types de données valides pour les attributs sont les suivants : chaîne, booléen, entier, flottant, lien, e-mail, devise, date, date et heure, téléphone, texte.

Avant de modifier l’attribut principal d’un type d’activité, rétrogradez l’attribut principal existant en définissant `isPrimary` sur false.

### Créer des attributs

Pour créer un attribut, transmettez le paramètre de chemin d’accès `apiName` requis. Les paramètres `name` et `dataType` sont également requis. La description et les paramètres de `isPrimary` sont facultatifs.

```http
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

Lors de la mise à jour des attributs, l’attribut `apiName` est la clé primaire et doit déjà exister. Vous ne pouvez pas modifier les `apiName` avec une mise à jour.

```http
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

Pour supprimer un attribut, transmettez le paramètre de chemin d’accès `apiName` requis pour l’activité personnalisée. Transmettez également le paramètre d’attribut obligatoire sous la forme d’un tableau d’objets d’attribut. Chaque objet doit contenir un paramètre `apiName` pour le type d’activité personnalisée.

```http
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

Les activités personnalisées sont des enregistrements non réinscriptibles d’activités historiques pour des enregistrements de personnes individuels. Les administrateurs Marketo peuvent gérer leur schéma dans Marketo ou une intégration d’API peut le gérer à distance.

Utilisez le point d’entrée [Ajouter des activités personnalisées](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/addCustomActivityUsingPOST) pour ajouter des activités personnalisées aux enregistrements de prospect. Le champ `leadId` associe chaque activité à un prospect. Affichez les activités personnalisées dans le journal d’activité du prospect ou récupérez-les via l’option Obtenir les activités du prospect en spécifiant l’identifiant du type d’activité personnalisée.

Utilisez des activités personnalisées pour les données relatives à une personne qui n’ont pas besoin d’être mises à jour ou remplacées. Par exemple, enregistrez la participation à un événement en tant qu’activité « Événement auquel vous avez participé ».

Utilisez des objets personnalisés pour les enregistrements liés à une personne qui peuvent changer, comme l’inscription des étudiants. Les objets personnalisés peuvent être mis à jour, mais les activités personnalisées ne le peuvent pas.

Le membre d’entrée est un tableau d’objets d’activité. Vous pouvez envoyer un maximum de 300 enregistrements d’activité à la fois.

Les membres `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue` et attributs sont requis. Le tableau d’attributs doit contenir l’attribut non principal. Spécifiez-la avec le nom (nom du champ) ou l’apiName (nom de l’API), puis saisissez la valeur à définir.

```http
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

Les points d’entrée des activités ont un délai d’expiration de 30 s, à l’exception des points d’entrée suivants :

- Obtenir le jeton de pagination : 300s
- Ajouter une activité personnalisée : 90s
