---
title: Extraction de membre de programme en bloc
feature: REST API
description: Traitement par lots de l’extraction des données des membres.
exl-id: 6e0a6bab-2807-429d-9c91-245076a34680
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '1142'
ht-degree: 5%

---

# Extraction de membre de programme en bloc

[Référence de point d’entrée d’extraction de membre de programme en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members)

L’ensemble d’API REST d’extraction de membres de programme en bloc fournit une interface de programmation pour récupérer de grands ensembles d’enregistrements de membres de programme en dehors de Marketo. Il s’agit de l’interface recommandée pour les cas d’utilisation qui nécessitent un échange continu de données entre Marketo et un ou plusieurs systèmes externes, à des fins d’ETL, d’entreposage de données et d’archivage.

## Autorisations

Les API Bulk Program Member Extract nécessitent que l&#39;utilisateur propriétaire de l&#39;API dispose d&#39;un rôle avec l&#39;une ou l&#39;autre des autorisations Read-Only Lead (Lead en lecture seule) ou Read-Write Lead (Lead en lecture-écriture).

## Décrire

[Décrire le membre de programme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) est la principale source de vérité pour savoir si les champs sont disponibles et pour connaître les métadonnées sur ces champs. L’attribut `name` contient le nom de l’API REST.

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

Les membres du programme prennent en charge diverses options de filtre. Plusieurs types de filtre peuvent être spécifiés pour une tâche. Dans ce cas, ils sont ET combinés. Vous devez spécifier le filtre `programId` ou `programIds`. Tous les autres filtres sont facultatifs. Le filtre `updatedAt` nécessite des composants d’infrastructure supplémentaires qui n’ont pas encore été déployés sur tous les abonnements.

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
      <td>Accepte l’identifiant d’un programme. Les tâches renvoient tous les enregistrements accessibles qui sont membres du programme au moment où la tâche commence le traitement.Récupérez les ID de programme à l’aide du point d’entrée <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Obtenir les programmes</a>. Ne peut pas être utilisé avec le filtre programIds.</td>
    </tr>
    <tr>
      <td>programIds</td>
      <td>Tableau[Entier]</td>
      <td>Accepte un tableau contenant jusqu’à 10 identifiants de programme. Les traitements renvoient tous les enregistrements accessibles qui sont membres des programmes au moment où le traitement commence. Un champ supplémentaire « programId » est ajouté au fichier d’exportation en tant que premier champ. Ce champ identifie le programme à partir duquel un enregistrement d’appartenance à un programme a été extrait. Récupérez les ID de programme à l’aide du point d’entrée <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Obtenir les programmes</a>. Ne peut pas être utilisé avec le filtre programId.</td>
    </tr>
    <tr>
      <td>isExhausted</td>
      <td>Booléen</td>
      <td>Accepte une valeur booléenne utilisée pour filtrer les enregistrements d’adhésion au programme pour <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content">les personnes qui ont épuisé le contenu</a>.</td>
    </tr>
    <tr>
      <td>nurtureCadence</td>
      <td>Chaîne</td>
      <td>Accepte une chaîne utilisée pour filtrer les enregistrements d’appartenance à un programme pour une cadence d’apprentissage donnée. Les valeurs autorisées sont les suivantes :
        <ul>
          <li>pause - le rythme est suspendu</li>
          <li>norme - cadence normale</li>
        </ul></td>
    </tr>
    <tr>
      <td>statusNames</td>
      <td>Array[String]</td>
      <td>Accepte un tableau de noms de statut des membres du programme. Plusieurs noms de statut sont OU combinés. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles dont le statut de membre du programme correspond à l’un des noms de statut spécifiés. Il est possible d’utiliser à la fois les noms de statut par défaut et les noms de statut définis par l’utilisateur. Si le filtre statusNames est utilisé avec le filtre « programIds », chaque programme est vérifié pour rechercher les enregistrements d’appartenance dont le statut correspond à l’un des noms de statut. Si aucun programme ne trouve un nom de statut, l’erreur « 1003, Données non valides » est renvoyée.
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
      <td>Période</td>
      <td>Accepte un objet JSON avec les membres startAt et endAt. startAt accepte une valeur datetime représentant le filigrane inférieur et endAt accepte une valeur datetime représentant le filigrane supérieur. La plage doit être de 31 jours ou moins. Les heures de date doivent être au format ISO-8601, sans millisecondes. Les tâches avec ce type de filtre renvoient tous les enregistrements accessibles qui ont été mis à jour le plus récemment pendant la période.</td>
    </tr>
  </tbody>
</table>

Le type de filtre n’est pas disponible pour certains abonnements. Si vous n’êtes pas disponible pour votre abonnement, vous recevez une erreur lors de l’appel du point d’entrée Créer une tâche de membre du programme d’exportation (« 1035, Type de filtre non pris en charge pour l’abonnement cible »). Les clients peuvent contacter l’assistance Marketo pour que cette fonctionnalité soit activée dans leur abonnement.

