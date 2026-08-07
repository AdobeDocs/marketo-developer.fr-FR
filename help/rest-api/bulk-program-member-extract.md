---
title: Extraction de membre de programme en bloc
feature: REST API
description: Utilisez les API REST d’extraction de membre de programme en bloc Marketo pour exporter des enregistrements de membre volumineux pour ETL, l’entreposage de données et l’archivage, avec des autorisations et des métadonnées de champ.
exl-id: 6e0a6bab-2807-429d-9c91-245076a34680
TQID: https://experienceleague.adobe.com/w4qaVTKSe0EORaSiURB6WbJXi29JUdEgfkb2dnfuVFw
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 1081
ht-degree: 5%

---

# Extraction de membre de programme en bloc

[Référence de point d’entrée d’extraction de membre de programme en bloc](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members)

Les API Bulk Program Member Extract récupèrent de grands ensembles d’enregistrements de membres de programme à partir de Marketo. Utilisez ces API pour un échange de données continu entre Marketo et des systèmes externes, ETL, Data Warehouse et l’archivage.

## Autorisations

L’utilisateur de l’API doit disposer d’un rôle avec l’autorisation Lead en lecture seule, l’autorisation Lead en lecture-écriture ou les deux.

## Décrire

Utilisez [Décrire le membre de programme](https://developer.adobe.com/marketo-apis/api/mapi#operation/describeProgramMemberUsingGET2) pour déterminer les champs disponibles et récupérer leurs métadonnées. L’attribut `name` contient le nom du champ API REST.

```http
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

Les exportations de membres de programme prennent en charge plusieurs options de filtre. Lorsqu’une tâche spécifie plusieurs types de filtre, l’API les combine avec une opération AND.

Chaque traitement doit spécifier `programId` ou `programIds`. Tous les autres filtres sont facultatifs. Le filtre `updatedAt` nécessite une infrastructure qui n&#39;est pas disponible dans tous les abonnements.

<table>
  <tbody>
    <tr>
      <td>Type de filtre</td>
      <td>Type de données</td>
      <td>Notes</td>
    </tr>
    <tr>
      <td>programId</td>
      <td>Nombre entier</td>
      <td>Accepte l’identifiant d’un programme. Les traitements renvoient tous les enregistrements accessibles qui sont membres du programme au moment où le traitement du traitement commence.Récupérez les identifiants de programme à l’aide du point d’entrée <a href="https://developer.adobe.com/marketo-apis/api/asset#tag/Programs">Obtenir les programmes</a>.Ne peut pas être utilisé avec le filtre programIds.</td>
    </tr>
    <tr>
      <td>programIds</td>
      <td>Tableau[Entier]</td>
      <td>Accepte un tableau contenant jusqu’à 10 identifiants de programme. Les traitements renvoient tous les enregistrements accessibles qui sont membres des programmes au moment où le traitement du traitement commence.Un champ supplémentaire « programId » est ajouté au fichier d’exportation en tant que premier champ. Ce champ identifie le programme à partir duquel un enregistrement d’adhésion au programme a été extrait.Récupérez les identifiants de programme à l’aide du point d’entrée <a href="https://developer.adobe.com/marketo-apis/api/asset#tag/Programs">Obtenir les programmes</a>.Ne peut pas être utilisé avec le filtre programId.</td>
    </tr>
    <tr>
      <td>isExhausted</td>
      <td>Booléen</td>
      <td>Accepte une valeur booléenne utilisée pour filtrer les enregistrements d’adhésion au programme pour <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content">les personnes qui ont épuisé le contenu</a>.</td>
    </tr>
    <tr>
      <td>nurtureCadence</td>
      <td>Chaîne</td>
      <td>Accepte une chaîne utilisée pour filtrer les enregistrements d’appartenance à un programme pour une cadence d’apprentissage donnée.Les valeurs autorisées sont les suivantes :
        <ul>
          <li>pause - le rythme est suspendu</li>
          <li>norme - cadence normale</li>
        </ul></td>
    </tr>
    <tr>
      <td>statusNames</td>
      <td>Array[String]</td>
      <td>Accepte un tableau de noms de statut des membres du programme. Plusieurs noms de statut sont regroupés en OR.Les traitements avec ce type de filtre renvoient tous les enregistrements accessibles dont le statut de membre du programme correspond à l’un des noms de statut spécifiés. Vous pouvez utiliser les noms de statut par défaut et définis par l’utilisateur.Si le filtre statusNames est utilisé avec le filtre « programIds », chaque programme est vérifié pour rechercher les enregistrements d’abonnement dont le statut correspond à l’un des noms de statut. Si aucun programme ne trouve un nom de statut, l’erreur « 1003, Données non valides » est renvoyée.
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
              <td>Désabonné ou désabonnée</td>
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
      <td>Période</td>
      <td>Accepte un objet JSON avec les membres startAt et endAt. startAt accepte une valeur datetime représentant le filigrane inférieur et endAt accepte une valeur datetime représentant le filigrane supérieur. La plage doit être de 31 jours ou moins. Les heures de date doivent être au format ISO-8601, sans millisecondes.Les traitements avec ce type de filtre renvoient tous les enregistrements accessibles qui ont été mis à jour le plus récemment au cours de la période.</td>
    </tr>
  </tbody>
</table>

Certains abonnements ne prennent pas en charge ce type de filtre. S’il n’est pas disponible, le point d’entrée Créer une tâche membre du programme d’exportation renvoie `1035, Unsupported filter type for target subscription`. Contactez l’assistance Marketo pour demander cette fonctionnalité pour votre abonnement.

## Options

Le point d’entrée Créer une tâche de membre du programme d’exportation fournit les options suivantes :

- Spécifiez les champs à inclure dans le fichier d’exportation.
- Renommez les en-têtes de colonne exportés.
- Spécifiez le format du fichier d’exportation.

| Paramètre | Type de données | Obligatoire | Notes |
| --- | --- | --- | --- |
| Champs | Array[String] | Oui | Le paramètre fields accepte un tableau JSON de chaînes. Les champs répertoriés sont inclus dans le fichier exporté. Les types de champs suivants peuvent être exportés :`LeadCustom` `LeadProgram` MemberCustom `ProgramMember`. Spécifiez un champ en utilisant son nom d’API REST qui peut être récupéré à l’aide des points d’entrée Décrire le lead2 et/ou Décrire le membre de programme . |
| columnHeaderNames | Objet | Non | Un objet JSON contenant des paires clé-valeur de noms d’en-tête de champ et de colonne. La clé doit être le nom d’un champ inclus dans la tâche d’exportation. La valeur est le nom de l’en-tête de colonne exporté pour ce champ. |
| format | Chaîne | Non | Accepte l’un des formats suivants : CSV, TSV, SSV. Le fichier exporté est rendu sous la forme d’un fichier de valeurs séparées par des virgules, des valeurs séparées par des tabulations ou des valeurs séparées par des espaces, respectivement, s’il est défini. La valeur par défaut est CSV si cette valeur n’est pas définie. |

## Création d’un traitement

Utilisez le point d’entrée [Créer une tâche de membre du programme d’exportation](https://developer.adobe.com/marketo-apis/api/mapi#operation/createExportProgramMembersUsingPOST) pour définir la tâche d’exportation. Spécifiez un `filter` contenant l’ID du programme et le `fields` à exporter. Vous pouvez également spécifier des `format` et des `columnHeaderNames`.

```http
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

La réponse confirme la création du traitement, mais le démarrage de l’exportation n’est pas automatique. Transmettez le `exportId` renvoyé au point d’entrée [Mettre en file d’attente la tâche du membre du programme d’exportation](https://developer.adobe.com/marketo-apis/api/mapi#operation/enqueueExportProgramMembersUsingPOST) pour démarrer la tâche :

```http
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

La réponse mise en file d&#39;attente renvoie initialement un statut `Queued`. Lorsqu’un emplacement d’exportation devient disponible, le statut passe à `Processing`.

## Interroger le statut de la tâche

Vous ne pouvez récupérer le statut que pour les tâches créées par le même utilisateur de l’API.

Comme l’exportation s’exécute de manière asynchrone, utilisez le point d’entrée [Obtenir le statut de la tâche du membre du programme d’exportation](https://developer.adobe.com/marketo-apis/api/mapi#operation/getExportLeadsStatusUsingGET) pour interroger sa progression. Le statut n’est mis à jour qu’une fois toutes les 60 secondes. N’effectuez donc pas d’interrogations plus fréquentes.

Le statut peut être `Created`, `Queued`, `Processing`, `Canceled`, `Completed` ou `Failed`.

```http
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

Cette réponse indique que la tâche est toujours en cours de traitement et que le fichier n’est donc pas disponible. Lorsque le statut de la tâche passe à `Completed`, le fichier est prêt à être téléchargé.

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

Pour récupérer une exportation de membre de programme terminée, transmettez le `exportId` au point d&#39;entrée [Obtenir le fichier de membre de programme d&#39;exportation](https://developer.adobe.com/marketo-apis/api/mapi#operation/getExportProgramMembersFileUsingGET).

Le point d’entrée renvoie le fichier au format configuré pour la tâche. Si un champ de membre de programme demandé ne contient aucune donnée, le champ d’exportation correspondant contient `null`.

```http
GET /bulk/v1/program/members/export/{exportId}/file.json
```

```text
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

Pour une récupération partielle ou pouvant être reprise, le point d’entrée du fichier prend en charge l’en-tête `Range` HTTP facultatif avec un type de plage de `bytes`. Si vous ne définissez pas l’en-tête , le point d’entrée renvoie l’intégralité du fichier. Pour plus d’informations, voir [ Extraction en bloc ](bulk-extract.md).

## Annulation d’un traitement

Pour annuler une tâche mal configurée ou qui n’est plus nécessaire, appelez le point d’entrée [ Annuler la tâche du membre du programme d’exportation ](https://developer.adobe.com/marketo-apis/api/mapi#operation/cancelExportProgramMembersUsingPOST) :

```http
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

Le statut de la réponse indique que le traitement est annulé.
