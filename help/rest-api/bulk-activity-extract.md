---
title: "Extraction d’activité en bloc"
feature: REST API
description: "Données d’activité de traitement par lot de Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1381'
ht-degree: 7%

---


# Extraction d’activité en bloc

[Référence du point de terminaison d’extraction d’activité en bloc](https://developer.adobe.com/marketo-apis/api/mapi/)

L’ensemble d’extraction d’activité en bloc des API REST fournit une interface de programmation pour récupérer de grandes quantités de données d’activité à partir de Marketo.  Pour les cas qui ne nécessitent pas de faible latence et qui doivent transférer un volume important de données d’activité hors de Marketo, comme l’intégration CRM, ETL, l’entrepôt de données et l’archivage des données.

## Permissions

Les API Bulk Activity Extract nécessitent que l’utilisateur de l’API dispose des autorisations &quot;Activité en lecture seule&quot; ou &quot;Activité en lecture-écriture&quot;.

## Filtres

<table>
  <tbody>
    <tr>
      <td>Type de filtre</td>
      <td>Type de données</td>
      <td>Requis</td>
      <td>Notes</td>
    </tr>
    <tr>
      <td>createdAt</td>
      <td>Plage de dates</td>
      <td>Oui</td>
      <td>Accepte un objet JSON avec les membres startAt et endAt. startAt accepte une date et une heure représentant le filigrane faible, et endAt accepte une date et une heure représentant le filigrane élevé. La plage doit être de 31 jours ou moins. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles créés dans la plage de dates. Datetimes doit être au format ISO-8601, sans millisecondes.</td>
    </tr>
    <tr>
      <td>activityTypeIds</td>
      <td>Array[Integer]</td>
      <td>Non</td>
      <td>Accepte un objet JSON avec un membre, activityTypeIds. La valeur doit être un tableau d’entiers, correspondant aux types d’activité souhaités. L’activité "Supprimer le prospect" n’est pas prise en charge (utilisez <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET">Obtenir les pistes supprimées</a>point d’entrée à la place).Récupérez les identifiants de type d’activité à l’aide de la variable<a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET">Obtention des types d’activité</a>point de terminaison .</td>
    </tr>
    <tr>
      <td>primaryAttributeValueIds</td>
      <td>Array[Integer]</td>
      <td>Non</td>
      <td>Accepte un objet JSON avec un membre, primaryAttributeValueIds. La valeur est un tableau d’identifiants qui spécifie les attributs principaux sur lesquels filtrer. Vous pouvez spécifier un maximum de 50 identifiants. Ces identifiants sont l’identifiant unique d’un champ de piste ou d’une ressource. Ils peuvent être récupérés en appelant le point de terminaison de l’API REST approprié. Par exemple, pour filtrer selon un formulaire spécifique pour l’activité "Remplir le formulaire", transmettez le nom du formulaire à la variable <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET">Obtenir le formulaire par nom</a> point de terminaison pour récupérer l’ID de formulaire. Voici une liste des types d’activité pour lesquels le filtrage d’attribut principal est pris en charge.
        <table>
          <tbody>
            <tr>
              <td>Type d’activité</td>
              <td>ID de valeur d’attribut de Principal</td>
              <td>Point de terminaison de récupération</td>
              <td>Groupe de ressources</td>
            </tr>
            <tr>
              <td>Modifier la valeur des données</td>
              <td>Identifiant du champ de piste</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Description de la piste</a></td>
              <td>Nom attribut</td>
            </tr>
            <tr>
              <td>Modifier évaluation</td>
              <td>Identifiant du champ de piste</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Description de la piste</a></td>
              <td>Nom attribut</td>
            </tr>
            <tr>
              <td>Modifier le statut de progression</td>
              <td>Identifiant du programme</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET">Obtenir le programme par nom</a></td>
              <td>Programme Marketing</td>
            </tr>
            <tr>
              <td>Ajouter à la liste</td>
              <td>Identifiant de liste statique</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET">Obtention d’une liste statique par nom</a></td>
              <td>Liste statique</td>
            </tr>
            <tr>
              <td>Supprimer de la liste</td>
              <td>Identifiant de liste statique</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET">Obtention d’une liste statique par nom</a></td>
              <td>Liste statique</td>
            </tr>
            <tr>
              <td>Remplir formulaire</td>
              <td>Identifiant de formulaire</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET">Obtenir le formulaire par nom</a></td>
              <td>Formulaire web</td>
            </tr>
          </tbody>
        </table>
        Lors de l’utilisation de primaryAttributeValueIds, le filtre activityTypeIds doit être présent et ne contenir que les identifiants d’activité correspondant au groupe de ressources correspondant. Par exemple, si vous filtrez sur des ressources de formulaire web, seul l’identifiant de type d’activité "Remplir le formulaire" est autorisé dans activityTypeIds.Exemple Corps de requête :{"filter":{"startAt": "2021-07-01T23": ":59:59-00:00","endAt": "2021-07-02T23:59:59-00:00"},"activityTypeIds":[2],"primaryAttributeValueIds" : [16,102,95,8]}}primaryAttributeIds et primaryAttributeValues ne peuvent pas être utilisés ensemble.</td>
    </tr>
    <tr>
      <td>primaryAttributeValues</td>
      <td>Array[String]</td>
      <td>Non</td>
      <td>Accepte un objet JSON avec un membre, primaryAttributeValues. La valeur est un tableau de noms qui spécifie les attributs principaux sur lesquels filtrer. Vous pouvez spécifier un maximum de 50 noms. Ces noms sont l’identifiant unique d’un champ de piste ou d’une ressource. Ils peuvent être récupérés en appelant le point de terminaison de l’API REST approprié. Par exemple, pour filtrer selon un formulaire spécifique pour l’activité "Remplir le formulaire", transmettez l’ID de formulaire à la variable <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5">Obtenir le formulaire par identifiant</a> point de terminaison pour récupérer le nom du formulaire. Voici une liste des types d’activité pour lesquels le filtrage des attributs principaux est pris en charge.
        <table>
          <tbody>
            <tr>
              <td>Type d’activité</td>
              <td>Valeur d’attribut de Principal</td>
              <td>Point de terminaison de récupération</td>
              <td>Groupe de ressources</td>
            </tr>
            <tr>
              <td>Modifier la valeur des données</td>
              <td>Nom d’affichage du champ de piste</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Description de la piste</a></td>
              <td>Nom attribut</td>
            </tr>
            <tr>
              <td>Modifier évaluation</td>
              <td>Nom d’affichage du champ de piste</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Description de la piste</a></td>
              <td>Nom attribut</td>
            </tr>
            <tr>
              <td>Modifier le statut de progression</td>
              <td>Nom de programme</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET">Obtention du programme par identifiant</a></td>
              <td>Programme Marketing</td>
            </tr>
            <tr>
              <td>Ajouter à la liste</td>
              <td>Nom de liste statique</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET">Obtention d’une liste statique par identifiant</a></td>
              <td>Liste statique</td>
            </tr>
            <tr>
              <td>Supprimer de la liste</td>
              <td>Nom de liste statique</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET">Obtention d’une liste statique par identifiant</a></td>
              <td>Liste statique</td>
            </tr>
            <tr>
              <td>Remplir formulaire</td>
              <td>Nom du formulaire</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5">Obtenir le formulaire par identifiant</a></td>
              <td>Formulaire web</td>
            </tr>
          </tbody>
        </table>
        Notez que vous devez utiliser "&lt;<em>program</em>&gt;.&lt;<em>ressource</em>&gt;" pour spécifier de manière unique le nom des groupes de ressources suivants : Programme marketing, Liste statique, Formulaire web. Exemple : un formulaire nommé "MPS sortant" et intitulé "GL_OP_ALL_2021" est spécifié sous "GL_OP_ALL_2021" sous "GL_OP_ALL_2021.MPS sortant".Exemple de corps de requête :{"filter" : "createdAt":{"startAt": "2021-07-01T23:59:59-00:00","endAt": "2021-07-02T23:59:59-00:00"},"activityTypeIds":[2],"primaryAttributeValues":["GL_OP_ALL_2021.MPS Outbound"]}}Lors de l’utilisation de primaryAttributeValues, le filtre activityTypeIds doit être présent et ne contenir que les identifiants d’activité correspondant au groupe de ressources correspondant. Par exemple, si vous filtrez sur des ressources de formulaire web, seul l’ID de type d’activité "Remplir le formulaire" est autorisé dans activityTypeIds.primaryAttributeValues et primaryAttributeValueIds ne peuvent pas être utilisés ensemble.</td>
    </tr>
  </tbody>
