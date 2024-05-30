---
title: "Extraction de pistes en bloc"
feature: REST API
description: "Extraction par lots des données de piste."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1173'
ht-degree: 2%

---


# Extraction de pistes en bloc

[Référence du point de fin d’extraction de pistes en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads)

L’ensemble des API REST d’extraction de pistes en bloc fournit une interface programmatique pour récupérer de grands ensembles d’enregistrements de pistes/de personnes à partir de Marketo. Il peut également être utilisé pour récupérer les pistes de manière incrémentielle en fonction de la date de création de l’enregistrement, de la mise à jour la plus récente, de l’appartenance à une liste statique ou de l’appartenance à une liste dynamique. Interface recommandée pour les cas d’utilisation qui nécessitent un échange continu de données entre Marketo et un ou plusieurs systèmes externes, à des fins d’ETL, d’entrepôt de données et d’archivage.

## Permissions

Les API Bulk Lead Extract exigent que l’utilisateur de l’API propriétaire dispose d’un rôle avec l’une des autorisations de piste en lecture seule ou de piste en lecture-écriture, ou les deux.

## Filtres

Les pistes prennent en charge diverses options de filtrage. Certains filtres, dont le `updatedAt`, `smartListName`, et `smartListId` nécessitent des composants d’infrastructure supplémentaires qui n’ont pas encore été déployés pour tous les abonnements. Un seul type de filtre peut être spécifié par tâche d’exportation.

| Type de filtre | Type de données | Notes |
|---|---|---|
| createdAt | Plage de dates | Accepte un objet JSON avec les membres `startAt` et `endAt`. `startAt` accepte une date et une heure représentant le filigrane bas, et `endAt` accepte une date et une heure représentant le filigrane élevé. La période doit être de 31 jours ou moins. Datetimes doit être au format ISO-8601, sans millisecondes. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles créés au cours de la période. |
| updatedAt* | Plage de dates | Accepte un objet JSON avec les membres `startAt` et `endAt`. `startAt` accepte une date et une heure représentant le filigrane bas, et `endAt` accepte une date et une heure représentant le filigrane élevé. La période doit être de 31 jours ou moins. Datetimes doit être au format ISO-8601, sans millisecondes. Remarque : Ce filtre ne filtre pas le champ visible &quot;updatedAt&quot;, qui reflète uniquement les mises à jour apportées aux champs standard. Elle filtre les données en fonction du moment où la mise à jour la plus récente des champs a été effectuée vers un enregistrementJobs de piste avec ce type de filtre renvoie tous les enregistrements accessibles qui ont été mis à jour le plus récemment au cours de la période. |
| staticListName | Chaîne | Accepte le nom d’une liste statique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres de la liste statique au moment où le traitement de la tâche commence. Récupérez les noms de liste statiques à l’aide du point de terminaison Get List. |
| staticListId | Entier | Accepte l’identifiant d’une liste statique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres de la liste statique au moment où le traitement de la tâche commence. Récupérez les identifiants de liste statiques à l’aide du point de terminaison Get List. |
| smartListName* | Chaîne | Accepte le nom d’une liste dynamique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres des listes dynamiques au moment où le traitement de la tâche commence. Récupérez les noms de liste dynamique à l’aide du point de terminaison Get Smart Lists. |
| smartListId* | Entier | Accepte l’identifiant d’une liste dynamique. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui sont membres des listes dynamiques au moment où le traitement de la tâche commence. Récupérez les identifiants de liste dynamique à l’aide du point de terminaison Get Smart Lists. |


Le type de filtre n’est pas disponible pour certains abonnements. En cas d’indisponibilité de votre abonnement, vous recevez une erreur lors de l’appel du point de terminaison Create Export Lead Job (&quot;1035, Unsupported filter type for target subscription&quot;). Les clients peuvent contacter le support Marketo pour que cette fonctionnalité soit activée dans leur abonnement.

## Options

Le point de fin Créer une tâche de piste d’exportation fournit plusieurs options de mise en forme, ce qui permet à l’utilisateur d’inclure des champs spécifiques dans le fichier exporté, de renommer les en-têtes de colonne de ces champs et de définir le format du fichier exporté.

