---
title: Programmes
feature: REST API, Programs
description: Guide des programmes Marketo pour l’API REST Asset qui couvre les types, les canaux, les balises, les statuts de membre et les points d’entrée pour obtenir par identifiant ou nom, parcourir et filtrer par statut.
exl-id: 30700de2-8f4a-4580-92f2-7036905deb80
TQID: https://experienceleague.adobe.com/5ILyahSn3Pp-lF6YPogVnkXjXP-QLtEmyLm7iKMIgo0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: ebde5b41-29c9-4f5e-9ef6-1197e85409e3
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 718
ht-degree: 2%

---

# Programmes

[Référence des points d’entrée de programmes](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs)

Les programmes organisent des activités marketing Marketo et effectuent le suivi de l’adhésion et du succès des prospects pour chaque initiative marketing. Un programme peut contenir la plupart des types de ressources, à l’exception des pages de destination, des modèles d’e-mail et des fichiers.

## Types de programmes

Il existe cinq principaux types de programmes dans Marketo :

- Par défaut
- Événement
- Événement avec webinaire
- Engagement
- E-mail

Les programmes d’engagement peuvent contenir tous les autres types de programme. Par défaut, Événement et Événement avec les programmes de webinaire ne peuvent contenir que des programmes de messagerie.

Chaque programme a un canal. Le canal définit les statuts des membres du programme disponibles et peut être récupéré avec l’API Get Channels.

Un programme peut également comporter des balises. Les balises sont des champs personnalisables qui peuvent être facultatifs ou obligatoires pour un type de programme. Chaque balise utilise une valeur d’une liste configurée dans Marketo Admin.

## Requête

