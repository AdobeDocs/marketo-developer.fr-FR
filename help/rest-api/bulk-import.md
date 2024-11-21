---
title: Importation en bloc
feature: REST API
description: Importation par lots de données de personne.
exl-id: f7922fd2-8408-4d04-8955-0f8f58914d24
source-git-commit: e7d893a81d3ed95e34eefac1ee8f1ddd6852f5cc
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 2%

---

# Importation en bloc

Marketo fournit des interfaces pour l’insertion de grands ensembles de données liées aux personnes et aux personnes, appelés Importation en bloc. Actuellement, les interfaces sont proposées pour trois types d’objets :

- Leads (Personnes)
- Objets personnalisés
- Membres du programme

L’importation en bloc est effectuée en créant une tâche, puis en attendant que la tâche termine la lecture d’un fichier. Ces traitements sont exécutés de manière asynchrone et peuvent être interrogés pour récupérer l’état de l’importation. Les fichiers sont chargés à l’aide de HTTP multipart/form-data selon la norme RFC 2399.

Les points d’entrée d’API en bloc ne sont pas précédés de &quot;/rest&quot; comme les autres points d’entrée.

## Authentification

Les API d’importation en bloc utilisent la même méthode d’authentification OAuth 2.0 que les autres API REST Marketo.  Cela nécessite un jeton d’accès valide envoyé en tant qu’en-tête HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>La prise en charge de l’authentification à l’aide du paramètre de requête **access_token** a été supprimée le 30 juin 2025. Si votre projet utilise un paramètre de requête pour transmettre le jeton d’accès, il doit être mis à jour afin d’utiliser l’en-tête **Authorization** dès que possible. Le nouveau développement doit utiliser exclusivement l’en-tête **Authorization**.

## Limites

- Nb max. de tâches d’importation simultanée : 2
- Tâches d’importation en file d’attente max. (y compris les tâches d’importation en cours) : 10
- Taille max. du fichier d’importation : 10 Mo

## Autorisations

L’importation en bloc utilise le même modèle d’autorisations que l’API REST Marketo. Elle ne nécessite aucune autorisation spéciale supplémentaire pour être utilisée, bien que des autorisations spécifiques soient requises pour chaque ensemble de points de terminaison.

## Opérations d’enregistrement

L’import en masse est une opération d’enregistrement &quot;insert or update&quot;. Si un enregistrement correspondant se trouve dans la base de données, il est mis à jour. Dans le cas contraire, un nouvel enregistrement est créé. La réponse d’importation en bloc n’indique pas si un enregistrement donné a été mis à jour ou inséré.

## Création d’une tâche

Les API d’import en bloc Marketo utilisent le concept d’un traitement pour exécuter l’import de données. Examinons la création d’une tâche d’importation de pistes simple à l’aide du point de terminaison [Import Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST).  Notez que ce point de terminaison utilise [multipart/form-data comme type de contenu](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Cela peut s’avérer difficile à obtenir. Il est donc recommandé d’utiliser une bibliothèque de support HTTP pour votre langue de choix.  Si vous vous mouillez juste les pieds, nous vous suggérons d&#39;utiliser [curl](https://curl.se/).

```
POST /bulk/v1/leads.json?format=csv
```

```
Content-Type: multipart/form-data; boundary=--------------------------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email
Able,Baker,ablebaker@marketo.com
Charlie,Dog,charliedog@marketo.com
Easy,Fox,easyfox@marketo.com
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

Cette requête crée une tâche qui importe les valeurs contenues dans le fichier CSV nommé &quot;leads.csv&quot; avec les en-têtes de colonne &quot;Prénom&quot;, &quot;Nom&quot;, &quot;Adresse électronique&quot;, &quot;Société&quot;.

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

Lorsque nous envoyons la tâche, elle renvoie un identifiant de lot, que nous pouvons ensuite utiliser pour vérifier son état.

### Paramètres communs

Chaque point de fin de création de tâche partage certains paramètres communs pour la configuration du format de fichier, des noms de champ et du filtre d’une tâche d’extraction en masse.  Chaque sous-type de tâche d’extraction peut comporter des paramètres supplémentaires :

| Paramètre | Type de données | Notes |
|---|---|---|
| format | Chaîne | Détermine le format de fichier des données importées avec des options pour les valeurs séparées par des virgules, les valeurs séparées par des tabulations et les valeurs séparées par des points-virgules. Accepte l’un des paramètres suivants : CSV, SSV, TSV. Le format par défaut est CSV. |
| fichier | Chaîne | Les données sont spécifiées par le biais de données de formulaire en plusieurs parties dans le fichier . |


## État de la tâche d’interrogation

Il est simple de déterminer l’état de la tâche à l’aide du point de terminaison [Get Import Lead Status](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadStatusUsingGET).

```
GET /bulk/v1/leads/batch/{batchId}.json
```

```json
{
    "requestId": "1f63#15d6738fd15",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Complete",
            "numOfLeadsProcessed": 3,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "message": "Import succeeded, 3 records imported (3 members)"
        }
    ],
    "success": true
}
```

Le membre `status` interne indique la progression de la tâche et peut être l’une des valeurs suivantes : En file d’attente, Importation, Terminé, Échec. Dans ce cas, notre travail est terminé, nous pouvons donc arrêter les sondages.

## Échecs

Les échecs sont indiqués par l’attribut `numOfRowsFailed` dans la réponse Get Import Lead Status. Si `numOfRowsFailed` est supérieur à zéro, cette valeur indique le nombre d’échecs qui se sont produits.

Pour récupérer les enregistrements et les causes des lignes qui ont échoué, vous devez récupérer le fichier d’échec à l’aide du point de terminaison [Obtenir les échecs de piste d’importation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadFailuresUsingGET).

```
GET /bulk/v1/leads/batch/{batchId}/failures.json
```

Le fichier indique les lignes qui ont échoué, ainsi qu’un message indiquant pourquoi l’enregistrement a échoué.
