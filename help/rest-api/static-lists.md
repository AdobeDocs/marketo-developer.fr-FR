---
title: Listes statiques
feature: REST API, Static Lists
description: Utilisez les API REST Marketo pour interroger, créer, mettre à jour et supprimer des listes statiques, avec des points d’entrée pour l’identifiant, le nom, la navigation, la portée de dossier, la pagination et les filtres de date.
exl-id: 20679fd2-fae2-473e-84bc-cb4fdf2f5151
TQID: https://experienceleague.adobe.com/DSV9h6d4F3ZrIUT-VtqlmFAnpdxOuTf05ajCqiGegqk
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 333
ht-degree: 2%

---

# Listes statiques

[Référence des points d’entrée des listes statiques](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists)

Utilisez les API REST de listes statiques pour interroger, créer, mettre à jour et supprimer des listes statiques.

Pour les opérations de base de données de leads sur les membres de liste, voir [Abonnement à la liste](list-membership.md).

## Requête

Effectuer des requêtes sur des listes statiques [par ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getStaticListByIdUsingGET), [par nom](https://developer.adobe.com/marketo-apis/api/asset#operation/getStaticListByNameUsingGET) ou par [navigation](https://developer.adobe.com/marketo-apis/api/asset#operation/getStaticListsUsingGET).

### Par Id

[Requête par ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getStaticListByIdUsingGET) prend un paramètre de chemin d’accès `id` de liste statique et renvoie l’enregistrement correspondant.

```http
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

[Requête par nom](https://developer.adobe.com/marketo-apis/api/asset#operation/getStaticListByNameUsingGET) prend un paramètre de `name` de liste statique. Le point d’entrée effectue une correspondance exacte par rapport aux noms de liste statique et renvoie l’enregistrement correspondant.

```http
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

Utilisez le point d’entrée de navigation pour [récupérer des listes statiques par lots](https://developer.adobe.com/marketo-apis/api/asset#operation/getStaticListsUsingGET). Le paramètre facultatif `folder` définit la portée de la requête sur un dossier parent. Transmettez le dossier en tant qu’objet JSON contenant `id` et `type`.

Utilisez `offset` et `maxReturn` pour la pagination. Utilisez `earliestUpdatedAt` et `latestUpdatedAt` comme limites de date et d’heure basses et élevées. Ces paramètres renvoient des listes créées ou mises à jour dans la plage. Utilisez les valeurs ISO-8601 sans millisecondes.

```http
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

Envoyez une requête `application/x-www-form-urlencoded` POST pour [créer une liste statique](https://developer.adobe.com/marketo-apis/api/asset#operation/createStaticListUsingPOST). Les paramètres `folder` et `name` sont requis.

Transmettez `folder` en tant qu’objet JSON contenant des `id` et des `type`. Le `name` doit être unique. Le paramètre facultatif `description` décrit la liste.

```http
POST /rest/asset/v1/staticLists.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Utilisez le point d’entrée de mise à jour pour [modifier une liste statique](https://developer.adobe.com/marketo-apis/api/asset#operation/updateStaticListUsingPOST). Le paramètre de `description` facultatif modifie la description. Le paramètre `name` facultatif modifie le nom et doit être unique.

```http
POST /rest/asset/v1/staticList/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Pour [supprimer une liste statique](https://developer.adobe.com/marketo-apis/api/asset#operation/deleteStaticListByIdUsingPOST), transmettez son `id` en tant que paramètre de chemin d’accès. Vous ne pouvez pas supprimer une liste utilisée par une importation, une exportation ou une autre ressource.

```http
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