</table>

## Options

| Paramètre | Type de données | Requis | Notes |
|---|---|---|---|
| Filtre | Tableau[Objet] | Oui | Accepte un tableau de filtres. Un filtre createdAt doit être inclus dans le tableau. Un filtre optionnel activityTypeIds peut être inclus. Les filtres sont appliqués à l’ensemble d’activités accessibles et l’ensemble d’activités qui en résulte est renvoyé par la tâche d’exportation. |
| format | Chaîne | Non | Accepte l’un des éléments suivants : CSV, TSV, SSV Le fichier exporté est rendu sous la forme de valeurs séparées par des virgules, de valeurs séparées par des tabulations ou de fichiers de valeurs séparées par des espaces, respectivement s’il est défini. La valeur par défaut est CSV s’il n’est pas défini. |
| columnHeaderNames | Objet | Non | Objet JSON contenant des paires clé-valeur de noms d’en-tête de champ et de colonne. La clé doit être le nom d’un champ inclus dans la tâche d’exportation. La valeur est le nom de l’en-tête de colonne exporté pour ce champ. |
| Champs | Tableau[Chaîne] | Non | Tableau facultatif de chaînes contenant des valeurs de champ. Les champs listés sont inclus dans le fichier exporté. Par défaut, les champs suivants sont renvoyés : `marketoGUIDleadId` `activityDate` `activityTypeId` `campaignId` `primaryAttributeValueId` `primaryAttributeValueattributes`,Ce paramètre peut être utilisé pour réduire le nombre de champs renvoyés en spécifiant un sous-ensemble dans la liste ci-dessus.Exemple :&quot;champs&quot; : [&quot;leadId&quot;, &quot;activityDate&quot;, &quot;activityTypeId&quot;]Un champ supplémentaire &quot;actionResult&quot; peut être spécifié pour inclure l’action de l’activité (&quot;succeeded&quot;, &quot;skipped&quot; ou &quot;failed&quot;). |