| Paramètre | Type de données | Requis | Notes |
|---|---|---|---|
| Champs | Tableau[Chaîne] | Oui | Le paramètre fields accepte un tableau JSON de chaînes. Chaque chaîne doit correspondre au nom de l’API REST d’un champ de piste Marketo. Les champs répertoriés sont inclus dans le fichier exporté. L’en-tête de colonne de chaque champ est le nom de l’API REST de chaque champ, sauf s’il est remplacé par columnHeader. Remarque : Lorsque la variable [!DNL Adobe Experience Cloud Audience Sharing] est activée, un processus de synchronisation des cookies qui associe [!DNL Adobe Experience Cloud] Identifiant (ECID) avec des pistes Marketo. Vous pouvez spécifier le champ &quot;ecids&quot; pour inclure des ECID dans le fichier d’exportation. |
| columnHeaderNames | Objet | Non | Objet JSON contenant des paires clé-valeur de noms d’en-tête de champ et de colonne. La clé doit être le nom d’un champ inclus dans la tâche d’exportation. Il s’agit du nom de l’API du champ qui peut être récupéré en appelant &quot;Description de la piste&quot;. La valeur est le nom de l’en-tête de colonne exporté pour ce champ. |
| format | Chaîne | Non | Accepte l’un des paramètres suivants : CSV, TSV, SSV. Le fichier exporté est rendu sous la forme d’un fichier de valeurs séparées par des virgules, de valeurs séparées par des tabulations ou de valeurs séparées par des espaces, respectivement, s’il est défini. La valeur par défaut est CSV si elle n’est pas définie. |


## Création d’une tâche

Les paramètres de la tâche sont définis avant le démarrage de l’exportation à l’aide de la fonction [Création d’une tâche de piste d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/createExportLeadsUsingPOST) point de terminaison . Nous devons définir la variable `fields` qui sont nécessaires à l&#39;export, le type de paramètres du `filter`, la variable `format` du fichier et les noms des en-têtes de colonne, le cas échéant.

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

Cette requête commencera à exporter un ensemble de pistes créées entre le 1er janvier 2017 et le 31 janvier 2017, y compris les valeurs de la variable `firstName`, `lastName`, `id`, et `email` des champs.

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

Cela renvoie une réponse d’état indiquant que la tâche a été créée. La tâche a été définie et créée, mais elle n’a pas encore été lancée. Pour ce faire, la variable [Enqueue Export Lead Job](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/enqueueExportLeadsUsingPOST) endpoint doit être appelé à l’aide de l’exportId de la réponse d’état de création :

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

Cette fonction répond par une `status` de &quot;En file d’attente&quot;, après quoi il sera défini sur &quot;Traitement&quot; lorsqu’un emplacement d’exportation est disponible.

## État de la tâche d’interrogation

`Note:` L’état ne peut être récupéré que pour les tâches qui ont été créées par le même utilisateur de l’API.

Puisqu’il s’agit d’un point de terminaison asynchrone, après la création de la tâche, nous devons interroger son état pour déterminer sa progression. Sondage à l’aide du [Obtenir le statut de la tâche de piste d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET) point de terminaison . Le statut n’est mis à jour qu’une fois toutes les 60 secondes, donc une fréquence d’interrogation inférieure à celle-ci n’est pas recommandée, et dans presque tous les cas est toujours excessive. Regardons un peu les sondages.

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

Le point de terminaison d’état répond indiquant que la tâche est toujours en cours de traitement. Le fichier n’est donc pas encore disponible pour la récupération. Une fois que l’état de la tâche est défini sur &quot;Terminé&quot;, elle est préparée pour téléchargement.

Le champ d’état peut répondre avec l’une des réponses suivantes :

- Créé
- En fil d&#39;attente
- En cours de traitement
- Annulé
- Terminé
- Échec

## Récupération de vos données

Pour récupérer le fichier d’une exportation de piste terminée, appelez simplement le [Obtenir un fichier de piste d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsFileUsingGET) Point de terminaison avec votre `exportId`.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

La réponse contient un fichier formaté selon la configuration de la tâche. Le point de terminaison répond avec le contenu du fichier.

Si un champ de piste demandé est vide (ne contient aucune donnée), `null` est placé dans le champ correspondant du fichier d&#39;export. Dans l’exemple ci-dessous, le champ email pour le prospect renvoyé est vide.

```csv
firstName,lastName,email,cookies
Russell,Wilson,null,_mch-localhost-1536605780000-12105
```

Pour prendre en charge la récupération partielle et conviviale en cas de reprise des données extraites, le point de fin de fichier prend éventuellement en charge la plage d’en-tête HTTP du type bytes. Si l’en-tête n’est pas défini, l’intégralité du contenu est renvoyée. En savoir plus sur l’utilisation de l’en-tête Range avec Marketo [Extraction en bloc](bulk-extract.md).

## Annulation d’une tâche

Si une tâche a été mal configurée ou devient inutile, elle peut être facilement annulée à l’aide de la fonction [Annuler l’exportation de la tâche](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/cancelExportLeadsUsingPOST) endpoint :

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

Ceci répond avec un état indiquant que la tâche a été annulée.
