---
title: Programmes
feature: REST API, Programs
description: Créez et modifiez les informations du programme.
exl-id: 30700de2-8f4a-4580-92f2-7036905deb80
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 2%

---

# Programmes

[Référence du point de terminaison de programmes](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs)

Les programmes sont une composante essentielle de l’organisation des activités marketing de Marketo. Ils peuvent être parents de la plupart des types de ressources et permettre le suivi de l’adhésion et du succès des pistes dans le cadre d’initiatives marketing individuelles. Les programmes peuvent être des parents de tous les types d’enregistrements, à l’exception de LP, des modèles de courrier électronique et des fichiers.

## Types de programme

Il existe cinq types principaux de programmes dans Marketo :

- Par défaut
- Événement
- Événement avec webinaire
- Engagement
- E-mail

Les programmes d’engagement peuvent être des parents d’un type de programme à l’autre, tandis que les programmes Par défaut, Événement et Événement avec le webinaire peuvent uniquement être des programmes parents/e-mail.

Les programmes ont toujours un canal. Ils obtiennent la configuration possible des états membres du programme du canal avec lequel ils ont été créés, qui peut être récupéré avec l’API Get Channels (Obtenir les canaux). Un programme peut également avoir un ensemble de balises associées. Les balises sont des champs personnalisables qui peuvent être configurés pour être facultatifs ou obligatoires pour n’importe quel type de programme donné, pour lequel une valeur est sélectionnée dans une liste configurée dans Marketo Admin.

## Requête

Les programmes suivent le modèle standard pour les requêtes de ressources avec une option supplémentaire permettant d’effectuer des requêtes par type de balise et par valeurs. Les balises et valeurs disponibles peuvent être récupérées avec [Get Tag Types](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags/operation/getTagTypesUsingGET).

### Par identifiant

Le point d’entrée [Get Program by Id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) nécessite un paramètre de chemin d’accès `id`.

L’ID de programme peut être obtenu à partir de l’URL du programme dans l’interface utilisateur, où l’URL ressemblera à `https://app-\*\*\*.marketo.com/#PG1001A1`. Dans cette URL, le `id` est 1001. Il sera toujours compris entre le premier ensemble de lettres de l’URL et le second ensemble de lettres.

```
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

Le point d’entrée [Get Program by Name](https://developer.adobe.com/marketo-apis/api/asset/) nécessite un paramètre de requête `name`. Les paramètres de requête booléens facultatifs sont `includeTags` et `includeCosts`, utilisés pour renvoyer respectivement les balises de programme et les coûts de programme.

```
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

Le point d’entrée [Obtenir des programmes](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) vous permet de rechercher des programmes.

Le paramètre facultatif `status` vous permet de filtrer selon l’état du programme. Ce paramètre s’applique uniquement aux programmes Engagement et Courrier électronique. Les valeurs possibles sont &quot;activé&quot; et &quot;désactivé&quot; pour les programmes d’engagement et &quot;déverrouillé&quot; pour les programmes de messagerie.

Le paramètre facultatif `maxReturn` contrôle le nombre de programmes à renvoyer (200 au maximum, 20 par défaut). Le paramètre facultatif `offset` utilisé pour les résultats de la pagination (la valeur par défaut est 0).

Notez que les balises associées à un programme ne sont pas renvoyées par ce point de terminaison. Les balises de programme peuvent être récupérées en utilisant [Obtenir des programmes par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) ou [Obtenir des programmes par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET).

```
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

### Par période

Les paramètres `earliestUpdatedAt` et `latestUpdatedAt` de notre point d’entrée [Obtenir des programmes](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) vous permettent de définir des filigranes de date et d’heure faibles pour renvoyer les programmes qui ont été mis à jour ou initialement créés dans la plage donnée.

```
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

Le point d’entrée [Obtenir des programmes par balise](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramListByTagUsingGET) récupère une liste de programmes correspondant au type de balise et aux valeurs de balise fournies.

Il existe deux paramètres requis, `tagType` qui est le type de balise sur lequel filtrer les données, et `tagValue` qui est la valeur de balise sur laquelle filtrer les données.  Il existe un paramètre facultatif integer `maxReturn` qui contrôle le nombre de programmes à renvoyer (le maximum est 200, la valeur par défaut est 20) et un paramètre facultatif integer `offset` utilisé pour les résultats de pagination (la valeur par défaut est 0).  Les résultats sont renvoyés dans un ordre aléatoire.

```
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

Les programmes [Creating]https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/createProgramUsingPOST) et [update](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) suivent le modèle de ressource standard et ont `folder`, `name`, `type` et `channel` comme paramètres requis, avec `description`, `costs` et `tags` comme paramètres facultatifs. Le canal et le type ne peuvent être définis qu’à la création du programme. Seules la description, le nom, `tags` et `costs` peuvent être mis à jour après la création, avec un paramètre `costsDestructiveUpdate` supplémentaire autorisé. Si vous définissez `costsDestructiveUpdate` sur true, tous les coûts existants seront effacés et remplacés par les coûts inclus dans l’appel. Notez que des balises peuvent être requises pour certains types de programmes dans certains abonnements, mais cela dépend de la configuration et doit d’abord être vérifié avec Obtenir des balises pour voir s’il existe des exigences spécifiques à une instance.

Lors de la création ou de la mise à jour d&#39;un programme de messagerie, un `startDate` et un `endDate` peuvent également être transmis.

### Créer

```
POST /rest/asset/v1/programs.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Lors de la mise à jour des coûts du programme, pour ajouter de nouveaux coûts, ajoutez-les simplement à votre tableau `costs`. Pour effectuer une mise à jour destructrice, transmettez vos nouveaux coûts, ainsi que le paramètre `costsDestructiveUpdate` défini sur `true`. Pour effacer tous les coûts d&#39;un programme, ne transmettez pas de paramètre `costs` et transmettez simplement `costsDestructiveUpdate` défini sur `true`.

```
POST /rest/asset/v1/program/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Les programmes de messagerie peuvent être approuvés ou non, à distance, ce qui entraîne l’exécution du programme à la date de début donnée et sa conclusion à la date de fin donnée. Ces deux options doivent être définies pour approuver le programme, ainsi qu’un email valide et approuvé et une liste dynamique doivent être configurés via l’interface utilisateur.

### Approuver

```
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

```
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

[Les programmes de clonage](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/cloneProgramUsingPOST) suivent le modèle de ressource standard du nouveau nom et du nouveau dossier comme paramètres requis et une description facultative.  Le paramètre `name` doit être unique au niveau global et ne peut pas dépasser 255 caractères.  Le paramètre `folder` est le dossier parent.  L’attribut de type de paramètre `folder` doit être défini sur &quot;Dossier&quot; et le dossier cible doit se trouver dans le même espace de travail que le programme en cours de clonage.

Les programmes contenant certains types de ressources ne peuvent pas être clonés via cette API, y compris les notifications push, les messages In-App, les rapports et Social Assets. Il est possible que les programmes In-App ne soient pas clonés via cette API.

```
POST /rest/asset/v1/program/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

La suppression de programmes suit le modèle standard de suppression de ressources.

```
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
