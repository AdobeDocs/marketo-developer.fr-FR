---
title: Importation d’objets personnalisés en bloc
feature: Custom Objects
description: Découvrez comment importer en bloc des objets personnalisés Marketo via REST à l’aide de fichiers CSV, TSV ou SSV.
exl-id: e795476c-14bc-4e8c-b611-1f0941a65825
TQID: https://experienceleague.adobe.com/C1LKLZDEvv95XXH3AEoxIXsLK55tgKTrvyxvs4LnYWw
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 736
ht-degree: 0%

---

# Importation d’objets personnalisés en bloc

[Référence de point d’entrée d’importation d’objet personnalisé en bloc](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects)

Utilisez l’API en bloc pour importer de manière asynchrone un grand nombre d’enregistrements d’objets personnalisés. Fournissez les enregistrements dans un fichier plat délimité par des virgules, des tabulations ou des points-virgules d’une taille inférieure à 10 Mo. Si le fichier est plus volumineux, l’API renvoie un code d’état HTTP 413.

Le contenu du fichier dépend de la définition de l’objet personnalisé. La première ligne doit être un en-tête et chaque champ d’en-tête doit correspondre à un nom d’API. Chaque ligne restante contient un enregistrement.

L’importation d’objets personnalisés en bloc prend uniquement en charge l’opération d’enregistrement « insérer ou mettre à jour ».

## Limites de traitement

Chaque demande d’importation en bloc est ajoutée sous la forme d’une tâche à une file d’attente Premier entré, Premier sorti (FIFO). Les limites suivantes s’appliquent :

- Deux traitements au maximum peuvent être traités simultanément.
- 10 tâches au maximum peuvent se trouver dans la file d’attente, y compris les deux tâches en cours de traitement.

Si vous dépassez la limite de 10 tâches, l’API renvoie une erreur `1016, Too many imports`.

## Exemple d’objet personnalisé

Avant d’utiliser l’API en bloc, utilisez l’interface utilisateur d’administration de Marketo pour [créer votre objet personnalisé](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects).

Cet exemple utilise un objet personnalisé `Car` avec des champs `Color`, `Make`, `Model` et `VIN`. Le champ VIN est utilisé pour la déduplication. Les écrans de l’interface utilisateur d’administration mettent en surbrillance les noms d’API requis par les points d’entrée API en masse.

![Insérer un objet personnalisé](assets/bulk-insert-co-car-1.png)

Voici les champs d’objet personnalisés tels qu’ils sont présentés dans l’interface utilisateur d’administration.

![Insérer des champs d’objet personnalisés](assets/bulk-insert-co-car-fields.png)

### Noms d’API

