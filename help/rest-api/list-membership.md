---
title: Appartenance À Une Liste (Listes Statiques)
feature: REST API, Static Lists
description: Utilisez les API REST de la base de données des prospects Marketo pour ajouter des prospects aux listes statiques, supprimer des prospects, récupérer les membres de la liste et vérifier l’appartenance à la liste.
exl-id: b8f74bcf-834a-44db-81fd-621048afeba4
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 5%

---

# Appartenance À Une Liste (Listes Statiques)

[Référence du point d’entrée de l’appartenance à une liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

Les API List Membership fournissent des points d’entrée de base de données de prospect pour travailler avec des membres de liste statiques. Ces points d’entrée peuvent être utilisés pour ajouter des prospects à une liste, supprimer des prospects d’une liste, récupérer les membres d’une liste et déterminer si un ou plusieurs prospects sont membres d’une liste.

## Points d’entrée

| Point d’entrée | Méthode | Chemin |
| --- | --- | --- |
| Ajouter à la liste | POST | `/rest/v1/lists/{listId}/leads.json` |
| Supprimer de la liste | DELETE | `/rest/v1/lists/{listId}/leads.json` |
| Obtenir les leads par ID de liste | GET | `/rest/v1/lists/{listId}/leads.json` |
| Membre de la liste | GET | `/rest/v1/lists/{listId}/leads/ismember.json` |

## Ajouter à la liste

Le point d’entrée [Ajouter à la liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/addLeadsToListUsingPOST) permet d’ajouter un ou plusieurs membres à une liste. Le point d’entrée prend un paramètre de chemin d’accès `listId` obligatoire et un ou plusieurs paramètres de requête `id` contenant des ID de lead (la valeur maximale autorisée est de 300).

La réponse contient un tableau `result` composé d’objets JSON avec le statut de chaque ID de prospect spécifié dans la requête.

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

Le point d’entrée [Supprimer de la liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) permet de supprimer un ou plusieurs membres d’une liste. Le point d’entrée prend un paramètre de chemin d’accès `listId` obligatoire et un ou plusieurs paramètres de requête `id` contenant des ID de lead (la valeur maximale autorisée est de 300).

La réponse contient un tableau `result` composé d’objets JSON avec le statut de chaque ID de prospect spécifié dans la requête.

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

Le point d’entrée [Get Leads by List Id](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET) permet de récupérer les membres d’une liste. Le point d’entrée prend un paramètre de chemin d’accès `listId` obligatoire et permet à plusieurs paramètres de requête facultatifs de spécifier des critères de filtrage.

Le paramètre `batchSize` est utilisé pour spécifier le nombre d’enregistrements de prospect à renvoyer dans un seul appel. La valeur par défaut et la valeur maximale sont 300.

Le paramètre `nextPageToken` est utilisé pour paginer dans des jeux de résultats volumineux. Ce paramètre n’est pas transmis lors du premier appel, mais uniquement lors des appels suivants pour la pagination.

Le paramètre `fields` contient une liste séparée par des virgules de noms de champ à renvoyer dans la réponse. Si le paramètre `fields` n’est pas inclus dans cette requête, les champs par défaut suivants sont renvoyés : `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` et `id`.

La réponse contient un tableau `result` composé d’objets JSON contenant les champs de prospect spécifiés dans la requête.

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

Le point d’entrée [Membre de la liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) permet de voir si un ou plusieurs prospects sont membres d’une liste. Le point d’entrée prend un paramètre de chemin d’accès `listId` obligatoire et un ou plusieurs paramètres de requête `id` contenant des ID de lead (la valeur maximale autorisée est de 300).

La réponse contient un tableau `result` composé d’objets JSON avec le statut de chaque ID de prospect spécifié dans la requête.

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
