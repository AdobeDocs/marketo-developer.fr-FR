---
title: Jetons de pagination
feature: REST API
description: Utilisez les jetons de pagination de l’API REST Marketo pour récupérer les activités et les prospects, en couvrant les jetons basés sur la date et la position, la norme ISO 8601 SinceDatetime et les erreurs 414.
exl-id: 63fbbf03-8daf-4add-85b0-a8546c825e5b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---

# Jetons de pagination

Pour parcourir les résultats ou récupérer les données mises à jour relatives à une donnée donnée donnée, Marketo fournit des jetons de pagination.

Dans certains cas, de longues chaînes de jeton de pagination peuvent être renvoyées. Cela peut vous entraîner à rencontrer un code d’erreur HTTP 414. Vous trouverez plus d’informations sur la façon de gérer ces [erreurs](error-codes.md).

Consultez la documentation [API de jeton d’échange](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET).

## Types de jetons

Marketo fournit deux types de jetons de pagination associés, mais distincts :

- Basé sur la date
- Basé sur la position

## Basé sur la date

Le premier est un jeton de pagination qui représente une date. Ils sont utilisés pour récupérer les activités, les modifications de valeur de données et les prospects supprimés qui se sont produits après la date représentée par le jeton de pagination. Ce type de jeton de pagination est généré en appelant le point d’entrée [Get Paging Token](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET) et en incluant une date-heure.

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

Le format du paramètre `sinceDateTime` doit être conforme à la notation de date standard [ISO 8601](https://fr.wikipedia.org/wiki/ISO_8601). Pour de meilleurs résultats, utilisez une date-heure complète incluant le fuseau horaire. Le fuseau horaire peut être représenté sous la forme d’un décalage par rapport à GMT à l’aide du format suivant :

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Ou utilisez un « Z » majuscule comme raccourci pour représenter le format UTC comme suit :

`yyyy-mm-ddThh:mm:ssZ`

Exemples

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Étant donné que `sinceDateTime` est un paramètre de requête, il doit être encodé en URL.

La chaîne `nextPageToken` est ensuite fournie à un appel [Obtenir les activités du lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET), [Obtenir les modifications du lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) ou [Obtenir les leads supprimés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) et les activités sont récupérées après la date et l’heure fournies à l’API Obtenir le jeton de pagination.

```
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Basé sur la position

Le deuxième type de jeton de pagination peut être renvoyé par tout appel de récupération par lots à une API de base de données de leads. Ce type de jeton de pagination est similaire en concept à un curseur de base de données qui permet de parcourir les enregistrements. Par exemple, un appel Get Leads By Filter Type peut représenter un ensemble supérieur à la taille de lot donnée, généralement la valeur maximale et par défaut de 300. Lorsqu’il y a d’autres résultats, le champ moreResult est true dans la réponse et une `nextPageToken` est renvoyée. Pour récupérer les enregistrements supplémentaires dans le jeu de résultats, un appel supplémentaire incluant `nextPageToken` avec la valeur reçue de la réponse précédente dans le nouvel appel. La réponse obtenue renvoie la page suivante dans le jeu de résultats.