## Création d’une tâche

Pour exporter des enregistrements, vous devez d&#39;abord définir la tâche et l&#39;ensemble des enregistrements à récupérer.  Créez la tâche à l’aide du [Création d’une tâche d’activité d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST) point de terminaison .  Lors de l&#39;export d&#39;activités, deux filtres principaux peuvent être appliqués : `createdAt`, qui est toujours nécessaire, et `activityTypeIds`, qui est facultatif.  Le filtre createdAt permet de définir une période au cours de laquelle les activités ont été créées, à l’aide de la variable `startAt` et `endAt` , qui sont tous deux des champs datetime et qui représentent respectivement la date de création la plus ancienne et la date de création autorisée la plus récente.  Vous pouvez également, en option, filtrer uniquement certains types d’activités en utilisant la variable `activityTypeIds` filtre.  Cela s’avère utile pour supprimer des résultats qui ne sont pas pertinents pour votre cas d’utilisation.

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

La tâche a désormais le statut &quot;Créée&quot;, mais elle ne se trouve pas encore dans la file d’attente de traitement.  Pour le placer dans la file d’attente afin qu’il puisse commencer le traitement, nous devons appeler la fonction [Traitement de l’activité d’exportation en attente](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) point d’entrée à l’aide de l’exportId de la réponse d’état de création.

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

L’état indique maintenant que la tâche a été mise en file d’attente.  Lorsqu’un programme de travail devient disponible pour cette tâche, l’état est alors activé sur &quot;Traitement&quot; et la tâche commence à agréger des enregistrements à partir de Marketo.

## État de la tâche d’interrogation

L’état de la tâche ne peut être récupéré que pour les tâches créées par le même utilisateur de l’API.

Marketo Bulk Activity Extract est un point de terminaison asynchrone. Par conséquent, l’état de la tâche doit être interrogé pour déterminer quand la tâche est terminée.  Sondage à l’aide du [Obtention de l’état de la tâche d’activité d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) endpoint comme suit :

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

Une fois la tâche terminée, récupérez vos données à l’aide de la fonction [Obtenir le fichier d’activité d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET) point de terminaison .

```
GET /bulk/v1/activities/export/{exportId}/file.json
```

La réponse contient un fichier formaté selon la configuration de la tâche. Le point de terminaison répond avec le contenu du fichier.

Si un champ de piste demandé est vide (ne contient aucune donnée), `then null` est placé dans le champ correspondant du fichier d&#39;export.  Dans l’exemple ci-dessous, le champ campaignId de l’activité renvoyée est vide.

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Pour prendre en charge la récupération partielle et conviviale des données extraites, le point de terminaison de fichier prend éventuellement en charge l’en-tête HTTP. `Range` du type `bytes`.  Si l’en-tête n’est pas défini, l’ensemble du contenu est renvoyé.  Vous pouvez en savoir plus sur l’utilisation de l’en-tête Plage avec Marketo [Extraction en bloc](bulk-extract.md).

## Annulation d’une tâche

Si une tâche a été mal configurée ou devient inutile, elle peut être facilement annulée à l’aide de la fonction [Annuler l’exportation de la tâche d’activité](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST) endpoint :

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
