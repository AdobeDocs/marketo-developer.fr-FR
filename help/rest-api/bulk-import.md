---
title: Importation en bloc
feature: REST API
description: Import en bloc Marketo pour le chargement de prospects, d’objets personnalisés et de membres de programme par le biais de chargements multipartie, la création de tâches asynchrones, le statut d’interrogation et la gestion des échecs.
exl-id: f7922fd2-8408-4d04-8955-0f8f58914d24
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 2%

---

# Importation en bloc

Marketo fournit des interfaces pour l’insertion de grands ensembles de données de personne et de données liées à la personne, appelées Importation en bloc. Actuellement, des interfaces sont proposées pour trois types d’objets :

- Leads (personnes)
- Objets personnalisés
- Membres du programme

L’importation en bloc est effectuée en créant une tâche, puis en attendant que la tâche termine la lecture d’un fichier. Ces traitements sont exécutés de manière asynchrone et peuvent être interrogés pour récupérer le statut de l’importation. Les fichiers sont chargés à l’aide de HTTP multipart/form-data selon la norme RFC 2399.

Les points d’entrée de l’API en bloc ne sont pas précédés du préfixe « /rest » comme les autres points d’entrée.

## Authentification

Les API d’importation en bloc utilisent la même méthode d’authentification OAuth 2.0 que les autres API REST Marketo.  Un jeton d’accès valide envoyé en tant que `Authorization: Bearer {_AccessToken_}` d’en-tête HTTP est nécessaire.

>[!IMPORTANT]
>
>La prise en charge de l’authentification à l’aide du paramètre de requête **access_token** sera supprimée le 30 juin 2025. Si votre projet utilise un paramètre de requête pour transmettre le jeton d’accès, il doit être mis à jour afin d’utiliser l’en-tête **Authorization** dès que possible. Le nouveau développement doit utiliser exclusivement l’en-tête **Authorization**.

## Limites

- Nbre max. de traitements d&#39;importation simultanés : 2
- Nombre maximal de tâches d&#39;importation en file d&#39;attente (y compris les tâches d&#39;importation actuelles) : 10
- Taille max du fichier d&#39;importation : 10 Mo

## Autorisations

L’importation en bloc utilise le même modèle d’autorisations que l’API REST Marketo et ne nécessite aucune autorisation spéciale supplémentaire pour utiliser , bien que des autorisations spécifiques soient requises pour chaque ensemble de points d’entrée.

## Opérations d’enregistrement

L’importation en bloc est une opération d’enregistrement « insérer ou mettre à jour ». Si un enregistrement correspondant est trouvé dans la base de données, il est mis à jour. Dans le cas contraire, un nouvel enregistrement est créé. La réponse d’importation en bloc n’indique pas si un enregistrement donné a été mis à jour ou inséré.

## Création d’un traitement

Les API d’import en bloc Marketo utilisent le concept d’une tâche pour exécuter l’import de données. Examinons la création d’une tâche d’importation de prospect simple à l’aide du point d’entrée [Importer des prospects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST).  Notez que ce point d’entrée utilise [multipart/form-data comme type de contenu](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Comme il peut s’avérer difficile d’y parvenir, il est recommandé d’utiliser une bibliothèque de prise en charge HTTP pour la langue de votre choix.  Si vous êtes juste en train de vous mouiller les pieds, nous vous suggérons d&#39;utiliser [curl](https://curl.se/).

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

Cette requête va créer un traitement qui importera les valeurs contenues dans le fichier CSV nommé « leads.csv » avec les en-têtes de colonne « FirstName », « LastName », « Email », « Company ».

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

Lorsque nous soumettons le traitement, il renvoie un batchId, que nous pouvons ensuite utiliser pour vérifier son statut.

### Paramètres communs

Chaque point d’entrée de création de tâche partage certains paramètres communs pour la configuration du format de fichier, des noms de champ et du filtre d’une tâche d’extraction en bloc.  Chaque sous-type de tâche d’extraction peut avoir des paramètres supplémentaires :

| Paramètre | Type de données | Notes |
|---|---|---|
| format | Chaîne | Détermine le format de fichier des données importées avec des options pour les valeurs séparées par des virgules, des tabulations et des points-virgules. Accepte l’un des formats suivants : CSV, SSV, TSV. Le format par défaut est CSV. |
| fichier | Chaîne | Les données sont spécifiées via des données de formulaire à parties multiples dans le fichier . |

## Interroger le statut de la tâche

La détermination du statut de la tâche est simple à l’aide du point d’entrée [Obtenir le statut du prospect d’importation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadStatusUsingGET).

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

Le membre de `status` interne indique la progression de la tâche. Il peut s’agir de l’une des valeurs suivantes : En file d’attente, Importation, Terminé, Échec. Dans ce cas, notre travail est terminé, alors nous pouvons arrêter les sondages.

## Échecs

Les échecs sont indiqués par l’attribut `numOfRowsFailed` dans la réponse Obtenir le statut du lead d’importation . Si `numOfRowsFailed` est supérieur à zéro, cette valeur indique le nombre d’échecs qui se sont produits.

Pour récupérer les enregistrements et les causes des lignes ayant échoué, vous devez récupérer le fichier d’échec à l’aide du point d’entrée [Get Import Lead Failures](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadFailuresUsingGET).

```
GET /bulk/v1/leads/batch/{batchId}/failures.json
```

Le fichier indique les lignes ayant échoué, ainsi qu’un message indiquant pourquoi l’enregistrement a échoué.
