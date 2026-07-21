---
title: Importation de membres de programme en bloc
feature: REST API
description: Découvrez comment importer des membres de programme en bloc via l’API REST Marketo à l’aide de fichiers CSV TSV ou SSV de moins de 10 Mo, des limites de file d’attente, des paramètres requis et du statut de la tâche d’interrogation.
exl-id: b0e1039a-fe9b-4fb7-9aa6-9980a06da673
TQID: https://experienceleague.adobe.com/T1PAzLN1mnp38kJ0jwh6kPv6r1Uvxc7-o9zeTHetIV0
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
source-wordcount: 771
ht-degree: 0%

---

# Importation de membres de programme en bloc

[Référence de point d’entrée d’importation de membre de programme en bloc](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members)

Utilisez l’[API en bloc](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members) pour importer de manière asynchrone un grand nombre d’enregistrements de membres du programme. Fournissez les enregistrements dans un fichier plat délimité par des virgules, des tabulations ou des points-virgules d’une taille inférieure à 10 Mo.

L’importation de membres de programme en bloc prend uniquement en charge l’opération d’enregistrement « insérer ou mettre à jour ».

## Limites de traitement

Chaque demande d’importation en bloc est ajoutée sous la forme d’une tâche à une file d’attente Premier entré, Premier sorti (FIFO). Les limites suivantes s’appliquent :

- Deux traitements au maximum peuvent être traités simultanément.
- 10 tâches au maximum peuvent se trouver dans la file d’attente, y compris les deux tâches en cours de traitement.

Si vous dépassez la limite de 10 tâches, l’API renvoie une erreur `1016, Too many imports`.

## Importer fichier

La première ligne du fichier doit être un en-tête qui répertorie les noms de champ de l’API REST auxquels les valeurs de chaque ligne correspondent. Récupérez ces noms à l’aide des points d’entrée [Décrire le prospect](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) et [Décrire le membre de programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeProgramMemberUsingGET).

Les enregistrements peuvent contenir des champs de prospect, des champs de prospect personnalisés et des champs de membre de programme personnalisés.

Un fichier type suit ce modèle :

```text
email,firstName,lastName
test@example.com,John,Doe
```

Envoyez la requête à l’aide du type de contenu `multipart/form-data`. Utilisez une implémentation de bibliothèque existante pour construire la requête multipartie.

## Création d’un traitement

Le point d’entrée [Importer des membres de programme](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) lit les enregistrements de membre de programme à partir d’un fichier et les ajoute à un programme avec un statut spécifié. Les enregistrements peuvent contenir des champs de prospect et des champs de membre de programme personnalisés.

Chaque enregistrement doit inclure le champ d’e-mail, qui est utilisé à des fins de déduplication.

Le paramètre `programId` path spécifie le programme auquel les membres sont ajoutés.

La requête nécessite trois paramètres de requête :

- `format` : format du fichier d’importation (`CSV`, `TSV` ou `SSV`).
- `programMemberStatus` : statut du programme attribué aux membres importés.
- `file` : nom du fichier contenant les enregistrements des membres du programme.

```http
POST /bulk/v1/program/{programId}/members/import.json?format=csv&programMemberStatus=On List
```

```text
Content-Type: multipart/form-data; boundary=--------------------------118046853683028616211319
Content-Length: 772
Host: <munchkinId>.mktorest.com
```

```text
----------------------------118046853683028616211319
Content-Disposition: form-data; name="file"; filename="Lead-House-Lannister.csv"
Content-Type: text/csv

firstName,lastName,email,title,company,leadScore
Joanna,Lannister,Joanna@Lannister.com,Lannister,House Lannister,0
Tywin,Lannister,Tywin@Lannister.com,Lannister,House Lannister,0
Cersei,Lannister,Cersei@Lannister.com,Lannister,House Lannister,0
Jamie,Lannister,Jamie@Lannister.com,Lannister,House Lannister,0
Tyrion,Lannister,Tyrion@Lannister.com,Lannister,House Lannister,0
Kevan,Lannister,Kevan@Lannister.com,Lannister,House Lannister,0
Dorna,Lannister,Dorna@Lannister.com,Lannister,House Lannister,0
Lancel,Lannister,Lancel@Lannister.com,Lannister,House Lannister,0

----------------------------118046853683028616211319--
```

```json
{
    "requestId": "17f4a#16f87f87325",
    "result": [
        {
            "batchId": 1040,
            "importId": "1040",
            "status": "Queued"
        }
    ],
    "success": true
}
```

Comme le point d’entrée est asynchrone, la réponse contient des champs `batchId` et `status`. Le statut peut être `Queued`, `Importing` ou `Failed`.

Conservez les `batchId` pour vérifier le statut de l’importation et récupérer les échecs ou les avertissements une fois l’importation terminée. Le `batchId` reste valable sept jours.

La requête cURL de ligne de commande suivante envoie l’exemple de tâche :

```bash
curl -i -F format='csv' -F programMemberStatus='On List' -F file='@Lead-House-Lannister.csv' -F access_token='<Access Token>' <REST API Endpoint Base URL>/bulk/v1/program/{programId}/members/import.json
```

