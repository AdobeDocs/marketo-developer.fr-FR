---
title: Extraction en bloc
feature: REST API
description: Opérations par lots pour extraire des données Marketo.
exl-id: 6a15c8a9-fd85-4c7d-9f65-8b2e2cba22ff
source-git-commit: e7d893a81d3ed95e34eefac1ee8f1ddd6852f5cc
workflow-type: tm+mt
source-wordcount: '1683'
ht-degree: 1%

---

# Extraction en bloc

Marketo fournit des interfaces pour la récupération de grands ensembles de données liées aux personnes et aux personnes, appelés Extraction en bloc. Actuellement, les interfaces sont proposées pour trois types d’objets :

- Leads (Personnes)
- Activités
- Membres du programme
- Objets personnalisés

L’extraction en bloc est réalisée en créant une tâche, en définissant l’ensemble de données à récupérer, en mettant la tâche en file d’attente, en attendant que la tâche termine l’écriture d’un fichier, puis en récupérant le fichier sur HTTP. Ces traitements sont exécutés de manière asynchrone et peuvent être interrogés pour récupérer l’état de l’exportation.

`Note:` Les points d’entrée d’API en bloc ne comportent pas le préfixe &quot;/rest&quot; comme les autres points d’entrée.

## Authentification

Les API d’extraction en masse utilisent la même méthode d’authentification OAuth 2.0 que les autres API REST Marketo. Pour ce faire, un jeton d’accès valide doit être envoyé en tant qu’en-tête HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>La prise en charge de l’authentification à l’aide du paramètre de requête **access_token** a été supprimée le 30 juin 2025. Si votre projet utilise un paramètre de requête pour transmettre le jeton d’accès, il doit être mis à jour afin d’utiliser l’en-tête **Authorization** dès que possible. Le nouveau développement doit utiliser exclusivement l’en-tête **Authorization**.

## Limites

