---
title: Importation de leads en bloc
feature: REST API
description: Créez et surveillez des importations de leads en bloc asynchrones dans Marketo avec CSV TSV ou SSV.
exl-id: 615f158b-35f9-425a-b568-0a7041262504
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---

# Importation de leads en bloc

[Référence de point d’entrée d’importation de leads en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads)

Pour un grand nombre d’enregistrements de prospect, les prospects peuvent être importés de manière asynchrone à l’aide de l’[API en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST). Vous pouvez ainsi importer une liste d’enregistrements dans Marketo à l’aide d’un fichier plat avec les délimiteurs (virgule, tabulation ou point-virgule). Le fichier peut contenir un nombre illimité d’enregistrements, à condition que la taille totale du fichier soit inférieure à 10 Mo. L’opération d’enregistrement est « insert or update » uniquement.

## Limites de traitement

Vous êtes autorisé à soumettre plusieurs demandes d’importation en bloc, avec certaines limitations. Chaque demande est ajoutée en tant que tâche à une file d’attente FIFO pour être traitée. Deux traitements au maximum sont traités en même temps. Dix tâches au maximum sont autorisées dans la file d’attente à tout moment donné (y compris les deux en cours de traitement). Si vous dépassez le nombre maximal de dix traitements, une erreur « 1016, Too many imports » est renvoyée.

## Importer fichier

La première ligne du fichier doit être un en-tête qui répertorie les champs API REST correspondants pour mapper les valeurs de chaque ligne. Un fichier type suit ce modèle de base :

```
email,firstName,lastName
test@example.com,John,Doe
```

Le champ `externalCompanyId` peut être utilisé pour lier l’enregistrement du prospect à un enregistrement d’entreprise. Le champ `externalSalesPersonId` peut être utilisé pour lier l&#39;enregistrement du prospect à un enregistrement de vendeur.

L’appel lui-même est effectué à l’aide du type de contenu `multipart/form-data`.

Ce type de requête pouvant être difficile à implémenter, il est vivement recommandé d’utiliser une implémentation de bibliothèque existante.

## Création d’un traitement

Pour effectuer une demande d’importation en bloc, vous devez définir votre en-tête de type de contenu sur « multipart/form-data » et inclure au moins un paramètre de fichier avec le contenu de votre fichier, ainsi qu’un paramètre de format avec la valeur « csv », « tsv » ou « ssv » indiquant votre format de fichier.

```
POST /bulk/v1/leads.json?format=csv
```

```
Content-Type: multipart/form-data; boundary=------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

FirstName,LastName,Email,Company
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

Ce point d’entrée utilise [multipart/form-data comme type de contenu](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Comme il peut s’avérer difficile d’y parvenir, il est recommandé d’utiliser une bibliothèque de prise en charge HTTP pour la langue de votre choix. Voici un moyen simple d’effectuer cette opération avec cURL à partir de la ligne de commande :

```
curl -i -F format=csv -F file=@lead_data.csv -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/leads.json
```

Où le fichier d’importation « lead_data.csv » contient les éléments suivants :

```
FirstName,LastName,Email,Company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
```

Vous pouvez également inclure de manière facultative les paramètres `lookupField`, `listId` et `partitionName` dans votre requête. `lookupField` vous permet de sélectionner un champ spécifique à dédupliquer, tout comme les prospects de synchronisation, et utilise par défaut les e-mails. Vous pouvez spécifier `id` comme `lookupField` pour indiquer une opération « mise à jour uniquement ». `listId` vous permet de sélectionner une liste statique vers laquelle importer la liste de prospects. De ce fait, les prospects de la liste deviendront membres de cette liste statique, en plus des créations ou mises à jour provoquées par l’importation. `partitionName` sélectionne une partition spécifique à importer. Voir la section Espaces de travail et partitions pour plus d’informations.

Notez dans la réponse à notre appel qu’il n’existe pas de liste de succès ou d’échecs, comme avec les pistes de synchronisation, mais un batchId et un champ de statut pour l’enregistrement dans le tableau de résultats. Cela est dû au fait que cette API est asynchrone et peut renvoyer un statut de Mise en file d’attente, Importation ou Échec. Vous devez conserver l’ID de lot pour obtenir le statut de la tâche d’importation et récupérer les échecs et/ou les avertissements une fois l’importation terminée. L’ID de lot reste valide pendant sept jours.

## Interroger le statut de la tâche

Il est recommandé d’interroger le traitement toutes les 5 à 30 secondes, selon la latence requise et les limitations d’appels API, pour voir le statut du traitement d’importation. Vous pouvez le faire à l’aide de l’API Get Import Lead Status .

```
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

Cette réponse affiche un import terminé, mais le statut peut être l’un des suivants :

- Terminée
- En fil d&#39;attente
- Importation
- Échec

Si la tâche est terminée, vous disposez d’une liste du nombre de lignes traitées, ayant échoué, pour celles comportant des avertissements. Le paramètre message peut également générer le message failure si le statut est Failed.

## Échecs

Les échecs sont indiqués par l’attribut « numOfRowsFailed » dans la réponse Get Import Lead Status. Si « numOfRowsFailed » est supérieur à zéro, cette valeur indique le nombre d’échecs qui se sont produits.

Pour récupérer les enregistrements et les causes des lignes ayant échoué, vous devez récupérer le fichier d’échec :

```
GET /bulk/v1/leads/batch/{id}/failures.json
```

L’API répond avec un fichier indiquant les lignes ayant échoué, ainsi qu’un message indiquant pourquoi l’enregistrement a échoué. Le format du fichier est identique à celui spécifié dans le paramètre « format » lors de la création de la tâche. Un champ supplémentaire est ajouté à chaque enregistrement avec une description de l’échec.

## Avertissements

Les avertissements sont indiqués par l’attribut « numOfRowsWithWarning » dans la réponse Get Import Lead Status. Si « numOfRowsWithWarning » est supérieur à zéro, cette valeur indique le nombre d’avertissements qui se sont produits.

Pour récupérer les enregistrements et les causes des lignes d&#39;avertissement, récupérez le fichier d&#39;avertissement :

```
GET /bulk/v1/leads/batch/{id}/warnings.json
```

L’API répond avec un fichier indiquant les lignes qui ont généré des avertissements, ainsi qu’un message indiquant pourquoi l’enregistrement a échoué. Le format du fichier est identique à celui spécifié dans le paramètre « format » lors de la création de la tâche. Un champ supplémentaire est ajouté à chaque enregistrement avec une description de l&#39;avertissement.
