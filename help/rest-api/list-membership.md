---
title: Appartenance À Une Liste (Listes Statiques)
feature: REST API, Static Lists
description: Utilisez les API REST de la base de données des prospects Marketo pour ajouter des prospects aux listes statiques, supprimer des prospects, récupérer les membres de la liste et vérifier l’appartenance à la liste.
exl-id: b8f74bcf-834a-44db-81fd-621048afeba4
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 6%

---

# Appartenance À Une Liste (Listes Statiques)

[Référence du point d’entrée de l’appartenance à une liste](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists)

Les API List Membership fournissent des points d’entrée de base de données de prospect pour gérer les membres de liste statiques. Utilisez ces points d’entrée pour :

- Ajouter des prospects à une liste.
- Supprimer des prospects d’une liste.
- Récupérer les membres d’une liste.
- Déterminer si les prospects sont membres d’une liste.

## Points d’entrée

| Point d’entrée | Méthode | Chemin |
| --- | --- | --- |
| Ajouter à la liste | POST | `/rest/v1/lists/{listId}/leads.json` |
| Supprimer de la liste | DELETE | `/rest/v1/lists/{listId}/leads.json` |
| Obtenir les leads par ID de liste | GET | `/rest/v1/lists/{listId}/leads.json` |
| Membre de la liste | GET | `/rest/v1/lists/{listId}/leads/ismember.json` |

## Ajouter à la liste

Utilisez le point d’entrée [Ajouter à la liste](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/addLeadsToListUsingPOST) pour ajouter un ou plusieurs membres à une liste. Transmettez le paramètre de chemin d’accès au `listId` requis et un ou plusieurs paramètres de requête `id` contenant des identifiants de prospect. Le nombre maximal d’ID de lead est de 300.

La réponse contient un tableau `result` avec le statut de chaque ID de prospect dans la requête.

```http
POST /rest/v1/lists/{listId}/leads.json?id=318594&id=318595
```

```json
{
    "requestId": "6860#1706170ba29",
    "result": [
        {
            "id": 318594,
            "status": "added"
        },
        {
            "id": 318595,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

## Suppression de la liste

Utilisez le point d’entrée [Supprimer de la liste](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) pour supprimer un ou plusieurs membres d’une liste. Transmettez le paramètre de chemin d’accès au `listId` requis et un ou plusieurs paramètres de requête `id` contenant des identifiants de prospect. Le nombre maximal d’ID de lead est de 300.

La réponse contient un tableau `result` avec le statut de chaque ID de prospect dans la requête.

```http
DELETE /rest/v1/lists/{listId}/leads.json?id=318603&id=318595&id=999999
```

```json
{
    "requestId": "9e79#17061689ac3",
    "result": [
        {
            "id": 318603,
            "status": "removed"
        },
        {
            "id": 318595,
            "status": "removed"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

## Obtenir les leads par ID de liste

Utilisez le point d’entrée [Get Leads by List Id](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/getLeadsByListIdUsingGET) pour récupérer les membres d’une liste. Transmettez le paramètre de chemin d’accès au `listId` requis. Vous pouvez également transmettre des paramètres de requête facultatifs pour spécifier des critères de filtrage.

Les paramètres de requête facultatifs sont les suivants :

- `batchSize` : indique le nombre d’enregistrements de prospect à renvoyer dans un seul appel. La valeur par défaut et la valeur maximale sont 300.
- `nextPageToken` : pagine dans des jeux de résultats volumineux. Omettez ce paramètre du premier appel et incluez-le dans les appels suivants.
- `fields` : spécifie une liste de noms de champ séparés par des virgules à renvoyer. Si vous omettez ce paramètre, la réponse inclut `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` et `id`.

La réponse contient un tableau `result` avec les champs de prospect spécifiés dans la requête.

```http
GET /rest/v1/lists/{listId}/leads.json?batchSize=3
```

```json
{
    "requestId": "ddae#170615ba0cc",
    "result": [
        {
            "id": 318594,
            "firstName": "Hanna",
            "lastName": "Crawford",
            "email": "208161Robert.L.Deacon@pookmail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318595,
            "firstName": "Bertha",
            "lastName": "Fulton",
            "email": "208160Tyrone.V.Dyer@trashymail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318596,
            "firstName": "Faith",
            "lastName": "England",
            "email": "208159Rex.M.Bailey@dodgit.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        }
    ],
    "success": true,
    "nextPageToken": "PS5VL5WD4UOWGOUCJR6VY7JQO24LC2U5DRBU4WO4RQMPHDHTK2T3BEZOR75VLQXYB3245WW2GMDSK==="
}
```

## Membre de la liste

Utilisez le point d’entrée [Membre de la liste](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) pour déterminer si un ou plusieurs prospects sont membres d’une liste. Transmettez le paramètre de chemin d’accès au `listId` requis et un ou plusieurs paramètres de requête `id` contenant des identifiants de prospect. Le nombre maximal d’ID de lead est de 300.

La réponse contient un tableau `result` avec le statut de chaque ID de prospect dans la requête.

```http
GET /rest/v1/lists/{listId}/leads/ismember.json?id=309901&id=318603&id=999999
```

```json
{
    "requestId": "693a#17061475cf9",
    "result": [
        {
            "id": 309901,
            "status": "memberof"
        },
        {
            "id": 318603,
            "status": "notmemberof"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```
