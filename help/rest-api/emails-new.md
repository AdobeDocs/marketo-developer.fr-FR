---
title: E-mails
feature: REST API
description: Utilisez l’API REST Marketo Asset pour interroger, créer, mettre à jour, cloner, supprimer, approuver et inspecter les dépendances des ressources d’e-mail.
exl-id: b41a3ae5-2b25-4103-84b4-320fc2c44bd6
source-git-commit: 59684e1c5a8082ad12f1e4bfc854c0d2dde35d2a
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 5%

---

# E-mails

[Référence du point d’entrée de courrier électronique](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails_New)

Les e-mails sont des enregistrements de ressources qui définissent les métadonnées des messages, la configuration du contenu, les paramètres et le statut d’approbation.

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

Vous pouvez récupérer les métadonnées de l’e-mail par `id` de ressource ou avec le point d’entrée de filtre.

### Par ID

#### Requête

```http
GET /rest/asset/v2/email/{id}
```

#### Réponse

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7b3c#1900ab12cd3",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "description": "Main announcement email",
      "status": "draft"
    }
  ]
}
```

### Filtre

Le point d’entrée du filtre prend en charge la recherche dans un espace de travail et le rétrécissement des résultats avec des paramètres de requête supplémentaires.

`workspaceId` est obligatoire.

Paramètres de requête pris en charge :

| Paramètre | Description |
| - | - |
| `workspaceId` | Identifiant d’espace de travail obligatoire utilisé pour définir la portée de la requête de filtre. |
| `folderId` | Filtre les résultats dans un seul dossier. |
| `folderIds` | Paramètre répété qui filtre les résultats sur plusieurs dossiers. |
| `status` | Paramètre répété qui filtre un ou plusieurs statuts d’e-mail. |
| `pageIndex` | Index de page basé sur zéro pour les résultats paginés. |
| `pageSize` | Nombre maximal de résultats à renvoyer par page. |
| `createdBy` | Filtre les résultats en fonction de l’utilisateur créateur. |
| `createdAtStart` | Renvoie les ressources créées le ou après cet horodatage. |
| `createdAtEnd` | Renvoie les ressources créées le ou avant cet horodatage. |
| `modifiedBy` | Filtre les résultats en fonction du dernier utilisateur ayant effectué la modification. |
| `modifiedAtStart` | Renvoie les ressources modifiées le ou après cet horodatage. |
| `modifiedAtEnd` | Renvoie les ressources modifiées le ou avant cet horodatage. |
| `name` | Filtre les résultats par nom d’e-mail. |
| `sortKey` | Sélectionne le champ utilisé pour trier les résultats. |
| `sortOrder` | Définit le sens du tri. |
| `isCreatedByMe` | Limite les résultats aux ressources créées par l’utilisateur actuel. |
| `isModifiedByMe` | Limite les résultats aux ressources modifiées en dernier par l’utilisateur actuel. |
| `templateId` | Filtre les résultats par identifiant de modèle. |
| `scriptEngine` | Filtre les résultats en fonction de la configuration du moteur de script. |
| `isValueNonNullable` | Filtres basés sur si la valeur ne peut pas contenir la valeur null. |
| `includeArchived` | Inclut les ressources archivées dans les résultats. |

#### Requête

```http
GET /rest/asset/v2/email/filter?workspaceId=1001&name=Spring%20Launch&status=draft&status=approved&pageIndex=0&pageSize=20
```

#### Réponse

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "5dd1#1900ab13011",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "status": "draft"
    }
  ]
}
```

Utilisez le paramètre `name` lorsque vous devez rechercher un e-mail par nom.

## Créer

Créez un e-mail en envoyant une payload JSON. `name`, `appData` et `headers` sont requis. `headers.subject` est obligatoire et `appData` devez inclure au moins l’un des éléments `folderId`, `workspaceId` ou `programId`.

### Requête

