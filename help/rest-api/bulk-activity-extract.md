---
title: Extraction d’activité en bloc
feature: REST API
description: API REST d’extraction d’activité en bloc Marketo pour exporter des données d’activité volumineuses à l’aide d’une période de 31 jours, de filtres d’activité et d’attributs principaux pour ETL et CRM.
exl-id: 6bdfa78e-bc5b-4eea-bcb0-e26e36cf6e19
TQID: https://experienceleague.adobe.com/lIlXNjatN-F77Dv3xsVkQ3hAWwLZ4wlSW0zKNkFJFMA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1268
ht-degree: 7%

---

# Extraction d’activité en bloc

[Référence de point d’entrée de l’extraction d’activité en bloc](https://developer.adobe.com/marketo-apis/api/mapi)

Les API Bulk Activity Extract REST récupèrent d’importants volumes de données d’activité dans Marketo. Utilisez ces API pour les processus qui ne nécessitent pas de faible latence, comme l’intégration CRM, ETL, l’entreposage des données et l’archivage des données.

## Autorisations

L’utilisateur de l’API doit disposer de l’autorisation « Activité en lecture seule » ou « Activité en lecture-écriture ».

## Filtres

| Type de filtre | Type de données | Obligatoire | Notes |
| --- | --- | --- | --- |
| `createdAt` | Période | Oui | Un objet JSON contenant des `startAt` et des `endAt`. `startAt` est l’heure du filigrane bas, et `endAt` est l’heure du filigrane haut. La plage doit être de 31 jours ou moins. La tâche renvoie tous les enregistrements accessibles créés au cours de la période. Utilisez les valeurs de date et d’heure ISO-8601 sans millisecondes. |
| `activityTypeIds` | Tableau\[Entier\] | Non | Tableau d’entiers pour les types d’activités demandés. L’activité « Supprimer le prospect » n’est pas prise en charge. Utilisez plutôt le point d’entrée [Obtenir les leads supprimés](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET). Récupérez les identifiants de type d’activité avec le point d’entrée [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET). |
| [`primaryAttributeValueIds`](#primaryattributevalueids-options) | Tableau\[Entier\] | Non | Un tableau qui accepte un maximum de 50 identifiants pour les attributs principaux. Chaque identifiant identifie de manière unique un champ ou une ressource de prospect. Récupérez les identifiants en appelant le point d’entrée de l’API REST approprié. Par exemple, pour filtrer sur un formulaire spécifique pour l’activité « Remplir le formulaire », transmettez le nom du formulaire au point d’entrée [Obtenir le formulaire par nom](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET) pour récupérer l’ID de formulaire. Voir [options primaryAttributeValueIds](#primaryattributevalueids-options) pour connaître les types d’activité pris en charge. |
| [`primaryAttributeValues`](#primaryattributevalues-options) | Tableau\[Chaîne\] | Non | Un tableau qui accepte un maximum de 50 noms pour les attributs principaux. Chaque nom identifie de manière unique un champ ou une ressource de prospect. Récupérez les noms en appelant le point d’entrée de l’API REST approprié. Par exemple, pour filtrer sur un formulaire spécifique pour l’activité « Remplir le formulaire », transmettez l’ID de formulaire au point d’entrée [Obtenir le formulaire par ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByIdUsingGET) pour récupérer le nom du formulaire. Voir [options primaryAttributeValues](#primaryattributevalues-options) pour connaître les types d’activité pris en charge. |

### options primaryAttributeValueIds {#primaryattributevalueids-options}

| Type d’activité | ID de valeur d’attribut de Principal | Point d’entrée de récupération | Groupe de ressources |
| --- | --- | --- | --- |
| Modification de la valeur des données | ID de champ de lead | [Décrire le lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nom de l’attribut |
| Modifier évaluation | ID de champ de lead | [Décrire le lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nom de l’attribut |
| Modifier le statut dans la progression | ID du programme | [Obtenir le programme par nom](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByNameUsingGET) | Programme Marketing |
| Ajouter à la liste | Identifiant de liste statique | [Obtenir la liste statique par nom](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Liste statique |
| Suppression de la liste | Identifiant de liste statique | [Obtenir la liste statique par nom](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Liste statique |
| Remplir formulaire | ID du formulaire | [Obtenir le formulaire par nom](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET) | Formulaire web |

Lorsque vous utilisez `primaryAttributeValueIds`, vous devez également inclure le filtre `activityTypeIds`. Ce filtre ne peut contenir que les ID d’activité correspondant au groupe de ressources correspondant. Par exemple, lors du filtrage de ressources de formulaire web, `activityTypeIds` ne peut contenir que l’identifiant de type d’activité « Remplir le formulaire ».

La requête suivante inclut le filtre `primaryAttributeValueIds` :

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
| Modification de la valeur des données | Nom d’affichage du champ de lead | [Décrire le lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nom de l’attribut |
| Modifier évaluation | Nom d’affichage du champ de lead | [Décrire le lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nom de l’attribut |
| Modifier le statut dans la progression | Nom de programme | [Obtenir le programme par ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByIdUsingGET) | Programme Marketing |
| Ajouter à la liste | Nom de liste statique | [Obtenir une liste statique par ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Liste statique |
| Suppression de la liste | Nom de liste statique | [Obtenir une liste statique par ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Liste statique |
| Remplir formulaire | Nom du formulaire | [Obtenir le formulaire par ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByIdUsingGET) | Formulaire web |

Utilisez la notation `&lt;program&gt;.&lt;asset&gt;` pour spécifier les noms des groupes de ressources Programme marketing, Liste statique et Formulaire web . Par exemple, spécifiez le formulaire « MPS Outbound » dans le programme « GL_OP_ALL_2021 » comme « GL_OP_ALL_2021.MPS Outbound ».

La requête suivante inclut le filtre `primaryAttributeValues` :

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

Lorsque vous utilisez `primaryAttributeValues`, vous devez également inclure le filtre `activityTypeIds`. Ce filtre ne peut contenir que les ID d’activité correspondant au groupe de ressources correspondant. Par exemple, lors du filtrage de ressources de formulaire web, `activityTypeIds` ne peut contenir que l’identifiant de type d’activité « Remplir le formulaire ».

`primaryAttributeValues` et `primaryAttributeValueIds` ne peuvent pas être utilisés ensemble.

## Options

| Paramètre | Type de données | Obligatoire | Notes |
| --- | --- | --- | --- |
| `filter` | Objet | Oui | Objet contenant des filtres qui s’appliquent au jeu d’activités accessible. Incluez exactement un filtre `createdAt`. Vous pouvez également inclure un filtre `activityTypeIds`. La tâche d’exportation renvoie le jeu d’activités obtenu. |
| `format` | Chaîne | Non | Format du fichier d’exportation : CSV, TSV ou SSV. Ces valeurs produisent respectivement des valeurs séparées par des virgules, des tabulations ou des espaces. La valeur par défaut est CSV. |
| `columnHeaderNames` | Objet | Non | Objet JSON de paires champ-clé-valeur d’en-tête de colonne. Chaque clé doit nommer un champ inclus dans la tâche d’exportation. Sa valeur définit l’en-tête de colonne exporté pour ce champ. |
| `fields` | Tableau\[Chaîne\] | Non | Tableau de champs à inclure dans le fichier d’exportation. Par défaut, la réponse inclut `marketoGUID`, `leadId`, `activityDate`, `activityTypeId`, `campaignId`, `primaryAttributeValueId`, `primaryAttributeValue` et `attributes`. Pour renvoyer un sous-ensemble, spécifiez les champs de cette liste, tels que `"fields": ["leadId", "activityDate", "activityTypeId"]`. Vous pouvez également définir la `actionResult` d’inclusion de l’action d’activité : `("succeeded", "skipped", or "failed")`. |

## Création d’un traitement

Créez une tâche d’exportation pour définir les enregistrements à récupérer. Utilisez le point d’entrée [Créer une tâche d’activité d’exportation](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST).

Chaque tâche nécessite un filtre `createdAt`. Ses paramètres datetime `startAt` et `endAt` définissent les dates de création d’activité autorisées au plus tôt et au plus tard. Pour exclure les types d’activité qui ne sont pas pertinents, incluez également le filtre `activityTypeIds` facultatif.

La requête suivante crée une tâche d’exportation CSV pour les types d’activités sélectionnés au cours d’une période :

```http
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

La réponse renvoie un `exportId` et un statut de « Créé ». Une tâche créée ne se trouve pas encore dans la file d’attente de traitement.

Pour ajouter la tâche à la file d’attente, appelez le point d’entrée [Mettre en file d’attente la tâche d’exportation](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) avec la `exportId` de la réponse de création.

```http
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

Le statut de la réponse est désormais « En file d’attente ». Lorsqu’un programme de travail est disponible, le statut passe à « Traitement » et le traitement commence à agréger les enregistrements à partir de Marketo.

## Interroger le statut de la tâche

Le statut des tâches ne peut être récupéré que pour les tâches créées par le même utilisateur de l’API.

L’extraction d’activité en bloc traite les tâches de manière asynchrone. Interrogez le point d’entrée [Obtenir le statut de la tâche d’activité d’exportation](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) pour déterminer quand une tâche est terminée :

```http
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

Le champ `status` renvoie l’une des valeurs suivantes :

- `Created`
- `Queued`
- `Processing`
- `Canceled`
- `Completed`
- `Failed`

## Récupération de vos données

Lorsque le statut de la tâche est « Terminé », récupérez les données exportées avec le point d’entrée [Obtenir le fichier d’activité d’exportation](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET) :

```http
GET /bulk/v1/activities/export/{exportId}/file.json
```

Le corps de la réponse contient le fichier au format configuré pour la tâche.

Si un champ d’activité demandé ne contient aucune donnée, `null` apparaît dans le champ du fichier d’exportation correspondant. L’exemple suivant illustre les données d’activité exportées :

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Pour une récupération partielle ou pouvant être reprise, le point d’entrée du fichier prend en charge l’en-tête `Range` HTTP facultatif avec une plage de `bytes`. Si vous omettez cet en-tête, le point d’entrée renvoie le fichier entier. Pour plus d’informations sur l’utilisation de l’en-tête `Range`, voir [Extraction en bloc](bulk-extract.md).

## Annulation d’un traitement

Pour arrêter un traitement incorrectement configuré ou inutile, appelez le point d’entrée [ Annuler le traitement de l’activité d’exportation ](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST) :

```http
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

Le statut de la réponse indique que le traitement est annulé.
