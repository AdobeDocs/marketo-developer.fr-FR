---
title: "Importation d’objets personnalisés en bloc"
feature: Custom Objects
description: "Importation par lots d’objets personnalisés."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '855'
ht-degree: 0%

---


# Importation d’objets personnalisés en bloc

[Référence du point de fin d’importation d’objet personnalisé en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects)

Lorsque vous devez importer de nombreux enregistrements d’objets personnalisés, il est recommandé de les importer de manière asynchrone à l’aide de l’API en bloc. Pour ce faire, importez un fichier plat contenant des enregistrements délimités (virgule, tabulation ou point-virgule). Le fichier peut contenir n’importe quel nombre d’enregistrements, à condition que sa taille soit inférieure à 10 Mo (dans le cas contraire, un code d’état HTTP 413 est renvoyé). Le contenu du fichier dépend de votre définition d’objet personnalisé. La première ligne contient toujours un en-tête qui répertorie les champs dans lesquels mapper les valeurs de chaque ligne. Tous les noms de champ dans l’en-tête doivent correspondre à un nom d’API (comme décrit ci-dessous). Les lignes restantes contiennent les données à importer, un enregistrement par ligne. L&#39;opération d&#39;enregistrement est &quot;insert or update&quot; uniquement.

## Limites de traitement

Vous êtes autorisé à soumettre plusieurs demandes d’importation en bloc, dans certaines limites. Chaque requête est ajoutée en tant que tâche à une file d’attente FIFO à traiter. Au maximum deux tâches sont traitées simultanément. Au maximum dix tâches sont autorisées dans la file d’attente à un moment donné (y compris les deux tâches en cours de traitement). Si vous dépassez la limite de dix tâches, une erreur &quot;1016, Trop d&#39;importations&quot; est renvoyée.

## Exemple d’objet personnalisé

Avant d’utiliser l’API en bloc, vous devez d’abord utiliser l’interface utilisateur d’administration de Marketo pour [créer votre objet personnalisé](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects). Par exemple, supposons que nous ayons créé un objet personnalisé &quot;Voiture&quot; avec les champs &quot;Couleur&quot;, &quot;Créer&quot;, &quot;Modèle&quot; et &quot;VIN&quot;. Vous trouverez ci-dessous des écrans de l’interface utilisateur d’administration présentant l’objet personnalisé. Vous pouvez voir que nous avons utilisé le champ VIN pour le dédoublonnage. Les noms d’API sont mis en surbrillance, car ils doivent être utilisés lors de l’appel de points de terminaison associés à l’API en masse.

![Insérer un objet personnalisé](assets/bulk-insert-co-car-1.png)

Voici les champs d’objet personnalisés tels que présentés dans l’interface utilisateur d’administration.

![Insérer des champs d’objet personnalisés](assets/bulk-insert-co-car-fields.png)

### Noms des API

