---
title: Jetons
feature: REST API, Tokens
description: Gestion des jetons dans Marketo.
exl-id: 4f8d87d7-ba2a-4c90-8b39-4d20679d404a
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 4%

---

# Jetons

[Référence du point d’entrée du jeton](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

Les jetons dans Marketo sont des chaînes spéciales similaires aux codes à raccourcis qui sont remplacés par un élément de données distinct au moment de l’exécution. Plusieurs types de jetons sont disponibles dans Marketo, mais seuls Mes jetons peuvent être édités via l’API. Mes jetons sont des jetons enfants qui sont locaux à un dossier ou à un programme spécifique. Les jetons peuvent être lus, créés et supprimés via l’API.

## Type de données

Les jetons peuvent être créés avec les types de données suivants :

| Type | Description |
|---------------|----------------------------------------------------|
| Date | Valeur de date du formulaire &quot;aaaa-MM-jj&quot;. |
| Nombre | Nombre entier ou flottant |
| Texte complet | Chaîne d’HTML |
| Évaluation | Entier signé 32 bits |
| campagne sfdc | Utilisé dans l’intégration de la gestion de campagne Salesforce |
| Texte | Chaîne de texte |


Il s’agit des seuls types de données pouvant être utilisés lors de la création d’un jeton via l’API.

## Requête

[Obtenir des jetons par ID de dossier](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/getTokensByFolderIdUsingGET) utilise un paramètre de chemin `id` comme type de programme ou de dossier. Ce type est spécifié par le paramètre `folderType` .

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

Le point d’entrée [Create Token](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/addTokenTOFolderUsingPOST) crée des jetons ou, s’ils existent, les met à jour avec les valeurs envoyées. Les jetons sont créés dans le contexte d&#39;un dossier ou d&#39;un programme. Le paramètre de chemin d’accès `id` requis est l’identifiant du dossier auquel le jeton sera associé. `name`, `type`, `value` et `folderType` sont tous les paramètres requis du jeton. Les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON. Le champ `name` du jeton ne peut pas dépasser 50 caractères.

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

[Delete Token by Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/deleteTokenByNameUsingPOST) utilise un identifiant comme paramètre de chemin d’accès d’un type de programme ou de dossier. Ce type est spécifié par le paramètre `folderType` . Les jetons sont supprimés en fonction de leur dossier parent, de `name` et du `type` du jeton, dont chacun est requis. Les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON.

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
