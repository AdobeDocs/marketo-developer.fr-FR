---
title: Extraction En Masse
feature: REST API
description: Opérations par lots pour extraire des données Marketo.
exl-id: 6a15c8a9-fd85-4c7d-9f65-8b2e2cba22ff
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '1682'
ht-degree: 1%

---

# Extraction En Masse

Marketo fournit des interfaces pour la récupération de grands ensembles de données relatives aux personnes et à la personne, appelées Extraction en bloc. Actuellement, des interfaces sont proposées pour trois types d’objets :

- Leads (personnes)
- Activités
- Membres du programme
- Objets personnalisés

L’extraction en bloc est effectuée en créant une tâche, en définissant l’ensemble des données à récupérer, en mettant la tâche en file d’attente, en attendant que la tâche termine d’écrire un fichier, puis en récupérant le fichier via HTTP. Ces traitements sont exécutés de manière asynchrone et peuvent être interrogés pour récupérer le statut de l’exportation.

`Note:` points d’entrée de l’API en bloc ne sont pas précédés du préfixe « /rest » comme les autres points d’entrée.

## Authentification

Les API d’extraction en masse utilisent la même méthode d’authentification OAuth 2.0 que les autres API REST Marketo. Pour ce faire, un jeton d’accès valide doit être envoyé en tant que `Authorization: Bearer {_AccessToken_}` d’en-tête HTTP.

>[!IMPORTANT]
>
>La prise en charge de l’authentification à l’aide du paramètre de requête **access_token** sera supprimée le 30 juin 2025. Si votre projet utilise un paramètre de requête pour transmettre le jeton d’accès, il doit être mis à jour afin d’utiliser l’en-tête **Authorization** dès que possible. Le nouveau développement doit utiliser exclusivement l’en-tête **Authorization**.

## Limites

- Nombre maximal de tâches d’exportation simultanées : 2
- Nombre maximal de tâches d’exportation en file d’attente (y compris les tâches d’exportation actuelles) : 10
- Période de conservation des fichiers : sept jours
- Allocation quotidienne d’exportation par défaut : 500MB (qui se réinitialise tous les jours à 12:00AM CST). Augmente la disponibilité à l’achat.
- Durée maximale pour le filtre de plage de dates (createdAt ou updatedAt) : 31 jours

Les filtres d’extraction de leads en bloc pour UpdatedAt et la liste dynamique ne sont pas disponibles pour certains types d’abonnement. S’il n’est pas disponible, un appel au point d’entrée Créer une tâche d’exportation de prospect renvoie une erreur « 1035, Unsupported filter type for target subscription » (1035, type de filtre non pris en charge pour l’abonnement cible). Les clients peuvent contacter l’assistance Marketo pour que cette fonctionnalité soit activée dans leur abonnement.

### File d&#39;attente

Les API d’extraction en masse utilisent une file d’attente de tâches (partagée entre les prospects, les activités, les membres du programme et les objets personnalisés). Les tâches d’extraction doivent d’abord être créées, puis mises en file d’attente en appelant les points d’entrée Créer une tâche d’exportation de lead/activité/membre de programme et Mettre en file d’attente une tâche d’exportation de lead/activité/membre de programme. Une fois placés en file d’attente, les tâches sont extraites de la file d’attente et démarrées lorsque les ressources de calcul sont disponibles.

Le nombre maximal de tâches dans la file d’attente est de 10. Si vous essayez de mettre en file d’attente une tâche lorsque la file d’attente est pleine, le point d’entrée Mettre en file d’attente la tâche d’exportation renvoie une erreur « 1029, Trop de tâches en file d’attente ». Deux traitements au maximum peuvent être exécutés simultanément (statut : « Traitement »).

### Taille du fichier

Les API d’extraction en bloc sont limitées en fonction de la taille sur le disque des données récupérées par une tâche d’extraction en bloc. La taille explicite en octets d’une tâche peut être déterminée en lisant l’attribut `fileSize` à partir de la réponse de statut terminée d’une tâche d’exportation.

