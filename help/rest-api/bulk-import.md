---
title: Importation en bloc
feature: REST API
description: Import en bloc Marketo pour le chargement de prospects, d’objets personnalisés et de membres de programme par le biais de chargements multipartie, la création de tâches asynchrones, le statut d’interrogation et la gestion des échecs.
exl-id: f7922fd2-8408-4d04-8955-0f8f58914d24
TQID: https://experienceleague.adobe.com/lr9dyX-fY-oJ2LM5P0zE1m24HtFYKQYYbxMkVe--PkE
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 526
ht-degree: 3%

---

# Importation en bloc

L’importation en bloc fournit des interfaces pour insérer de grands ensembles de données de personne et de données liées à la personne. Vous pouvez importer trois types d&#39;objets :

- Leads (personnes)
- Objets personnalisés
- Membres du programme

Pour effectuer un import en bloc, créez une tâche qui lit un fichier chargé. La tâche s’exécute de manière asynchrone. Consultez-la pour récupérer le statut de l’importation.

Chargez les fichiers à l’aide du `multipart/form-data` HTTP selon la norme RFC 2399.

Contrairement aux autres points d’entrée, les points d’entrée de l’API en bloc ne comportent pas de préfixe `/rest`.

## Authentification

Les API d’importation en bloc utilisent la même méthode d’authentification OAuth 2.0 que les autres API REST Marketo. Envoyez un jeton d’accès valide dans l’en-tête HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>La prise en charge de l’authentification à l’aide du paramètre de requête **access_token** sera supprimée le 30 juin 2025. Si votre projet utilise un paramètre de requête pour transmettre le jeton d’accès, il doit être mis à jour afin d’utiliser l’en-tête **Authorization** dès que possible. Le nouveau développement doit utiliser exclusivement l’en-tête **Authorization**.

## Limites

- Nombre maximal de traitements d’importation simultanés : 2
- Nombre maximal de tâches d&#39;importation en file d&#39;attente, y compris les tâches en cours d&#39;importation : 10
- Taille maximale du fichier d’importation : 10 Mo

## Autorisations

L’importation en bloc utilise le même modèle d’autorisations que l’API REST Marketo. Il ne nécessite pas d’autorisations supplémentaires, mais chaque ensemble de points d’entrée nécessite des autorisations spécifiques.

## Opérations d’enregistrement

L’importation en bloc est une opération d’enregistrement « insérer ou mettre à jour ». Si la base de données contient un enregistrement correspondant, l&#39;opération le met à jour. Dans le cas contraire, l’opération crée un enregistrement.

La réponse d’importation en bloc n’indique pas si un enregistrement individuel a été mis à jour ou inséré.

## Création d’un traitement

Créez une tâche d’importation de prospect en appelant le point d’entrée [Importer des prospects](https://developer.adobe.com/marketo-apis/api/mapi#operation/importLeadUsingPOST). Ce point d’entrée utilise [multipart/form-data comme type de contenu](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html).

Utilisez une bibliothèque de prise en charge HTTP pour la langue de votre choix afin de créer la requête multipartie. Vous pouvez également utiliser [curl](https://curl.se/) pour commencer.

```http
POST /bulk/v1/leads.json?format=csv
```

```text
Content-Type: multipart/form-data; boundary=--------------------------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```text
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email
Able,Baker,ablebaker@marketo.com
Charlie,Dog,charliedog@marketo.com
Easy,Fox,easyfox@marketo.com
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

Cette requête crée une tâche qui importe des valeurs du fichier CSV nommé `leads.csv`.

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

La réponse renvoie un `batchId`. Utilisez cette valeur pour vérifier le statut de la tâche.

### Paramètres communs

Chaque point d’entrée de création de tâche partage les paramètres de configuration du fichier d’importation. Un sous-type d’importation peut également prendre en charge des paramètres supplémentaires.

| Paramètre | Type de données | Notes |
| --- | --- | --- |
| format | Chaîne | Détermine le format de fichier des données importées avec des options pour les valeurs séparées par des virgules, des tabulations et des points-virgules. Accepte l’un des formats suivants : CSV, SSV, TSV. Le format par défaut est CSV. |
| fichier | Chaîne | Les données sont spécifiées via des données de formulaire à parties multiples dans le fichier . |

## Interroger le statut de la tâche

Transmettez le `batchId` au point d’entrée [Obtenir le statut du lead d’importation](https://developer.adobe.com/marketo-apis/api/mapi#operation/getImportLeadStatusUsingGET) pour récupérer le statut de la tâche.

```http
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

Le membre `status` indique la progression du traitement. Sa valeur peut être `Queued`, `Importing`, `Complete` ou `Failed`.

Dans cet exemple, la tâche est terminée. L’interrogation peut donc s’arrêter.

## Échecs

L’attribut `numOfRowsFailed` dans la réponse Get Import Lead Status indique le nombre de lignes ayant échoué. Une valeur supérieure à zéro signifie que des échecs se sont produits.

Pour récupérer les enregistrements ayant échoué et leurs causes, utilisez le point d’entrée [Obtenir les échecs d’importation des leads](https://developer.adobe.com/marketo-apis/api/mapi#operation/getImportLeadFailuresUsingGET).

```http
GET /bulk/v1/leads/batch/{batchId}/failures.json
```

Le fichier d’échec identifie chaque ligne ayant échoué et explique pourquoi l’enregistrement a échoué.
