---
title: "Paging Tokens"
feature: REST API
description: "Afficher les données des jetons de pagination."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 0%

---


# Jetons de pagination

Pour parcourir les résultats ou récupérer les données mises à jour par rapport à une donnée, Marketo fournit des jetons de pagination.

Dans certains cas, de longues chaînes de jetons de pagination peuvent être renvoyées. Cela peut entraîner la présence d’un code d’erreur HTTP 414. Vous trouverez plus d’informations sur la façon de les gérer. [errors](error-codes.md).

## Types de jetons

Il existe deux types de jetons de pagination associés, mais distincts, que Marketo fournit :

- Basé sur des dates
- Basé sur la position

## Basé sur des dates

Le premier est un jeton de pagination qui représente une date. Ils sont utilisés pour récupérer les activités, les modifications de valeur de données et les pistes supprimées qui se sont produites après la date représentée par le jeton de pagination. Ce type de jeton de pagination est généré en appelant la fonction [Obtenir le jeton de pagination](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET) point d’entrée et inclusion d’une date et d’une heure.

```
GET /rest/v1/activities/pagingtoken.json?sinceDatetime=2014-10-06T13:22:17-08:00
```

```json
{
    "requestId": "1607c#14884f3e74e",
    "success": true,
    "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
}
```

Le format de la variable `sinceDateTime` doit être conforme à [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) notation de date standard. Pour de meilleurs résultats, utilisez une date et une heure complètes incluant le fuseau horaire. Le fuseau horaire peut être représenté sous la forme d’un décalage par rapport à GMT au format suivant :

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Ou en utilisant une majuscule &quot;Z&quot; comme abréviation pour représenter le UTC comme ceci :

`yyyy-mm-ddThh:mm:ssZ`

Exemples

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Parce que `sinceDateTime` est un paramètre de requête, il doit être encodé en URL.

La variable `nextPageToken` est ensuite fournie à une [Obtenir des activités de piste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET), [Obtenir des modifications de piste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET), ou [Obtenir les pistes supprimées](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) Les activités et sont récupérées à partir de la date et de l’heure fournies à l’API Get Paging Token.

```
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Basé sur la position

Le deuxième type de jeton de pagination peut être renvoyé par tout appel de récupération par lots à une API de base de données de piste. Ce type de jeton de pagination est similaire en concept à un curseur de base de données qui permet la traversée d’enregistrements. Par exemple, un appel Get Leads By Filter Type peut représenter un ensemble supérieur à la taille de lot donnée, généralement la valeur maximale et par défaut de 300. S’il y a d’autres résultats, le champ moreResult est true dans la réponse et un `nextPageToken` est renvoyée. Pour récupérer les enregistrements supplémentaires dans le jeu de résultats, un appel supplémentaire comprenant `nextPageToken` avec la valeur reçue de la réponse précédente dans le nouvel appel . La réponse qui en résulte renvoie la page suivante dans le jeu de résultats.