Le quota quotidien est de 500MB maximum par jour, qui est partagé entre les prospects, les activités, les membres du programme et les objets personnalisés. Lorsque le quota est dépassé, vous ne pouvez pas créer ou mettre en file d’attente un autre traitement tant que le quota quotidien n’est pas réinitialisé à minuit [heure du Centre](https://en.wikipedia.org/wiki/Central_Time_Zone). Jusque-là, une erreur « 1029, Quota quotidien d’exportation dépassé » est renvoyée. Outre le quota journalier, il n&#39;existe pas de taille de fichier maximale.

Une fois qu’un traitement est mis en file d’attente, il s’exécute jusqu’à la fin (sauf erreur ou annulation du traitement). Si une tâche échoue pour une raison quelconque, vous devez la recréer. Les fichiers sont entièrement écrits uniquement lorsqu’une tâche atteint le statut Terminé (les fichiers partiels ne sont jamais écrits). Vous pouvez vérifier qu’un fichier a été entièrement écrit en calculant son hachage SHA-256 et en le comparant à la somme de contrôle renvoyée par les points d’entrée de statut de tâche.

Vous pouvez déterminer la quantité totale de disque utilisée pour la journée en cours en appelant la fonction Obtenir les tâches de lead/activité/membre de programme d’exportation. Ces points d’entrée renvoient une liste de toutes les tâches au cours des sept derniers jours. Vous pouvez filtrer cette liste en ne retenant que les tâches terminées au cours de la journée en cours (à l’aide des attributs `status` et `finishedAt`). Additionnez ensuite les tailles de fichiers de ces tâches pour obtenir la quantité totale utilisée. Impossible de supprimer un fichier pour récupérer de l’espace disque.

## Autorisations

L’extraction en bloc utilise le même modèle d’autorisations que l’API REST Marketo et ne nécessite aucune autorisation spéciale supplémentaire à utiliser, bien que des autorisations spécifiques soient requises pour chaque ensemble de points d’entrée.

Les tâches d’extraction en bloc ne sont accessibles que par l’utilisateur de l’API qui les a créées, y compris l’interrogation des statuts et la récupération du contenu des fichiers.

Les points d’entrée d’extraction en bloc ne connaissent pas les espaces de travail Marketo. Les requêtes d’extraction incluent toujours des données sur tous les espaces de travail, quelle que soit la manière dont vous définissez l’utilisateur API uniquement pour votre service personnalisé.

## Création d’un traitement

Les API d’extraction en bloc de Marketo utilisent le concept d’une tâche pour initier et exécuter l’extraction de données. Examinons la création d’une tâche d’exportation de prospect simple.

```
POST /bulk/v1/leads/export/create.json
```

```json
{
   "fields": [
      "firstName",
      "lastName"
   ],
   "format": "CSV",
   "columnHeaderNames": {
      "firstName": "First Name",
      "lastName": "Last Name"
   },
   "filter": {
      "createdAt": {
         "startAt": "2023-01-01T00:00:00Z",
         "endAt": "2023-01-31T00:00:00Z"
      }
   }
}
```

Cette requête simple crée une tâche qui renvoie les valeurs contenues dans les champs « firstName » et « lastName », avec les en-têtes de colonne « First Name » et « Last Name » dans un fichier CSV, contenant chaque prospect créé entre le 1er janvier 2023 et le 31 janvier 2023.

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2023-01-21T11:47:30-08:00",
         "queuedAt": "2023-01-21T11:48:30-08:00",
         "format": "CSV",
      }
   ]
}
```

Lorsque nous créons la tâche, elle renvoie un ID de tâche dans l’attribut `exportId` . Nous pouvons ensuite utiliser cet ID de tâche pour mettre la tâche en file d’attente, l’annuler, vérifier son statut ou récupérer le fichier terminé.

### Paramètres communs

Chaque point d’entrée de création de tâche partage certains paramètres communs pour la configuration du format de fichier, des noms de champ et du filtre d’une tâche d’extraction en bloc. Chaque sous-type de tâche d’extraction peut avoir des paramètres supplémentaires :

| Paramètre | Type de données | Notes |
|---|---|---|
| format | Chaîne | Détermine le format de fichier des données extraites avec des options pour les valeurs séparées par des virgules, les valeurs séparées par des tabulations et les valeurs séparées par des points-virgules. Accepte l’un des formats suivants : CSV, SSV, TSV. Le format par défaut est CSV. |
| columnHeaderNames | Objet | Permet de définir les noms des en-têtes de colonne dans le fichier renvoyé. Chaque clé de membre correspond au nom de l’en-tête de colonne à renommer et la valeur correspond au nouveau nom de l’en-tête de colonne. Par exemple, « columnHeaderNames »: { « firstName »: « First Name », « lastName »: « Last Name » }, |
| filter | Objet | Filtre appliqué à la tâche d’extraction. Les types et options varient selon les types de tâche. |

## Récupération des tâches

Parfois, vous devrez peut-être récupérer vos tâches récentes. Pour ce faire, utilisez facilement les Tâches d’exportation Get pour le type d’objet correspondant. Chaque point d’entrée Get Export Jobs prend en charge un champ de filtre `status`, un  `batchSize` de limiter le nombre de tâches renvoyées et de `nextPageToken` la pagination dans des jeux de résultats volumineux. Le filtre de statut prend en charge chaque statut valide pour une tâche d’exportation : Créé, En file d’attente, En cours de traitement, Annulé, Terminé et En échec. batchSize a un maximum et une valeur par défaut de 300. Obtenons la liste des tâches d’exportation de leads :

```
GET /bulk/v1/leads/export.json?status=Completed,Failed
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 122323,
         "fileSize": 123424,
         "fileChecksum": "sha256:c16514c7e80fcac5ea055dacae9617fc3c29aff5365e3743071313ce0ed2a815"
      }
      ...
   ]
}
```

Le point d’entrée répond avec `status` réponse de chaque tâche créée au cours des sept derniers jours pour ce type d’objet dans le tableau de résultats. La réponse inclut uniquement les résultats des tâches détenues par l’utilisateur de l’API effectuant l’appel.

## Démarrage d’un traitement

Maintenant que notre identifiant de tâche est en main, nous allons commencer la tâche :

```
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

