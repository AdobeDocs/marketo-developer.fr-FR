---
title: Balises
feature: REST API, Tags
description: interroger les types de balises, obtenir les valeurs autorisées par nom, mettre à jour ou supprimer des balises de programme dans Marketo via l’API REST Asset, avec des exemples de requête ;
exl-id: 64731d1a-a749-4d6f-b336-16c733d002f0
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 2%

---

# Balises

[Référence du point d’entrée des balises](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags)

Les balises sont des champs définis par l’utilisateur pour les programmes. Chaque balise peut s’appliquer à un ou plusieurs types de programme et peut être obligatoire ou facultatif, selon la définition de la balise. Les balises peuvent également fournir une liste de valeurs autorisées qui doivent être sélectionnées pour une utilisation.

## Requête

Les balises sont interrogées avec le modèle de ressource standard, mais n’ont pas de point d’entrée pour l’ID By. La liste des valeurs autorisées pour une balise n’est renvoyée que lorsque la balise est interrogée par son nom.

### Obtenir les balises

```
GET /rest/asset/v1/tagTypes.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1488a#1504ecfccf8",
    "result": [
        {
            "tagType": "AAA1 Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": true
        },
        {
            "tagType": "AAA2 Required Event Tag Type",
            "applicableProgramTypes": "[event]",
            "required": true
        },
        {
            "tagType": "AAA3 Not Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": false
        }
    ]
}
```

### Par nom

```
GET /rest/asset/v1/tagType/byName.json?name=AAA1 Required Tag Type
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "8a44#1504ed0da2f",
    "result": [
        {
            "tagType": "AAA1 Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": true,
            "allowableValues": "[AAA1 RT1, AAA1 RT2, AAA1 RT3, AAA1 RT4]"
        }
    ]
}
```

## Mise à jour 

Le point d’entrée [Mettre à jour la balise de programme](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) vous permet de mettre à jour la valeur d’un type de balise donné. Le point d’entrée prend un `id` et `tagType` paramètres de chemin d’accès qui spécifient l’identifiant du programme et le type de balise à mettre à jour. Un paramètre de requête `tagValue` est utilisé pour spécifier la nouvelle valeur pour le type de balise. Tous les paramètres sont requis.

```
POST /rest/asset/v1/program/{id}/tag/{tagType}.json?tagValue=David
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "fd84#17f84a885a6",
    "warnings": [],
    "result": [
        {
            "id": 1067
        }
    ]
}
```

Les balises peuvent être mises à jour en masse à l’aide du point d’entrée [Mettre à jour les métadonnées de programme](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST). Vous trouverez un exemple [ici](programs.md#update).

## Supprimer

Le point d’entrée [Supprimer la balise de programme](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/deleteProgramUsingPOST) vous permet de supprimer un type de balise superflu. Le point d’entrée prend les paramètres de chemin d’accès `id` et `tagType` qui spécifient l’identifiant du programme et le type de balise à supprimer.

```
POST /rest/asset/v1/program/{id}/tag/{tagType}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d998#17f84ad36a7",
    "warnings": [],
    "result": [
        {
            "id": 1067
        }
    ]
}
```
