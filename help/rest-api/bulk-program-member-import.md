---
title: Importation de membres de programme en bloc
feature: REST API
description: Import par lots des données de membre.
exl-id: b0e1039a-fe9b-4fb7-9aa6-9980a06da673
source-git-commit: 8a785b0719e08544ed1a87772faf90bd9dda3077
workflow-type: tm+mt
source-wordcount: '837'
ht-degree: 0%

---

# Importation de membres de programme en bloc

[Référence de point d’entrée d’importation de membre de programme en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members)

Pour de grandes quantités d’enregistrements de membre de programme, les membres de programme peuvent être importés de manière asynchrone avec l’[API en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members). Vous pouvez ainsi importer une liste d’enregistrements dans Marketo à l’aide d’un fichier plat avec les délimiteurs (virgule, tabulation ou point-virgule). Le fichier peut contenir un nombre illimité d’enregistrements, à condition que la taille totale du fichier soit inférieure à 10 Mo. L’opération d’enregistrement est « insert or update » uniquement.

## Limites de traitement

Vous êtes autorisé à soumettre plusieurs demandes d’importation en bloc, avec certaines limitations. Chaque demande est ajoutée en tant que tâche à une file d’attente FIFO pour être traitée. Deux traitements au maximum sont traités en même temps. Dix tâches au maximum sont autorisées dans la file d’attente à tout moment donné (y compris les deux en cours de traitement). Si vous dépassez le nombre maximal de dix traitements, une erreur « 1016, Too many imports » est renvoyée.

## Importer fichier

La première ligne du fichier doit être un en-tête qui répertorie les noms d’API REST correspondants comme des champs dans lesquels mapper les valeurs de chaque ligne. Les noms d’API REST peuvent être récupérés à l’aide des points d’entrée [Décrire le prospect](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) et/ou [Décrire le membre de programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeProgramMemberUsingGET). Les enregistrements peuvent contenir des champs de prospect, des champs de prospect personnalisés et des champs de membre de programme personnalisés.

Un fichier type suit ce modèle de base :

```
email,firstName,lastName
test@example.com,John,Doe
```

L’appel lui-même est effectué à l’aide du type de contenu `multipart/form-data`.

Ce type de requête pouvant être difficile à implémenter, il est vivement recommandé d’utiliser une implémentation de bibliothèque existante.

## Création d’un traitement

Le point d’entrée [Importer des membres de programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) lit un fichier contenant les enregistrements des membres du programme et les ajoute à un programme avec un statut donné. Les enregistrements peuvent contenir à la fois des champs de prospect et des champs personnalisés de membre de programme. Tous les enregistrements doivent inclure le champ d’e-mail, qui est utilisé à des fins de déduplication.

Le paramètre `programId` path spécifie le programme auquel les membres sont ajoutés.

Trois paramètres de requête sont requis. Le paramètre `format` spécifie le format du fichier d&#39;importation (CSV, TSV ou SSV), le paramètre `programMemberStatus` spécifie le statut du programme pour les membres qui sont ajoutés au programme et le paramètre `file` contient le nom du fichier d&#39;importation qui contient les enregistrements de membre de programme.

```
POST /bulk/v1/program/{programId}/members/import.json?format=csv&programMemberStatus=On List
```

```
Content-Type: multipart/form-data; boundary=--------------------------118046853683028616211319
Content-Length: 772
Host: <munchkinId>.mktorest.com
```

```
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

Notez dans la réponse à notre appel qu’il existe un champ `batchId` et `status` pour l’enregistrement dans le tableau de résultats. Comme ce point d’entrée est asynchrone, il peut renvoyer le statut En file d’attente, Importation ou En échec. Vous devez conserver les `batchId` pour obtenir le statut de la tâche d’importation et pour récupérer les échecs et/ou les avertissements une fois l’importation terminée. Le `batchId` reste valable sept jours.

Dans l’exemple ci-dessus, un moyen simple d’appeler le point d’entrée consiste à utiliser cURL à partir de la ligne de commande :

```bash
curl -i -F format='csv' -F programMemberStatus='On List' -F file='@Lead-House-Lannister.csv' -F access_token='<Access Token>' <REST API Endpoint Base URL>/bulk/v1/program/{programId}/members/import.json
```

Où le fichier d’importation « Lead-House-Lannister.csv » contient les éléments suivants :

```
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

Une fois le traitement d’import créé, vous devez interroger son statut. Il est recommandé d’interroger le traitement d’importation toutes les 5 à 30 secondes. Pour ce faire, transmettez le paramètre de chemin d’accès `batchId` au point d’entrée [Obtenir le statut du membre du programme d’importation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET).

```
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

Cette réponse affiche un import terminé. Le statut peut être l’un des suivants : Terminé, En file d’attente, Importation, En échec.

Si la tâche est terminée, vous disposez d’une liste du nombre de lignes traitées, ayant échoué ou comportant des avertissements. Le paramètre message peut également générer le message failure si le statut est Failed.

## Échecs

Les échecs sont indiqués par l’attribut `numOfRowsFailed` dans la réponse [Obtenir le statut du membre du programme d’importation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET). Si numOfRowsFailed est supérieur à zéro, cette valeur indique le nombre d’échecs survenus.

Utilisez le point d’entrée Get Import Program Member Failures pour récupérer les enregistrements et les causes des lignes ayant échoué en transmettant le paramètre de chemin d’accès `batchId`.

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

Le point d’entrée répond avec un fichier indiquant les lignes ayant échoué, ainsi qu’un message indiquant pourquoi l’enregistrement a échoué. Le format du fichier est identique à celui spécifié dans `format` paramètre lors de la création de la tâche. Un champ supplémentaire est ajouté à chaque enregistrement avec une description de l’échec.

Supposons, par exemple, que vous importiez le fichier suivant avec un score de prospect non valide :

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

Lorsque vous vérifiez le statut de la tâche, vous voyez `numOfRowsFailed` est 1, ce qui indique qu’un échec s’est produit :

```
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

Récupérez ensuite le fichier des échecs pour obtenir plus d’informations sur l’échec :

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

```
firstName,lastName,email,title,company,leadScore,Import Failure Reason
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD,Invalid data type in field Lead Score
```

## Avertissements

Les avertissements sont indiqués par l’attribut `numOfRowsWithWarning` dans la réponse [Obtenir le statut de membre du programme d’importation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET). Si `numOfRowsWithWarning` est supérieur à zéro, cette valeur indique le nombre d’avertissements qui se sont produits.

Utilisez le point d’entrée [Get Import Program Member Warnings](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET) pour récupérer les enregistrements et les causes des lignes d’avertissement en transmettant le paramètre de chemin d’accès `batchId`.

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

Le point d’entrée répond avec un fichier indiquant les lignes qui ont généré des avertissements, ainsi qu’un message indiquant pourquoi l’enregistrement a généré un avertissement. Le format du fichier est identique à celui spécifié dans `format` paramètre lors de la création de la tâche. Un champ supplémentaire est ajouté à chaque enregistrement avec une description de l&#39;avertissement.

Supposons, par exemple, que vous importiez le fichier suivant avec une adresse e-mail non valide :

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

Lorsque vous vérifiez le statut de la tâche, vous voyez `numOfRowsWithWarning` est 1, ce qui indique qu’un avertissement s’est produit :

```
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

Vous récupérez ensuite le fichier d’avertissements pour plus d’informations sur l’avertissement :

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

```
firstName,lastName,email,title,company,leadScore,Import Warning Reason
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0,Invalid email address
```