Cette opération déclenche l’exécution du traitement et renvoie une réponse de statut. Puisque l’exportation est toujours effectuée de manière asynchrone, nous devons interroger le statut du traitement pour déterminer s’il est terminé. Le statut d’une tâche donnée n’est pas mis à jour plus d’une fois toutes les 60 secondes. Par conséquent, le statut ne doit jamais être interrogé plus fréquemment que cela. Gardez toutefois à l’esprit que la plupart des cas d’utilisation ne doivent pas nécessiter d’interrogations plus fréquentes qu’une fois toutes les 5 minutes. Les données de chaque exportation réussie sont conservées pendant 10 jours.

## Interroger le statut de la tâche

La détermination du statut de la tâche est simple.

Le statut ne peut être interrogé que pour les tâches créées par le même utilisateur de l’API qui les a créées.

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
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 122323,
         "fileSize": 123424,
         "fileChecksum": "sha256:d9c73f0b6960c71623c8bafe29603b3e8e20fd0e4eeaefd119c0227506ea9be4"
      }
   ]
}
```

Le membre de `status` interne indique la progression de la tâche et peut être l’une des valeurs suivantes : Créé, Mis en file d’attente, Traitement, Annulé, Terminé, Échec. Dans ce cas, notre tâche est terminée. Nous pouvons donc arrêter l’interrogation et continuer à récupérer le fichier. Une fois l’opération terminée, le membre de `fileSize` indique la longueur totale du fichier en octets, et le membre de `fileChecksum` contient le hachage SHA-256 du fichier. Le statut de la tâche est disponible pendant 30 jours après que le statut Terminé ou En échec a été atteint.

## Récupération de vos données

Une fois la tâche terminée, vous pouvez facilement récupérer le fichier.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

La réponse contient un fichier formaté selon la configuration de la tâche. Le point d’entrée répond avec le contenu du fichier . Si une tâche n’est pas terminée ou si un ID de tâche incorrect est transmis, les points d’entrée de fichier répondent avec un statut 404 Introuvable et un message d’erreur en texte brut comme payload, contrairement à la plupart des autres points d’entrée REST Marketo.

Pour prendre en charge la récupération partielle et conviviale des données extraites, le point d’entrée du fichier prend éventuellement en charge le `Range` d’en-tête HTTP de type `bytes` (selon la norme [RFC 7233](https://datatracker.ietf.org/doc/html/rfc7233)). Si l’en-tête n’est pas défini, l’intégralité du contenu est renvoyée. Pour récupérer les 10 000 premiers octets d’un fichier, vous devez transmettre l’en-tête suivant dans le cadre de votre requête GET au point d’entrée , à partir de l’octet 0 :

```
Range: bytes=0-9999
```

Lors de la récupération du fichier partiel, le point d’entrée répond avec le code d’état 206 et renvoie les en-têtes Accept-Range, Content-Length et Content-Range :

```
Accept-Ranges: bytes
Content-Length: 1000
Content-Range: bytes 0-9999/123424
```

### Récupération et reprise partielles

Les fichiers peuvent être récupérés en partie ou reprendre ultérieurement à l’aide de l’en-tête `Range`. La plage d&#39;un fichier commence à l&#39;octet 0 et se termine à la valeur `fileSize` moins 1. La longueur du fichier est également indiquée comme dénominateur dans la valeur de l’en-tête de réponse `Content-Range` lors de l’appel d’un point d’entrée Get Export File. Si une récupération échoue partiellement, elle peut être reprise ultérieurement. Par exemple, si vous essayez de récupérer un fichier de 1 000 octets de long, mais que seuls les 725 premiers octets ont été reçus, la récupération peut être tentée à nouveau à partir du point d’échec en appelant à nouveau le point d’entrée et en transmettant une nouvelle plage :

```
Range: bytes 724-999
```

Cette opération renvoie les 275 octets restants du fichier.

#### Vérification de l&#39;intégrité du fichier

Les points d’entrée du statut de la tâche renvoient une somme de contrôle dans l’attribut `fileChecksum` lorsque `status` est « Terminé ». La somme de contrôle est un hachage SHA-256 du fichier exporté. Vous pouvez comparer la somme de contrôle avec le hachage SHA-256 du fichier récupéré pour vérifier qu’il est terminé.

Voici un exemple de réponse contenant la somme de contrôle :

```json
{
    "exportId": "45547609-6732-418a-bb7b-17b0160b2317",
    "format": "CSV",
    "status": "Completed",
    "createdAt": "2019-06-04T23:13:12Z",
    "queuedAt": "2019-06-04T23:14:02Z",
    "startedAt": "2019-06-04T23:15:19Z",
    "finishedAt": "2019-06-04T23:36:40Z",
    "numberOfRecords": 1776,
    "fileSize": 400785,
    "fileChecksum": "sha256:83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6"
}
```

Voici un exemple de création du hachage SHA-256 d’un fichier récupéré nommé « bulk_lead_export.csv » à l’aide de l’utilitaire de ligne de commande sha256sum :

```
$ sha256sum bulk_lead_export.csv
83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6 *bulk_lead_export.csv
```

## Annulation d’un traitement

Si une tâche n’a pas été configurée correctement ou devient inutile, elle peut être facilement annulée :

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
         "format": "CSV",
      }
   ]
}
```

Cette opération répond avec un statut indiquant que la tâche a été annulée.
