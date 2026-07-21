---
title: Extraction En Masse
feature: REST API
description: Découvrez comment utiliser l’API REST d’extraction en bloc Marketo pour exporter des prospects, des activités, des membres de programme et des objets personnalisés, avec OAuth, des files d’attente de tâches et des limites quotidiennes 500MB.
exl-id: 6a15c8a9-fd85-4c7d-9f65-8b2e2cba22ff
TQID: https://experienceleague.adobe.com/ECSchsjqp8fyxXbUGl5DgXHUkXuN0sIUc3yJfVaIe1E
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1549
ht-degree: 1%

---

# Extraction En Masse

L’extraction en bloc Marketo fournit des interfaces pour récupérer de grands ensembles de données de personne et liées à la personne. Les interfaces sont actuellement disponibles pour quatre types d’objets :

- Leads (personnes)
- Activités
- Membres du programme
- Objets personnalisés

Pour effectuer une extraction en bloc :

1. Créez une tâche et définissez les données à récupérer.
1. Mettre le traitement en file d’attente.
1. Attendez que le traitement ait fini d’écrire le fichier.
1. Récupérez le fichier via HTTP.

Les traitements d’extraction en bloc s’exécutent de manière asynchrone. Interrogez la tâche pour récupérer le statut d’exportation.

`Note:` points d’entrée de l’API en bloc ne sont pas précédés du préfixe « /rest » comme les autres points d’entrée.

## Authentification

Les API d’extraction en masse utilisent la même méthode d’authentification OAuth 2.0 que les autres API REST Marketo. Envoyez un jeton d’accès valide dans l’en-tête HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>La prise en charge de l’authentification à l’aide du paramètre de requête **access_token** sera supprimée le 31 août 2026. Si votre projet utilise un paramètre de requête pour transmettre le jeton d’accès, il doit être mis à jour afin d’utiliser l’en-tête **Authorization** dès que possible. Le nouveau développement doit utiliser exclusivement l’en-tête **Authorization**.

## Limites

- Nombre maximal de traitements d’exportation simultanés : 2
- Nombre maximal de tâches d’exportation en file d’attente, y compris les tâches en cours d’exportation : 10
- Durée de conservation des fichiers : sept jours
- Affectation quotidienne par défaut des exportations : 500MB. L’affectation se réinitialise tous les jours à 00 h 00 (heure de Paris). Les augmentations peuvent être achetées.
- Durée maximale du filtre de période (`createdAt` ou `updatedAt`) : 31 jours

Les filtres d’extraction de leads en bloc pour UpdatedAt et la liste dynamique ne sont pas disponibles pour certains types d’abonnement. Si ces filtres ne sont pas disponibles, le point d’entrée Créer une tâche d’exportation de prospect renvoie l’erreur « 1035, Unsupported filter type for target subscription » (1035, type de filtre non pris en charge pour l’abonnement cible). Contactez l’assistance Marketo pour activer cette fonctionnalité dans le cadre de votre abonnement.

### File d&#39;attente

Les API d’extraction en bloc utilisent une file d’attente de tâches partagée entre les prospects, les activités, les membres du programme et les objets personnalisés. Tout d’abord, appelez un point d’entrée Créer une tâche d’exportation de lead/activité/membre de programme pour créer une tâche d’extraction. Ensuite, appelez le point d’entrée correspondant Exporter le prospect/l’activité/le membre de programme en file d’attente pour mettre la tâche en file d’attente. La tâche commence lorsque les ressources informatiques sont disponibles.

La file d’attente peut contenir jusqu’à 10 tâches. Si vous essayez de mettre en file d’attente une tâche lorsque la file d’attente est pleine, le point d’entrée Mettre en file d’attente la tâche d’exportation renvoie l’erreur « 1029, Trop de tâches en file d’attente ». Deux traitements au maximum peuvent avoir le statut « Traitement » et s’exécuter simultanément.

### Taille du fichier

Les API d’extraction en bloc sont limitées en fonction de la taille sur le disque des données récupérées par une tâche d’extraction en bloc. Pour déterminer la taille du fichier en octets, lisez l’attribut `fileSize` dans la réponse de statut terminée pour une tâche d’exportation.