- Nb max. de tâches d’exportation simultanées : 2
- Nb max. de tâches d&#39;export en file d&#39;attente (y compris les tâches d&#39;export actuelles) : 10
- Période de rétention des fichiers : sept jours
- Affectation de l’exportation quotidienne par défaut : 500 Mo (qui se réinitialise tous les jours à 00h00 du matin (heure de la côte Est). Augmente la disponibilité à l’achat.
- Durée max. pour le filtre de plage de dates (createdAt ou updatedAt) : 31 jours

Les filtres d’extraction de pistes en bloc pour UpdatedAt et Smart List ne sont pas disponibles pour certains types d’abonnement. En cas d’indisponibilité, un appel au point de terminaison Créer une tâche de piste d’exportation renvoie l’erreur &quot;1035, Type de filtre non pris en charge pour l’abonnement cible&quot;. Les clients peuvent contacter le support Marketo pour que cette fonctionnalité soit activée dans leur abonnement.

### File d&#39;attente

Les API d’extraction en masse utilisent une file d’attente de tâches (partagée entre les pistes, les activités, les membres de programme et les objets personnalisés). Les tâches d’extraction doivent d’abord être créées, puis mises en file d’attente en appelant Créer une tâche de piste d’exportation/d’activité/de membre de programme et Mettre en file d’attente les points de terminaison de tâche de membre de programme/d’activité/d’activité d’exportation. Une fois mises en file d’attente, les tâches sont retirées de la file d’attente et démarrées lorsque les ressources de calcul deviennent disponibles.

Le nombre maximal de tâches dans la file d’attente est de 10. Si vous essayez de mettre une tâche en file d’attente lorsque la file d’attente est pleine, le point de terminaison Enqueue Export Job renvoie une erreur &quot;1029, Trop de tâches dans la file d’attente&quot;. Au maximum, deux tâches peuvent s’exécuter simultanément (l’état est &quot;Traitement&quot;).

### Taille du fichier

Les API d’extraction en masse sont mesurées en fonction de la taille sur le disque des données récupérées par une tâche d’extraction en masse. La taille explicite en octets d’une tâche peut être déterminée en lisant l’attribut `fileSize` à partir de la réponse d’état terminée d’une tâche d’exportation.

Le quota quotidien est de 500 Mo au maximum par jour, qui est partagé entre les pistes, les activités, les membres de programme et les objets personnalisés. Lorsque le quota est dépassé, vous ne pouvez pas créer ou mettre en file d’attente une autre tâche tant que le quota quotidien n’est pas réinitialisé à minuit [heure normale du Centre](https://en.wikipedia.org/wiki/Central_Time_Zone). Dans l&#39;intervalle, l&#39;erreur &quot;1029, Exporter le quota quotidien dépassé&quot; est renvoyée. Outre le quota quotidien, il n’existe pas de taille de fichier maximale.

Une fois qu’une tâche est en file d’attente ou en cours de traitement, elle s’exécute jusqu’à la fin (sauf erreur ou annulation d’une tâche). Si une tâche échoue, vous devez la recréer. Les fichiers sont entièrement écrits uniquement lorsqu’une tâche atteint l’état terminé (les fichiers partiels ne sont jamais écrits). Vous pouvez vérifier qu’un fichier a été entièrement écrit en calculant son hachage SHA-256 et en le comparant à la somme de contrôle renvoyée par les points de terminaison de l’état de la tâche.

Vous pouvez déterminer la quantité totale de disque utilisée pour la journée en cours en appelant Get Export Lead/Activity/Program Member Jobs. Ces points de terminaison renvoient une liste de toutes les tâches des sept derniers jours. Vous pouvez filtrer cette liste jusqu’aux tâches qui se sont terminées au cours du jour en cours (à l’aide des attributs `status` et `finishedAt` ). Ensuite, additionnez les tailles de fichiers pour ces tâches afin de produire le montant total utilisé. Il n’existe aucun moyen de supprimer un fichier pour récupérer de l’espace disque.

## Autorisations

L’extraction en bloc utilise le même modèle d’autorisations que l’API REST Marketo et ne nécessite aucune autorisation spéciale supplémentaire à utiliser, bien que des autorisations spécifiques soient requises pour chaque ensemble de points de terminaison.

Les tâches d’extraction en bloc ne sont accessibles que par l’utilisateur de l’API qui les a créées, notamment l’interrogation de l’état et la récupération du contenu du fichier.

Les points de fin d’extraction en bloc ne prennent pas en compte les espaces de travail Marketo. Les demandes d’extraction incluent toujours des données dans tous les espaces de travail, quelle que soit la manière dont vous définissez l’utilisateur API uniquement pour votre service personnalisé.

## Création d’une tâche

Les API d’extraction en masse de Marketo utilisent le concept de tâche pour initier et exécuter l’extraction de données. Examinons la création d’une tâche d’exportation de pistes simple.

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

Cette requête simple crée une tâche qui renverra les valeurs contenues dans les champs &quot;firstName&quot; et &quot;lastName&quot;, avec les en-têtes de colonne &quot;Prénom&quot; et &quot;Nom&quot; comme fichier CSV, contenant chaque piste créée entre le 1er janvier 2023 et le 31 janvier 2023.

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

Lorsque nous créons la tâche, elle renvoie un identifiant de tâche dans l’attribut `exportId`. Nous pouvons ensuite utiliser cet identifiant de tâche pour mettre la tâche en file d’attente, l’annuler, vérifier son état ou récupérer le fichier terminé.

### Paramètres communs

Chaque point de fin de création de tâche partage certains paramètres communs pour la configuration du format de fichier, des noms de champ et du filtre d’une tâche d’extraction en masse. Chaque sous-type de tâche d’extraction peut comporter des paramètres supplémentaires :

| Paramètre | Type de données | Notes |
|---|---|---|
| format | Chaîne | Détermine le format de fichier des données extraites avec des options pour les valeurs séparées par des virgules, les valeurs séparées par des tabulations et les valeurs séparées par des points-virgules. Accepte l’un des paramètres suivants : CSV, SSV, TSV. Le format par défaut est CSV. |
| columnHeaderNames | Objet | Permet de définir les noms des en-têtes de colonne dans le fichier renvoyé. Chaque clé de membre est le nom de l’en-tête de colonne à renommer et la valeur est le nouveau nom de l’en-tête de colonne. Par exemple, &quot;columnHeaderNames&quot;: { &quot;firstName&quot;: &quot;First Name&quot;, &quot;lastName&quot;: &quot;Last Name&quot; }, |
| filter | Objet | Filtre appliqué à la tâche d’extraction. Les types et options varient selon les types de tâche. |


## Récupération des tâches

Parfois, vous devrez peut-être récupérer vos tâches récentes. Cela est facilement effectué avec Obtenir les tâches d’exportation pour le type d’objet correspondant. Chaque point de fin Get Export Jobs prend en charge un champ de filtre `status`, ainsi qu’une  `batchSize` pour limiter le nombre de tâches renvoyées et `nextPageToken` pour la pagination par des jeux de résultats volumineux. Le filtre d’état prend en charge chaque état valide d’une tâche d’exportation : création, mise en file d’attente, traitement, annulée, terminée et en échec. Le paramètre batchSize a une valeur maximale et une valeur par défaut de 300. Obtenons la liste des tâches d’exportation de pistes :

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

Le point de terminaison répond avec la réponse `status` de chaque tâche créée au cours des sept derniers jours pour ce type d’objet dans le tableau de résultats. La réponse inclura uniquement les résultats des tâches appartenant à l’utilisateur de l’API qui effectue l’appel.

## Démarrage d’une tâche

Avec notre identifiant de tâche en main, commençons le traitement :

```
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

Cela déclenche l’exécution de la tâche et renvoie une réponse d’état. Comme l’exportation est toujours effectuée de manière asynchrone, nous devons interroger l’état de la tâche pour déterminer si elle a été effectuée. L’état d’une tâche donnée ne sera pas mis à jour plus souvent qu’une fois toutes les 60 secondes. Par conséquent, l’état ne doit jamais être interrogé plus fréquemment. Gardez toutefois à l’esprit que la plupart des cas d’utilisation ne doivent pas nécessiter d’interrogations plus fréquentes qu’une fois toutes les 5 minutes. Les données de chaque exportation réussie sont conservées pendant 10 jours.

## État de la tâche d’interrogation

Il est simple de déterminer l’état de la tâche.

L’état ne peut être interrogé que pour les tâches créées par le même utilisateur de l’API que celui qui les a créées.

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

Le membre `status` interne indique la progression de la tâche et peut être l’une des valeurs suivantes : Créé, En file d’attente, Traitement, Annulé, Terminé, Échec. Dans ce cas, notre tâche est terminée, nous pouvons donc arrêter l’interrogation et continuer à récupérer le fichier. Une fois terminé, le membre `fileSize` indique la longueur totale du fichier en octets, et le membre `fileChecksum` contient le hachage SHA-256 du fichier. L’état de la tâche est disponible pendant 30 jours après que l’état Terminé ou Échec a été atteint.

## Récupération de vos données

Une fois votre tâche terminée, vous pouvez facilement récupérer le fichier.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

La réponse contient un fichier formaté selon la configuration de la tâche. Le point de terminaison répond avec le contenu du fichier. Si une tâche n’est pas terminée ou qu’un ID de tâche incorrect est transmis, les points de fin de fichier répondent avec l’état 404 Not Found et un message d’erreur en texte brut comme charge utile, contrairement à la plupart des autres points de fin REST Marketo.

Pour prendre en charge la récupération partielle et conviviale des données extraites, le point de terminaison du fichier prend éventuellement en charge l’en-tête HTTP `Range` de type `bytes` (par [RFC 7233](https://datatracker.ietf.org/doc/html/rfc7233)). Si l’en-tête n’est pas défini, l’ensemble du contenu est renvoyé. Pour récupérer les 10 000 premiers octets d’un fichier, vous transmettez l’en-tête suivant dans le cadre de votre requête de GET au point de terminaison, en commençant par l’octet 0 :

```
Range: bytes=0-9999
```

Lors de la récupération du fichier partiel, le point de terminaison répond avec le code d’état 206 et renvoie les en-têtes Accept-Plages, Content-Length et Content-Range :

```
Accept-Ranges: bytes
Content-Length: 1000
Content-Range: bytes 0-9999/123424
```

### Récupération partielle et reprise

Les fichiers peuvent être récupérés en partie ou repris ultérieurement à l’aide de l’en-tête `Range`. La plage d’un fichier commence à l’octet 0 et se termine à la valeur de `fileSize` moins 1. La longueur du fichier est également signalée comme dénominateur dans la valeur de l’en-tête de réponse `Content-Range` lors de l’appel d’un point de terminaison Get Export File . Si une récupération échoue partiellement, elle peut être reprise ultérieurement. Par exemple, si vous essayez de récupérer un fichier de 1 000 octets, mais que seuls les 725 premiers octets ont été reçus, la récupération peut être retentée à partir du point d’échec en appelant de nouveau le point de terminaison et en transmettant une nouvelle plage :

```
Range: bytes 724-999
```

Cette opération renvoie les 275 octets restants du fichier.

#### Vérification de l’intégrité des fichiers

Les points de terminaison de l’état de la tâche renvoient une somme de contrôle dans l’attribut `fileChecksum` lorsque `status` est &quot;terminé&quot;. La somme de contrôle est un hachage SHA-256 du fichier exporté. Vous pouvez comparer la somme de contrôle au hachage SHA-256 du fichier récupéré pour vérifier qu’il est terminé.

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

Voici un exemple de création du hachage SHA-256 d’un fichier récupéré nommé &quot;bulk_lead_export.csv&quot; à l’aide de l’utilitaire de ligne de commande sha256sum :

```
$ sha256sum bulk_lead_export.csv
83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6 *bulk_lead_export.csv
```

## Annulation d’une tâche

Si une tâche a été mal configurée ou devient inutile, elle peut être facilement annulée :

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

Ceci répond avec un état indiquant que la tâche a été annulée.
