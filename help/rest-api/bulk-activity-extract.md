---
title: Extraction d’activité en bloc
feature: REST API
description: Traitement par lots des données d’activité de Marketo.
exl-id: 6bdfa78e-bc5b-4eea-bcb0-e26e36cf6e19
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '1332'
ht-degree: 7%

---

# Extraction d’activité en bloc

[Référence de point d’entrée de l’extraction d’activité en bloc](https://developer.adobe.com/marketo-apis/api/mapi/)

Le jeu d’API REST d’extraction d’activité en bloc fournit une interface de programmation pour récupérer de grandes quantités de données d’activité hors de Marketo.  Pour les cas qui ne nécessitent pas de faible latence et qui doivent transférer des volumes importants de données d’activité en dehors de Marketo, comme l’intégration CRM, ETL, l’entreposage de données et l’archivage de données.

## Autorisations

Les API Bulk Activity Extract nécessitent que l’utilisateur de l’API dispose des autorisations « Activité en lecture seule » ou « Activité en lecture-écriture ».

## Filtres

| Type de filtre | Type de données | Obligatoire | Notes |
| --- | --- | --- | --- |
| createdAt | Période | Oui | Accepte un objet JSON avec les membres `startAt` et `endAt`. `startAt` accepte une valeur datetime représentant le filigrane bas et `endAt` une valeur datetime représentant le filigrane haut. La plage doit être de 31 jours ou moins. Les traitements avec ce type de filtre renvoient tous les enregistrements accessibles qui ont été créés au cours de la période. Les heures de date doivent être au format ISO-8601, sans millisecondes. |
| activityTypeIds | Tableau\[Entier\] | Non | Accepte un objet JSON avec un membre, `activityTypeIds`. La valeur doit être un tableau de nombres entiers correspondant aux types d’activités souhaités. L’activité « Supprimer le prospect » n’est pas prise en charge (utilisez plutôt le point d’entrée [Obtenir les prospects supprimés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET)). Récupérez les identifiants de type d’activité à l’aide du point d’entrée [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET). |
| [primaryAttributeValueIds](#primaryattributevalueids-options) | Tableau\[Entier\] | Non | Accepte un objet JSON avec un membre, `primaryAttributeValueIds`. La valeur est un tableau d’identifiants qui spécifie les attributs principaux sur lesquels effectuer le filtrage. Un maximum de 50 identifiants peut être spécifié. Les identifiants sont l’identifiant unique d’un champ de prospect ou d’une ressource et peuvent être récupérés en appelant le point d’entrée de l’API REST approprié. Par exemple, pour filtrer sur un formulaire spécifique pour l’activité « Remplir le formulaire », transmettez le nom du formulaire au point d’entrée [Obtenir le formulaire par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) pour récupérer l’ID de formulaire. Voici une liste des types d’activité pour lesquels le filtrage des attributs principaux est pris en charge. |
| [primaryAttributeValues](#primaryattributevalues-options) | Tableau\[Chaîne\] | Non | Accepte un objet JSON avec un membre, `primaryAttributeValues`. La valeur est un tableau de noms qui spécifient les attributs principaux sur lesquels effectuer le filtrage. 50 noms au maximum peuvent être spécifiés. Les noms sont l’identifiant unique d’un champ de prospect ou d’une ressource et peuvent être récupérés en appelant le point d’entrée approprié de l’API REST. Par exemple, pour filtrer sur un formulaire spécifique pour l’activité « Remplir le formulaire », transmettez l’ID de formulaire au point d’entrée [Obtenir le formulaire par ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) pour récupérer le nom du formulaire. Voici une liste des types d’activité pour lesquels le filtrage des attributs principaux est pris en charge. |

### options primaryAttributeValueIds {#primaryattributevalueids-options}

| Type d’activité | ID de valeur d’attribut de Principal | Point d’entrée de récupération | Groupe de ressources |
| --- | --- | --- | --- |
| Modifier la valeur des données | ID de champ de lead | [Décrire le lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nom attribut |
| Modifier évaluation | ID de champ de lead | [Décrire le lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nom attribut |
| Modifier le statut de progression | ID du programme | [Obtenir le programme par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET) | Programme Marketing |
| Ajouter à la liste | Identifiant de liste statique | [Obtenir la liste statique par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Liste statique |
| Supprimer de la liste | Identifiant de liste statique | [Obtenir la liste statique par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Liste statique |
| Remplir formulaire | ID du formulaire | [Obtenir le formulaire par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) | Formulaire web |

Lors de l’utilisation de `primaryAttributeValueIds`, le filtre `activityTypeIds` doit être présent et ne contenir que les identifiants d’activité correspondant au groupe de ressources correspondant. Par exemple, si vous filtrez sur des ressources de formulaire web, seul l’identifiant de type d’activité « Remplir le formulaire » est autorisé dans `activityTypeIds`.

Exemple de corps de requête :

```json
{
  "filter": {
    "createdAt": {
      "startAt": "2021-07-01T23:59:59-00:00",
      "endAt": "2021-07-02T23:59:59-00:00"
    },
    "activityTypeIds": [
      2
    ],
    "primaryAttributeValueIds": [
      16,102,95,8
    ]
  }
}
```

`primaryAttributeValueIds` et `primaryAttributeValues` ne peuvent pas être utilisés ensemble.

### options primaryAttributeValues {#primaryattributevalues-options}

| Type d’activité | Valeur d’attribut de Principal | Point d’entrée de récupération | Groupe de ressources |
| --- | --- | --- | --- |
| Modifier la valeur des données | Nom d’affichage du champ de lead | [Décrire le lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nom attribut |
| Modifier évaluation | Nom d’affichage du champ de lead | [Décrire le lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nom attribut |
| Modifier le statut de progression | Nom de programme | [Obtenir le programme par ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) | Programme Marketing |
| Ajouter à la liste | Nom de liste statique | [Obtenir une liste statique par ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Liste statique |
| Supprimer de la liste | Nom de liste statique | [Obtenir une liste statique par ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Liste statique |
| Remplir formulaire | Nom du formulaire | [Obtenir le formulaire par ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) | Formulaire web |

Notez que vous devez utiliser « &lt;<em>program</em>>.Notation &lt;<em>asset</em>> » pour spécifier le nom des groupes de ressources suivants : Programme marketing, Liste statique, Formulaire web. Par exemple, un formulaire portant le nom « MPS Outbound » et résidant sous un programme nommé « GL_OP_ALL_2021 » est spécifié comme « GL_OP_ALL_2021.MPS Outbound ».

Exemple de corps de requête :

```json
{
  "filter": {
    "createdAt": {
      "startAt": "2021-07-01T23:59:59-00:00",
      "endAt": "2021-07-02T23:59:59-00:00"
    },
    "activityTypeIds": [
      2
    ],
    "primaryAttributeValues": [
      "GL_OP_ALL_2021.MPS Outbound"
    ]
  }
}
```

Lors de l’utilisation de `primaryAttributeValues`, le filtre `activityTypeIds` doit être présent et ne contenir que les identifiants d’activité correspondant au groupe de ressources correspondant. Par exemple, si vous filtrez sur des ressources de formulaire web, seul l’identifiant de type d’activité « Remplir le formulaire » est autorisé dans `activityTypeIds`. `primaryAttributeValues` et `primaryAttributeValueIds` ne peuvent pas être utilisés ensemble.

## Options

| Paramètre | Type de données | Obligatoire | Notes |
|---|---|---|---|
| filter | Array[Object] | Oui | Accepte un tableau de filtres. Un seul filtre `createdAt` doit être inclus dans le tableau . Un filtre `activityTypeIds` facultatif peut être inclus. Les filtres sont appliqués au jeu d’activités accessible et le jeu d’activités résultant est renvoyé par la tâche d’exportation. |
| format | Chaîne | Non | Accepte l’une des valeurs suivantes : CSV, TSV, SSV Le fichier exporté est rendu sous la forme de valeurs séparées par des virgules, de valeurs séparées par des tabulations ou de valeurs séparées par des espaces, respectivement, s’il est défini. La valeur par défaut est CSV si cette valeur n’est pas définie. |
| columnHeaderNames | Objet | Non | Un objet JSON contenant des paires clé-valeur de noms d’en-tête de champ et de colonne. La clé doit être le nom d’un champ inclus dans la tâche d’exportation. La valeur est le nom de l’en-tête de colonne exporté pour ce champ. |
| Champs | Array[String] | Non | Tableau facultatif de chaînes contenant des valeurs de champ. Les champs répertoriés sont inclus dans le fichier exporté. Par défaut, les champs suivants sont renvoyés : <ul><li>`marketoGUIDleadId`</li><li> `activityDate` </li><li>`activityTypeId` </li><li>`campaignId`</li><li> `primaryAttributeValueId` </li><li>`primaryAttributeValue`</li><li> `attributes`</li></ul>. Ce paramètre peut être utilisé pour réduire le nombre de champs renvoyés en spécifiant un sous-ensemble de la liste ci-dessus :`"fields": ["leadId", "activityDate", "activityTypeId"]`. Un `actionResult` de champ supplémentaire peut être spécifié pour inclure l’action d’activité : `("succeeded", "skipped", or "failed")`. |


## Création d’un traitement

Pour exporter des enregistrements, vous devez d’abord définir la tâche et le jeu d’enregistrements à récupérer.  Créez la tâche à l’aide du point d’entrée [Créer une tâche d’activité d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST).  Lors de l’exportation d’activités, deux filtres principaux peuvent être appliqués : `createdAt`, qui est toujours obligatoire, et `activityTypeIds`, qui est facultatif.  Le filtre `createdAt` est utilisé pour définir une période dans laquelle les activités ont été créées, à l’aide des paramètres `startAt` et `endAt` , qui sont tous deux des champs de date et d’heure, et qui représentent respectivement la date de création autorisée la plus proche et la date de création autorisée la plus récente.  Vous pouvez également filtrer uniquement certains types d’activités à l’aide du filtre `activityTypeIds`.  Cela s’avère utile pour supprimer les résultats qui ne sont pas pertinents pour votre cas d’utilisation.

```
POST /bulk/v1/activities/export/create.json
```

```json
{
   "format": "CSV",
   "filter": {
      "createdAt": {
         "startAt": "2017-07-01T23:59:59-00:00",
         "endAt": "2017-07-31T23:59:59-00:00"
      },
      "activityTypeIds": [
         1,
         12,
         13
      ]
   }
}
```

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

Le statut de la tâche est désormais « Créé », mais elle ne se trouve pas encore dans la file d’attente de traitement.  Pour le mettre en file d’attente afin qu’il puisse commencer le traitement, appelez le point d’entrée [Mettre en file d’attente la tâche d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) à l’aide de l’exportId de la réponse de statut de création.

```
POST /bulk/v1/activities/export/{exportId}/enqueue.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Queued",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Désormais, le statut indique que le traitement a été mis en file d’attente.  Lorsqu’un programme de travail devient disponible pour cette tâche, le statut passe à « Traitement » et la tâche commence à agréger les enregistrements de Marketo.

## Interroger le statut de la tâche

Le statut des tâches ne peut être récupéré que pour les tâches créées par le même utilisateur de l’API.

L’extraction d’activité en bloc Marketo est un point d’entrée asynchrone. Par conséquent, le statut de la tâche doit être interrogé pour déterminer quand la tâche est terminée.  Interrogez à l’aide du point d’entrée [Obtenir le statut de la tâche d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) comme suit :

```
GET /bulk/v1/activities/export/{exportId}/status.json
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
         "numberOfRecords": 15423,
         "fileSize": 12342,
         "fileChecksum": "sha256:c16514c7e80fcac5ea055dacae9617fc3c29aff5365e3743071313ce0ed2a815"
      }
   ]
}
```

Le champ de statut peut répondre avec l’une des valeurs suivantes :

- Créé
- En fil d&#39;attente
- En cours de traitement
- Annulé
- Terminé
- Échec

## Récupération de vos données

Une fois la tâche terminée, récupérez vos données à l’aide du point d’entrée [Obtenir le fichier d’activité d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET).

```
GET /bulk/v1/activities/export/{exportId}/file.json
```

La réponse contient un fichier formaté selon la configuration de la tâche. Le point d’entrée répond avec le contenu du fichier .

Si un champ de prospect demandé est vide (ne contient aucune donnée), `then null` est placé dans le champ correspondant dans le fichier d’exportation.  Dans l&#39;exemple ci-dessous, le champ `campaignId` de l&#39;activité renvoyée est vide.

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Pour prendre en charge la récupération partielle et conviviale des données extraites, le point d’entrée du fichier prend éventuellement en charge le `Range` d’en-tête HTTP de type `bytes`.  Si l’en-tête n’est pas défini, l’intégralité du contenu est renvoyée.  Vous pouvez en savoir plus sur l’utilisation de l’en-tête de plage avec Marketo [Extraction en bloc](bulk-extract.md).

## Annulation d’un traitement

Si une tâche n’a pas été configurée correctement ou devient inutile, elle peut facilement être annulée à l’aide du point d’entrée [ Annuler la tâche d’exportation de l’activité ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST) :

```
POST /bulk/v1/activities/export/{exportId}/cancel.json
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

Cette réponse a un statut indiquant que le traitement a été annulé.
