---
title: Jetons
feature: REST API, Tokens
description: Gestion des jetons dans Marketo.
exl-id: 4f8d87d7-ba2a-4c90-8b39-4d20679d404a
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 4%

---

# Jetons

[Référence de point d’entrée de jeton](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

Les jetons dans Marketo sont des chaînes spéciales similaires à des shortcodes qui sont remplacés par un élément de données distinct au moment de l’exécution. Plusieurs types de jetons sont disponibles dans Marketo, mais seuls Mes jetons peuvent être modifiés à l’aide de l’API. Mes jetons sont des jetons enfants qui sont locaux à un dossier ou un programme spécifique. Les jetons peuvent être lus, créés et supprimés à l’aide de l’API .

## Type de données

Les jetons peuvent être créés avec les types de données suivants :

| Type | Description |
|---------------|----------------------------------------------------|
| Date | Valeur de date du formulaire « aaaa-MM-jj » |
| Nombre | Nombre entier ou à virgule flottante |
| Texte complet | Une chaîne HTML |
| Évaluation | Nombre entier 32 bits signé |
| sfdc campaign | Utilisé dans l’intégration de la gestion de campagnes Salesforce |
| texte | Chaîne de texte |

Il s’agit des seuls types de données qui peuvent être utilisés lors de la création d’un jeton via l’API.

## Requête

La méthode [Get Tokens by Folder Id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/getTokensByFolderIdUsingGET) utilise un `id` comme paramètre de chemin d’accès d’un type de programme ou de dossier. Ce type est spécifié par le paramètre `folderType`.

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

Le point d’entrée [Créer un jeton](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/addTokenTOFolderUsingPOST) crée des jetons ou, s’ils existent, les met à jour avec les valeurs envoyées. Les jetons sont créés dans le contexte d’un dossier ou d’un programme. Le paramètre de chemin d’accès `id` obligatoire est l’identifiant du dossier auquel le jeton sera associé. Les paramètres `name`, `type`, `value` et `folderType` sont tous des paramètres requis du jeton. Les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON. Le champ `name` du jeton ne peut pas dépasser 50 caractères.

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

Le paramètre [Supprimer le jeton par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/deleteTokenByNameUsingPOST) prend un identifiant en tant que paramètre de chemin d’accès d’un type de programme ou de dossier. Ce type est spécifié par le paramètre `folderType`. Les jetons sont supprimés en fonction de leur dossier parent, du `name` et de la `type` du jeton, chacun d’eux étant obligatoire. Les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON.

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
