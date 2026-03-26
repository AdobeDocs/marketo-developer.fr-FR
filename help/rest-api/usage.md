---
title: Utilisation
feature: REST API
description: Surveillez l’utilisation et les erreurs de l’API REST Marketo avec des points d’entrée de statistiques quotidiens et sur les 7 derniers jours, y compris les nombres par utilisateur et les totaux des codes d’erreur.
source-git-commit: 73fa4c85ecabd4cfd24bc6591aad11dc4e75010a
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 9%

---

# Utilisation

[Référence du point d’entrée d’utilisation](https://developer.adobe.com/marketo-apis/api/mapi#tag/Usage)

Les API d’utilisation fournissent un résumé de la consommation d’API REST et de l’activité d’erreur pour votre abonnement. Ces points d’entrée sont utiles pour surveiller les intégrations, suivre le volume d’appels quotidien et identifier les tendances d’erreur au fil du temps.

Les données d’utilisation comprennent un nombre total d’appels API et une répartition par utilisateur. Les données d’erreur incluent un nombre total d’erreurs et une répartition par code d’erreur.

Les API d’utilisation utilisent la même méthode d’authentification que les autres API REST Marketo. Transmettez le jeton d’accès dans l’en-tête `Authorization: Bearer {accessToken}`.

## Points d’entrée

| Méthode | Chemin | Description |
| --- | --- | --- |
| GET | `/rest/v1/stats/usage.json` | Récupère l’utilisation de l’API pour la journée en cours. |
| GET | `/rest/v1/stats/usage/last7days.json` | Récupère l’utilisation de l’API pour les 7 derniers jours. |
| GET | `/rest/v1/stats/errors.json` | Récupère les erreurs API pour la journée en cours. |
| GET | `/rest/v1/stats/errors/last7days.json` | Récupère les erreurs API des 7 derniers jours. |

## Utilisation quotidienne

Récupère l’utilisation de l’API pour la journée en cours.

```
GET /rest/v1/stats/usage.json
```

```json
{
   "requestId": "5f7f#17d7d8f2b6f",
   "success": true,
   "result": [
      {
         "date": "2015-10-17",
         "total": 1120,
         "users": [
            {
               "userId": "some.body@yahoo.com",
               "count": 200
            },
            {
               "userId": "some.body@marketo.com",
               "count": 200
            },
            {
               "userId": "some.body@gmail.com",
               "count": 720
            }
         ]
      }
   ]
}
```

Chaque objet du tableau `result` contient un jour de totaux d’utilisation et une répartition par utilisateur.

## Utilisation des 7 derniers jours

Récupère l’utilisation de l’API pour les 7 derniers jours. Chaque élément du tableau `result` représente un jour.

```
GET /rest/v1/stats/usage/last7days.json
```

## Erreurs quotidiennes

Récupère les erreurs API pour la journée en cours.

```
GET /rest/v1/stats/errors.json
```

```json
{
   "requestId": "5f7f#17d7d8f2b6f",
   "success": true,
   "result": [
      {
         "date": "2015-10-17",
         "total": 73,
         "errors": [
            {
               "errorCode": "604",
               "count": 1
            },
            {
               "errorCode": "609",
               "count": 56
            },
            {
               "errorCode": "610",
               "count": 16
            }
         ]
      }
   ]
}
```

Chaque objet du tableau `result` contient un jour d’erreur total et une répartition par code d’erreur.

## Erreurs des 7 derniers jours

Récupère les erreurs API des 7 derniers jours. Chaque élément du tableau `result` représente un jour.

```
GET /rest/v1/stats/errors/last7days.json
```

## Membres de la réponse

### Objet de résultat d&#39;utilisation

| Nom | Type de données | Description |
| --- | --- | --- |
| `date` | Chaîne | Date du résumé d’utilisation au format `YYYY-MM-DD`. |
| `total` | Entier | Nombre total d’appels API pour ce jour. |
| `users` | Tableau | Liste des nombres d’utilisations par utilisateur pour ce jour. |

### Objet Utilisateur D’Utilisation

| Nom | Type de données | Description |
| --- | --- | --- |
| `userId` | Chaîne | Identifiant de l’utilisateur API. |
| `count` | Entier | Nombre d’appels API effectués par cet utilisateur pour la journée. |

### Objet de résultat d’erreur

| Nom | Type de données | Description |
| --- | --- | --- |
| `date` | Chaîne | Date du résumé des erreurs au format `YYYY-MM-DD`. |
| `total` | Entier | Nombre total d’erreurs API pour ce jour. |
| `errors` | Tableau | Liste des nombres par code d’erreur pour ce jour. |

### Objet d’erreur

| Nom | Type de données | Description |
| --- | --- | --- |
| `errorCode` | Chaîne | Code d’erreur Marketo. |
| `count` | Entier | Nombre de fois où cette erreur s’est produite pour la journée. |

## Notes

Chacun de vos utilisateurs d’API est signalé individuellement dans la réponse d’utilisation. Le fractionnement des intégrations entre des utilisateurs d’API distincts facilite l’identification du service qui consomme des quotas et produit des erreurs.