```http
POST /rest/asset/v2/email
Content-Type: application/json
```

```json
{
  "name": "Spring Launch Email",
  "description": "Main announcement email",
  "appData": {
    "workspaceId": "1001",
    "folderId": "2002",
    "editorType": "email"
  },
  "headers": {
    "subject": "Introducing the Spring Launch",
    "fromName": "Marketing Team",
    "fromEmail": "marketing@example.com",
    "replyEmail": "reply@example.com",
    "preheader": "See what changed this quarter",
    "ccEmails": [
      "owner@example.com"
    ]
  },
  "settings": {
    "isOperational": false,
    "isTextOnly": false,
    "isWebPageView": true,
    "enableUrlTracking": true
  },
  "templateId": "3003"
}
```

### Réponse

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "238c#1900ab1313f",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "status": "draft"
    }
  ]
}
```

Le corps de la requête peut également inclure `data`, `editorContext`, `themeId`, `appType` et `status`.

### Créer des champs

* `appData` identifie l’emplacement de création de l’e-mail et la manière dont il est modifié.
* `headers` contient les valeurs de l’en-tête du message, notamment l’objet, l’expéditeur, l’adresse de réponse, le pré-en-tête et les destinataires CC facultatifs.
* `settings` contrôle le comportement opérationnel et de rendu, tel que le mode texte seul, l’affichage de pages web et le suivi d’URL.
* `templateId` identifie le modèle d’e-mail utilisé lors de la création de l’e-mail.

## Mise à jour

Mettre à jour un email par identifiant de ressource. Le corps de la requête utilise le schéma `UpdateEmailRequest` et toutes les propriétés sont facultatives, de sorte que vous ne pouvez envoyer que les champs que vous souhaitez modifier.

### Requête

```http
POST /rest/asset/v2/email/{id}/update
Content-Type: application/json
```

```json
{
  "description": "Updated announcement email",
  "headers": {
    "subject": "Spring Launch Is Live",
    "preheader": "Read the latest release notes"
  },
  "settings": {
    "enableUrlTracking": true,
    "isWebPageView": true
  }
}
```

### Réponse

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "9fd3#1900ab13210",
  "result": [
    {
      "id": "1017"
    }
  ]
}
```

## Gérer l’état

Les e-mails utilisent un cycle de vie brouillon et approuvé. Utilisez le point d’entrée de transition d’état pour approuver un e-mail, le désapprouver, ignorer un brouillon ou en créer un nouveau.

Les valeurs de `action` valides sont les suivantes :

* `approve`
* `unapprove`
* `discard`
* `create_draft`

### Requête

```http
POST /rest/asset/v2/email/state/transition
Content-Type: application/json
```

### Réponse

```json
{
  "contentId": "1017",
  "action": "approve"
}
```

## Cloner

Utilisez le point d’entrée de clone pour créer une copie d’un e-mail existant.

### Requête

```http
POST /rest/asset/v2/email/clone
Content-Type: application/json
```

### Réponse

```json
{
  "assetId": "1017",
  "newAsset": {
    "name": "Spring Launch Email Copy",
    "description": "Cloned from Spring Launch Email"
  }
}
```

## Supprimer

Supprimer un e-mail par ID de ressource.

### Requête

```http
POST /rest/asset/v2/email/{id}/delete
Content-Type: application/json
```

Ce point d’entrée prend la ressource `id` dans le chemin d’accès et ne définit pas de corps de requête.

## Utilisation par

Utilisez le point d’entrée `usedby` pour récupérer les ressources qui font référence à un e-mail donné.

### Requête

```http
POST /rest/asset/v2/email/usedby
Content-Type: application/json
```

### Réponse

```json
{
  "assetId": "1017",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```

## Notes

Les points d’entrée de requête renvoient des métadonnées pour la ressource. Utilisez la référence de point d’entrée pour le schéma complet des champs disponibles et des propriétés spécifiques à un environnement.