Dans cet exemple, le fichier d&#39;import `Lead-House-Lannister.csv` contient les données suivantes :

```text
firstName,lastName,email,title,company,leadScore
Joanna,Lannister,Joanna@Lannister.com,Lannister,House Lannister,0
Tywin,Lannister,Tywin@Lannister.com,Lannister,House Lannister,0
Cersei,Lannister,Cersei@Lannister.com,Lannister,House Lannister,0
Jamie,Lannister,Jamie@Lannister.com,Lannister,House Lannister,0
Tyrion,Lannister,Tyrion@Lannister.com,Lannister,House Lannister,0
Kevan,Lannister,Kevan@Lannister.com,Lannister,House Lannister,0
Dorna,Lannister,Dorna@Lannister.com,Lannister,House Lannister,0
Lancel,Lannister,Lancel@Lannister.com,Lannister,House Lannister,0
```

## Interroger le statut de la tâche

Après avoir créé la tâche d’importation, interrogez-la toutes les 5 à 30 secondes. Transmettez le paramètre de chemin d’accès `batchId` au point d’entrée [Obtenir le statut du membre du programme d’importation](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET).

```http
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
    "requestId": "e0cb#16f87f8b177",
    "result": [
        {
            "batchId": 1040,
            "importId": "1040",
            "status": "Complete",
            "numOfLeadsProcessed": 8,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "message": "Import succeeded, 8 records imported (8 members)"
        }
    ],
    "success": true
}
```

Cette réponse affiche un import terminé. Le statut peut être `Complete`, `Queued`, `Importing` ou `Failed`.

Une fois la tâche terminée, la réponse répertorie le nombre de lignes traitées, ayant échoué et traitées avec des avertissements. Le paramètre `message` peut également fournir un message d’échec lorsque le statut est `Failed`.

## Échecs

L’attribut `numOfRowsFailed` dans la réponse [Obtenir le statut du membre du programme d’importation](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) indique le nombre de lignes ayant échoué. Une valeur supérieure à zéro signifie que des échecs se sont produits.

Transmettez le paramètre de chemin d’accès `batchId` au point d’entrée Get Import Program Member Failures pour récupérer les enregistrements en échec et leurs causes.

```http
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

Le point d’entrée renvoie un fichier qui identifie chaque ligne en échec et explique pourquoi l’enregistrement a échoué. Le fichier utilise le format spécifié par le paramètre `format` lors de la création de la tâche. Un champ supplémentaire sur chaque enregistrement décrit l’échec.

Supposons, par exemple, que vous importiez le fichier suivant avec un score de prospect non valide :

```text
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

L’état de la tâche renvoie la `numOfRowsFailed` 1, ce qui indique qu’un échec s’est produit :

```http
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
    "requestId": "4c2d#16f8b32c8ef",
    "result": [
        {
            "batchId": 1046,
            "importId": "1046",
            "status": "Complete",
            "numOfLeadsProcessed": 0,
            "numOfRowsFailed": 1,
            "numOfRowsWithWarning": 0,
            "message": "Import completed with errors, 0 records imported (0 members), 1 failed"
        }
    ],
    "success": true
}
```

Récupérez le fichier d’échec pour plus d’informations :

```http
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

```text
firstName,lastName,email,title,company,leadScore,Import Failure Reason
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD,Invalid data type in field Lead Score
```

## Avertissements

L’attribut `numOfRowsWithWarning` dans la réponse [Obtenir le statut du membre du programme d’importation](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) indique le nombre de lignes avec des avertissements. Une valeur supérieure à zéro signifie que des avertissements se sont produits.

Transmettez le paramètre de chemin d’accès `batchId` au point d’entrée [Obtenir les avertissements des membres du programme d’importation](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET) pour récupérer les enregistrements concernés et leurs causes.

```http
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

Le point d’entrée renvoie un fichier qui identifie chaque ligne avec un avertissement et explique pourquoi l’avertissement s’est produit. Le fichier utilise le format spécifié par le paramètre `format` lors de la création de la tâche. Un champ supplémentaire sur chaque enregistrement décrit l&#39;avertissement.

Supposons, par exemple, que vous importiez le fichier suivant avec une adresse e-mail non valide :

```text
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

L’état de la tâche renvoie la `numOfRowsWithWarning` 1, indiquant qu’un avertissement s’est produit :

```http
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
   "requestId":"4ca1#16f883c2003",
   "result":[
      {
         "batchId":1041,
         "importId":"1041",
         "status":"Complete",
         "numOfLeadsProcessed":1,
         "numOfRowsFailed":0,
         "numOfRowsWithWarning":1,
         "message":"Import succeeded, 1 records imported (1 members), 1 warning."
      }
   ],
   "success":true
}
```

Récupérez le fichier d’avertissement pour plus d’informations :

```http
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

```text
firstName,lastName,email,title,company,leadScore,Import Warning Reason
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0,Invalid email address
```