Interroger les programmes par identifiant, nom, navigation ou type de balise et valeur. Utilisez [Get Tag Types](https://developer.adobe.com/marketo-apis/api/asset#operation/getTagTypesUsingGET) pour récupérer les balises et les valeurs disponibles.

### Par Id

Le point d’entrée [Obtenir le programme par ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getProgramByIdUsingGET) nécessite un paramètre de chemin d’accès `id`.

Vous pouvez obtenir l’ID du programme à partir de son URL d’interface utilisateur, par exemple `https://app-\*\*\*.marketo.com/#PG1001A1`. Dans cet exemple, l’ID est `1001`, entre le premier et le deuxième ensemble de lettres.

```http
GET /rest/asset/v1/program/{id}.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#14db037ec71",
    "result": [
        {
            "id": 1107,
            "name": "AAA2QueryProgramName",
            "description": "AssetAPI: getProgram tests",
            "createdAt": "2015-05-21T22:45:13Z+0000",
            "updatedAt": "2015-05-21T22:45:13Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1107A1",
            "type": "Default",
            "channel": "Online Advertising",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "",
            "workspace": "Default",
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                }
            ],
            "costs": null,
            "headStart": false
        }
    ]
}
```

### Par nom

Le point d’entrée [Obtenir le programme par nom](https://developer.adobe.com/marketo-apis/api/asset) nécessite un paramètre de requête `name`. Définissez les paramètres booléens facultatifs `includeTags` et `includeCosts` pour renvoyer les balises et les coûts, respectivement.

```http
GET /rest/asset/v1/program/byName.json?name=TestProgramName&includeTags=true
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#14db03e070c",
    "result": [
        {
            "id": 1107,
            "name": "AAA2QueryProgramName",
            "description": "AssetAPI: getProgram tests",
            "createdAt": "2015-05-21T22:45:13Z+0000",
            "updatedAt": "2015-05-21T22:45:13Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1107A1",
            "type": "Default",
            "channel": "Online Advertising",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "",
            "workspace": "Default",
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                }
            ],
            "costs": null,
            "headStart": false
        }
    ]
}
```

### Parcourir

Utilisez le point d’entrée [Obtenir les programmes](https://developer.adobe.com/marketo-apis/api/asset#operation/browseProgramsUsingGET) pour parcourir les programmes.

Le paramètre facultatif `status` filtre les programmes d’engagement et d’e-mail par statut. Les valeurs valides sont `on` et `off` pour les programmes d’engagement et `unlocked` pour les programmes de messagerie électronique.

Le paramètre facultatif `maxReturn` contrôle le nombre de programmes renvoyés. La valeur par défaut est 20 et la valeur maximale est 200. Utilisez le paramètre `offset` facultatif pour la pagination ; sa valeur par défaut est 0.

Ce point d’entrée ne renvoie pas de balises de programme. Récupérez les balises avec [Obtenir les programmes par ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getProgramByIdUsingGET) ou [Obtenir les programmes par nom](https://developer.adobe.com/marketo-apis/api/asset#operation/getProgramByNameUsingGET).

```http
GET /rest/asset/v1/programs.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "7a39#1511bf8a41c",
    "result": [
        {
            "id": 1035,
            "name": "clone it",
            "description": "",
            "createdAt": "2015-11-18T15:25:35Z+0000",
            "updatedAt": "2015-11-18T15:25:46Z+0000",
            "url": "https://app-devlocal1.marketo.com/#NP1035A1",
            "type": "Engagement",
            "channel": "Nurture",
            "folder": {
                "type": "Folder",
                "value": 28,
                "folderName": "Nurturing"
            },
            "status": "on",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1032,
            "name": "email prog",
            "description": "",
            "createdAt": "2015-11-18T14:56:28Z+0000",
            "updatedAt": "2015-11-18T14:56:28Z+0000",
            "url": "https://app-devlocal1.marketo.com/#EBP1032A1",
            "type": "Email",
            "channel": "Email Send",
            "folder": {
                "type": "Folder",
                "value": 26,
                "folderName": "Data Management"
            },
            "status": "unlocked",
            "workspace": "Default",
            "headStart": false
        }
    ]
}
```

### Par Période

Utilisez les paramètres `earliestUpdatedAt` et `latestUpdatedAt` avec [Obtenir les programmes](https://developer.adobe.com/marketo-apis/api/asset#operation/browseProgramsUsingGET) pour définir des limites de date et d’heure basses et élevées. Le point d’entrée renvoie les programmes créés ou mis à jour dans la plage.

```http
GET /rest/asset/v1/programs.json?earliestUpdatedAt=2017-01-01T00:00:00-05:00&latestUpdatedAt=2017-01-30T00:00:00-05:00
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1225a#15f82a83875",
    "warnings": [],
    "result": [
        {
            "id": 1070,
            "name": "Bulk Import - Test",
            "description": "",
            "createdAt": "2017-01-13T19:34:17Z+0000",
            "updatedAt": "2017-01-13T19:34:18Z+0000",
            "url": "https://app-abm.marketo.com/#PG1070A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 637,
                "folderName": "Avention"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1069,
            "name": "Program With Email",
            "description": "",
            "createdAt": "2017-01-03T22:53:14Z+0000",
            "updatedAt": "2017-01-03T22:53:15Z+0000",
            "url": "https://app-abm.marketo.com/#EBP1069A1",
            "type": "Email",
            "channel": "Email Send",
            "folder": {
                "type": "Folder",
                "value": 621,
                "folderName": "Smartling"
            },
            "status": "unlocked",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1071,
            "name": "Program with Guided Landing Page Template",
            "description": "",
            "createdAt": "2017-01-24T22:59:21Z+0000",
            "updatedAt": "2017-01-24T22:59:22Z+0000",
            "url": "https://app-abm.marketo.com/#PG1071A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 621,
                "folderName": "Smartling"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1047,
            "name": "ReachForce List Update",
            "description": "",
            "createdAt": "2016-05-24T19:38:35Z+0000",
            "updatedAt": "2017-01-13T19:28:09Z+0000",
            "url": "https://app-abm.marketo.com/#PG1047A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 407,
                "folderName": "Everly Tests"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        }
    ]
}
```

### Par type de balise

Le point d’entrée [Obtenir les programmes par balise](https://developer.adobe.com/marketo-apis/api/asset#operation/getProgramListByTagUsingGET) renvoie les programmes qui correspondent au type et à la valeur de balise spécifiés.

Les paramètres `tagType` et `tagValue` sont requis. L’entier facultatif `maxReturn` contrôle le nombre de programmes renvoyés ; la valeur par défaut est 20 et la valeur maximale est 200. Utilisez le `offset` entier facultatif pour la pagination ; sa valeur par défaut est 0. Les résultats sont renvoyés dans un ordre aléatoire.

```http
GET /rest/asset/v1/program/byTag.json?tagType=Presenter&tagValue=Dennis
```

```json
{
    "success" : true,
    "warnings" : [],
    "errors" : [],
    "requestId" : "13b6d#152b38d5be4",
    "result" : [{
            "id" : 1004,
            "name" : "It's a Program",
            "description" : "",
            "createdAt" : "2013-02-26T00:37:37Z+0000",
            "updatedAt" : "2013-03-11T15:32:02Z+0000",
            "url" : "https://app-sjst.marketo.com/#PG1004A1",
            "type" : "Default",
            "channel" : "Email Blast",
            "folder" : {
                "type" : "Folder",
                "value" : 38,
                "folderName" : "Test"
            },
            "status" : "",
            "workspace" : "Default",
            "tags" : [{
                    "tagType" : "Presenter",
                    "tagValue" : "Dennis"
                }
            ],
                        "headStart": false
    ]
}
```

## Créer et mettre à jour

[La création](https://developer.adobe.com/marketo-apis/api/asset#operation/createProgramUsingPOST) d’un programme nécessite `folder`, `name`, `type` et `channel`. Les paramètres facultatifs sont `description`, `costs` et `tags`. Certains abonnements nécessitent des balises pour des types de programmes spécifiques. Utilisez Get Tags pour vérifier les exigences relatives aux instances.

Lors de la [mise à jour](https://developer.adobe.com/marketo-apis/api/asset#operation/updateProgramUsingPOST), vous ne pouvez modifier que la description, le nom, le `tags` et le `costs`. Vous ne pouvez définir le canal et le type que lors de la création. Définir `costsDestructiveUpdate` sur `true` efface tous les coûts existants et les remplace par les coûts inclus dans la demande.

Lors de la création ou de la mise à jour d’un programme de messagerie, un `startDate` et un `endDate` peuvent également être transmis en tant que date/heure UTC :

`"startDate": "2022-10-19T15:00:00.000Z"`
`"endDate": "2022-10-19T15:00:00.000Z"`

### Créer

```http
POST /rest/asset/v1/programs.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=API Test Program&folder={"id":1035,"type":"Folder"}&description=Sample API Program&type=Default&channel=Email Blast&costs=[{"startDate":"2015-01-01","cost":2000}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "d505#14d9bd96352",
    "result": [
        {
            "id": 1207,
            "name": "newProgram",
            "description": "This is a test",
            "createdAt": "2015-05-28T18:47:15Z+0000",
            "updatedAt": "2015-05-28T18:47:15Z+0000",
            "url": "https://app-devlocal1.marketo.com/#ME1207A1",
            "type": "Event",
            "channel": "channelOne",
            "folder": {
                "type": "Folder",
                "value": 59,
                "folderName": "blah blah"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
            "tags": null,
            "costs": [
                {
                    "startDate":"2015-01-01",
                    "cost":2000
                }
            ]
        }
    ]
}
```

### Mise à jour

Pour ajouter des coûts de programme, ajoutez-les au tableau `costs`. Pour remplacer les coûts existants, transmettez les nouveaux coûts et définissez `costsDestructiveUpdate` sur `true`. Pour effacer tous les coûts, omettez `costs` et `costsDestructiveUpdate` sur `true`.

```http
POST /rest/asset/v1/program/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
description=This is an updated description&name=Updated Program Name&costs=[{"startDate":"2016-01-01","cost":200,"note":"Google Adwords"}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5c37#14db05608aa",
    "result": [
        {
            "id": 1110,
            "name": "Updated Program Name",
            "description": "This is a updated description",
            "createdAt": "2015-05-21T22:45:14Z+0000",
            "updatedAt": "2015-06-01T18:13:58Z+0000",
            "url": "https://app-devlocal1.marketo.com/#NP1110A1",
            "type": "Engagement",
            "channel": "Nurture",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "on",
            "workspace": "Default",
            "headStart": false,
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                },
                {
                    "tagType": "tagTypeOne",
                    "tagValue": "tagTypeValue1"
                }
            ],
            "costs": [
                {
                    "startDate": "2016-01-01",
                    "cost": 200,
                    "note": "Google Adwords"
                }
            ]
        }
    ]
}
```

## Validation

Vous pouvez approuver ou annuler l’approbation de programmes de messagerie à distance. Un programme approuvé fonctionne à son `startDate` et se termine à son `endDate`.

Avant l’approbation, définissez les deux dates et configurez un e-mail et une liste dynamique valides et approuvés dans l’interface utilisateur.

### Approuver

```http
POST /rest/asset/v1/program/{id}/approve.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#150b5bf7692",
    "result": [
        {
            "id": 11062
        }
    ]
}
```

### Désapprouver

```http
POST /rest/asset/v1/program/{id}/unapprove.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#150b5bf7692",
    "result": [
        {
            "id": 11062
        }
    ]
}
```

## Cloner

Le [clonage de programmes](https://developer.adobe.com/marketo-apis/api/asset#operation/cloneProgramUsingPOST) nécessite un nouveau nom et un nouveau dossier parent. La description est facultative. Le `name` doit être globalement unique et ne peut pas dépasser 255 caractères.

Définissez l’attribut type du paramètre `folder` sur `Folder`. Le dossier cible doit se trouver dans le même espace de travail que le programme source.

Vous ne pouvez pas utiliser cette API pour cloner des programmes in-app ou des programmes contenant des notifications push, des messages in-app, des rapports ou des ressources sociales.

```http
POST /rest/asset/v1/program/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Cloned Program - PHP&folder={"id":5562,"type":"Folder"}&description=Description
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "3a7f#14db06990cc",
    "result": [
        {
            "id": 1221,
            "name": "cloneProgram",
            "description": "This is a description for the cloned program",
            "createdAt": "2015-06-01T18:36:57Z+0000",
            "updatedAt": "2015-06-01T18:36:57Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1221A1",
            "type": "Default",
            "channel": "Blog",
            "folder": {
                "type": "Folder",
                "value": 59,
                "folderName": "blah blah"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
            "tags": null,
            "costs": null
        }
    ]
}
```

## Supprimer le programme

La suppression de programmes suit le modèle de suppression de ressources standard.

```http
POST /rest/asset/v1/program/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16501#14db042c6b7",
    "result": [
        {
            "id": 1109
        }
    ]
}
```