Le quota quotidien est 500MB et partagé entre les prospects, les activités, les membres du programme et les objets personnalisés. Lorsque vous dépassez le quota, vous ne pouvez pas créer ni mettre en file d’attente une autre tâche tant que le quota n’a pas été réinitialisé à minuit [heure du Centre](https://en.wikipedia.org/wiki/Central_Time_Zone). Jusqu’à la réinitialisation, l’API renvoie l’erreur « 1 029, Quota d’exportation quotidien dépassé ». Outre le quota journalier, il n&#39;existe pas de taille de fichier maximale.

Une fois qu’une tâche est mise en file d’attente ou en cours de traitement, elle s’exécute jusqu’à la fin, sauf si une erreur se produit ou si vous annulez la tâche. Si une tâche échoue, vous devez la recréer.

L’API écrit le fichier complet uniquement lorsque la tâche atteint le statut Terminé . Il n’écrit pas de fichiers partiels. Pour vérifier le fichier, calculez son hachage SHA-256 et comparez-le à la somme de contrôle renvoyée par le point d’entrée de statut de la tâche.

Pour déterminer l’espace disque total utilisé pour la journée en cours, appelez un point d’entrée Obtenir l’exportation des tâches de prospect/activité/membre de programme . Ces points d’entrée renvoient toutes les tâches des sept derniers jours.

Filtrez la liste en fonction des tâches effectuées au cours de la journée en cours à l’aide des attributs `status` et `finishedAt`. Ajoutez ensuite les tailles de fichier pour ces tâches. Vous ne pouvez pas supprimer un fichier pour libérer de l’espace disque.

## Autorisations

L’extraction en bloc utilise le même modèle d’autorisations que l’API REST Marketo. Il ne nécessite pas d’autorisations spéciales supplémentaires, mais chaque ensemble de points d’entrée nécessite des autorisations spécifiques.

Seul l’utilisateur de l’API qui a créé une tâche d’extraction en bloc peut y accéder, interroger son statut ou récupérer le contenu de son fichier.

Les points d’entrée d’extraction en bloc ne connaissent pas les espaces de travail Marketo. Les requêtes d’extraction incluent les données de tous les espaces de travail, quelle que soit la manière dont vous définissez l’API uniquement pour l’utilisateur de votre service personnalisé.

## Création d’un traitement

Les API d’extraction en masse de Marketo utilisent des tâches pour lancer et exécuter des extractions de données. La requête suivante crée une tâche d’exportation de prospect :

```http
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

Cette requête crée une tâche qui exporte chaque prospect créé entre le 1er janvier 2023 et le 31 janvier 2023. Le fichier CSV contient des valeurs des champs « firstName » et « lastName » et utilise les en-têtes de colonne « First Name » et « Last Name ».

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

La réponse renvoie l’ID de tâche dans l’attribut `exportId`. Utilisez cet ID de tâche pour mettre la tâche en file d’attente ou l’annuler, vérifier son statut ou récupérer le fichier terminé.

### Paramètres communs

Chaque point d’entrée de création de tâche comporte des paramètres communs pour la configuration du format de fichier, des noms de champ et du filtre. Chaque sous-type de tâche d’extraction peut également comporter des paramètres supplémentaires :

| Paramètre | Type de données | Notes |
| --- | --- | --- |
| format | Chaîne | Détermine le format de fichier des données extraites avec des options pour les valeurs séparées par des virgules, les valeurs séparées par des tabulations et les valeurs séparées par des points-virgules. Accepte l’un des formats suivants : CSV, SSV, TSV. Le format par défaut est CSV. |
| columnHeaderNames | Objet | Permet de définir les noms des en-têtes de colonne dans le fichier renvoyé. Chaque clé de membre correspond au nom de l’en-tête de colonne à renommer et la valeur correspond au nouveau nom de l’en-tête de colonne. Par exemple, « columnHeaderNames »: { « firstName »: « First Name », « lastName »: « Last Name » }, |
| filter | Objet | Filtre appliqué à la tâche d’extraction. Les types et options varient selon les types de tâche. |

## Récupération des tâches

Utilisez le point d’entrée Get Export Jobs pour le type d’objet correspondant afin de récupérer les tâches récentes. Chaque point d’entrée Get Export Jobs prend en charge les paramètres suivants :

- `status` filtre les tâches en fonction du statut d’exportation. Les valeurs valides sont Création, Mise en file d’attente, Traitement, Annulé, Terminé et Échec.
- `batchSize` limite le nombre de traitements renvoyés. La valeur par défaut et la valeur maximale sont 300.
- `nextPageToken` des pages à travers des jeux de résultats volumineux.

La requête suivante récupère les tâches d’exportation de leads avec un statut Terminé ou Échec :

```http
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

Le tableau de résultats contient la réponse de statut de chaque tâche créée pour ce type d’objet au cours des sept derniers jours. La réponse inclut uniquement les tâches détenues par l’utilisateur ou l’utilisatrice de l’API effectuant l’appel.

## Démarrage d’un traitement

Après avoir créé une tâche, utilisez son identifiant de tâche pour la mettre en file d’attente et la démarrer :

```http
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

La requête démarre la tâche et renvoie une réponse de statut. Comme les exportations s’exécutent de manière asynchrone, interrogez le statut de la tâche pour déterminer quand l’exportation est terminée.

## Interroger le statut de la tâche

Interroger le point d’entrée de statut pour déterminer la progression d’une tâche. Seul l’utilisateur de l’API qui a créé une tâche peut interroger son statut.

L’état d’une tâche n’est pas mis à jour plus d’une fois toutes les 60 secondes. N&#39;effectuez pas de sondages plus fréquents. Dans la plupart des cas d’utilisation, une interrogation toutes les 5 minutes est suffisante. Les données de chaque exportation réussie sont conservées pendant 10 jours.

```http
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

Le membre de `status` interne indique la progression du traitement. Sa valeur peut être Créé, En file d’attente, En cours de traitement, Annulé, Terminé ou En échec.

Dans cet exemple, la tâche est terminée. Vous pouvez donc arrêter l’interrogation et récupérer le fichier. Pour une tâche terminée, le membre `fileSize` indique la longueur totale du fichier en octets, et le membre `fileChecksum` contient le hachage SHA-256 du fichier. Le statut de la tâche est disponible pendant 30 jours après qu’elle a atteint le statut Terminée ou En échec .

## Récupération de vos données

Une fois la tâche terminée, récupérez le fichier exporté :

```http
GET /bulk/v1/leads/export/{exportId}/file.json
```

La réponse contient le fichier au format configuré pour la tâche. Si la tâche est incomplète ou si la requête contient un ID de tâche non valide, le point d’entrée du fichier renvoie un statut 404 Introuvable et un message d’erreur en texte brut. Cette réponse diffère de la plupart des autres réponses de point d’entrée REST Marketo.

Pour prendre en charge la récupération partielle et réutilisable, le point d’entrée du fichier prend en charge l’en-tête `Range` HTTP facultatif avec le type de `bytes`, tel que défini dans [RFC 7233](https://datatracker.ietf.org/doc/html/rfc7233). Si vous ne définissez pas l’en-tête , le point d’entrée renvoie l’intégralité du fichier.

Pour récupérer les 10 000 premiers octets d’un fichier, transmettez l’en-tête suivant dans la requête GET. La plage commence à l&#39;octet 0 :

```text
Range: bytes=0-9999
```

Pour un fichier partiel, le point d’entrée renvoie le code d’état 206 et les en-têtes Accept-Range, Content-Length et Content-Range :

```text
Accept-Ranges: bytes
Content-Length: 10000
Content-Range: bytes 0-9999/123424
```

### Récupération et reprise partielles

Utilisez l’en-tête `Range` pour récupérer une partie d’un fichier ou reprendre une récupération. La plage de fichiers commence à l&#39;octet 0 et se termine à la valeur `fileSize` moins 1. Le point d’entrée Get Export File indique également la longueur du fichier comme dénominateur dans l’en-tête de réponse `Content-Range`.

Si une récupération échoue partiellement, vous pouvez la reprendre. Par exemple, si vous essayez de récupérer un fichier de 1 000 octets mais que vous ne recevez que les 725 premiers octets, appelez à nouveau le point d’entrée et transmettez une nouvelle plage :

```text
Range: bytes=725-999
```

Cette requête renvoie les 275 octets restants du fichier.

#### Vérification de l&#39;intégrité du fichier

Lorsque `status` est « Terminé », les points d’entrée de statut de la tâche renvoient une somme de contrôle dans l’attribut `fileChecksum`. La somme de contrôle est le hachage SHA-256 du fichier exporté. Comparez-le au hachage SHA-256 du fichier récupéré pour vérifier que le fichier est terminé.

La réponse suivante contient une somme de contrôle :

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

L’exemple suivant utilise l’utilitaire de ligne de commande sha256sum pour créer le hachage SHA-256 d’un fichier récupéré nommé « bulk_lead_export.csv » :

```bash
$ sha256sum bulk_lead_export.csv
83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6 *bulk_lead_export.csv
```

## Annulation d’un traitement

Si une tâche n’est pas configurée correctement ou n’est plus nécessaire, annulez-la :

```http
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

Le statut de la réponse indique que le traitement a été annulé.
