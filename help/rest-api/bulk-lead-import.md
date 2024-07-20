---
title: Importation de pistes en bloc
feature: REST API
description: Importation par lots de données de piste.
exl-id: 615f158b-35f9-425a-b568-0a7041262504
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '803'
ht-degree: 0%

---

# Importation de pistes en bloc

[Référence du point de terminaison d’importation de pistes en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads)

Pour un grand nombre d’enregistrements de piste, les pistes peuvent être importées de manière asynchrone avec l’ [API en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST). Vous pouvez ainsi importer une liste d’enregistrements dans Marketo à l’aide d’un fichier plat avec les délimiteurs (virgule, tabulation ou point-virgule). Le fichier peut contenir n’importe quel nombre d’enregistrements, à condition que la taille totale du fichier soit inférieure à 10 Mo. L&#39;opération d&#39;enregistrement est &quot;insert or update&quot; uniquement.

## Limites de traitement

Vous pouvez envoyer plusieurs demandes d’importation en bloc, avec des restrictions. Chaque requête est ajoutée en tant que tâche à une file d’attente FIFO à traiter. Au maximum deux tâches sont traitées simultanément. Au maximum dix tâches sont autorisées dans la file d’attente à un moment donné (y compris les deux tâches en cours de traitement). Si vous dépassez la limite de dix tâches, une erreur &quot;1016, Trop d&#39;importations&quot; est renvoyée.

## Importer fichier

La première ligne du fichier doit être un en-tête qui répertorie les champs API REST correspondants dans lesquels mapper les valeurs de chaque ligne. Un fichier type suit ce modèle de base :

```
email,firstName,lastName
test@example.com,John,Doe
```

Le champ `externalCompanyId` peut être utilisé pour lier l’enregistrement de piste à un enregistrement de société. Le champ `externalSalesPersonId` peut être utilisé pour lier l’enregistrement de piste à un enregistrement de personne effectuant le vente.

L’appel lui-même est effectué à l’aide du type de contenu `multipart/form-data`.

Ce type de requête peut être difficile à implémenter. Il est donc vivement recommandé d’utiliser une implémentation de bibliothèque existante.

## Création d’une tâche

Pour effectuer une demande d’importation en bloc, vous devez définir l’en-tête content-type sur &quot;multipart/form-data&quot; et inclure au moins un paramètre de fichier avec votre contenu de fichier, ainsi qu’un paramètre de format avec la valeur &quot;csv&quot;, &quot;tsv&quot; ou &quot;ssv&quot; indiquant votre format de fichier.

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

Ce point d’entrée utilise [multipart/form-data comme type de contenu](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Cela peut s’avérer difficile à obtenir. Il est donc recommandé d’utiliser une bibliothèque de support HTTP pour votre langue de choix. Pour ce faire, une méthode simple avec cURL à partir de la ligne de commande se présente comme suit :

```
curl -i -F format=csv -F file=@lead_data.csv -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/leads.json
```

Où le fichier d’importation &quot;lead_data.csv&quot; contient les éléments suivants :

```
FirstName,LastName,Email,Company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
```

Vous pouvez également éventuellement inclure les paramètres `lookupField`, `listId` et `partitionName` dans votre requête. `lookupField` vous permet de sélectionner un champ spécifique sur lequel dédupliquer, tout comme les pistes de synchronisation, et les adresses email par défaut. Vous pouvez spécifier `id` comme `lookupField` pour indiquer une opération de &quot;mise à jour uniquement&quot;. `listId` vous permet de sélectionner une liste statique à laquelle importer la liste de pistes ; les pistes de la liste deviendront ainsi membres de cette liste statique, en plus des créations ou mises à jour provoquées par l’importation. `partitionName` sélectionne une partition spécifique à importer. Pour plus d’informations, reportez-vous à la section Espaces de travail et partition .

Notez dans la réponse à notre appel qu’il n’existe pas de liste de succès ou d’échecs comme avec les pistes de synchronisation, mais un identifiant de lot et un champ d’état pour l’enregistrement dans le tableau de résultats. En effet, cette API est asynchrone et peut renvoyer l’état En file d’attente, Importation ou Échec. Vous devez conserver le paramètre batchId pour obtenir l’état de la tâche d’importation et récupérer les échecs et/ou les avertissements une fois la tâche terminée. L’identifiant de lot reste valide pendant sept jours.

## État de la tâche d’interrogation

Il est recommandé d’interroger la tâche toutes les 5 à 30 secondes, en fonction de la latence requise et des limites des appels API, pour connaître l’état de la tâche d’importation. Vous pouvez le faire avec l’API Get Import Lead Status.

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

Cette réponse affiche un import terminé, mais l’état peut être l’un des suivants :

- Terminé
- En fil d&#39;attente
- Importation
- Échec

Si la tâche est terminée, vous disposez d’une liste du nombre de lignes traitées, ayant échoué, sur celles qui comportent des avertissements. Le paramètre message peut également donner le message d’échec si l’état est En échec.

## Échecs

Les échecs sont indiqués par l’attribut &quot;numOfRowsFailed&quot; dans la réponse Get Import Lead Status. Si &quot;numOfRowsFailed&quot; est supérieur à zéro, cette valeur indique le nombre d’échecs qui se sont produits.

Pour récupérer les enregistrements et les causes des lignes qui ont échoué, vous devez récupérer le fichier d’échec :

```
GET /bulk/v1/leads/batch/{id}/failures.json
```

L’API répond avec un fichier indiquant les lignes qui ont échoué, ainsi qu’un message indiquant pourquoi l’enregistrement a échoué. Le format du fichier est le même que celui spécifié dans le paramètre &quot;format&quot; lors de la création de la tâche. Un champ supplémentaire est ajouté à chaque enregistrement avec une description de l’échec.

## Avertissements

Les avertissements sont indiqués par l’attribut &quot;numOfRowsWithWarning&quot; dans la réponse Get Import Lead Status. Si &quot;numOfRowsWithWarning&quot; est supérieur à zéro, cette valeur indique le nombre d’avertissements qui se sont produits.

Pour récupérer les enregistrements et les causes des lignes d’avertissement, récupérez le fichier d’avertissement :

```
GET /bulk/v1/leads/batch/{id}/warnings.json
```

L’API répond avec un fichier indiquant les lignes qui ont généré des avertissements, ainsi qu’un message indiquant pourquoi l’enregistrement a échoué. Le format du fichier est le même que celui spécifié dans le paramètre &quot;format&quot; lors de la création de la tâche. Un champ supplémentaire est ajouté à chaque enregistrement avec une description de l’avertissement.
