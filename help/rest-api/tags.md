---
title: Balises
feature: REST API, Tags
description: interroger les types de balises, obtenir les valeurs autorisées par nom, mettre à jour ou supprimer des balises de programme dans Marketo via l’API REST Asset, avec des exemples de requête ;
exl-id: 64731d1a-a749-4d6f-b336-16c733d002f0
TQID: https://experienceleague.adobe.com/zjdyfoofVWytE0Q-K4lk598jmleTSFOD7tSRqeAHsjk
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 227
ht-degree: 2%

---

# Balises

[Référence du point d’entrée des balises](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags)

Les balises sont des champs définis par l’utilisateur pour les programmes. Une balise peut s’appliquer à un ou plusieurs types de programme et peut être obligatoire ou facultatif. Une balise peut également définir une liste de valeurs autorisées parmi lesquelles les utilisateurs doivent sélectionner.

## Requête

Interroger les balises avec le modèle de ressource standard. Les balises n’ont pas de point d’entrée By Id. Pour récupérer les valeurs autorisées pour une balise, interrogez la balise par son nom.

### Obtenir les balises

```http
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

```http
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

Utilisez le point d’entrée [Mettre à jour la balise de programme](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST) pour mettre à jour la valeur d’un type de balise. Tous les paramètres sont requis :

- Le paramètre de chemin d’accès `id` spécifie l’ID du programme.
- Le paramètre de chemin d’accès `tagType` spécifie le type de balise à mettre à jour.
- Le paramètre de requête `tagValue` spécifie la nouvelle valeur.

```http
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

Pour mettre à jour plusieurs balises, utilisez le point d’entrée [Mettre à jour les métadonnées du programme](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST). Voir l’exemple dans la section [Mise à jour des programmes](programs.md#update).

## Supprimer

Utilisez le point d’entrée [Supprimer la balise de programme](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/deleteProgramUsingPOST) pour supprimer un type de balise superflu. Le paramètre `id` path spécifie l’ID de programme, tandis que le paramètre `tagType` path spécifie le type de balise à supprimer.

```http
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
