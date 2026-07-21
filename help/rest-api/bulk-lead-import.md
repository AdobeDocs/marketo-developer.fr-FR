---
title: Importation de leads en bloc
feature: REST API
description: Créez et surveillez des importations de leads en bloc asynchrones dans Marketo avec CSV TSV ou SSV.
exl-id: 615f158b-35f9-425a-b568-0a7041262504
TQID: https://experienceleague.adobe.com/UamXYWis5J1ERqnp5lAnfUf3pFcgfSOLfKRXRB-Yg4I
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 623
ht-degree: 1%

---

# Importation de leads en bloc

[Référence de point d’entrée d’importation de leads en bloc](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads)

Utilisez l’[API en bloc](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) pour importer de manière asynchrone un grand nombre d’enregistrements de prospect. Fournissez les enregistrements dans un fichier plat délimité par des virgules, des tabulations ou des points-virgules d’une taille inférieure à 10 Mo.

L’importation de leads en bloc prend uniquement en charge l’opération d’enregistrement « insérer ou mettre à jour ».

## Limites de traitement

Chaque demande d’importation en bloc est ajoutée sous la forme d’une tâche à une file d’attente Premier entré, Premier sorti (FIFO). Les limites suivantes s’appliquent :

- Deux traitements au maximum peuvent être traités simultanément.
- 10 tâches au maximum peuvent se trouver dans la file d’attente, y compris les deux tâches en cours de traitement.

Si vous dépassez la limite de 10 tâches, l’API renvoie une erreur `1016, Too many imports`.

## Importer fichier

La première ligne du fichier doit être un en-tête qui répertorie les champs API REST auxquels les valeurs de chaque ligne correspondent. Un fichier type suit ce modèle :

```csv
email,firstName,lastName
test@example.com,John,Doe
```

Utilisez `externalCompanyId` pour lier un enregistrement de prospect à un enregistrement d’entreprise. Utilisez `externalSalesPersonId` pour lier un enregistrement de prospect à un enregistrement de vendeur.

Envoyez la requête à l’aide du type de contenu `multipart/form-data`. Utilisez une implémentation de bibliothèque existante pour construire la requête multipartie.

## Création d’un traitement

Pour créer une tâche d’importation en bloc, définissez le type de contenu sur `multipart/form-data` et incluez les paramètres suivants :

- `file` : contenu du fichier d’importation.
- `format` : format du fichier. Les valeurs valides sont `csv`, `tsv` et `ssv`.

```http
POST /bulk/v1/leads.json?format=csv
```

```text
Content-Type: multipart/form-data; boundary=------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```text
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email,company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

```json
{
    "requestId": "d01f#15d672f8560",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Queued"
        }
    ],
    "success": true
}
```

Ce point d’entrée utilise [multipart/form-data comme type de contenu](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Utilisez une bibliothèque de prise en charge HTTP pour la langue de votre choix afin de créer correctement la requête. L’exemple suivant utilise cURL à partir de la ligne de commande :

```bash
curl -i -F format=csv -F file=@lead_data.csv -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/leads.json
```

Dans cet exemple, le fichier d&#39;import `lead_data.csv` contient les données suivantes :

```text
firstName,lastName,email,company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
```

Vous pouvez également inclure les paramètres facultatifs suivants :

- `lookupField` : sélectionne le champ utilisé pour la déduplication et utilise la valeur par défaut `email`. Spécifiez `id` effectuer une opération « mise à jour uniquement ».
- `listId` : sélectionne une liste statique. Les prospects importés deviennent membres de cette liste en plus des enregistrements créés ou mis à jour par l’importation.
- `partitionName` : sélectionne la partition vers laquelle effectuer l’importation. Voir la section Espaces de travail et partitions pour plus d’informations.

L’API étant asynchrone, la réponse contient des champs `batchId` et `status` au lieu de succès et d’échecs individuels. Le statut peut être `Queued`, `Importing` ou `Failed`.

Conservez la `batchId` pour vérifier le statut de la tâche et récupérer les échecs ou les avertissements une fois l’opération terminée. Le `batchId` reste valable sept jours.

## Interroger le statut de la tâche

Utilisez l’API Get Import Lead Status pour interroger la tâche toutes les 5 à 30 secondes, en fonction des exigences de latence et des limitations des appels API.

```http
GET /bulk/v1/leads/batch/{id}.json
```

```json
{
   "requestId":"8136#146daebc2ed",
   "success":true,
   "result":[
      {
         "batchId":1022,
         "status":"Complete",
         "numOfLeadsProcessed":2,
         "numOfRowsFailed":1,
         "numOfRowsWithWarning":0,
         "message":"Import completed with errors, 2 records imported (2 members), 1 failed"
      }
   ]
}
```

Cette réponse affiche un import terminé. Le statut peut être l’une des valeurs suivantes :

- Terminée
- En fil d&#39;attente
- Importation
- Échec

Une fois la tâche terminée, la réponse répertorie le nombre de lignes traitées, ayant échoué et traitées avec des avertissements. Le paramètre `message` peut également fournir un message d’échec lorsque le statut est `Failed`.

## Échecs

L’attribut `numOfRowsFailed` dans la réponse Get Import Lead Status indique le nombre de lignes ayant échoué. Une valeur supérieure à zéro signifie que des échecs se sont produits.

Pour récupérer les enregistrements ayant échoué et leurs causes, demandez le fichier d’échec :

```http
GET /bulk/v1/leads/batch/{id}/failures.json
```

L’API renvoie un fichier qui identifie chaque ligne en échec et explique pourquoi l’enregistrement a échoué. Le fichier utilise le format spécifié par le paramètre `format` lors de la création de la tâche. Un champ supplémentaire sur chaque enregistrement décrit l’échec.

## Avertissements

L’attribut `numOfRowsWithWarning` dans la réponse Get Import Lead Status indique le nombre de lignes avec des avertissements. Une valeur supérieure à zéro signifie que des avertissements se sont produits.

Pour récupérer les enregistrements concernés et leurs causes, demandez le fichier d&#39;avertissement :

```http
GET /bulk/v1/leads/batch/{id}/warnings.json
```

L’API renvoie un fichier qui identifie chaque ligne avec un avertissement et explique pourquoi l’avertissement s’est produit. Le fichier utilise le format spécifié par le paramètre `format` lors de la création de la tâche. Un champ supplémentaire sur chaque enregistrement décrit l&#39;avertissement.
