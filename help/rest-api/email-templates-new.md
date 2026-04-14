---
title: Modèles d’e-mail
feature: REST API
description: Utilisez l’API REST Marketo Asset pour interroger, créer, mettre à jour, cloner, supprimer, approuver et inspecter les dépendances des modèles d’e-mail.
exl-id: 50bb0047-d6ea-4c94-a900-18c37b17a147
source-git-commit: 59684e1c5a8082ad12f1e4bfc854c0d2dde35d2a
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 9%

---

# Modèles d’e-mail

[Référence du point d’entrée du modèle d’e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates)

Les modèles d’e-mail définissent la structure et la disposition réutilisable utilisées lors de la création d’e-mails.

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

Vous pouvez récupérer les métadonnées du modèle d’e-mail par identifiant de ressource ou avec le point d’entrée de filtre.

### Par ID

#### Requête

```http
GET /rest/asset/v2/emailtemplate/{id}
```

#### Réponse

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "14f9e#1900ab14001",
  "result": [
    {
      "id": "19",
      "name": "Base Newsletter Template",
      "description": "Core responsive template",
      "status": "draft"
    }
  ]
}
```

### Filtre

Le point d’entrée du filtre prend en charge la recherche dans un espace de travail et le rétrécissement des résultats avec des paramètres de requête supplémentaires. `workspaceId` est obligatoire.

Les filtres pris en charge sont les suivants : `folderId`, `folderIds` répétées, `status` répétées, `pageIndex`, `pageSize`, `createdBy`, `createdAtStart`, `createdAtEnd`, `modifiedBy`, `modifiedAtStart`, `modifiedAtEnd`, `name`, `sortKey`, `sortOrder`, `isCreatedByMe`, `isModifiedByMe`, `scriptEngine`, `isValueNonNullable`, `includeArchived`, et.

#### Requête

```http
GET /rest/asset/v2/emailtemplate/filter?workspaceId=1001&name=Newsletter&pageIndex=0&pageSize=20
```

#### Réponse

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "33c4#1900ab1402f",
  "result": [
    {
      "id": "19",
      "name": "Base Newsletter Template",
      "status": "draft"
    }
  ]
}
```

Utilisez le paramètre `name` lorsque vous devez rechercher un modèle par nom.

## Créer

Créez un modèle d’e-mail en envoyant une payload JSON. `name` et `appData` sont requis. `appData` doit inclure au moins `folderId` ou `workspaceId`.

### Requête

```http
POST /rest/asset/v2/emailtemplate
Content-Type: application/json
```

```json
{
  "name": "Base Newsletter Template",
  "description": "Core responsive template",
  "appData": {
    "workspaceId": "1001",
    "folderId": "15",
    "editorType": "emailTemplate"
  },
  "themeId": "42"
}
```

### Réponse

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "a99f#1900ab1407e",
  "result": [
    {
      "id": "1022",
      "name": "Base Newsletter Template",
      "status": "draft"
    }
  ]
}
```

Le corps de la requête peut également inclure `data`, `editorContext`, `appType` et `status`.

### Créer des champs

`appData` identifie l’emplacement de stockage du modèle et la manière dont il est modifié.

`themeId` peut être utilisé pour associer un thème au modèle, le cas échéant.

## Mise à jour

Mettre à jour un modèle par ID de ressource

### Requête

```http
POST /rest/asset/v2/emailtemplate/{id}/update
Content-Type: application/json
```

```json
{
  "name": "Base Newsletter Template v2",
  "description": "Updated responsive template",
  "appData": {
    "folderId": "15"
  }
}
```

### Réponse

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "cf10#1900ab140b3",
  "result": [
    {
      "id": "1022"
    }
  ]
}
```

## Gérer l’état

Les modèles d’e-mail utilisent un cycle de vie brouillon et approuvé. Utilisez le point d’entrée de transition d’état pour approuver un modèle, le désapprouver, ignorer un brouillon ou en créer un nouveau.

Les valeurs de `action` valides sont les suivantes :

- `approve`
- `unapprove`
- `discard`
- `create_draft`

### Requête

```http
POST /rest/asset/v2/emailtemplate/state/transition
Content-Type: application/json
```

### Réponse

```json
{
  "contentId": "1022",
  "action": "approve"
}
```

## Cloner

Utilisez le point d’entrée de clone pour créer une copie d’un modèle existant.

### Requête

```http
POST /rest/asset/v2/emailtemplate/clone
Content-Type: application/json
```

### Réponse

```json
{
  "assetId": "1022",
  "newAsset": {
    "name": "Base Newsletter Template Copy",
    "description": "Cloned template"
  }
}
```

## Supprimer

Supprimer un modèle par ID de ressource.

### Requête

```http
POST /rest/asset/v2/emailtemplate/{id}/delete
Content-Type: application/json
```

Ce point d’entrée prend l’identifiant du modèle dans le chemin d’accès et ne définit pas de corps de requête.

## Utilisation par

Utilisez le point d’entrée `usedby` pour récupérer les ressources qui font référence à un modèle donné.

### Requête

```http
POST /rest/asset/v2/emailtemplate/usedby
Content-Type: application/json
```

### Réponse

```json
{
  "assetId": "1022",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```

## Notes

Les points d’entrée de requête renvoient des métadonnées pour la ressource. Utilisez la référence de point d’entrée pour le schéma de réponse complet et les propriétés spécifiques à un environnement.
