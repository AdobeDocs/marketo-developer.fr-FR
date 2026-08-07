---
title: Dossiers
feature: REST API
description: Guide de l’API REST Marketo pour les dossiers couvrant la création, la mise à jour, la suppression, la requête par identifiant et nom, la navigation en masse avec root, workspace, maxDepth et la pagination.
exl-id: 4b55c256-ef0a-42b4-9548-ff8a4106f064
TQID: https://experienceleague.adobe.com/OxCNdy8qW6jwq8u57RF9mqVKPVvH99UmuiOBjFprHCM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d65b4a73-87a3-4d56-b638-74e74d9939ce
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 792
ht-degree: 1%

---

# Dossiers

[Référence des points d’entrée des dossiers](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders)

Les dossiers sont les ressources organisationnelles principales dans Marketo. Tous les autres types de ressources ont au moins un parent qui est un dossier ou un programme. Un dossier est purement organisationnel, tandis qu’un programme a une relation fonctionnelle avec d’autres types de ressources et peut également contenir des ressources.

Utilisez l’API Folders pour créer, interroger, mettre à jour et supprimer des dossiers ou récupérer leur contenu. Les requêtes de dossier peuvent renvoyer des programmes, mais vous devez utiliser l’API de programmes pour créer, mettre à jour ou supprimer un programme.

## Requête

