---
title: Extrait de lead en masse
feature: REST API
description: Découvrez comment utiliser les API REST d’extraction de lead en bloc Marketo pour exporter en bloc des leads avec des filtres de date, de liste et de liste dynamique, des champs personnalisés et des formats CSV/TSV.
exl-id: 42796e89-5468-463e-9b67-cce7e798677b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1195'
ht-degree: 2%

---

# Extrait de lead en masse

[Référence de point d’entrée d’extraction de lead en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads)

L’ensemble d’API REST d’extraction de lead en bloc fournit une interface de programmation pour récupérer de grands ensembles d’enregistrements de lead/personne en dehors de Marketo. En outre, il peut être utilisé pour récupérer les prospects de manière incrémentielle en fonction de la date de création de l’enregistrement, de la mise à jour la plus récente, de l’appartenance à une liste statique ou à une liste dynamique. Interface recommandée pour les cas d’utilisation qui nécessitent un échange continu de données entre Marketo et un ou plusieurs systèmes externes, à des fins d’ETL, d’entreposage de données et d’archivage.

## Autorisations

Les API Bulk Lead Extract nécessitent que l’utilisateur de l’API propriétaire dispose d’un rôle avec une ou deux des autorisations Lecture seule, Lead ou Lead en lecture-écriture.

## Filtres

Les prospects prennent en charge différentes options de filtre. Certains filtres, notamment les `updatedAt`, `smartListName` et `smartListId`, nécessitent des composants d’infrastructure supplémentaires qui n’ont pas encore été déployés sur tous les abonnements. Un seul type de filtre peut être spécifié par tâche d&#39;exportation.

| Type de filtre | Type de données | Notes |
|---|---|---|
| createdAt | Période | Accepte un objet JSON avec les membres `startAt` et `endAt`. `startAt` accepte une valeur datetime représentant le filigrane bas et `endAt` une valeur datetime représentant le filigrane haut. La plage doit être de 31 jours ou moins. Les heures de date doivent être au format ISO-8601, sans millisecondes. Les traitements avec ce type de filtre renvoient tous les enregistrements accessibles qui ont été créés au cours de la période. |
| updatedAt* | Période | Accepte un objet JSON avec les membres `startAt` et `endAt`. `startAt` accepte une valeur datetime représentant le filigrane bas et `endAt` une valeur datetime représentant le filigrane haut. La plage doit être de 31 jours ou moins. Les heures de date doivent être au format ISO-8601, sans millisecondes. Remarque : ce filtre ne filtre pas sur le champ visible « updatedAt » qui reflète uniquement les mises à jour apportées aux champs standard. Il filtre en fonction de la date à laquelle la mise à jour la plus récente du champ a été apportée à un enregistrement de prospect. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui ont été mis à jour le plus récemment au cours de la période. |
| staticListName | Chaîne | Accepte le nom d’une liste statique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres de la liste statique au moment où la tâche commence le traitement. Récupérez les noms de listes statiques à l’aide du point d’entrée Get Lists. |
| staticListId | Nombre entier | Accepte l’identifiant d’une liste statique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres de la liste statique au moment où la tâche commence le traitement. Récupérez les identifiants de liste statiques à l’aide du point d’entrée Get Lists. |
| smartListName* | Chaîne | Accepte le nom d’une liste dynamique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres des listes dynamiques au moment où la tâche commence à être traitée. Récupérez les noms des listes dynamiques à l’aide du point d’entrée Get Smart Lists. |
| smartListId* | Nombre entier | Accepte l’identifiant d’une liste dynamique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres des listes dynamiques au moment où la tâche commence à être traitée. Récupérez les ID de liste dynamique à l’aide du point d’entrée Get Smart Lists. |

Le type de filtre n’est pas disponible pour certains abonnements. Si vous n’êtes pas disponible pour votre abonnement, vous recevez une erreur lors de l’appel du point d’entrée Créer une tâche d’exportation de lead (« 1035, Unsupported filter type for target subscription »). Les clients peuvent contacter l’assistance Marketo pour que cette fonctionnalité soit activée dans leur abonnement.

## Options

Le point d’entrée Créer une tâche d’exportation principale fournit plusieurs options de mise en forme, ce qui permet à l’utilisateur d’inclure des champs particuliers dans le fichier exporté, de renommer les en-têtes de colonne de ces champs et le format du fichier exporté.

| Paramètre | Type de données | Obligatoire | Notes |
|---|---|---|---|
| Champs | Array[String] | Oui | Le paramètre fields accepte un tableau JSON de chaînes. Chaque chaîne doit être le nom de l’API REST d’un champ de prospect Marketo. Les champs répertoriés sont inclus dans le fichier exporté. L’en-tête de colonne de chaque champ correspond au nom de l’API REST de chaque champ, sauf si il est remplacé par columnHeader. Remarque : lorsque la fonction [!DNL Adobe Experience Cloud Audience Sharing] est activée, un processus de synchronisation des cookies se produit et associe l’identifiant [!DNL Adobe Experience Cloud] (ECID) aux prospects Marketo. Vous pouvez spécifier le champ « ecid » pour inclure des ECID dans le fichier d’exportation. |
| columnHeaderNames | Objet | Non | Un objet JSON contenant des paires clé-valeur de noms d’en-tête de champ et de colonne. La clé doit être le nom d’un champ inclus dans la tâche d’exportation. Il s’agit du nom de l’API du champ qui peut être récupéré en appelant la fonction Décrire le prospect. La valeur est le nom de l’en-tête de colonne exporté pour ce champ. |
| format | Chaîne | Non | Accepte l’un des formats suivants : CSV, TSV, SSV. Le fichier exporté est rendu sous la forme d’un fichier de valeurs séparées par des virgules, des valeurs séparées par des tabulations ou des valeurs séparées par des espaces, respectivement, s’il est défini. La valeur par défaut est CSV si cette valeur n’est pas définie. |

