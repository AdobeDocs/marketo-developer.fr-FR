---
title: "Canaux"
feature: REST API
description: "Configuration des données de canaux avec les API Marketo."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '125'
ht-degree: 2%

---


# Canaux

[Référence du point de terminaison des canaux](https://developer.adobe.com/marketo-apis/api/asset/#tag/Channels)

Les canaux sont un champ standard et obligatoire pour tous les types de programmes. Chaque type de canal ne peut être utilisé qu’avec la variable `applicableProgramType` et fournit la liste des statuts de programme disponibles valides pour les membres du programme dans chaque programme. Si les statuts de programme d’un canal sont modifiés après la création d’un programme, la liste des statuts de programme vers lesquels une piste peut être modifiée correspondra à la liste donnée par le canal à ce moment-là, mais ne modifiera pas rétroactivement l’état du programme pour les enregistrements d’appartenance au programme existants.

## Requête

Les canaux peuvent être interrogés en tant que ressources standard, mais ne disposent pas d’un point de terminaison pour récupérer un canal par son identifiant.

### Parcourir

```
GET /rest/asset/v1/channels.json?offset=10
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "651#1504ebbbfcf",
    "result": [
        {
            "id": 3,
            "name": "Blog",
            "applicableProgramType": "program",
            "progressionStatuses": [
                {
                    "name": "Not in Program",
                    "step": 0,
                    "description": null,
                    "hidden": false,
                    "success": false
                },
                {
                    "name": "Invited",
                    "step": 10,
                    "description": null,
                    "hidden": false,
                    "success": false
                },
                {
                    "name": "Visited Booth",
                    "step": 20,
                    "description": null,
                    "hidden": false,
                    "success": false
                },
                {
                    "name": "Influenced",
                    "step": 30,
                    "description": null,
                    "hidden": false,
                    "success": true
                }
            ],
            "createdAt": "2015-07-15T11:40:57Z+0000",
            "updatedAt": "2015-07-15T11:40:57Z+0000"
        },
        {
            "id": 4,
            "name": "Online Advertising",
            "applicableProgramType": "program",
            "progressionStatuses": [
                {
                    "name": "Not in Program",
                    "step": 0,
                    "description": null,
                    "hidden": false,
                    "success": false
                },
                {
                    "name": "Invited",
                    "step": 10,
                    "description": null,
                    "hidden": false,
                    "success": false
                },
                {
                    "name": "Registered",
                    "step": 20,
                    "description": null,
                    "hidden": false,
                    "success": false
                },
                {
                    "name": "No Show",
                    "step": 30,
                    "description": null,
                    "hidden": false,
                    "success": false
                },
                {
                    "name": "Attended",
                    "step": 40,
                    "description": null,
                    "hidden": false,
                    "success": true
                }
            ],
            "createdAt": "2015-07-15T11:40:58Z+0000",
            "updatedAt": "2015-07-15T11:40:58Z+0000"
        }
    ]
}
```

### Par nom

```
GET /rest/asset/v1/channel/byName.json?name=Online Advertising
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "394c#1504eb476ed",
    "result": [
        {
            "id": 4,
            "name": "Online Advertising",
            "applicableProgramType": "program",
            "progressionStatuses": [
                {
                    "name": "Not in Program",
                    "step": 0,
                    "description": null,
                    "hidden": false,
                    "success": false
                },
                {
                    "name": "Invited",
                    "step": 10,
                    "description": null,
                    "hidden": false,
                    "success": false
                },
                {
                    "name": "Registered",
                    "step": 20,
                    "description": null,
                    "hidden": false,
                    "success": false
                },
                {
                    "name": "No Show",
                    "step": 30,
                    "description": null,
                    "hidden": false,
                    "success": false
                },
                {
                    "name": "Attended",
                    "step": 40,
                    "description": null,
                    "hidden": false,
                    "success": true
                }
            ],
            "createdAt": "2015-07-15T11:40:58Z+0000",
            "updatedAt": "2015-07-15T11:40:58Z+0000"
        }
    ]
}
```
