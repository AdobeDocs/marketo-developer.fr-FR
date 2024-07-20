---
title: Balises
feature: REST API, Tags
description: Gestion des balises pour les programmes dans Marketo.
exl-id: 64731d1a-a749-4d6f-b336-16c733d002f0
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 2%

---

# Balises

[Référence du point d’entrée des balises](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags)

Les balises sont des champs définis par l’utilisateur pour les programmes. Chaque balise peut s’appliquer à un ou plusieurs types de programme et peut être obligatoire ou facultatif, selon la manière dont la balise a été définie. Les balises peuvent également fournir une liste des valeurs autorisées à partir desquelles sélectionner pour les utiliser.

## Requête

Les balises sont interrogées avec le modèle de ressource standard, mais ne comportent pas de point de terminaison pour Par ID. La liste des valeurs autorisées pour une balise n’est renvoyée que lorsque la balise est interrogée par son nom.

### Obtenir des balises

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

Le point d’entrée [Update Program Tag](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) permet de mettre à jour la valeur d’un type de balise donné. Le point de terminaison utilise des paramètres de chemin `id` et `tagType` qui spécifient l’ID de programme et le type de balise à mettre à jour. Un paramètre de requête `tagValue` est utilisé pour spécifier la nouvelle valeur pour le type de balise. Tous les paramètres sont obligatoires.

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

Les balises peuvent être mises à jour en masse à l’aide du point d’entrée [Mettre à jour les métadonnées du programme](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) . Vous trouverez un exemple de cela [ici](programs.md#update).

## Supprimer

Le point d’entrée [Supprimer la balise de programme](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/deleteProgramUsingPOST) vous permet de supprimer un type de balise non requis. Le point de terminaison utilise les paramètres de chemin `id` et `tagType` qui spécifient l’ID de programme et le type de balise à supprimer.

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
