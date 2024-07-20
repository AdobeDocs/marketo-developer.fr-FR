---
title: Importation de membres de programme en bloc
feature: REST API
description: Importation par lots des données de membre.
exl-id: b0e1039a-fe9b-4fb7-9aa6-9980a06da673
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 0%

---

# Importation de membres de programme en bloc

[Référence du point de terminaison d’importation de membres du programme en masse](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members)

Pour de grandes quantités d’enregistrements de membres de programme, les membres de programme peuvent être importés de manière asynchrone avec l’ [API en masse](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members). Vous pouvez ainsi importer une liste d’enregistrements dans Marketo à l’aide d’un fichier plat avec les délimiteurs (virgule, tabulation ou point-virgule). Le fichier peut contenir n’importe quel nombre d’enregistrements, à condition que la taille totale du fichier soit inférieure à 10 Mo. L&#39;opération d&#39;enregistrement est &quot;insert or update&quot; uniquement.

## Limites de traitement

Vous pouvez envoyer plusieurs demandes d’importation en bloc, avec des restrictions. Chaque requête est ajoutée en tant que tâche à une file d’attente FIFO à traiter. Au maximum deux tâches sont traitées simultanément. Au maximum dix tâches sont autorisées dans la file d’attente à un moment donné (y compris les deux tâches en cours de traitement). Si vous dépassez la limite de dix tâches, une erreur &quot;1016, Trop d&#39;importations&quot; est renvoyée.

## Importer fichier

La première ligne du fichier doit être un en-tête qui répertorie les noms d’API REST correspondants sous forme de champs pour mapper les valeurs de chaque ligne. Les noms des API REST peuvent être récupérés à l’aide des points d’entrée [Description du prospect](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) et/ou [Description du membre du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeProgramMemberUsingGET). Les enregistrements peuvent contenir des champs de piste, des champs de piste personnalisés et des champs membres de programme personnalisés.

Un fichier type suit ce modèle de base :

```
email,firstName,lastName
test@example.com,John,Doe
```

L’appel lui-même est effectué à l’aide du type de contenu `multipart/form-data`.

Ce type de requête peut être difficile à implémenter. Il est donc vivement recommandé d’utiliser une implémentation de bibliothèque existante.

## Création d’une tâche

Le point d’entrée [Importer des membres de programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) lit un fichier contenant des enregistrements de membre de programme et les ajoute à un programme avec un état donné. Les enregistrements peuvent contenir à la fois des champs de piste et des champs personnalisés de membre de programme. Tous les enregistrements doivent inclure le champ email, qui est utilisé à des fins de déduplication.

Le paramètre de chemin `programId` spécifie le programme auquel les membres sont ajoutés.

Il existe trois paramètres de requête requis. Le paramètre `format` spécifie le format de fichier d’importation (CSV, TSV ou SSV), le paramètre `programMemberStatus` spécifie l’état du programme pour les membres ajoutés au programme et le paramètre `file` contient le nom du fichier d’importation contenant les enregistrements des membres du programme.

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

Notez dans la réponse à notre appel qu’il existe un champ `batchId` et `status` pour l’enregistrement dans le tableau de résultat. Ce point de terminaison étant asynchrone, il peut renvoyer l’état En file d’attente, Importation ou Échec. Vous devez conserver le `batchId` pour obtenir l’état de la tâche d’importation et récupérer les échecs et/ou les avertissements une fois la tâche terminée. Le `batchId` reste valide pendant sept jours.

Dans l’exemple ci-dessus, une méthode simple pour appeler le point de terminaison consiste à utiliser cURL à partir de la ligne de commande :

```bash
curl -i -F format='csv' -F programMemberStatus='On List' -F file='@Lead-House-Lannister.csv' -F access_token='<Access Token>' <REST API Endpoint Base URL>/bulk/v1/program/{programId}/members/import.json
```

Où le fichier d’importation &quot;Lead-House-Lannister.csv&quot; contient les éléments suivants :

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

## État de la tâche d’interrogation

Une fois le traitement d&#39;import créé, vous devez en interroger le statut. Il est recommandé d’interroger la tâche d’importation toutes les 5 à 30 secondes. Pour ce faire, transmettez le paramètre de chemin `batchId` au point de terminaison [Get Import Program Member Status](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) .

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

Cette réponse affiche une importation terminée. L’état peut être : Terminé, En file d’attente, Importation, Échec.

Si la tâche est terminée, vous disposez d’une liste du nombre de lignes traitées, échouées ou avec des avertissements. Le paramètre message peut également donner le message d’échec si l’état est En échec.

## Échecs

Les échecs sont indiqués par l’attribut `numOfRowsFailed` dans la réponse [Get Import Program Member Status](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) . Si numOfRowsFailed est supérieur à zéro, cette valeur indique le nombre d’échecs qui se sont produits.

Utilisez le point d’entrée [Obtenir les échecs des membres du programme d’importation](http://TODO) pour récupérer les enregistrements et les causes des lignes en échec en transmettant le paramètre de chemin d’accès `batchId`.

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

Le point de terminaison répond avec un fichier indiquant les lignes qui ont échoué, ainsi qu’un message indiquant pourquoi l’enregistrement a échoué. Le format du fichier est le même que celui spécifié dans le paramètre `format` lors de la création de la tâche. Un champ supplémentaire est ajouté à chaque enregistrement avec une description de l’échec.

Supposons, par exemple, que vous importiez le fichier suivant avec un score de piste non valide :

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

Lorsque vous vérifiez l’état de la tâche, `numOfRowsFailed` est 1 ce qui indique qu’un échec s’est produit :

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

Récupérez ensuite le fichier failure pour plus d’informations sur l’échec :

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

```
firstName,lastName,email,title,company,leadScore,Import Failure Reason
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD,Invalid data type in field Lead Score
```

## Avertissements

Les avertissements sont indiqués par l’attribut `numOfRowsWithWarning` dans la réponse [Get Import Program Member Status](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) . Si `numOfRowsWithWarning` est supérieur à zéro, cette valeur indique le nombre d’avertissements qui se sont produits.

Utilisez le point de terminaison [Get Import Program Member Warnings](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET) pour récupérer les enregistrements et les causes des lignes d’avertissement en transmettant le paramètre de chemin d’accès `batchId`.

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

Le point de terminaison répond avec un fichier indiquant les lignes qui ont généré des avertissements, ainsi qu’un message indiquant pourquoi l’enregistrement a généré un avertissement. Le format du fichier est le même que celui spécifié dans le paramètre `format` lors de la création de la tâche. Un champ supplémentaire est ajouté à chaque enregistrement avec une description de l’avertissement.

Supposons, par exemple, que vous importiez le fichier suivant avec une adresse électronique non valide :

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

Lorsque vous vérifiez l’état de la tâche, `numOfRowsWithWarning` est 1 ce qui indique qu’un avertissement s’est produit :

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

Vous récupérez ensuite le fichier des avertissements pour plus de détails sur l’avertissement :

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

```
firstName,lastName,email,title,company,leadScore,Import Warning Reason
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0,Invalid email address
```
