---
title: Extrait de lead en masse
feature: REST API
description: Découvrez comment utiliser les API REST d’extraction de lead en bloc Marketo pour exporter en bloc des leads avec des filtres de date, de liste et de liste dynamique, des champs personnalisés et des formats CSV/TSV.
exl-id: 42796e89-5468-463e-9b67-cce7e798677b
TQID: https://experienceleague.adobe.com/4eMJR87fHDdccrVid3wHtspvBVQmrBGHYMlIwFCSdEI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 1017
ht-degree: 3%

---

# Extrait de lead en masse

[Référence de point d’entrée d’extraction de leads en bloc](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads)

Les API REST d’extraction de leads en bloc récupèrent de grands ensembles d’enregistrements de leads/personnes dans Marketo. Vous pouvez également récupérer les prospects de manière incrémentielle en fonction de la date de création de l’enregistrement, de la mise à jour la plus récente, de l’appartenance à une liste statique ou dynamique.

Utilisez l’extraction de lead en bloc pour l’échange continu de données entre Marketo et des systèmes externes, y compris ETL, le Data Warehouse et les workflows d’archivage.

## Autorisations

L’utilisateur de l’API propriétaire de la tâche doit disposer d’un rôle avec l’autorisation Lecture seule, Lecture-écriture, ou les deux autorisations.

## Filtres

Les traitements d’exportation de leads prennent en charge plusieurs types de filtres. Chaque tâche d’exportation ne peut utiliser qu’un seul type de filtre.

Les filtres `updatedAt`, `smartListName` et `smartListId` nécessitent une infrastructure qui n’est pas disponible dans tous les abonnements.

| Type de filtre | Type de données | Notes |
| --- | --- | --- |
| createdAt | Période | Objet JSON avec des membres `startAt` et `endAt`. `startAt` est l’heure du filigrane bas, et `endAt` est l’heure du filigrane haut. Utilisez les valeurs de date et d’heure ISO-8601 sans millisecondes. La plage doit être de 31 jours ou moins. La tâche renvoie tous les enregistrements accessibles créés au cours de la période. |
| updatedAt* | Période | Objet JSON avec des membres `startAt` et `endAt`. `startAt` est l’heure du filigrane bas, et `endAt` est l’heure du filigrane haut. Utilisez les valeurs de date et d’heure ISO-8601 sans millisecondes. La plage doit être de 31 jours ou moins. Ce filtre n’utilise pas le champ `updatedAt` visible, qui reflète uniquement les mises à jour des champs standard. Il utilise plutôt l’heure de la mise à jour la plus récente du champ pour un enregistrement de prospect. La tâche renvoie tous les enregistrements accessibles les plus récemment mis à jour au cours de la période. |
| staticListName | Chaîne | Nom d’une liste statique. La tâche renvoie tous les enregistrements accessibles qui sont membres de la liste statique lorsque le traitement commence. Récupérez les noms de listes statiques à l’aide du point d’entrée Get Lists. |
| staticListId | Nombre entier | L’identifiant d’une liste statique. La tâche renvoie tous les enregistrements accessibles qui sont membres de la liste statique lorsque le traitement commence. Récupérez les identifiants de liste statiques à l’aide du point d’entrée Get Lists. |
| smartListName* | Chaîne | Nom d’une liste dynamique. La tâche renvoie tous les enregistrements accessibles qui sont membres de la liste dynamique lorsque le traitement commence. Récupérez les noms des listes dynamiques à l’aide du point d’entrée Get Smart Lists. |
| smartListId* | Nombre entier | Identifiant d’une liste dynamique. La tâche renvoie tous les enregistrements accessibles qui sont membres de la liste dynamique lorsque le traitement commence. Récupérez les identifiants de liste dynamique à l’aide du point d’entrée Get Smart Lists. |

Les types de filtres identifiés par un astérisque ne sont pas disponibles pour certains abonnements. Si un type de filtre n’est pas disponible pour votre abonnement, le point d’entrée Créer une tâche d’exportation de prospect renvoie l’erreur « 1035, Unsupported filter type for target subscription » (Type de filtre non pris en charge pour l’abonnement cible). Contactez l’assistance Marketo pour activer cette fonctionnalité dans le cadre de votre abonnement.

## Options

Le point d’entrée Créer une tâche d’exportation principale fournit des options pour sélectionner les champs exportés, renommer les en-têtes de colonne et définir le format du fichier.

