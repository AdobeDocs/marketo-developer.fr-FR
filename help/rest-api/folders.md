---
title: Dossiers
feature: REST API
description: Manipulation de dossiers avec l’API Marketo.
exl-id: 4b55c256-ef0a-42b4-9548-ff8a4106f064
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 1%

---

# Dossiers

[Référence du point d’entrée de dossiers](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders)

Les dossiers sont la ressource organisationnelle principale dans Marketo et chaque autre type de ressource comporte au moins un dossier en tant que parent. Ce dossier parent peut être soit un dossier purement organisationnel, soit un programme, qui a une relation fonctionnelle avec d’autres types de ressources et qui peut également être le parent d’autres ressources. Les dossiers peuvent être créés, interrogés, mis à jour et supprimés via l’API et permettent également de récupérer une liste de leurs contenus. Bien que les programmes puissent être renvoyés en interrogeant l’API Folders, la création, la mise à jour et la suppression de programmes doivent être effectuées via l’API Programmes.

## Requête

Les dossiers de requêtes suivent les types de requête standard pour les ressources [by id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByIdUsingGET), [by name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) et [browsing](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET).

### Par identifiant

```
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

Le paramètre type est obligatoire et doit être de type &quot;Dossier&quot; ou &quot;Programme&quot;.  Le type détermine si la recherche du dossier est effectuée par rapport à un ID de dossier ou à un ID de programme. Pour ce point de terminaison, un seul enregistrement est renvoyé dans le tableau de résultat. Notez le paramètre folderType dans la réponse. Cela peut indiquer de nombreux types de dossiers différents. Les dossiers d’activités Marketo ont un type de dossier marketing ou de programme, qui peut contenir de nombreux types de ressources différents, tandis que les dossiers de Design Studio ont un type correspondant au type de ressource qu’ils peuvent contenir. Par exemple, un dossier avec le type de dossier &quot;Email&quot; peut contenir uniquement des emails ou d’autres sous-dossiers, qui peuvent avoir un type de dossier &quot;Email&quot; ou un modèle de courrier électronique. Les types peuvent être les suivants :

- E-mail
- Modèle d&#39;e-mail
- Page de destination
- Modèle de page de destination
- Extrait
- Fichier

### Par nom

[La recherche par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) est également autorisée. Le point de fin de requête par nom est nommé comme seul paramètre obligatoire. Name effectue une correspondance de chaîne exacte par rapport au champ de nom des dossiers dans l’instance et renvoie les résultats de chaque dossier correspondant à ce nom. Il contient également les paramètres de requête facultatifs de &quot;type&quot;, qui peuvent être Dossier ou Programme, &quot;root&quot;, l’identifiant du dossier à rechercher, ou &quot;workspace&quot;, le nom de l’espace de travail dans lequel effectuer la recherche. Si le paramètre racine est défini, le paramètre type doit également être défini.

```
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

Lors d’une recherche par nom, il est important de noter que les activités marketing et Design Studio sont leurs propres dossiers racine afin qu’ils puissent être récupérés par nom et utilisés pour parcourir le reste de la hiérarchie de dossiers dans une instance de destination.

### Parcourir

Les dossiers peuvent également être [récupérés en bloc](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET). Le paramètre &quot;root&quot; peut être utilisé pour spécifier le dossier parent sous lequel la requête sera exécutée et est formaté en tant qu’objet JSON incorporé en tant que valeur du paramètre de requête. Root comporte deux membres :

1. id : identifiant du dossier ou du programme.
1. type : dossier ou programme, selon le type du dossier racine dans le navigateur.

Si le dossier racine n’est pas connu ou si l’intention est de récupérer tous les dossiers d’une zone donnée, la racine peut être spécifiée sous la forme de zones &quot;Activités marketing&quot;, &quot;Design Studio&quot; ou &quot;Base de données de piste&quot;. Les identifiants de chacun d’eux peuvent être récupérés via l’API [Get Folder By Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) et en spécifiant le nom de la zone souhaitée.