## Options

Le point d’entrée Créer une tâche de membre du programme d’exportation fournit plusieurs options de formatage. Ces options permettent à l’utilisateur ou à l’utilisatrice de :

- Spécifier les champs à inclure dans le fichier exporté
- Renommer les en-têtes de colonne de ces champs
- Spécifier le format du fichier exporté

| Paramètre | Type de données | Obligatoire | Notes |
|---|---|---|---|
| Champs | Array[String] | Oui | Le paramètre fields accepte un tableau JSON de chaînes. Les champs répertoriés sont inclus dans le fichier exporté. Les types de champs suivants peuvent être exportés :`LeadCustom` `LeadProgram` MemberCustom `ProgramMember`. Spécifiez un champ à l’aide de son nom d’API REST qui peut être récupéré à l’aide des points d’entrée Décrire le lead2 et/ou Décrire le membre de programme . |
| columnHeaderNames | Objet | Non | Un objet JSON contenant des paires clé-valeur de noms d’en-tête de champ et de colonne. La clé doit être le nom d’un champ inclus dans la tâche d’exportation. La valeur est le nom de l’en-tête de colonne exporté pour ce champ. |
| format | Chaîne | Non | Accepte l’un des formats suivants : CSV, TSV, SSV. Le fichier exporté est rendu sous la forme d’un fichier de valeurs séparées par des virgules, des valeurs séparées par des tabulations ou des valeurs séparées par des espaces, respectivement, s’il est défini. La valeur par défaut est CSV si cette valeur n’est pas définie. |

## Création d’un traitement

Les paramètres de la tâche sont définis avant le lancement de l’exportation à l’aide du point d’entrée [Créer une tâche de membre du programme d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/createExportProgramMembersUsingPOST). Nous devons définir les `filter` contenant l’ID du programme et les `fields` nécessaires à l’exportation. En option, nous pouvons définir le `format` du fichier et le `columnHeaderNames`.

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

Cette opération renvoie une réponse de statut indiquant que la tâche a été créée. La tâche a été définie et créée, mais elle n&#39;a pas encore été lancée. Pour ce faire, le point d’entrée [Mettre en file d’attente la tâche du membre du programme d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/enqueueExportProgramMembersUsingPOST) doit être appelé à l’aide du `exportId` de la réponse de statut de création :

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

Cela répondra avec une `status` initiale de « Mise en file d’attente », après quoi cela sera défini sur « Traitement » lorsqu’un emplacement d’exportation sera disponible.

## Interroger le statut de la tâche

Remarque : le statut ne peut être récupéré que pour les tâches créées par le même utilisateur de l’API.

Puisqu’il s’agit d’un point d’entrée asynchrone, après avoir créé la tâche, nous devons interroger son statut pour déterminer sa progression. Effectuez une enquête à l’aide du point d’entrée [Obtenir le statut de la tâche du membre du programme d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET). Le statut n’est mis à jour qu’une fois toutes les 60 secondes. Il est donc déconseillé d’utiliser une fréquence d’interrogation inférieure à cette fréquence, qui reste excessive dans presque tous les cas. Le champ de statut peut répondre avec l’une des options suivantes : Créé, En file d’attente, Traitement, Annulé, Terminé, En échec.

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

Le point d’entrée de statut répond indiquant que la tâche est toujours en cours de traitement et que le fichier n’est donc pas encore disponible pour récupération. Une fois que le `status` de traitement est remplacé par « Terminé », il peut être téléchargé.

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

Pour récupérer le fichier d&#39;une exportation de membre de programme terminée, appelez simplement le point d&#39;entrée [Obtenir le fichier de membre de programme d&#39;exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/getExportProgramMembersFileUsingGET) avec votre `exportId`.

La réponse contient un fichier formaté selon la configuration de la tâche. Le point d’entrée répond avec le contenu du fichier . Si un champ de membre de programme demandé est vide (ne contient aucune donnée), `null` est placé dans le champ correspondant dans le fichier d’exportation.

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

Pour prendre en charge la récupération partielle et conviviale des données extraites, le point d’entrée du fichier prend éventuellement en charge la plage d’en-têtes HTTP du type octets. Si l’en-tête n’est pas défini, l’intégralité du contenu est renvoyée. Vous pouvez en savoir plus sur l’utilisation de l’en-tête de plage avec Marketo [Extraction en bloc](bulk-extract.md).

## Annulation d’un traitement

Si une tâche n’a pas été configurée correctement ou devient inutile, elle peut facilement être annulée à l’aide du point d’entrée [Annuler la tâche du membre du programme d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/cancelExportProgramMembersUsingPOST) :

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

Cette opération répond avec un `status` indiquant que la tâche a été annulée.