| Paramètre | Type de données | Obligatoire | Notes |
| --- | --- | --- | --- |
| Champs | Array[String] | Oui | Un tableau JSON de chaînes. Chaque chaîne doit être le nom de l’API REST d’un champ de prospect Marketo. L’exportation inclut chaque champ répertorié et utilise son nom d’API REST comme en-tête de colonne, sauf si `columnHeaderNames` le remplace. Lorsque la fonction [!DNL Adobe Experience Cloud Audience Sharing] est activée, un processus de synchronisation des cookies associe l’identifiant [!DNL Adobe Experience Cloud] (ECID) aux prospects Marketo. Spécifiez le champ `ecids` pour inclure les ECID dans le fichier d’exportation. |
| columnHeaderNames | Objet | Non | Objet JSON de paires champ-clé-valeur d’en-tête de colonne. Chaque clé doit être le nom d’API d’un champ inclus dans la tâche d’exportation. Récupérez le nom de l’API en appelant Describe Lead. Chaque valeur correspond à l’en-tête de colonne exporté pour ce champ. |
| format | Chaîne | Non | Format du fichier d’exportation : CSV pour les valeurs séparées par des virgules, TSV pour les valeurs séparées par des tabulations ou SSV pour les valeurs séparées par des espaces. La valeur par défaut est CSV. |

## Création d’un traitement

Utilisez le point d’entrée [Créer une tâche d’exportation principale](https://developer.adobe.com/marketo-apis/api/mapi#operation/createExportLeadsUsingPOST) pour définir une tâche d’exportation. Indiquez le `fields` à exporter, un type de `filter` et ses paramètres, le `format` de fichier et les noms d’en-tête de colonne personnalisés.

```http
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
         "endAt": "2017-01-31T00:00:00Z"
      }
   }
}
```

Cette requête crée une tâche d’exportation pour les prospects créés entre le 1er janvier 2017 et le 31 janvier 2017. L’exportation inclut des valeurs des champs `firstName`, `lastName`, `id` et `email`.

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

La réponse confirme que le traitement est créé, mais pas démarré. Pour démarrer la tâche, appelez le point d’entrée [Mettre en file d’attente la tâche d’exportation principale](https://developer.adobe.com/marketo-apis/api/mapi#operation/enqueueExportLeadsUsingPOST) avec le `exportId` de la réponse de création.

```http
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

La réponse mise en file d&#39;attente a le `status` « En file d&#39;attente ». Lorsqu’un emplacement d’exportation devient disponible, le statut passe à « Traitement ».

## Interroger le statut de la tâche

Vous ne pouvez récupérer le statut que pour les tâches créées par le même utilisateur de l’API.

Les traitements d’exportation de leads s’exécutent de manière asynchrone. Interrogez le point d’entrée [Obtenir le statut de la tâche d’exportation du prospect](https://developer.adobe.com/marketo-apis/api/mapi#operation/getExportLeadsStatusUsingGET) pour suivre la progression de la tâche.

Le statut est mis à jour une seule fois toutes les 60 secondes. N&#39;effectuez pas de sondage plus fréquemment ; dans presque tous les cas, cet intervalle est toujours excessif.

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
         "status": "Processing",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Cette réponse indique que la tâche est toujours en cours de traitement et que le fichier n’est donc pas disponible. Lorsque le statut de la tâche passe à « Terminé », le fichier est prêt à être téléchargé.

Le champ `status` peut renvoyer l’une des valeurs suivantes :

- Création
- En fil d&#39;attente
- En cours de traitement
- Annulé
- Terminé
- Échec

## Récupération de vos données

Pour récupérer une exportation de prospect terminée, appelez le point d’entrée [Obtenir le fichier de prospect d’exportation](https://developer.adobe.com/marketo-apis/api/mapi#operation/getExportLeadsFileUsingGET) avec la `exportId` .

```http
GET /bulk/v1/leads/export/{exportId}/file.json
```

Le corps de la réponse contient le fichier au format configuré pour la tâche.

Si un champ de prospect demandé ne contient aucune donnée, le champ correspondant dans le fichier d’exportation contient `null`. Dans l’exemple suivant, le prospect renvoyé a un champ d’e-mail vide.

```csv
firstName,lastName,email,cookies
Russell,Wilson,null,_mch-localhost-1536605780000-12105
```

Pour une récupération partielle ou pouvant être reprise, le point d’entrée du fichier prend en charge l’en-tête `Range` HTTP facultatif avec le type de `bytes` . Si vous ne définissez pas l’en-tête , le point d’entrée renvoie tout le contenu. En savoir plus sur l’utilisation de l’en-tête `Range` avec Marketo [Extraction en bloc](bulk-extract.md).

## Annulation d’un traitement

Pour annuler une tâche incorrectement configurée ou inutile, appelez le point d’entrée [Annuler la tâche d’exportation du prospect](https://developer.adobe.com/marketo-apis/api/mapi#operation/cancelExportLeadsUsingPOST).

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
         "format": "CSV"
      }
   ]
}
```

La réponse confirme l’annulation du traitement.