Pour récupérer les noms d’API par programmation, transmettez le nom d’API d’objet personnalisé au point d’entrée [Décrire l’objet personnalisé](#describe).

```text
/rest/v1/customobjects/{apiName}/describe.json
```

```json
{
    "requestId": "46ff#15a686e66de",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It is a car.",
            "createdAt": "2017-02-22T19:55:51Z",
            "updatedAt": "2017-02-22T19:55:51Z",
            "idField": "marketoGUID",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ]
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                }
            ]
        }
    ],
    "success": true
}
```

### Importer fichier

Le fichier CSV suivant contient trois enregistrements d’objet personnalisés `Car` :

```text
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

La première ligne correspond à l’en-tête . Les lignes 2 à 4 contiennent les enregistrements de données d’objet personnalisés.

## Création d’un traitement

Pour créer la tâche d’importation en bloc, incluez le nom de l’API d’objet personnalisé dans le chemin d’accès au point d’entrée [Importer des objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi#tag/Identity/operation/identityUsingPOST). Inclure les paramètres suivants :

- `file` : nom du fichier d’importation.
- `format` : format du délimiteur de fichier (`csv`, `tsv` ou `ssv`).

```http
POST /bulk/v1/customobjects/{apiName}/import.json?format=csv
```

```text
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Length: 290
Host: <munchkinId>.mktorest.com
```

```text
------WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Disposition: form-data; name="file"; filename="custom_object_import.csv"
Content-Type: text/csv

color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
------WebKitFormBoundaryXjWP6BP8Ciq6bPeo--
```

```json
{
    "requestId": "c015#15a68a23418",
    "result": [
        {
            "batchId": 1013,
            "status": "Queued",
            "objectApiName": "car_c"
        }
    ],
    "success": true
}
```

Cet exemple spécifie le format de `csv` et nomme le fichier d&#39;importation `custom_object_import.csv`.

Comme l’appel est asynchrone, la réponse contient un `batchId` au lieu des succès et des échecs individuels renvoyés par le point d’entrée Synchroniser les objets personnalisés . Le `status` peut être `Queued`, `Importing` ou `Failed`.

Conservez les `batchId` pour vérifier le statut de l’importation et récupérer les échecs ou les avertissements une fois l’importation terminée. Le `batchId` reste valable sept jours.

La requête cURL de ligne de commande suivante envoie l’exemple de tâche :

```bash
curl -X POST -i -F format='csv' -F file='@custom_object_import.csv' -F access_token='<Access Token>' <REST API Endpoint URL>/bulk/v1/customobjects/car_c/import.json
```

Dans cet exemple, le fichier `custom_object_import.csv` contient les données suivantes :

```text
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

## Interroger le statut de la tâche

Après avoir créé la tâche d’importation, interrogez-la toutes les 5 à 30 secondes. Transmettez le nom et le `batchId` de l’API d’objet personnalisé dans le chemin d’accès au point d’entrée [Get Import Custom Object Status](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET).

```http
GET /bulk/v1/customobjects/{apiName}/import/{batchId}/status.json
```

```json
{
    "requestId": "2a5#15a68dd9be1",
    "result": [
        {
            "batchId": 1013,
            "operation": "import",
            "status": "Complete",
            "objectApiName": "car_c",
            "numOfObjectsProcessed": 3,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "importTime": "2 second(s)",
            "message": "Import succeeded, 3 records imported (3 members)"
        }
    ],
    "success": true
}
```

Cette réponse affiche un import terminé. Les `status` peuvent être `Complete`, `Queued`, `Importing` ou `Failed`.

Une fois la tâche terminée, la réponse répertorie le nombre de lignes traitées, ayant échoué et traitées avec des avertissements. L’attribut `message` peut fournir des informations supplémentaires sur la tâche.

## Échecs

L’attribut `numOfRowsFailed` dans la réponse [Get Import Custom Object Status](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET) indique le nombre de lignes ayant échoué. Une valeur supérieure à zéro signifie que des échecs se sont produits.

Transmettez le nom et le `batchId` de l’API d’objet personnalisé au point d’entrée [Get Import Custom Object Failures](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectFailuresUsingGET). Le point d’entrée renvoie un fichier avec les détails de l’échec. S’il n’existe aucun fichier d’échec, il renvoie un code d’état HTTP 404.

Pour démontrer un échec, modifiez l’en-tête en `vin` remplaçant par ` vin`, en ajoutant un espace entre la virgule et le `vin`.

```text
color,make,model, vin
```

Après avoir réimporté le fichier, la réponse de statut affiche `numRowsFailed` : 3, indiquant trois échecs.

```http
GET /bulk/v1/customobjects/car_c/import/{batchId}/status.json
```

```json
{
    "requestId": "12260#15a68f491ed",
    "result": [
        {
            "batchId": 1016,
            "operation": "import",
            "status": "Complete",
            "objectApiName": "car_c",
            "numOfObjectsProcessed": 0,
            "numOfRowsFailed": 3,
            "numOfRowsWithWarning": 0,
            "importTime": "1 second(s)",
            "message": "Import completed with errors, 0 records imported (0 members), 3 failed"
        }
    ],
    "success": true
}
```

Appelez le point d’entrée Get Import Custom Object Failures pour plus d’informations :

```http
GET /bulk/v1/customobjects/car_c/import/{batchId}/failures.json
```

```text
color,make,model, vin,Import Failure Reason
red,bmw,2002,WBA4R7C55HK895912,missing.dedupe.fields
yellow,bmw,320i,WBA4R7C30HK896061,missing.dedupe.fields
blue,bmw,325i,WBS3U9C52HP970604,missing.dedupe.fields
```

La réponse indique que le champ de déduplication `vin` est manquant.

## Avertissements

L&#39;attribut `numOfRowsWithWarning` dans la réponse Get Import Custom Object Status indique le nombre de lignes comportant des avertissements. Une valeur supérieure à zéro signifie que des avertissements se sont produits.

Transmettez le nom de l’API d’objet personnalisé et `batchId` dans le chemin d’accès au point d’entrée [Get Import Custom Object Warnings](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectWarningsUsingGET). Le point d’entrée renvoie un fichier avec des détails d’avertissement. S’il n’existe aucun fichier d’avertissement, il renvoie un code d’état HTTP 404.

```http
GET /bulk/v1/customobjects/car_c/import/{batchId}/warnings.json
```