## Création d’un traitement

Les paramètres de la tâche sont définis avant le lancement de l’exportation à l’aide du point d’entrée [Créer une tâche d’exportation principale](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/createExportLeadsUsingPOST). Nous devons définir les `fields` nécessaires à l’exportation, le type de paramètres du `filter`, le `format` du fichier et les noms des en-têtes de colonne, le cas échéant.

```
POST /bulk/v1/leads/export/create.json
```

```json
{
   "fields": [
      "firstName",
      "lastName",
      "id",
      "email"
   ],
   "format": "CSV",
   "columnHeaderNames": {
      "firstName": "First Name",
      "lastName": "Last Name",
      "id": "Marketo Id",
      "email": "Email Address"
   },
   "filter": {
      "createdAt": {
         "startAt": "2017-01-01T00:00:00Z",
         "`endAt`": "2017-01-31T00:00:00Z"
      }
   }
}
```

Cette requête commencera à exporter un ensemble de prospects créés entre le 1er janvier 2017 et le 31 janvier 2017, y compris les valeurs des champs `firstName`, `lastName`, `id` et `email` correspondants.

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Cette opération renvoie une réponse de statut indiquant que la tâche a été créée. La tâche a été définie et créée, mais elle n&#39;a pas encore été lancée. Pour ce faire, le point d’entrée [Mettre en file d’attente la tâche d’exportation principale](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/enqueueExportLeadsUsingPOST) doit être appelé à l’aide de l’exportId de la réponse de statut de création :

```
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

```json
{
    "requestId": "147e4#16b24d9b913",
    "result": [
        {
            "exportId": "fad2cd1b-e822-4025-be1e-9caa9cf1d4b8",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2019-06-04T23:35:43Z",
            "queuedAt": "2019-06-04T23:36:17Z"
        }
    ],
    "success": true
}
```

Cela répond par un `status` de « Mise en file d’attente », après quoi il est défini sur « Traitement » lorsqu’un emplacement d’exportation est disponible.

## Interroger le statut de la tâche

`Note:` statut ne peut être récupéré que pour les tâches créées par le même utilisateur de l’API.

Puisqu’il s’agit d’un point d’entrée asynchrone, une fois la tâche créée, nous devons interroger son statut pour déterminer sa progression. Interroger à l’aide du point d’entrée [Obtenir le statut de la tâche d’exportation du prospect](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET). Le statut n’est mis à jour qu’une fois toutes les 60 secondes. Il est donc déconseillé d’utiliser une fréquence d’interrogation inférieure à cette fréquence, qui reste excessive dans presque tous les cas. Jetons un coup d&#39;œil aux sondages.

```
GET /bulk/v1/leads/export/{exportId}/status.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Processing",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Le point d’entrée de statut répond indiquant que la tâche est toujours en cours de traitement et que le fichier n’est donc pas encore disponible pour récupération. Une fois que le statut de la tâche est passé à « Terminé », elle a été préparée pour téléchargement.

Le champ de statut peut répondre avec l’une des options suivantes :

- Créé
- En fil d&#39;attente
- En cours de traitement
- Annulé
- Terminé
- Échec

## Récupération de vos données

Pour récupérer le fichier d’une exportation de prospect terminée, appelez simplement le point d’entrée [Obtenir le fichier de prospect d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsFileUsingGET) avec votre `exportId`.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

La réponse contient un fichier formaté selon la configuration de la tâche. Le point d’entrée répond avec le contenu du fichier .

Si un champ de prospect demandé est vide (ne contient aucune donnée), `null` est placé dans le champ correspondant dans le fichier d’exportation. Dans l’exemple ci-dessous, le champ d’e-mail du prospect renvoyé est vide.

```csv
firstName,lastName,email,cookies
Russell,Wilson,null,_mch-localhost-1536605780000-12105
```

Pour prendre en charge la récupération partielle et conviviale des données extraites, le point d’entrée du fichier prend éventuellement en charge la plage d’en-têtes HTTP du type octets. Si l’en-tête n’est pas défini, l’intégralité du contenu est renvoyée. En savoir plus sur l’utilisation de l’en-tête de plage avec Marketo [Extraction en bloc](bulk-extract.md).

## Annulation d’un traitement

Si une tâche n’a pas été configurée correctement ou devient inutile, elle peut facilement être annulée à l’aide du point d’entrée [Annuler la tâche d’exportation du prospect](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/cancelExportLeadsUsingPOST) :

```
POST /bulk/v1/leads/export/{exportId}/cancel.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Cancelled",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Cette opération répond avec un statut indiquant que la tâche a été annulée.
