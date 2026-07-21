---
title: Jetons
feature: REST API, Tokens
description: Gérez mes jetons Marketo avec l’API REST de ressources. Voir les types de données pris en charge, obtenir par dossier ou programme, créer ou mettre à jour par POST codé par formulaire et supprimer par nom.
exl-id: 4f8d87d7-ba2a-4c90-8b39-4d20679d404a
TQID: https://experienceleague.adobe.com/uqOpu2vDuiQiZhILKuxZJQGadd0K14zwIaAdmNfK1-I
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 290
ht-degree: 4%

---

# Jetons

[Référence du point d’entrée du jeton](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens)

Les jetons sont des chaînes que Marketo remplace par d’autres données au moment de l’exécution. L’API ne peut modifier que Mes jetons, qui sont des jetons enfants locaux à un dossier ou un programme.

Utilisez l’API Tokens pour lire, créer, mettre à jour et supprimer mes jetons.

## Type de données

Les jetons peuvent être créés avec les types de données suivants :

| Type | Description |
| --- | --- |
| Date | Valeur de date du formulaire « aaaa-MM-jj » |
| Nombre | Nombre entier ou à virgule flottante |
| Texte complet | Une chaîne HTML |
| Évaluation | Nombre entier 32 bits signé |
| sfdc campaign | Utilisé dans l’intégration de la gestion de campagnes Salesforce |
| texte | Chaîne de texte |

L’API prend uniquement en charge ces types de données lors de la création d’un jeton.

## Requête

[Get Tokens by Folder ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/getTokensByFolderIdUsingGET) utilise l’identifiant d’un programme ou d’un dossier comme paramètre de chemin d’accès. Utilisez le paramètre `folderType` pour spécifier le type.

```http
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

Le point d’entrée [Créer un jeton](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/addTokenTOFolderUsingPOST) crée un jeton ou met à jour un jeton existant avec les valeurs envoyées. Les jetons appartiennent à un dossier ou à un programme.

Le paramètre de chemin d’accès `id` identifie le dossier parent. Les paramètres `name`, `type`, `value` et `folderType` sont requis. Transmettez les données en tant que `x-www-form-urlencoded` POST, et non en tant que JSON. Le jeton `name` ne peut pas dépasser 50 caractères.

```http
POST /rest/asset/v1/folder/{id}/tokens.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

[Supprimer le jeton par nom](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/deleteTokenByNameUsingPOST) utilise l’identifiant d’un programme ou d’un dossier comme paramètre de chemin d’accès. Utilisez `folderType` pour spécifier le type.

Le dossier parent, le `name` de jeton et le `type` de jeton sont requis. Transmettez les données en tant que `x-www-form-urlencoded` POST, et non en tant que JSON.

```http
POST /rest/asset/v1/folder/{id}/tokens/delete.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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
