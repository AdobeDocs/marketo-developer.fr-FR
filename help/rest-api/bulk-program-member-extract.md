---
title: "Extraction de membres de programme en bloc"
feature: REST API
description: "Traitement par lots de l’extraction des données membres."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '1142'
ht-degree: 4%

---


# Extraction de membres du programme en bloc

[Référence du point de terminaison d’extraction de membre du programme en masse](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members)

L’ensemble des API REST d’extraction de membres de programme en masse fournit une interface programmatique pour récupérer de grands ensembles d’enregistrements de membres de programme en dehors de Marketo. Il s’agit de l’interface recommandée pour les cas d’utilisation qui nécessitent un échange continu de données entre Marketo et un ou plusieurs systèmes externes, à des fins d’ETL, d’entrepôt de données et d’archivage.

## Permissions

Les API Bulk Program Member Extract exigent que l’utilisateur de l’API propriétaire dispose d’un rôle avec l’une des autorisations de piste en lecture seule ou de piste en lecture-écriture, ou les deux.

## Description

[Description du membre du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) est la principale source de vérité pour savoir si des champs sont disponibles et les métadonnées concernant ces champs. La variable `name` contient le nom de l’API REST.

```
GET /rest/v1/programs/members/describe.json
```

```json
{
    "requestId": "f813#1791563c7cc",
    "result": [
        {
            "name": "API Program Membership",
            "description": "Map for API program membership fields",
            "createdAt": "2021-03-20T01:30:05Z",
            "updatedAt": "2021-03-20T01:30:05Z",
            "dedupeFields": [
                "leadId",
                "programId"
            ],
            "searchableFields": [
                [
                    "leadId"
                ],
                [
                    "myCustomField"
                ],
                [
                    "reachedSuccess"
                ],
                [
                    "statusName"
                ]
            ],
            "fields": [
                {
                    "name": "acquiredBy",
                    "displayName": "acquiredBy",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "attendanceLikelihood",
                    "displayName": "attendanceLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "createdAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "isExhausted",
                    "displayName": "isExhausted",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "leadId",
                    "displayName": "leadId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "membershipDate",
                    "displayName": "membershipDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "nurtureCadence",
                    "displayName": "nurtureCadence",
                    "dataType": "string",
                    "length": 4,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "program",
                    "displayName": "program",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "programId",
                    "displayName": "programId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccess",
                    "displayName": "reachedSuccess",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccessDate",
                    "displayName": "reachedSuccessDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "registrationLikelihood",
                    "displayName": "registrationLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusName",
                    "displayName": "statusName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusReason",
                    "displayName": "statusReason",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "trackName",
                    "displayName": "trackName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "updatedAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "waitlistPriority",
                    "displayName": "waitlistPriority",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "myCustomField",
                    "displayName": "myCustomField",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "registrationCode",
                    "displayName": "registrationCode",
                    "dataType": "string",
                    "length": 100,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "webinarUrl",
                    "displayName": "webinarUrl",
                    "dataType": "string",
                    "length": 2000,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

## Filtres

Les membres du programme prennent en charge diverses options de filtre. Plusieurs types de filtre peuvent être spécifiés pour une tâche, auquel cas ils sont associés à l’opérateur AND. Vous devez spécifier le `programId` ou le `programIds` filtre. Tous les autres filtres sont facultatifs. La variable `updatedAt` Le filtre nécessite des composants d’infrastructure supplémentaires qui n’ont pas encore été déployés pour tous les abonnements.

<table>
  <tbody>
    <tr>
      <td>Type de filtre</td>
      <td>Type de données</td>
      <td>Notes</td>
    </tr>
    <tr>
      <td>programId</td>
      <td>Entier</td>
      <td>Accepte l’identifiant d’un programme. Les tâches renvoient tous les enregistrements accessibles qui sont membres du programme au moment où le traitement de la tâche commence. Récupérez les identifiants de programme à l’aide de la fonction <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Obtention de programmes</a> endpoint.Ne peut pas être utilisé avec le filtre programIds.</td>
    </tr>
    <tr>
      <td>programIds</td>
      <td>Array[Integer]</td>
      <td>Accepte un tableau de 10 identifiants de programme au maximum. Les tâches renvoient tous les enregistrements accessibles qui sont membres des programmes au moment où le traitement de la tâche commence. Un champ supplémentaire "programId" est ajouté au fichier d'export en tant que premier champ. Ce champ identifie le programme à partir duquel un enregistrement d’appartenance à un programme a été extrait. Récupérez les identifiants de programme à l’aide de la variable <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Obtention de programmes</a> endpoint.Ne peut pas être utilisé avec le filtre programId.</td>
    </tr>
    <tr>
      <td>isExhausted</td>
      <td>Booléenne</td>
      <td>Accepte une valeur booléenne utilisée pour filtrer les enregistrements d’adhésion au programme pour <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content">les personnes qui ont épuisé le contenu ;</a>.</td>
    </tr>
    <tr>
      <td>nourtureCadence</td>
      <td>Chaîne</td>
      <td>Accepte une chaîne utilisée pour filtrer les enregistrements d’appartenance à un programme pour une cadence de culture donnée. Les valeurs valides sont :
        <ul>
          <li>pause : mise en pause de la cadence</li>
          <li>standard - cadence est normale</li>
        </ul></td>
    </tr>
    <tr>
      <td>statusNames</td>
      <td>Array[String]</td>
      <td>Accepte un tableau de noms d’état des membres du programme. Plusieurs noms d’état sont OUed ensemble. Les tâches de ce type de filtre renvoient tous les enregistrements accessibles dont l’état de membre du programme correspond à l’un des noms d’état spécifiés. Les noms d’état par défaut et définis par l’utilisateur peuvent être utilisés. Si le filtre statusNames est utilisé avec le filtre "programIds", chaque programme est analysé pour les enregistrements d’appartenance dont l’état correspond à l’un des noms d’état. Si un nom d’état est introuvable dans l’un des programmes, l’erreur "1003, Invalid Data" est renvoyée.
        <table>
          <tbody>
            <tr>
              <td>A participé</td>
              <td>Participation à la demande</td>
              <td>Renvoi</td>
            </tr>
            <tr>
              <td>Cliqué</td>
              <td>Contacté</td>
              <td>Converti</td>
            </tr>
            <tr>
              <td>Engagé</td>
              <td>Formulaire rempli</td>
              <td>Influencé</td>
            </tr>
            <tr>
              <td>Invité</td>
              <td>Membre</td>
              <td>Ne s'est pas présenté</td>
            </tr>
            <tr>
              <td>Non inclus dans le programme</td>
              <td>Sur liste</td>
              <td>Ouvert</td>
            </tr>
            <tr>
              <td>Inscrit</td>
              <td>Enregistrement</td>
              <td>Erreur d'inscription</td>
            </tr>
            <tr>
              <td>Envoyé</td>
              <td>Abonné</td>
              <td>Désabonné</td>
            </tr>
            <tr>
              <td>Affiché</td>
              <td>Visité</td>
              <td>Kiosque visité</td>
            </tr>
            <tr>
              <td>Sur liste d’attente</td>
              <td>Contenu Web</td>
              <td></td>
            </tr>
          </tbody>
        </table></td>
    </tr>
    <tr>
      <td>updatedAt*</td>
      <td>Plage de dates</td>
      <td>Accepte un objet JSON avec les membres startAt et endAt. startAt accepte une date et une heure représentant le filigrane faible, et endAt accepte une date et une heure représentant le filigrane élevé. La période doit être de 31 jours ou moins. Les heures de données doivent être au format ISO-8601, sans millisecondes. Les tâches de ce type de filtre renvoient tous les enregistrements accessibles qui ont été mis à jour le plus récemment au cours de la période.</td>
    </tr>
  </tbody>
</table>

Le type de filtre n’est pas disponible pour certains abonnements. En cas d’indisponibilité de votre abonnement, vous recevez une erreur lors de l’appel du point de terminaison Create Export Program Member Job (&quot;1035, type de filtre non pris en charge pour l’abonnement cible&quot;). Les clients peuvent contacter le support Marketo pour que cette fonctionnalité soit activée dans leur abonnement.

## Options

Le point de fin Create Export Program Member Job fournit plusieurs options de formatage. Ces options permettent à l’utilisateur de :

- Spécifier les champs à inclure dans le fichier exporté
- Renommer les en-têtes de colonne de ces champs
- Définition du format du fichier exporté

| Paramètre | Type de données | Requis | Notes |
|---|---|---|---|
| Champs | Tableau[Chaîne] | Oui | Le paramètre fields accepte un tableau JSON de chaînes. Les champs répertoriés sont inclus dans le fichier exporté. Les types de champ suivants peuvent être exportés :`LeadCustom` `LeadProgram` MemberCustom `ProgramMember`. Spécifiez un champ à l’aide de son nom d’API REST qui peut être récupéré à l’aide des points de terminaison Description Lead2 et/ou Description des membres de programme . |
| columnHeaderNames | Objet | Non | Objet JSON contenant des paires clé-valeur de noms d’en-tête de champ et de colonne. La clé doit être le nom d’un champ inclus dans la tâche d’exportation. La valeur est le nom de l’en-tête de colonne exporté pour ce champ. |
| format | Chaîne | Non | Accepte l’un des paramètres suivants : CSV, TSV, SSV. Le fichier exporté est rendu sous la forme d’un fichier de valeurs séparées par des virgules, de valeurs séparées par des tabulations ou de valeurs séparées par des espaces, respectivement, s’il est défini. La valeur par défaut est CSV si elle n’est pas définie. |


## Création d’une tâche

Les paramètres de la tâche sont définis avant le démarrage de l’exportation à l’aide de la fonction [Création d’une tâche de membre de programme d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/createExportProgramMembersUsingPOST) point de terminaison . Nous devons définir la variable `filter` contenant l’identifiant de programme, et la variable `fields` qui sont nécessaires à l’exportation. En option, nous pouvons définir la variable `format` du fichier, et l’événement `columnHeaderNames`.

```
POST /bulk/v1/program/members/export/create.json
```

```json
{ 
   "format": "CSV",
   "fields": [ 
        "firstName",
        "lastName",
        "email",
        "membershipDate",
        "program",
        "statusName",
        "leadId",
        "reachedSuccess",
        "leadCustomField01",
        "leadCustomField02",
        "pMCustomField01",
        "pMCustomField02"
   ],
   "filter": { 
      "programId":1044
   }
}
```

```json
{
    "requestId": "4d44#16f92734f6e",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Created",
            "createdAt": "2020-01-11T02:33:48Z"
        }
    ],
    "success": true
}
```

Cela renvoie une réponse d’état indiquant que la tâche a été créée. La tâche a été définie et créée, mais elle n’a pas encore été lancée. Pour ce faire, la variable [Traitement membre du programme d’exportation d’enfiles](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/enqueueExportProgramMembersUsingPOST) endpoint doit être appelé à l’aide de la fonction `exportId` de la réponse d’état de création :

```
POST /bulk/v1/program/members/export/{exportId}/enqueue.json
```

```json
{
    "requestId": "d70b#16f9273ae32",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z"
        }
    ],
    "success": true
}
```

Cela répond avec une `status` de &quot;En file d’attente&quot;, après quoi il sera défini sur &quot;Traitement&quot; lorsqu’un emplacement d’exportation est disponible.

## État de la tâche d’interrogation

Remarque : l’état ne peut être récupéré que pour les tâches créées par le même utilisateur de l’API.

Puisqu’il s’agit d’un point de terminaison asynchrone, après la création de la tâche, nous devons interroger son état pour déterminer sa progression. Sondage à l’aide du [Obtenir l’état de la tâche des membres du programme d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET) point de terminaison . Le statut n’est mis à jour qu’une fois toutes les 60 secondes, donc une fréquence d’interrogation inférieure à celle-ci n’est pas recommandée, et dans presque tous les cas est toujours excessive. Le champ d’état peut répondre à l’un des types suivants : Créé, En file d’attente, En cours de traitement, Annulé, Terminé, Échec.

```
GET /bulk/v1/program/members/export/{exportId}/status.json
```

```json
{
    "requestId": "9a40#16f9274d250",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z",
            "startedAt": "2020-01-11T02:35:19Z"
        }
    ],
    "success": true
}
```

Le point de terminaison d’état répond indiquant que la tâche est toujours en cours de traitement. Le fichier n’est donc pas encore disponible pour la récupération. Une fois la tâche `status` Les modifications apportées à &quot;Terminé&quot; peuvent être téléchargées.

```json
{
    "requestId": "11ad1#16f9ff6da23",
    "result": [
        {
            "exportId": "1118dc83-273b-4d44-becb-4d212fece550",
            "format": "CSV",
            "status": "Completed",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z",
            "startedAt": "2020-01-11T02:35:19Z"
            "finishedAt": "2020-01-11T02:36:12Z",
            "numberOfRecords": 13,
            "fileSize": 1752,
            "fileChecksum": "sha256:b3c8e70e6e501cf1025e345a66b409d4fd07364c7da773cfa68a2b68ce1a7212"
        }
    ],
    "success": true
}
```

## Récupération de vos données

Pour récupérer le fichier d’une exportation de membre de programme terminée, appelez simplement le [Obtenir le fichier de membre du programme d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/getExportProgramMembersFileUsingGET) Point de terminaison avec votre `exportId`.

La réponse contient un fichier formaté selon la configuration de la tâche. Le point de terminaison répond avec le contenu du fichier. Si un champ de membre de programme demandé est vide (ne contient aucune donnée), `null` est placé dans le champ correspondant du fichier d&#39;export.

```
GET /bulk/v1/program/members/export/{exportId}/file.json
```

```
firstName,lastName,email,Member Date,Program,Status,Lead Id,Success,leadCustomField01,leadCustomField02,pMCustomField01,pMCustomField02
Meera,Reed,mree@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1789,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jon,Umber,jumb@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1790,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Lyanna,Mormont,lmor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1791,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rickon,Stark,rsta@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1792,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Hodor,null,hodor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1793,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Osha,null,osha@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1794,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jojen,Reed,Jree@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1795,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rickard,Karstark,rkar@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1796,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Maester,Luwin,mluw@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1797,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rodrik,Cassel,rcas@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1798,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jory,Cassel,jcas@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1799,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Septa,Mordane,smor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1800,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
```

Pour prendre en charge la récupération partielle et conviviale en cas de reprise des données extraites, le point de fin de fichier prend éventuellement en charge la plage d’en-tête HTTP du type bytes. Si l’en-tête n’est pas défini, l’ensemble du contenu est renvoyé. Vous pouvez en savoir plus sur l’utilisation de l’en-tête Plage avec Marketo [Extraction en bloc](bulk-extract.md).

## Annulation d’une tâche

Si une tâche a été mal configurée ou devient inutile, elle peut être facilement annulée à l’aide de la fonction [Annuler l’exportation de la tâche membre du programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/cancelExportProgramMembersUsingPOST) endpoint :

```
POST /bulk/v1/program/members/export/{exportId}/cancel.json
```

```json
{
    "requestId": "bb4f#16f86727f89",
    "result": [
        {
            "exportId": "f0d3520c-3a60-4568-9e71-2e619d3805a4",
            "format": "CSV",
            "status": "Cancelled",
            "createdAt": "2020-01-07T21:47:35Z"
        }
    ],
    "success": true
}
```

Cette fonction répond par une `status` indiquant que la tâche a été annulée.
