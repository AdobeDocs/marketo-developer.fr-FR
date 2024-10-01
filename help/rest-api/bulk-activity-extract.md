---
title: Extraction d’activité en bloc
feature: REST API
description: Données d’activité de traitement par lot de Marketo.
exl-id: 6bdfa78e-bc5b-4eea-bcb0-e26e36cf6e19
source-git-commit: 8c22255673fee1aa0f5b47393a241fcf6680778b
workflow-type: tm+mt
source-wordcount: '1343'
ht-degree: 7%

---

# Extraction d’activité en bloc

[Référence du point de terminaison d’extraction d’activité en masse](https://developer.adobe.com/marketo-apis/api/mapi/)

L’ensemble d’extraction d’activité en bloc des API REST fournit une interface de programmation pour récupérer de grandes quantités de données d’activité à partir de Marketo.  Dans les cas qui ne nécessitent pas de faible latence et qui doivent transférer un volume important de données d’activité hors de Marketo, comme l’intégration CRM, ETL, l’entrepôt de données et l’archivage des données.

## Autorisations

Les API Bulk Activity Extract nécessitent que l’utilisateur de l’API dispose des autorisations &quot;Activité en lecture seule&quot; ou &quot;Activité en lecture-écriture&quot;.

## Filtres

| Type de filtre | Type de données | Obligatoire | Notes |
| --- | --- | --- | --- |
| createdAt | Période | Oui | Accepte un objet JSON avec les membres `startAt` et `endAt`. `startAt` accepte une date et une heure représentant le filigrane bas, et `endAt` accepte une date et une heure représentant le filigrane haut. La période doit être de 31 jours ou moins. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles créés au cours de la période. Datetimes doit être au format ISO-8601, sans millisecondes. |
| activityTypeIds | Tableau\[Entier\] | Non | Accepte un objet JSON avec un membre, `activityTypeIds`. La valeur doit être un tableau d’entiers, correspondant aux types d’activité souhaités. L’activité &quot;Supprimer la piste&quot; n’est pas prise en charge (utilisez plutôt le point de terminaison [Get Deleted Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) ). Récupérez les identifiants de type d’activité à l’aide du point de terminaison [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET). |
| [primaryAttributeValueIds](#primaryattributevalueids-options) | Tableau\[Entier\] | Non | Accepte un objet JSON avec un membre, `primaryAttributeValueIds`. La valeur est un tableau d’identifiants qui spécifie les attributs principaux sur lesquels filtrer. Un maximum de 50 id peut être spécifié. Les identifiants sont l’identifiant unique d’un champ de piste ou d’une ressource. Ils peuvent être récupérés en appelant le point de terminaison de l’API REST approprié. Par exemple, pour filtrer sur un formulaire spécifique pour l’activité &quot;Remplir le formulaire&quot;, transmettez le nom du formulaire au point de terminaison [Obtenir le formulaire par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) pour récupérer l’ID de formulaire. Voici une liste des types d’activités pour lesquels le filtrage des attributs principaux est pris en charge. |
| [primaryAttributeValues](#primaryattributevalues-options) | Tableau\[String\] | Non | Accepte un objet JSON avec un membre, `primaryAttributeValues`. La valeur est un tableau de noms qui spécifie les attributs principaux sur lesquels filtrer. 50 noms au maximum peuvent être spécifiés. Les noms sont l’identifiant unique d’un champ de piste ou d’une ressource. Ils peuvent être récupérés en appelant le point de terminaison de l’API REST approprié. Par exemple, pour filtrer sur un formulaire spécifique pour l’activité &quot;Remplir le formulaire&quot;, transmettez l’ID de formulaire au point de terminaison [Obtenir le formulaire selon l’ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) pour récupérer le nom du formulaire. Voici une liste des types d’activités pour lesquels le filtrage des attributs principaux est pris en charge. |

### options primaryAttributeValueIds {#primaryattributevalueids-options}

| Type d’activité | ID de valeur d’attribut de Principal | Point de terminaison de récupération | Groupe de ressources |
| --- | --- | --- | --- |
| Modifier la valeur des données | Identifiant du champ de piste | [Description de la piste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nom attribut |
| Modifier évaluation | Identifiant du champ de piste | [Description de la piste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nom attribut |
| Modifier le statut de progression | Identifiant du programme | [Obtenir le programme par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET) | Programme Marketing |
| Ajouter à la liste | Identifiant de liste statique | [Obtenir la liste statique par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Liste statique |
| Supprimer de la liste | Identifiant de liste statique | [Obtenir la liste statique par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Liste statique |
| Remplir formulaire | Identifiant de formulaire | [Obtenir le formulaire par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) | Formulaire web |

Lors de l’utilisation de `primaryAttributeValueIds`, le filtre `activityTypeIds` doit être présent et ne contenir que les identifiants d’activité correspondant au groupe de ressources correspondant. Par exemple, si vous filtrez sur des ressources de formulaire web, seul l’ID de type d’activité &quot;Remplir le formulaire&quot; est autorisé dans `activityTypeIds`.

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

| Type d’activité | Valeur d’attribut de Principal | Point de terminaison de récupération | Groupe de ressources |
| --- | --- | --- | --- |
| Modifier la valeur des données | Nom d’affichage du champ de piste | [Description de la piste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nom attribut |
| Modifier évaluation | Nom d’affichage du champ de piste | [Description de la piste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nom attribut |
| Modifier le statut de progression | Nom de programme | [Obtenir le programme par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) | Programme Marketing |
| Ajouter à la liste | Nom de liste statique | [Obtenir la liste statique par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Liste statique |
| Supprimer de la liste | Nom de liste statique | [Obtenir la liste statique par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Liste statique |
| Remplir formulaire | Nom du formulaire | [Obtenir le formulaire par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) | Formulaire web |

Notez que vous devez utiliser &quot;&lt;<em>program</em>>&quot;.&lt;<em>asset</em>>&quot; pour spécifier le nom des groupes de ressources suivants : Programme marketing, Liste statique, Formulaire web. Par exemple, un formulaire nommé &quot;MPS Outbound&quot; qui réside sous un programme nommé &quot;GL_OP_ALL_2021&quot; sera spécifié comme &quot;GL_OP_ALL_2021.MPS Outbound&quot;.

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

Lors de l’utilisation de `primaryAttributeValues`, le filtre `activityTypeIds` doit être présent et ne contenir que les identifiants d’activité correspondant au groupe de ressources correspondant. Par exemple, si vous filtrez sur des ressources de formulaire web, alors seul l’ID de type d’activité &quot;Remplir le formulaire&quot; est autorisé dans `activityTypeIds`. `primaryAttributeValues` et `primaryAttributeValueIds` ne peuvent pas être utilisés ensemble.

## Options

| Paramètre | Type de données | Obligatoire | Notes |
|---|---|---|---|
| filter | Tableau[Objet] | Oui | Accepte un tableau de filtres. Exactement un filtre `createdAt` doit être inclus dans le tableau . Un filtre facultatif `activityTypeIds` peut être inclus. Les filtres sont appliqués au jeu d’activités accessible et le jeu d’activités qui en résulte est renvoyé par la tâche d’exportation. |
| format | Chaîne | Non | Accepte l’un des éléments suivants : CSV, TSV, SSV Le fichier exporté est rendu sous la forme d’un fichier de valeurs séparées par des virgules, de valeurs séparées par des tabulations ou de valeurs séparées par des espaces, respectivement s’il est défini. La valeur par défaut est CSV si elle n’est pas définie. |
| columnHeaderNames | Objet | Non | Objet JSON contenant des paires clé-valeur de noms d’en-tête de champ et de colonne. La clé doit être le nom d’un champ inclus dans la tâche d’exportation. La valeur est le nom de l’en-tête de colonne exporté pour ce champ. |
| Champs | Tableau[Chaîne] | Non | Tableau facultatif de chaînes contenant des valeurs de champ. Les champs répertoriés sont inclus dans le fichier exporté. Par défaut, les champs suivants sont renvoyés : `marketoGUIDleadId` `activityDate` `activityTypeId` `campaignId` `primaryAttributeValueId` `primaryAttributeValueattributes`. Ce paramètre peut être utilisé pour réduire le nombre de champs renvoyés en spécifiant un sous-ensemble dans la liste ci-dessus. Exemple : &quot;fields&quot;: [&quot;leadId&quot;, &quot;activityDate&quot;, &quot;activityTypeId&quot;]Un champ supplémentaire &quot;actionResult&quot; peut être spécifié pour inclure l’action d’activité (&quot;succeeded&quot;, &quot;skipped&quot; ou &quot;failed&quot;). |


## Création d’une tâche

Pour exporter des enregistrements, vous devez d&#39;abord définir la tâche et l&#39;ensemble des enregistrements à récupérer.  Créez la tâche à l’aide du point de terminaison [Créer une tâche d’activité d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST).  Lors de l’exportation d’activités, deux filtres principaux peuvent être appliqués : `createdAt`, qui est toujours requis, et `activityTypeIds`, qui est facultatif.  Le filtre `createdAt` est utilisé pour définir une plage de dates dans laquelle les activités ont été créées, à l’aide des paramètres `startAt` et `endAt`, qui sont tous deux des champs datetime, et qui représentent respectivement la date de création la plus ancienne et la dernière date de création autorisée.  Vous pouvez également, en option, filtrer uniquement certains types d’activités en utilisant le filtre `activityTypeIds`.  Cela s’avère utile pour supprimer les résultats qui ne sont pas pertinents pour votre cas d’utilisation.

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

La tâche a désormais le statut &quot;Créée&quot;, mais elle ne se trouve pas encore dans la file d’attente de traitement.  Pour le placer dans la file d’attente afin qu’il puisse commencer le traitement, appelez le point de terminaison [Enqueue Export Activity Job](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) à l’aide de l’exportId à partir de la réponse d’état de création.

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

Maintenant, l’état indique que la tâche a été mise en file d’attente.  Lorsqu’un programme de travail devient disponible pour cette tâche, l’état est alors défini sur &quot;Traitement&quot; et la tâche commence à agréger des enregistrements à partir de Marketo.

## État de la tâche d’interrogation

L’état de la tâche ne peut être récupéré que pour les tâches créées par le même utilisateur de l’API.

Marketo Bulk Activity Extract est un point de terminaison asynchrone. Par conséquent, l’état de la tâche doit être interrogé pour déterminer quand la tâche est terminée.  Sondage à l’aide du point de terminaison [Obtenir l’état de la tâche d’activité d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) comme suit :

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

Le champ statut peut répondre avec l’une des valeurs suivantes :

- Créé
- En fil d&#39;attente
- En cours de traitement
- Annulé
- Terminé
- Échec

## Récupération de vos données

Une fois la tâche terminée, récupérez vos données à l’aide du point de terminaison [Get Export Activity File](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET).

```
GET /bulk/v1/activities/export/{exportId}/file.json
```

La réponse contient un fichier formaté selon la configuration de la tâche. Le point de terminaison répond avec le contenu du fichier.

Si un champ de piste demandé est vide (ne contient aucune donnée), `then null` est placé dans le champ correspondant dans le fichier d’exportation.  Dans l’exemple ci-dessous, le champ campaignId de l’activité renvoyée est vide.

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Pour prendre en charge la récupération partielle et conviviale des données extraites, le point de terminaison du fichier prend éventuellement en charge l’en-tête HTTP `Range` du type `bytes`.  Si l’en-tête n’est pas défini, l’ensemble du contenu est renvoyé.  Vous pouvez en savoir plus sur l’utilisation de l’en-tête Plage avec Marketo [Extraction en bloc](bulk-extract.md).

## Annulation d’une tâche

Si une tâche a été mal configurée ou devient inutile, elle peut être facilement annulée à l’aide du point de terminaison [Annuler l’exportation de la tâche d’activité](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST) :

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

Un état indique que la tâche a été annulée.
