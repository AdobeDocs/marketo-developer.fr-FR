---
title: Fragments
feature: REST API
description: Utilisez l’API REST Marketo Asset pour interroger, créer, mettre à jour, cloner, supprimer, approuver et inspecter les dépendances des fragments.
exl-id: 9dd532d1-1dd7-4581-86dd-1943fab66cbb
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 9%

---

# Fragments

Les fragments sont des ressources de contenu réutilisables qui peuvent être référencées par d’autres ressources, telles que des e-mails.

## Accès

Les points d’entrée décrits dans cet article nécessitent un jeton d’accès :

```text
?access_token=<access_token>
```

Les requêtes nécessitent également l’en-tête `x-app-type` :

```text
x-app-type: <app-type>
```

## Requête

Vous pouvez récupérer des métadonnées de fragment par ID de ressource ou avec le point d’entrée de filtre.

### Par ID

#### Requête

```http
GET /rest/asset/v2/fragment/{id}
```

#### Réponse

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "fa0f#1900ab15001",
  "result": [
    {
      "id": "83",
      "name": "Hero Banner Fragment",
      "description": "Reusable hero block",
      "status": "approved"
    }
  ]
}
```

### Filtre

Le point d’entrée du filtre prend en charge la recherche dans un espace de travail et le rétrécissement des résultats avec des paramètres de requête supplémentaires. `workspaceId` est obligatoire.

tâche : faire de ce tableau un tableau
Les filtres pris en charge sont les suivants : `folderId`, `folderIds` répétées, `status` répétées, `pageIndex`, `pageSize`, `createdBy`, `createdAtStart`, `createdAtEnd`, `modifiedBy`, `modifiedAtStart`, `modifiedAtEnd`, `name`, `fragmentType`, `sortKey`, `sortOrder`, `isCreatedByMe`, `isModifiedByMe`, `scriptEngine`, `isValueNonNullable`, `includeArchived` et.

#### Requête

```http
GET /rest/asset/v2/fragment/filter?workspaceId=1001&fragmentType=email&pageIndex=0&pageSize=20
```

#### Réponse

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "f9cc#1900ab1504a",
  "result": [
    {
      "id": "83",
      "name": "Hero Banner Fragment",
      "status": "approved"
    }
  ]
}
```

Utilisez le paramètre `name` lorsque vous devez rechercher un fragment par nom.

## Créer

Créez un fragment en envoyant une payload JSON. `name`, `appData` et `settings` sont requis. `settings` doit inclure `fragmentType` et `supportedChannels`.

### Requête

```http
POST /rest/asset/v2/fragment
Content-Type: application/json
```

```json
{
  "name": "Hero Banner Fragment",
  "description": "Reusable hero block",
  "appData": {
    "workspaceId": "1001",
    "folderId": "395",
    "editorType": "fragment"
  },
  "settings": {
    "fragmentType": "email",
    "fragmentSubType": "html",
    "supportedChannels": [
      "email"
    ]
  }
}
```

### Réponse

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "bd57#1900ab1509d",
  "result": [
    {
      "id": "13",
      "name": "Hero Banner Fragment",
      "status": "draft"
    }
  ]
}
```

Le corps de la requête peut également inclure `data`, `editorContext`, `themeId`, `appType` et `status`.

### Créer des champs

* `appData` identifie l’emplacement de stockage du fragment et la manière dont il est modifié.
* `settings.fragmentType` identifie la catégorie de fragment, telle qu’un fragment d’e-mail.
* `settings.fragmentSubType` peut être utilisé pour définir plus précisément le format du fragment.
* `settings.supportedChannels` répertorie les canaux dans lesquels le fragment peut être utilisé.

## Mise à jour

Mettre à jour un fragment par identifiant de ressource.

### Requête

```http
POST /rest/asset/v2/fragment/{id}/update
Content-Type: application/json
```

```json
{
  "name": "Hero Banner Fragment v2",
  "description": "Updated reusable hero block",
  "settings": {
    "fragmentSubType": "html",
    "supportedChannels": [
      "email"
    ]
  }
}
```

### Réponse

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "73d9#1900ab150f0",
  "result": [
    {
      "id": "13"
    }
  ]
}
```

## Gérer l’état

Les fragments utilisent un cycle de vie en version brouillon et approuvé. Utilisez le point d’entrée de transition d’état pour approuver un fragment, le désapprouver, ignorer un brouillon ou en créer un nouveau.

Les valeurs de `action` valides sont les suivantes :

* `approve`
* `unapprove`
* `discard`
* `create_draft`

### Requête

```http
POST /rest/asset/v2/fragment/state/transition
Content-Type: application/json
```

### Réponse

```json
{
  "contentId": "13",
  "action": "approve"
}
```

## Cloner

Utilisez le point d’entrée de clone pour créer une copie d’un fragment existant.

### Requête

```http
POST /rest/asset/v2/fragment/clone
Content-Type: application/json
```

### Réponse

```json
{
  "assetId": "13",
  "newAsset": {
    "name": "Hero Banner Fragment Copy",
    "description": "Cloned fragment"
  }
}
```

## Supprimer

Supprimer un fragment par ID de ressource.

### Requête

```http
POST /rest/asset/v2/fragment/{id}/delete
Content-Type: application/json
```

Ce point d’entrée prend l’identifiant de fragment dans le chemin d’accès et ne définit pas de corps de requête.

## Utilisation par

Utilisez le point d’entrée `usedby` pour récupérer les ressources qui font référence à un fragment donné.

### Requête

```http
POST /rest/asset/v2/fragment/usedby
Content-Type: application/json
```

### Réponse

```json
{
  "assetId": "13",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```