Comme les autres points de terminaison de récupération de ressources en bloc, offset et maxReturn sont des paramètres facultatifs pour la pagination.   Les autres paramètres facultatifs sont les suivants :

- workSpace : nom de l’espace de travail sur lequel filtrer les données.
- maxDepth : nombre maximal de niveaux à parcourir dans la hiérarchie de dossiers. S’il est défini sur 0, seul le dossier spécifié à la racine est renvoyé. Si elle n’est pas spécifiée, la valeur par défaut est 2.

```
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

Une grande partie de la structure de réponse des dossiers s’explique d’elle-même, mais certains champs méritent d’être soulignés individuellement. Les champs `folderId` et parent sont des objets JSON qui incluent l’identifiant explicite et le type du dossier lui-même. Ce type est celui qui est utilisé dans les requêtes, les paramètres racine et parent par l’API pour garantir une bonne délimitation entre les types de dossiers Dossier et Programme. `folderType` reflète l’utilisation du dossier, qui peut être un des dossiers &quot;Dossier marketing&quot;, &quot;Programme&quot;, &quot;E-mail&quot;, &quot;Modèle de courrier électronique&quot;, &quot;Page d’entrée&quot;, &quot;Fragment de code&quot;, &quot;Image&quot;, &quot;Zone&quot; ou &quot;Fichier&quot;.  Les types Dossier et Programme marketing indiquent qu’ils existent dans les activités marketing et peuvent contenir plusieurs types de ressources. Les autres types indiquent qu’ils ne peuvent contenir que le type de ressource, les sous-dossiers et la version de modèle de ce type, le cas échéant. Le type Zone représente les dossiers au niveau racine qui se trouvent dans les activités marketing.

Le chemin d’un dossier affiche sa hiérarchie dans l’arborescence de dossiers, semblable à un chemin de style Unix. La première entrée du chemin d’accès sera toujours Activités marketing ou Design Studio. Si l’instance cible comporte des espaces de travail, la deuxième entrée dans le chemin sera le nom de l’espace de travail propriétaire. Le champ `url` affiche l’URL explicite de la ressource dans l’instance désignée. Ce lien n’est pas universel et doit être authentifié en tant qu’utilisateur pour fonctionner correctement. `isSystem` indique si le dossier est un dossier système. Si cette valeur est définie sur true, le dossier lui-même est en lecture seule, bien que les dossiers puissent être créés en tant qu’enfants.

## Créer et mettre à jour

[La création de dossiers](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/createFolderUsingPOST) est simple et est exécutée avec un POST application/x-www-form-urlencoded qui possède deux paramètres requis, &quot;name&quot;, une chaîne et &quot;parent&quot;, le parent dans lequel créer le dossier, qui est un objet JSON incorporé avec deux membres, id et type, Dossier ou Programme, selon le type du dossier cible. Vous pouvez également inclure une &quot;description&quot;, une chaîne, qui peut contenir jusqu’à 2 000 caractères.

```
POST /rest/asset/v1/folders.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Les mises à jour des dossiers sont effectuées par le biais d’un point de terminaison distinct. La description, le nom et `isArchive` sont des paramètres facultatifs pour la mise à jour. Si `isArchive` est modifié par une mise à jour, le dossier est archivé, s’il est modifié sur true ou non archivé, s’il est remplacé par false, dans l’interface utilisateur de Marketo. Les programmes ne peuvent pas être mis à jour avec cette API.

```
POST /rest/asset/v1/folder/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Des suppressions peuvent être effectuées sur des dossiers uniques s’ils sont vides, ce qui signifie qu’ils ne contiennent pas de ressources ni de sous-dossiers. Si un dossier est de type Programme ou si le champ isSystem est défini sur true, il ne peut pas être supprimé avec cette API.

```
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
