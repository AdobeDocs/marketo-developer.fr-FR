---
title: Importation d’objets personnalisés en bloc
feature: Custom Objects
description: Découvrez comment importer en bloc des objets personnalisés Marketo via REST à l’aide de fichiers CSV, TSV ou SSV.
exl-id: e795476c-14bc-4e8c-b611-1f0941a65825
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '866'
ht-degree: 0%

---

# Importation d’objets personnalisés en bloc

[Référence de point d’entrée d’importation d’objet personnalisé en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects)

Lorsque vous avez de nombreux enregistrements d’objet personnalisés à  Pour les importer, il est recommandé de les importer de manière asynchrone à l’aide de l’API en bloc. Pour ce faire, importez un fichier plat contenant des enregistrements délimités (virgule, tabulation ou point-virgule). Le fichier peut contenir un nombre illimité d’enregistrements, à condition que sa taille soit inférieure à 10 Mo (sinon, il s’agit d’un HTTP  Le code d&#39;état 413 est renvoyé). Le contenu du fichier dépend de votre définition d’objet personnalisée. La première ligne contient toujours un en-tête qui répertorie les champs dans lesquels mapper les valeurs de chaque ligne. Tous les noms de champ de l’en-tête doivent correspondre à un nom d’API (comme expliqué ci-dessous). Les lignes restantes contiennent les données à importer, un enregistrement par ligne. L’opération d’enregistrement est « insert or update » uniquement.

## Limites de traitement

Vous êtes autorisé à soumettre plusieurs demandes d’importation en bloc, dans certaines limites. Chaque demande est ajoutée en tant que tâche à une file d’attente FIFO pour être traitée. Deux traitements au maximum sont traités en même temps. Dix tâches au maximum sont autorisées dans la file d’attente à tout moment donné (y compris les deux en cours de traitement). Si vous dépassez le nombre maximal de dix traitements, une erreur « 1016, Too many imports » est renvoyée.

## Exemple d’objet personnalisé

Avant d’utiliser l’API en bloc, vous devez d’abord utiliser l’interface utilisateur d’administration de Marketo pour [créer votre objet personnalisé](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects). Par exemple, supposons que nous ayons créé un objet personnalisé « Voiture » avec les champs « Couleur », « Marque », « Modèle » et « VIN ». Vous trouverez ci-dessous les écrans de l’interface utilisateur d’administration affichant l’objet personnalisé. Vous pouvez voir que nous avons utilisé le champ VIN pour le dédoublonnement. Les noms d’API sont mis en surbrillance car ils doivent être utilisés lors de l’appel de points d’entrée liés à l’API en bloc.

![Insérer un objet personnalisé](assets/bulk-insert-co-car-1.png)

Voici les champs d’objet personnalisés tels qu’ils sont présentés dans l’interface utilisateur d’administration.

![Insérer des champs d’objet personnalisés](assets/bulk-insert-co-car-fields.png)

### Noms d’API

Vous pouvez récupérer les noms d’API par programmation en transmettant le nom d’API d’objet personnalisé au point d’entrée [Décrire l’objet personnalisé](#describe).

```
/rest/v1/customobjects/{apiName}/describe.json
```

```json
{
    "requestId": "46ff#15a686e66de",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It's a car.",
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

Supposons maintenant que vous souhaitiez importer trois enregistrements d’objets personnalisés « Voiture ». En utilisant le format délimité par des virgules (CSV), le fichier peut se présenter comme suit :

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

La ligne 1 est l’en-tête et les lignes 2 à 4 sont les enregistrements de données d’objet personnalisés.

## Création d’un traitement

Pour effectuer la demande d’importation en bloc, vous devez inclure le nom d’API de l’objet personnalisé dans le chemin d’accès au point d’entrée [Importer des objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Identity/operation/identityUsingPOST). Vous devez également inclure un paramètre « file » qui fait référence au nom de votre fichier d’importation, ainsi qu’un paramètre « format » qui spécifie le mode de délimitation de votre fichier d’importation (« csv », « tsv » ou « ssv »).

```
POST /bulk/v1/customobjects/{apiName}/import.json?format=csv
```

```
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Length: 290
Host: <munchkinId>.mktorest.com
```

```
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

Dans cet exemple, nous avons spécifié le format « csv » et nommé notre fichier d’importation « custom_object_import.csv ».

Notez que dans la réponse à notre appel, il n’y a aucune liste de succès ou d’échecs comme ceux que vous obtiendriez du point d’entrée des objets personnalisés de synchronisation. Au lieu de cela, vous recevez un `batchId`. En effet, l’appel est asynchrone et peut renvoyer un `status` de type « En file d’attente », « En cours d’importation » ou « En échec ». Vous devez conserver batchId afin d’obtenir le statut de la tâche d’importation ou de récupérer les échecs et/ou les avertissements une fois l’importation terminée. L’ID de lot reste valide pendant sept jours.

Une méthode simple pour répliquer la requête d’importation en bloc consiste à utiliser curl à partir de la ligne de commande :

```
curl -X POST -i -F format='csv' -F file='@custom_object_import.csv' -F access_token='<Access Token>' <REST API Endpoint URL>/bulk/v1/customobjects/car_c/import.json
```

Où le fichier d’importation « custom_object_import.csv » contient les éléments suivants :

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

## Interroger le statut de la tâche

Une fois le traitement d’import créé, vous devez interroger son statut. Il est recommandé d’interroger le traitement d’importation toutes les 5 à 30 secondes. Pour ce faire, transmettez le nom d’API de l’objet personnalisé et le `batchId` dans le chemin d’accès au point d’entrée [Get Import Custom Object Status](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET).

```
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

Cette réponse affiche un import terminé, mais le `status` peut être l’un des éléments suivants : Terminé, En file d’attente, Importation, Échec. Si la tâche est terminée, vous disposez d’une liste du nombre de lignes traitées, avec des échecs et des avertissements. L’attribut de message est également un bon endroit pour rechercher des informations de tâche supplémentaires.

## Échecs

Les échecs sont indiqués par l’attribut `numOfRowsFailed` dans la réponse [Get Import Custom Object Status](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET). Si numOfRowsFailed est supérieur à zéro, cette valeur indique le nombre d’échecs survenus. Appelez le point d’entrée [Get Import Custom Object Failures](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectFailuresUsingGET) pour obtenir un fichier avec les détails de l’échec. Là encore, vous devez transmettre le nom et le `batchId` de l’API d’objet personnalisé dans le chemin d’accès. S’il n’existe aucun fichier d’échec, un code d’état HTTP 404 est renvoyé.

En poursuivant avec l’exemple, nous pouvons forcer un échec en modifiant l’en-tête et en remplaçant « vin » par « vin » (en ajoutant un espace entre la virgule et « vin »).

```
color,make,model, vin
```

Lorsque nous réimportons et vérifions le statut, nous voyons cette réponse avec `numRowsFailed` : 3. Cela indique trois échecs.

```
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

Maintenant, nous effectuons l’appel de point d’entrée Get Import Custom Object Failures pour obtenir des détails supplémentaires sur l’échec :

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/failures.json
```

```
color,make,model, vin,Import Failure Reason
red,bmw,2002,WBA4R7C55HK895912,missing.dedupe.fields
yellow,bmw,320i,WBA4R7C30HK896061,missing.dedupe.fields
blue,bmw,325i,WBS3U9C52HP970604,missing.dedupe.fields
```

Et nous pouvons voir que nous manquons notre `vin` de champ de déduplication.

## Avertissements

Les avertissements sont indiqués par l&#39;attribut `numOfRowsWithWarning` dans la réponse Get Import Custom Object Status. Si numOfRowsWithWarning est supérieur à zéro, cette valeur indique le nombre d&#39;avertissements qui se sont produits. Appelez le point d’entrée [Get Import Custom Object Warnings](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectWarningsUsingGET) pour obtenir un fichier avec les détails de l’avertissement. Là encore, vous devez transmettre le nom et le `batchId` de l’API d’objet personnalisé dans le chemin d’accès. S’il n’existe aucun fichier d’avertissement, un code d’état HTTP 404 est renvoyé.

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/warnings.json
```