Vous pouvez récupérer les noms d’API par programmation en transmettant le nom de l’API d’objet personnalisé au [Description d’objet personnalisé](#describe) point de terminaison .

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

Maintenant, supposons que vous souhaitiez importer trois enregistrements d’objet personnalisé &quot;Car&quot;. En utilisant le format CSV (valeurs délimitées par des virgules), le fichier peut se présenter comme suit :

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

La ligne 1 est l’en-tête et les lignes 2 à 4 sont les enregistrements de données d’objet personnalisé.

## Création d’une tâche

Pour effectuer la demande d’importation en bloc, vous devez inclure le nom de l’API de l’objet personnalisé dans le chemin d’accès à la variable [Importation d’objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Identity/operation/identityUsingPOST) point de terminaison . Vous devez également inclure un paramètre &quot;file&quot; qui référence le nom de votre fichier d’importation, ainsi qu’un paramètre &quot;format&quot; qui spécifie la façon dont votre fichier d’importation est délimité (&quot;csv&quot;, &quot;tsv&quot; ou &quot;ssv&quot;).

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

Dans cet exemple, nous avons spécifié le format &quot;csv&quot; et nommé notre fichier d’importation &quot;custom_object_import.csv&quot;.

Notez que dans la réponse à notre appel, il n’existe aucune liste de succès ou d’échecs comme vous le feriez pour revenir du point de terminaison Synchroniser les objets personnalisés . Vous recevez plutôt un `batchId`. En effet, l’appel est asynchrone et peut renvoyer une valeur `status` de &quot;En file d’attente&quot;, &quot;Importation&quot; ou &quot;Échec&quot;. Vous devez conserver le paramètre batchId afin que vous puissiez obtenir l’état de la tâche d’importation ou récupérer les échecs et/ou les avertissements une fois la tâche terminée. L’identifiant de lot reste valide pendant sept jours.

Pour répliquer la requête d’importation en bloc, utilisez curl à partir de la ligne de commande :

```
curl -X POST -i -F format='csv' -F file='@custom_object_import.csv' -F access_token='<Access Token>' <REST API Endpoint URL>/bulk/v1/customobjects/car_c/import.json
```

Où le fichier d’importation &quot;custom_object_import.csv&quot; contient les éléments suivants :

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

## État de la tâche d’interrogation

Une fois le traitement d&#39;import créé, vous devez en interroger le statut. Il est recommandé d’interroger la tâche d’importation toutes les 5 à 30 secondes. Pour ce faire, transmettez le nom de l’API de l’objet personnalisé et de la variable `batchId` dans le chemin d’accès au [Obtenir le statut d’objet personnalisé d’importation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET) point de terminaison .

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

Cette réponse affiche un import terminé, mais la variable `status` peut être l’un des suivants : Terminé, En file d’attente, Importation, Échec. Si la tâche est terminée, vous disposez d’une liste du nombre de lignes traitées, avec des échecs et des avertissements. L’attribut message est également un bon endroit pour rechercher des informations de tâche supplémentaires.

## Échecs

Les échecs sont indiqués par la variable `numOfRowsFailed` dans [Obtenir le statut d’objet personnalisé d’importation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET) réponse. Si numOfRowsFailed est supérieur à zéro, cette valeur indique le nombre d’échecs qui se sont produits. Appeler [Obtention des échecs d’importation d’objet personnalisé](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectFailuresUsingGET) point d’entrée pour obtenir un fichier avec les détails de l’échec. Encore une fois, vous devez transmettre le nom de l’API d’objet personnalisé et `batchId` dans le chemin. S’il n’existe aucun fichier d’échec, un code d’état HTTP 404 est renvoyé.

Pour poursuivre l’exemple, nous pouvons forcer un échec en modifiant l’en-tête et en remplaçant &quot;vin&quot; par &quot;vin&quot; (en ajoutant un espace entre la virgule et &quot;vin&quot;).

```
color,make,model, vin
```

Lorsque nous réimportons et vérifions l’état, cette réponse s’affiche avec `numRowsFailed`: 3. Cela indique trois échecs.

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

Nous allons maintenant appeler le point de terminaison Obtenir les échecs d’objet personnalisé d’importation pour obtenir des détails d’échec supplémentaires :

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/failures.json
```

```
color,make,model, vin,Import Failure Reason
red,bmw,2002,WBA4R7C55HK895912,missing.dedupe.fields
yellow,bmw,320i,WBA4R7C30HK896061,missing.dedupe.fields
blue,bmw,325i,WBS3U9C52HP970604,missing.dedupe.fields
```

Et nous pouvons voir que nous manquons notre champ de déduplication `vin`.

## Avertissements

Les avertissements sont indiqués par la variable `numOfRowsWithWarning` dans la réponse Obtenir un état d’objet personnalisé. Si numOfRowsWithWarning est supérieur à zéro, cette valeur indique le nombre d’avertissements qui se sont produits. Appeler [Obtenir des avertissements d’objet personnalisé d’importation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectWarningsUsingGET) point d’entrée pour obtenir un fichier avec les détails de l’avertissement. Encore une fois, vous devez transmettre le nom de l’API d’objet personnalisé et `batchId` dans le chemin. S’il n’existe aucun fichier d’avertissement, un code d’état HTTP 404 est renvoyé.

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/warnings.json
```
