---
title: Jetons de pagination
feature: REST API
description: Utilisez les jetons de pagination de l’API REST Marketo pour récupérer les activités et les prospects, en couvrant les jetons basés sur la date et la position, la norme ISO 8601 SinceDatetime et les erreurs 414.
exl-id: 63fbbf03-8daf-4add-85b0-a8546c825e5b
TQID: https://experienceleague.adobe.com/Ut05n-Y-qPJnvcNRs9liwE3NVBMbJlvaGyv-nExRsek
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 387
ht-degree: 2%

---

# Jetons de pagination

Marketo fournit des jetons de pagination pour paginer les résultats ou récupérer les données mises à jour par rapport à une date spécifique.

Certaines réponses renvoient de longues chaînes de jeton de pagination, ce qui peut entraîner une erreur HTTP 414. Voir les informations sur la gestion de ces [erreurs](error-codes.md).

Consultez la documentation [API de jeton d’échange](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getActivitiesPagingTokenUsingGET).

## Types de jetons

Marketo fournit deux types de jetons de pagination associés, mais distincts :

- Les jetons basés sur la date récupèrent les enregistrements qui se produisent après une date/heure spécifiée.
- Les jetons basés sur la position parcourent les enregistrements d’un jeu de résultats.

## Basé sur la date

Un jeton de pagination basé sur la date représente une date-heure. Utilisez-le pour récupérer les activités, les modifications de valeur des données et les prospects supprimés qui se produisent après cette date et heure.

Générez un jeton basé sur la date en appelant le point d’entrée [Get Paging Token](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getActivitiesPagingTokenUsingGET) avec une date et une heure :

```http
GET /rest/v1/activities/pagingtoken.json?sinceDatetime=2014-10-06T13:22:17-08:00
```

```json
{
    "requestId": "1607c#14884f3e74e",
    "success": true,
    "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
}
```

Le paramètre `sinceDateTime` doit utiliser la notation de date standard [ISO 8601](https://fr.wikipedia.org/wiki/ISO_8601). Pour de meilleurs résultats, fournissez une date-heure complète avec un fuseau horaire.

Représente le fuseau horaire sous la forme d’un décalage par rapport à GMT au format suivant :

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Vous pouvez également utiliser un « Z » majuscule pour représenter UTC :

`yyyy-mm-ddThh:mm:ssZ`

Par exemple :

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Étant donné que `sinceDateTime` est un paramètre de requête, encode-URL sa valeur.

Transmettez la chaîne de `nextPageToken` renvoyée à un appel [Obtenir les activités du lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET), [Obtenir les modifications du lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadChangesUsingGET) ou [Obtenir les leads supprimés](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET). L’appel récupère les enregistrements qui se produisent après la date et l’heure fournies à l’API Get Paging Token.

```http
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Basé sur la position

Un jeton de pagination basé sur la position peut être renvoyé par tout appel de récupération par lots à une API de base de données de lead. Le jeton fonctionne comme un curseur de base de données et permet de parcourir les enregistrements.

Par exemple, un appel Get Leads By Filter Type peut renvoyer un jeu de résultats plus volumineux que la taille de lot demandée, qui a généralement une valeur maximale et par défaut de 300. Lorsque d’autres résultats sont disponibles, la réponse définit le champ moreResult sur true et renvoie une `nextPageToken`.

Pour récupérer la page suivante, effectuez un autre appel et transmettez la valeur `nextPageToken` de la réponse précédente. La réponse renvoie la page suivante dans le jeu de résultats.
