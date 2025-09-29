---
title: Listes statiques
feature: REST API, Static Lists
description: Utilisez les API REST Marketo pour interroger, créer, mettre à jour et supprimer des listes statiques, avec des points d’entrée pour l’identifiant, le nom, la navigation, la portée de dossier, la pagination et les filtres de date.
exl-id: 20679fd2-fae2-473e-84bc-cb4fdf2f5151
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 1%

---

# Listes statiques

[Référence des points d’entrée des listes statiques](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

[Liste de références de point d’entrée d’appartenance](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

Marketo propose un ensemble d’API REST pour effectuer des opérations CRUD sur des listes statiques. Ces API suivent le modèle d’interface standard des API de ressources en fournissant les options Requête, Créer, Mettre à jour et Supprimer .

## Requête

L’interrogation de listes statiques suit les types d’interrogation standard pour les ressources de [par id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET), [par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) et [parcourir](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET).

### Par Id

[Requête par ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) prend un seul `id` de liste statique comme paramètre de chemin d’accès et renvoie un seul enregistrement de liste statique.

```
GET /rest/asset/v1/staticList/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "843c#1641f969e96",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Par nom

[Requête par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET) prend un `name` de liste statique comme paramètre et renvoie un seul enregistrement de liste statique. Une correspondance de chaîne exacte est effectuée par rapport à tous les noms de liste statiques dans l’instance et renvoie un résultat pour la liste statique correspondant à ce nom.

```
GET /rest/asset/v1/staticList/byName.json?name=Foundation Seed List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "28ab#1641fa246b9",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Parcourir

Les listes statiques peuvent également être [récupérées par lots](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET). Le paramètre `folder` peut être utilisé pour spécifier le dossier parent sous lequel la requête sera exécutée et est formaté en tant qu’objet JSON contenant l’ID et le type. Comme les autres points d’entrée de récupération de ressources en bloc, `offset` et `maxReturn` sont des paramètres facultatifs qui peuvent être utilisés pour la pagination. Les paramètres `earliestUpdatedAt` et `latestUpdatedAt` vous permettent de définir des filigranes datetime bas et haut pour renvoyer des listes statiques créées ou mises à jour dans la plage donnée. Les valeurs Datetime doivent être des chaînes ISO-8601 valides et ne doivent pas inclure de millisecondes

```
GET /rest/asset/v1/staticLists.json?folder={"id":13,"type":"Folder"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2dc0#1641f846633",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        },
        {
            "id": 1022,
            "name": "Blacklist Seed List",
            "createdAt": "2017-07-27T23:19:33Z+0000",
            "updatedAt": "2017-07-27T23:21:29Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1022A1"
        },
        {
            "id": 1023,
            "name": "Possible Duplicates Seed List",
            "createdAt": "2017-07-28T00:10:02Z+0000",
            "updatedAt": "2017-07-28T00:11:22Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1023A1"
        }
    ]
}
```

## Créer et mettre à jour

[La création d’une liste statique](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/createStaticListUsingPOST) est exécutée avec une application/x-www-form-urlencoded POST avec deux paramètres obligatoires. Le paramètre `folder` est utilisé pour spécifier le dossier parent sous lequel la liste statique sera créée et est formaté comme un objet JSON contenant l’ID et le type. Le paramètre `name` est utilisé pour nommer la liste statique et doit être unique. Le paramètre `description` peut éventuellement être utilisé pour décrire la liste statique.

```
POST /rest/asset/v1/staticLists.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
folder={"id":1034,"type":"Program"}&name=My Static List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1269d#164209d6e1e",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "createdAt": "2018-06-21T04:32:25Z+0000",
            "updatedAt": "2018-06-21T04:32:25Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

Les [mises à jour d’une liste statique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/updateStaticListUsingPOST) sont effectuées via un point d’entrée distinct avec deux paramètres facultatifs. Le paramètre `description` peut être utilisé pour mettre à jour la description de la liste statique. Le paramètre `name` peut être utilisé pour mettre à jour le nom de la liste statique et doit être unique.

```
POST /rest/asset/v1/staticList/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
description=This is a static list used for testing
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "f84f#16420b4c746",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "description": "This is a static list used for testing",
            "createdAt": "2018-06-21T04:32:26Z+0000",
            "updatedAt": "2018-06-21T04:57:55Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

### Supprimer

[La suppression d’une liste statique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/deleteStaticListByIdUsingPOST) utilise une seule liste statique `id` comme paramètre de chemin d’accès. Les listes statiques utilisées par une opération d’importation ou d’exportation ou par d’autres ressources ne peuvent pas être supprimées.

```
POST /rest/asset/v1/staticList/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2c79#16420ded0e9",
    "result": [
        {
            "id": 1027
        }
    ]
}
```

## Appartenance à la liste

Les points d’entrée d’appartenance à une liste permettent d’ajouter, de supprimer et d’interroger des membres de liste statiques. En outre, vous pouvez interroger l’appartenance à une liste statique.

### Ajouter à la liste

Le point d’entrée [Ajouter à la liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/addLeadsToListUsingPOST) permet d’ajouter un ou plusieurs membres à une liste. Le point d’entrée prend un paramètre de chemin d’accès `listId` obligatoire et un ou plusieurs paramètres de requête d’identifiant contenant des ID de prospect (la valeur maximale autorisée est de 300).

La réponse contient un tableau `result` composé d’objets JSON avec le statut de chaque ID de prospect spécifié dans la requête.

```
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

### Supprimer de la liste

Le point d’entrée [Supprimer de la liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) permet de supprimer un ou plusieurs membres d’une liste. Le point d’entrée prend un paramètre de chemin d’accès `listId` obligatoire et un ou plusieurs paramètres de requête `id` contenant des ID de lead (la valeur maximale autorisée est de 300).

La réponse contient un tableau `result` composé d’objets JSON avec le statut de chaque ID de prospect spécifié dans la requête.

```
DELETE /rest/v1/lists/{listId}/leads.json?id=318603&id=318595&id=999999
```

```
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

### Liste de requêtes

Le point d’entrée [Get Leads by List Id](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET) permet de récupérer les membres d’une liste. Le point d’entrée prend un paramètre de chemin d’accès `listId` obligatoire et permet à plusieurs paramètres de requête facultatifs de spécifier des critères de filtrage.

Le paramètre `batchSize` est utilisé pour spécifier le nombre d’enregistrements de prospect à renvoyer dans un seul appel (la valeur par défaut et la valeur maximale sont 300).

Le paramètre `nextPageToken` est utilisé pour paginer dans des jeux de résultats volumineux. Ce paramètre n’est pas transmis lors du premier appel, mais uniquement lors des appels suivants de la pagination.

Le paramètre `fields` contient une liste séparée par des virgules de noms de champ à renvoyer dans la réponse. Si le paramètre fields n’est pas inclus dans cette requête, les champs par défaut suivants sont renvoyés : email, updatedAt, createdAt, lastName, firstName et id.

La réponse contient un tableau `result` composé d’objets JSON contenant les champs de prospect spécifiés dans la requête.

```
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

#### Demander l’appartenance à une liste par ID de lead

Le point d’entrée [Membre de la liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) permet de voir si un ou plusieurs prospects sont membres d’une liste. Le point d’entrée prend un paramètre de chemin d’accès `listId` obligatoire et un ou plusieurs paramètres de requête `id` contenant des ID de lead (la valeur maximale autorisée est de 300).

La réponse contient un tableau `result` composé d’objets JSON avec le statut de chaque ID de prospect spécifié dans la requête.

```
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
