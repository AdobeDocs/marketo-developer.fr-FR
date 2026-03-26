---
title: Listes statiques
feature: REST API, Static Lists
description: Utilisez les API REST Marketo pour interroger, créer, mettre à jour et supprimer des listes statiques, avec des points d’entrée pour l’identifiant, le nom, la navigation, la portée de dossier, la pagination et les filtres de date.
exl-id: 20679fd2-fae2-473e-84bc-cb4fdf2f5151
source-git-commit: 73fa4c85ecabd4cfd24bc6591aad11dc4e75010a
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 1%

---

# Listes statiques

[Référence des points d’entrée des listes statiques](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

Marketo propose un ensemble d’API REST pour effectuer des opérations CRUD sur des listes statiques. Ces API suivent le modèle d’interface standard des API de ressources en fournissant les options Requête, Créer, Mettre à jour et Supprimer .

Pour les opérations de base de données de leads sur les membres de liste, voir [Abonnement à la liste](list-membership.md).

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

[Requête par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) prend un `name` de liste statique comme paramètre et renvoie un seul enregistrement de liste statique. Une correspondance de chaîne exacte est effectuée par rapport à tous les noms de liste statiques dans l’instance et renvoie un résultat pour la liste statique correspondant à ce nom.

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

Les listes statiques peuvent également être [récupérées par lots](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET). Le paramètre `folder` peut être utilisé pour spécifier le dossier parent sous lequel la requête sera exécutée et est formaté en tant qu’objet JSON contenant des `id` et des `type`. Comme les autres points d’entrée de récupération de ressources en bloc, `offset` et `maxReturn` sont des paramètres facultatifs qui peuvent être utilisés pour la pagination. Les paramètres `earliestUpdatedAt` et `latestUpdatedAt` vous permettent de définir des filigranes datetime bas et haut pour renvoyer des listes statiques créées ou mises à jour dans la plage donnée. Les valeurs de date et d’heure doivent être des chaînes ISO-8601 valides et ne doivent pas inclure de millisecondes.

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

[La création d’une liste statique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/createStaticListUsingPOST) est exécutée avec un POST `application/x-www-form-urlencoded` avec deux paramètres obligatoires. Le paramètre `folder` est utilisé pour spécifier le dossier parent sous lequel la liste statique sera créée et est formaté comme un objet JSON contenant des `id` et des `type`. Le paramètre `name` est utilisé pour nommer la liste statique et doit être unique. Le paramètre `description` peut éventuellement être utilisé pour décrire la liste statique.

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

La [mise à jour d’une liste statique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/updateStaticListUsingPOST) s’effectue via un point d’entrée distinct avec deux paramètres facultatifs. Le paramètre `description` peut être utilisé pour mettre à jour la description de la liste statique. Le paramètre `name` peut être utilisé pour mettre à jour le nom de la liste statique et doit être unique.

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

## Supprimer

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