Les dossiers prennent en charge les modèles de requête de ressource standard : [par ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getFolderByIdUsingGET), [par nom](https://developer.adobe.com/marketo-apis/api/asset#operation/getFolderByNameUsingGET) et par [navigation](https://developer.adobe.com/marketo-apis/api/asset#operation/getFolderUsingGET).

### Par Id

```http
GET /rest/asset/v1/folder/{id}.json?type=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1241b#14e21ca814a",
    "result": [
        {
            "name": "Social Media",
            "description": null,
            "createdAt": "2011-03-04T17:01:32Z+0000",
            "updatedAt": "2011-03-04T17:01:32Z+0000",
            "url": null,
            "folderId": {
                "id": 341,
                "type": "Folder"
            },
            "folderType": "Email",
            "parent": {
                "id": 11,
                "type": "Folder"
            },
            "path": "/Design Studio/Default/Emails/Social Media",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 341
        }
    ]
}
```

Le paramètre `type` est obligatoire et doit être `Folder` ou `Program`. Il détermine si le point d’entrée recherche un ID de dossier ou un ID de programme. Le point d’entrée renvoie un enregistrement dans le tableau de résultats.

Le `folderType` de réponse identifie ce que le dossier peut contenir. Les dossiers d’activités marketing ont un type de dossier ou de programme marketing et peuvent contenir plusieurs types de ressources. Les dossiers Design Studio sont dotés d’un type qui correspond aux ressources qu’ils peuvent contenir. Par exemple, un dossier E-mail peut contenir des e-mails et des sous-dossiers avec un type de dossier E-mail ou Modèle d’e-mail.

Les types de dossier sont les suivants :

- E-mail
- Modèle d’e-mail
- Page de destination
- Modèle de page de destination
- Extrait
- Fichier

### Par nom

Le point d’entrée [requête par nom](https://developer.adobe.com/marketo-apis/api/asset#operation/getFolderByNameUsingGET) nécessite `name`, qui effectue une correspondance exacte avec les noms de dossier et renvoie chaque dossier correspondant.

Le point d’entrée accepte également les paramètres facultatifs suivants :

- `type` : type de dossier, `Folder` ou `Program`.
- `root` : ID du dossier à rechercher. Si vous définissez des `root`, vous devez également définir des `type`.
- `workspace` : nom de l’espace de travail dans lequel effectuer la recherche.

```http
GET /rest/asset/v1/folder/byName.json?name=Test%2010%20-%20deverly
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "19#14e1f2f3688",
    "result": [
        {
            "name": "Test 10 - deverly",
            "description": "This is a test",
            "createdAt": "2015-06-23T06:27:04Z+0000",
            "updatedAt": "2015-06-23T06:27:04Z+0000",
            "url": "https://app-abm.marketo.com/#MF1070A1",
            "folderId": {
                "id": 454,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 416,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Marketing Programs - deverly/Test 10 - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 454
        }
    ]
}
```

Les activités marketing et Design Studio sont des dossiers racine. Récupérez l’une des racines par nom, puis utilisez-la pour parcourir la hiérarchie de dossiers dans l’instance de destination.

### Parcourir

Vous pouvez également [récupérer des dossiers en bloc](https://developer.adobe.com/marketo-apis/api/asset#operation/getFolderUsingGET). Utilisez le paramètre `root` pour spécifier le dossier parent sous lequel effectuer la requête. Transmettez `root` en tant qu’objet JSON incorporé avec deux membres :

1. `id` : ID du dossier ou du programme.
1. `type` : `Folder` ou `Program`, selon le type de dossier racine.

Si vous ne connaissez pas le dossier racine ou si vous souhaitez récupérer tous les dossiers d’une zone, utilisez la racine Activités marketing, Design Studio ou Base de données de leads. Récupérez l’ID racine en transmettant le nom de la zone à l’API [Obtenir le dossier par nom](https://developer.adobe.com/marketo-apis/api/asset#operation/getFolderByNameUsingGET).

Comme avec d’autres points d’entrée de récupération de ressources en bloc, utilisez les paramètres facultatifs `offset` et `maxReturn` pour la pagination. Les autres paramètres facultatifs sont les suivants :

- `workSpace` : nom de l’espace de travail en fonction duquel effectuer le filtrage.
- `maxDepth` : nombre maximal de niveaux à parcourir dans la hiérarchie des dossiers. Une valeur égale à 0 renvoie uniquement le dossier spécifié par `root`. La valeur par défaut est 2.

```http
GET /rest/asset/v1/folders.json?root={"id":14,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "9bd8#14e1f49047c",
    "result": [
        {
            "name": "Marketing Activities",
            "description": "Root node for the Marketing Activities app area",
            "createdAt": "2010-03-27T18:27:45Z+0000",
            "updatedAt": "2010-03-27T18:27:45Z+0000",
            "url": null,
            "folderId": {
                "id": 14,
                "type": "Folder"
            },
            "folderType": "Zone",
            "parent": null,
            "path": "/Marketing Activities",
            "isArchive": false,
            "isSystem": true,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 14
        },
        {
            "name": "Default",
            "description": "Root node of the Marketing activities Default",
            "createdAt": "2010-03-27T18:27:45Z+0000",
            "updatedAt": "2010-03-27T18:27:45Z+0000",
            "url": null,
            "folderId": {
                "id": 15,
                "type": "Folder"
            },
            "folderType": "Zone",
            "parent": {
                "id": 14,
                "type": "Folder"
            },
            "path": "/Marketing Activities/Default",
            "isArchive": false,
            "isSystem": true,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 15
        },
        {
            "name": "Archive",
            "description": "",
            "createdAt": "2010-03-27T18:28:17Z+0000",
            "updatedAt": "2010-03-27T18:28:17Z+0000",
            "url": "https://app-abm.marketo.com/#MF157A1",
            "folderId": {
                "id": 310,
                "type": "Folder"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "Folder"
            },
            "path": "/Marketing Activities/Default/Archive",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 310
        }
    ]
}
```

## Structure de réponse

Les champs `folderId` et `parent` sont des objets JSON contenant l’ID et le type de dossier. L’API utilise ce type dans les paramètres de requête, de `root` et de `parent` pour distinguer les types de dossiers Dossier et Programme .

Le champ `folderType` décrit l’utilisation du dossier. Sa valeur peut être Dossier marketing, Programme, E-mail, Modèle d’e-mail, Page de destination, Modèle de page de destination, Extrait de code, Image, Zone ou Fichier. Le dossier et le programme marketing existent dans les activités marketing et peuvent contenir plusieurs types de ressources. Les autres types de dossiers contiennent uniquement le type de ressource correspondant, les sous-dossiers et la version du modèle de ce type de ressource, le cas échéant. La zone représente un dossier de niveau racine dans les activités marketing.

Le dossier `path` affiche sa hiérarchie sous la forme d’un chemin de style Unix. La première entrée est toujours Activités marketing ou Design Studio. Si l’instance comporte des espaces de travail, la deuxième entrée est le nom de l’espace de travail propriétaire.

Le champ `url` contient l’URL de la ressource pour l’instance désignée. Il ne s’agit pas d’un lien universel qui nécessite une authentification de l’utilisateur. Le champ `isSystem` indique si le dossier est un dossier système en lecture seule. Vous pouvez créer des dossiers enfants sous un dossier système.

## Créer et mettre à jour

Pour [créer un dossier](https://developer.adobe.com/marketo-apis/api/asset#operation/createFolderUsingPOST), envoyez une requête POST `application/x-www-form-urlencoded` avec les paramètres suivants :

- `name` : chaîne obligatoire contenant le nom du dossier.
- `parent` : objet JSON incorporé obligatoire contenant des `id` et des `type`. Le type est `Folder` ou `Program`, selon le parent.
- `description` : chaîne facultative de 2 000 caractères maximum.

```http
POST /rest/asset/v1/folders.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
parent={"id":416,"type":"Folder"}&name=Test 10 - deverly&description=This is a test
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "111be#14e1f193e31",
    "result": [
        {
            "name": "Test 10 - deverly",
            "description": "This is a test",
            "createdAt": "2015-06-23T06:27:04Z+0000",
            "updatedAt": "2015-06-23T06:27:04Z+0000",
            "url": "https://app-abm.marketo.com/#MF1070A1",
            "folderId": {
                "id": 454,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 416,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Test 10 - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 454
        }
    ]
}
```

Utilisez le point d’entrée de mise à jour pour modifier les paramètres facultatifs `description`, `name` ou `isArchive`. La définition de `isArchive` pour `true` archive le dossier dans l’interface utilisateur de Marketo. La définition sur `false` supprime le dossier de l’archive.

Vous ne pouvez pas mettre à jour les programmes avec cette API.

```http
POST /rest/asset/v1/folder/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
type=Folder&description=This is a test (update 01)
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "c5b2#14e1f3954bf",
    "result": [
        {
            "name": "Learning - deverly",
            "description": "This is a test (update 01)",
            "createdAt": "2015-03-17T00:17:02Z+0000",
            "updatedAt": "2015-06-23T07:02:07Z+0000",
            "url": "https://app-abm.marketo.com/#MF1044A1",
            "folderId": {
                "id": 407,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Learning - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 407
        }
    ]
}
```

### Supprimer

Vous ne pouvez supprimer un seul dossier que s’il ne contient aucune ressource ou sous-dossier. Vous ne pouvez pas utiliser cette API pour supprimer un programme ou un dossier dont le champ `isSystem` est `true`.

```http
POST /rest/asset/v1/folder/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "4180#14e1f3fc017",
    "result": [
        {
            "id": 453
        }
    ]
}
```
