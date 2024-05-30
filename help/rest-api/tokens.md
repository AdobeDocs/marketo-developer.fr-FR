---
title: "Jetons"
feature: REST API, Tokens
description: "Gestion des jetons dans Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 4%

---


# Jetons

[Référence du point de fin du jeton](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

Les jetons dans Marketo sont des chaînes spéciales similaires aux codes à raccourcis qui sont remplacés par un élément de données distinct au moment de l’exécution. Plusieurs types de jetons sont disponibles dans Marketo, mais seuls Mes jetons peuvent être édités via l’API. Mes jetons sont des jetons enfants qui sont locaux à un dossier ou à un programme spécifique. Les jetons peuvent être lus, créés et supprimés via l’API.

## Type de données

Les jetons peuvent être créés avec les types de données suivants :

| Type | Description |
|---------------|----------------------------------------------------|
| Date | Valeur de date du formulaire &quot;aaaa-MM-jj&quot;. |
| Nombre | Nombre entier ou flottant |
| Texte complet | Chaîne de HTML |
| Évaluation | Entier signé 32 bits |
| campagne sfdc | Utilisé dans l’intégration de la gestion de campagne Salesforce |
| Texte | Chaîne de texte |


Il s’agit des seuls types de données pouvant être utilisés lors de la création d’un jeton via l’API.

## Requête

[Obtention de jetons par ID de dossier](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/getTokensByFolderIdUsingGET) prend une `id` comme paramètre de chemin d’accès d’un type Programme ou Dossier. Ce type est spécifié par la variable `folderType` .

```curl
GET /rest/asset/v1/folder/{id}/tokens.json?folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "4fbe#14e27fc9bbf",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "AprilFool - deverly",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

## Créer et mettre à jour

La variable [Créer un jeton](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/addTokenTOFolderUsingPOST) endpoint crée des jetons ou, s’ils existent, les met à jour avec les valeurs envoyées. Les jetons sont créés dans le contexte d&#39;un dossier ou d&#39;un programme. La variable `id` Le paramètre path est l’identifiant du dossier auquel le jeton sera associé. La variable `name`, `type`, `value`, et `folderType` sont tous les paramètres requis du jeton. Les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON. La variable `name` ne peut pas dépasser 50 caractères.

```
POST /rest/asset/v1/folder/{id}/tokens.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=April Fools&type=date&value=2015-04-01&folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "e3c2#14e280db5dc",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "April Fools",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

## Supprimer

[Supprimer le jeton par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/deleteTokenByNameUsingPOST) prend un id comme paramètre de chemin d’accès d’un type Programme ou Dossier. Ce type est spécifié par la variable `folderType` . Les jetons sont supprimés en fonction de leur dossier parent, le `name`, et la variable `type` du jeton, dont chacun est requis. Les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON.

```
POST /rest/asset/v1/folder/{id}/tokens/delete.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=AprilFool - deverly&type=date&folderType=Program
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "12ed2#14e2800f89c",
    "result": [
        {
            "id": 416
        }
    ]
}
```
