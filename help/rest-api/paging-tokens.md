---
title: Jetons de pagination
feature: REST API
description: Affichez les données des jetons de pagination.
exl-id: 63fbbf03-8daf-4add-85b0-a8546c825e5b
source-git-commit: a00583f367c2da36d9d1d6e0b05bfd4216573fbb
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 1%

---

# Jetons de pagination

Pour parcourir les résultats ou récupérer les données mises à jour par rapport à une donnée, Marketo fournit des jetons de pagination.

Dans certains cas, de longues chaînes de jetons de pagination peuvent être renvoyées. Cela peut entraîner la présence d’un code d’erreur HTTP 414. Vous trouverez plus d’informations sur la façon de gérer ces [erreurs](error-codes.md).

Consultez la documentation [API de jeton de pagination](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET).

## Types de jetons

Il existe deux types de jetons de pagination associés, mais distincts, que Marketo fournit :

- Basé sur des dates
- Basé sur la position

## Basé sur des dates

Le premier est un jeton de pagination qui représente une date. Ils sont utilisés pour récupérer les activités, les modifications de valeur de données et les pistes supprimées qui se sont produites après la date représentée par le jeton de pagination. Ce type de jeton de pagination est généré en appelant le point de terminaison [Get Paging Token](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET) et en incluant une date et une heure.

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

Le format du paramètre `sinceDateTime` doit être conforme à la notation de date standard [ISO 8601](https://fr.wikipedia.org/wiki/ISO_8601). Pour de meilleurs résultats, utilisez une date et une heure complètes incluant le fuseau horaire. Le fuseau horaire peut être représenté sous la forme d’un décalage par rapport à GMT au format suivant :

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Ou en utilisant une majuscule &quot;Z&quot; comme abréviation pour représenter le UTC comme ceci :

`yyyy-mm-ddThh:mm:ssZ`

Exemples

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

`sinceDateTime` étant un paramètre de requête, il doit être encodé en URL.

La chaîne `nextPageToken` est ensuite fournie à un appel [Get Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET), [Get Lead Changes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) ou [Get Deleted Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET), et les activités sont récupérées après la date et l’heure fournies à l’API Get Paging Token.

```
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Basé sur la position

Le deuxième type de jeton de pagination peut être renvoyé par tout appel de récupération par lots à une API de base de données de piste. Ce type de jeton de pagination est similaire en concept à un curseur de base de données qui permet la traversée d’enregistrements. Par exemple, un appel Get Leads By Filter Type peut représenter un ensemble supérieur à la taille de lot donnée, généralement la valeur maximale et par défaut de 300. S’il y a d’autres résultats, le champ moreResult est true dans la réponse et un `nextPageToken` est renvoyé. Pour récupérer les enregistrements supplémentaires dans le jeu de résultats, un appel supplémentaire comprenant `nextPageToken` avec la valeur reçue de la réponse précédente dans le nouvel appel. La réponse qui en résulte renvoie la page suivante dans le jeu de résultats.
