---
title: Prospects
feature: REST API
description: Explorez les fonctionnalités de l’API REST des leads Marketo, notamment la description, la requête par ID ou filtre, les champs par défaut, les limites et la récupération des ECID.
exl-id: 0a2f7c38-02ae-4d97-acfe-9dd108a1f733
TQID: https://experienceleague.adobe.com/jZ-ecWTmHwq9gvp4fMaeuuGba6cgwYx0QCCyfkrEDHQ
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 2728
ht-degree: 3%

---

# Prospects

[Référence du point d’entrée des leads](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads)

L’API Leads de Marketo prend en charge les opérations CRUD sur les enregistrements de leads. Vous pouvez également modifier l’appartenance d’un prospect à des listes et programmes statiques et lancer le traitement Smart Campaign pour les prospects.

## Décrire

Utilisez Décrire les prospects pour récupérer les champs disponibles par le biais de l’API REST et les métadonnées de chaque champ :

- Type de données
- Nom de l’API REST
- Longueur, le cas échéant
- Statut en lecture seule
- Libellé convivial

Describe est la principale source de vérité pour la disponibilité des champs et des métadonnées.

### Requête

```http
GET /rest/v1/leads/describe.json
```

### Réponse

```json
{
   "requestId":"37ca#1475b74e276",
   "success":true,
   "result":[
      {
         "id":2,
         "displayName":"Company Name",
         "dataType":"string",
         "length":255,
         "rest":{
            "name":"company",
            "readOnly":false
         },
         "soap":{
            "name":"Company",
            "readOnly":false
         }
      }
}
```

Les réponses réelles incluent davantage de champs dans le tableau de résultats. Chaque élément représente un champ disponible dans l’enregistrement de prospect et contient au moins un id, un displayName et un type de données.

Les objets enfants rest et soap n’apparaissent que lorsque le champ est valide pour l’API correspondante. La propriété `readOnly` indique si l’API correspondante peut mettre à jour le champ. Lorsqu’elle est présente, la propriété length indique la longueur maximale du champ, et la propriété dataType indique le type de données du champ.

## Requête

Utilisez l’une des deux méthodes principales suivantes pour récupérer les prospects :

- L’option Obtenir le lead par ID utilise un ID de lead comme paramètre de chemin d’accès et renvoie un enregistrement de lead.
- L’option Obtenir les prospects par type de filtre recherche les enregistrements dont le champ sélectionné correspond à l’une des valeurs fournies.

Pour Obtenir le prospect par ID, transmettez éventuellement un paramètre de champ avec une liste de noms de champs séparés par des virgules à renvoyer. Si la requête omet des champs, la réponse inclut `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` et `id`. Si un champ demandé n’est pas renvoyé, sa valeur est implicitement nulle.

### Requête

```http
GET /rest/v1/lead/{id}.json
```

### Réponse

```json
{
   "requestId": "10226#14d3049e51b",
   "success": true,
   "result": [
      {
         "id": 318581,
         "updatedAt":"2015-05-07T11:47:30-08:00"
         "lastName": "Doe",
         "email": "jdoe@marketo.com",
         "createdAt": "2015-05-01T16:47:30-08:00",
         "firstName": "John"
      }
   ]
}
```

L’option Obtenir le prospect par ID renvoie toujours un enregistrement à la première position du tableau de résultats.

L’option Obtenir les prospects par type de filtre renvoie le même type d’enregistrement et peut renvoyer jusqu’à 300 enregistrements par page. Les paramètres de requête `filterType` et `filterValues` sont requis.

`filterType` accepte n’importe quel champ personnalisé et les champs les plus couramment utilisés. Appelez le point d’entrée `Describe2` pour récupérer les champs pouvant faire l’objet d’une recherche autorisés pour `filterType`. Lors d’une recherche par champ personnalisé, les types de données pris en charge sont `string`, `email` et `integer`. Utilisez la méthode Describe pour récupérer les détails du champ, tels que la description et le type.

`filterValues` accepte jusqu’à 300 valeurs séparées par des virgules. L’appel renvoie les enregistrements pour lesquels le champ de prospect sélectionné correspond à l’une de ces valeurs. Si plus de 1 000 prospects correspondent au filtre, l’API renvoie « 1 003, trop de résultats correspondent au filtre ».

Si le nombre total de requêtes GET dépasse 8 Ko, l’API renvoie « 414, URI too long » sous RFC 7231. Pour contourner cette limite, modifiez GET en POST, ajoutez le paramètre _method=GET et placez la chaîne de requête dans le corps de la requête.

### Requête

```http
GET /rest/v1/leads.json?filterType=id&filterValues=318581,318592
```

### Réponse

```json
{
    "requestId": "12951#15699db5c97",
    "result": [
        {
            "id": 318581,
            "updatedAt": "2016-05-17T22:11:45Z",
            "lastName": "Lincoln",
            "email": "abe@usa.gov",
            "createdAt": "2015-03-17T00:18:40Z",
            "firstName": "Abraham"
        },
        {
            "id": 318592,
            "updatedAt": "2016-05-17T22:20:51Z",
            "lastName": "Washington",
            "email": "george@usa.gov",
            "createdAt": "2015-04-06T16:29:21Z",
            "firstName": "George"
        }
    ],
    "success": true
}
```

Cet appel renvoie les enregistrements dont les identifiants correspondent aux valeurs de `filterValues`.

Si aucun enregistrement ne correspond, la réponse indique la réussite et contient un tableau de résultats vide.

### Réponse

```json
{
"requestId": "177a1#1578b643357",
"result": [],
"success": true
}
```

Les deux options Obtenir les prospects par ID et Obtenir les prospects par type de filtre acceptent un paramètre de requête de champ contenant une liste de champs API séparés par des virgules. Lorsque des champs sont présents, chaque enregistrement de réponse inclut les champs répertoriés. Si elle est omise, la réponse inclut `id`, `email`, `updatedAt`, `createdAt`, `firstName` et `lastName`.

## ADOBE ECID

Lorsque le partage d’audiences Adobe Experience Cloud est activé, la synchronisation des cookies associe les valeurs d’identifiant Adobe Experience Cloud (ECID) aux prospects Marketo. Pour récupérer les valeurs ECID associées avec les méthodes de récupération de prospect précédentes, incluez `ecids` dans le paramètre fields . Par exemple : `&fields=email,firstName,lastName,ecids`.

## Créer et mettre à jour

L’API Leads peut créer, mettre à jour et supprimer des enregistrements de leads. Les opérations de création et de mise à jour utilisent le même point d’entrée, avec le type d’opération défini dans la requête. Une requête peut créer ou mettre à jour jusqu’à 300 enregistrements.

>[!NOTE]
>
> La mise à jour des champs Société à l’aide du point d’entrée [Leads de synchronisation](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST) n’est pas prise en charge. Utilisez plutôt le point d’entrée [Synchroniser les entreprises](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/syncCompaniesUsingPOST).

>[!NOTE]
>
> Lors de la création ou de la mise à jour de la valeur d’e-mail sur un enregistrement Personne, seuls les caractères ASCII sont pris en charge dans le champ d’adresse e-mail.

### Requête

```http
POST /rest/v1/leads.json
```

### Corps

```json
{
   "action":"createOnly",
   "lookupField":"email",
   "input":[
      {
         "email":"kjashaedd-1@klooblept.com",
         "firstName":"Kataldar-1",
         "postalCode":"04828"
      },
      {
         "email":"kjashaedd-2@klooblept.com",
         "firstName":"Kataldar-2",
         "postalCode":"04828"
      },
      {
         "email":"kjashaedd-3@klooblept.com",
         "firstName":"Kataldar-3",
         "postalCode":"04828"
      }
   ]
}
```

### Réponse

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "id":50,
         "status":"created"
      },
      {
         "id":51,
         "status":"created"
      },
      {
         "id":52,
         "status":"created"
      }
   ]
}
```

La requête utilise deux champs importants :

- `action` indique le type d’opération : `createOrUpdate`, `createOnly`, `updateOnly` ou `createDuplicate`. En cas d’omission, la valeur par défaut est `createOrUpdate`.
- `lookupField` spécifie la clé lorsque l’action est `createOrUpdate` ou `updateOnly`. En cas d’omission, la valeur par défaut est `email`.

Par défaut, l’opération utilise la partition par défaut. Le paramètre de `partitionName` facultatif fonctionne uniquement lorsque l’action est `createOnly` ou `createOrUpdate`. Pour utiliser `partitionName` comme critère de déduplication supplémentaire, incluez-le dans le type de source pour les règles de déduplication personnalisées.

Lors d’une mise à jour, l’API renvoie une erreur si le prospect n’existe pas dans la partition spécifiée ou si l’utilisateur uniquement API ne peut pas accéder à cette partition.

Étant donné qu’`id` est une clé unique gérée par le système, incluez-la uniquement avec l’action `updateOnly`.

La requête doit inclure un paramètre `input` contenant un tableau d’enregistrements de prospect. Chaque enregistrement de prospect est un objet JSON comportant un nombre illimité de champs de prospect. Les clés doivent être uniques dans chaque enregistrement et toutes les chaînes JSON doivent utiliser le codage UTF-8.

Utilisez `externalCompanyId` pour lier un enregistrement de prospect à un enregistrement d’entreprise. Utilisez `externalSalesPersonId` pour lier un enregistrement de prospect à un enregistrement de vendeur.

Des requêtes upsert simultanées ou étroitement synchronisées peuvent créer des enregistrements en double lorsque plusieurs requêtes utilisent la même valeur de clé avant le retour de la première requête. Pour éviter les doublons, utilisez `createOnly` ou `updateOnly` selon vos besoins. Vous pouvez également mettre en file d’attente les appels et attendre que chaque appel revienne avant d’envoyer un autre upsert avec la même clé.

## Champs

L’objet de prospect contient des champs standard et des champs personnalisés facultatifs. Des champs standard existent dans chaque abonnement Marketo Engage, tandis que les utilisateurs créent des champs personnalisés selon leurs besoins.

Chaque définition de champ contient des attributs de métadonnées tels que le nom d’affichage, le nom de l’API et le dataType.

Utilisez les points d’entrée suivants pour interroger, créer et mettre à jour des champs sur l’objet de prospect. Le rôle de l’utilisateur de l’API doit disposer de l’autorisation Champ standard du schéma en lecture-écriture, de l’autorisation Champ personnalisé du schéma en lecture-écriture ou des deux.

## Champs de requête

Interroger un champ de lead par nom d’API ou interroger tous les champs de lead. Selon les autorisations du rôle, la réponse peut inclure des champs standard, des champs personnalisés et des champs masqués.

## Par nom

Le point d’entrée Get Lead Field by Name récupère les métadonnées d’un champ de prospect. Le paramètre de chemin d’accès fieldApiName obligatoire spécifie le nom de l’API du champ.

La réponse ressemble à la réponse Décrire le lead, mais elle comprend des métadonnées supplémentaires. Par exemple, l’attribut isCustom indique si le champ est personnalisé.

### Requête

```http
GET /rest/v1/leads/schema/fields/{fieldApiName}.json
```

### Réponse

```json
{
    "requestId": "cd97#1793ee0fec4",
    "result": [
        {
            "displayName": "Email Address",
            "name": "email",
            "description": null,
            "dataType": "email",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        }
    ],
    "success": true
}
```

## Parcourir

Le point d’entrée Get Lead Fields récupère les métadonnées de tous les champs de l’objet du prospect. Par défaut, elle renvoie un maximum de 300 enregistrements. Utilisez le paramètre de requête `batchSize` pour réduire ce nombre.

Si `moreResult` est vrai, d’autres résultats sont disponibles. Transmettez le `nextPageToken` renvoyé à chaque appel suivant jusqu’à ce que `moreResult` ait la valeur false.

### Requête

```http
GET /rest/v1/leads/schema/fields.json
```

### Réponse (tronquée)

```json
{
    "requestId": "142c3#1793eb976d8",
    "result": [
        {
            "displayName": "Salutation",
            "name": "salutation",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "First Name",
            "name": "firstName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Middle Name",
            "name": "middleName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Last Name",
            "name": "lastName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Date of Birth",
            "name": "dateOfBirth",
            "description": null,
            "dataType": "date",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Email Address",
            "name": "email",
            "description": null,
            "dataType": "email",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Phone Number",
            "name": "phone",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Mobile Phone Number",
            "name": "mobilePhone",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Fax Number",
            "name": "fax",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Job Title",
            "name": "title",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Unsubscribed",
            "name": "unsubscribed",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": true,
            "isCustom": false
        },
        ...
    ],
    "success": true,
    "moreResult": false
}
```

## Créer des champs

Le point d’entrée Créer des champs de prospect crée un ou plusieurs champs personnalisés sur l’objet de prospect et fournit des fonctionnalités comparables à celles de l’interface utilisateur de Marketo Engage. Vous pouvez créer jusqu’à 100 champs personnalisés avec ce point d’entrée.

Examinez attentivement chaque champ avant de le créer dans une instance de production. Une fois un champ créé, vous pouvez le masquer, mais ne pouvez pas le supprimer. Les champs non utilisés encombrent l’instance.

Le paramètre d’entrée requis est un tableau d’objets de champ de prospect. Chaque objet nécessite les attributs suivants :

- `displayName` est le nom d’affichage de l’interface utilisateur du champ.
- `name` est le nom de l’API du champ.
- `dataType` est le type de champ.

Les attributs facultatifs sont `description`, `isHidden`, `isHtmlEncodingInEmail` et `isSensitive`.

L’attribut name doit être unique, commencer par une lettre et contenir uniquement des lettres, des chiffres ou des traits de soulignement. Le `displayName` doit être unique et ne peut pas contenir de caractères spéciaux.

Une convention courante applique la casse mixte aux `displayName` pour produire le nom. Par exemple, une `displayName` de « Mon champ personnalisé » génère le nom « myCustomField ».

### Requête

```http
POST /rest/v1/leads/schema/fields.json
```

### Corps

```json
{
  "input": [
      {
        "displayName": "Acme Access Code",
        "name": "acmeAccessCode",
        "description": "Acme Direct Mail Integration",
        "dataType": "string"
      },
      {
        "displayName": "Acme Mail Date",
        "name": "acmeMailDate",
        "description": "Acme Direct Mail Integration",
        "dataType": "string"
      }
  ]
}
```

### Réponse

```json
{
    "requestId": "d9f1#17943666811",
    "result": [
        {
            "name": "acmeAccessCode",
            "status": "created"
        },
        {
            "name": "acmeMailDate",
            "status": "created"
        }
    ],
    "success": true
}
```

## Mettre à jour le champ

Le point d’entrée Mettre à jour le champ de prospect met à jour un champ personnalisé sur l’objet de prospect. La plupart des mises à jour des champs disponibles dans l’interface utilisateur de Marketo Engage le sont également via l’API. Le tableau suivant résume les différences.

<table>
<tbody>
<tr>
<td style="width: 26.5306%;" rowspan="2"><strong>Attribut</strong></td>
<td style="width: 35%;" colspan="2"><strong>Champ standard</strong></td>
<td style="width: 38.2654%;" colspan="2"><strong>Champ personnalisé</strong></td>
</tr>
<tr>
<td style="width: 17.449%;"><strong>Mis à jour par l’API ?</strong></td>
<td style="width: 17.551%;"><strong>Peut-on les mettre à jour par l’interface utilisateur ?</strong></td>
<td style="width: 19.3878%;"><strong>Mis à jour par l’API ?</strong></td>
<td style="width: 18.8776%;"><strong>Peut-on les mettre à jour par l’interface utilisateur ?</strong></td>
</tr>
<tr>
<td style="width: 26.5306%;">dataType</td>
<td style="width: 17.449%;">non</td>
<td style="width: 17.551%;">non</td>
<td style="width: 19.3878%;">non</td>
<td style="width: 18.8776%;">oui</td>
</tr>
<tr>
<td style="width: 26.5306%;">description</td>
<td style="width: 17.449%;">oui</td>
<td style="width: 17.551%;">oui</td>
<td style="width: 19.3878%;">oui</td>
<td style="width: 18.8776%;">oui</td>
</tr>
<tr>
<td style="width: 26.5306%;">displayName</td>
<td style="width: 17.449%;">non</td>
<td style="width: 17.551%;">non</td>
<td style="width: 19.3878%;">oui</td>
<td style="width: 18.8776%;">oui</td>
</tr>
<tr>
<td style="width: 26.5306%;">isCustom</td>
<td style="width: 17.449%;">non</td>
<td style="width: 17.551%;">non</td>
<td style="width: 19.3878%;">non</td>
<td style="width: 18.8776%;">non</td>
</tr>
<tr>
<td style="width: 26.5306%;">isHidden</td>
<td style="width: 17.449%;">non</td>
<td style="width: 17.551%;">oui</td>
<td style="width: 19.3878%;">oui (si créé par l’API)</td>
<td style="width: 18.8776%;">oui</td>
</tr>
<tr>
<td style="width: 26.5306%;">isHtmlEncodingInEmail</td>
<td style="width: 17.449%;">oui</td>
<td style="width: 17.551%;">oui</td>
<td style="width: 19.3878%;">oui</td>
<td style="width: 18.8776%;">oui</td>
</tr>
<tr>
<td style="width: 26.5306%;">isSensible</td>
<td style="width: 17.449%;">oui</td>
<td style="width: 17.551%;">oui</td>
<td style="width: 19.3878%;">oui</td>
<td style="width: 18.8776%;">oui</td>
</tr>
<tr>
<td style="width: 26.5306%;">length</td>
<td style="width: 17.449%;">non</td>
<td style="width: 17.551%;">non</td>
<td style="width: 19.3878%;">non</td>
<td style="width: 18.8776%;">non</td>
</tr>
<tr>
<td style="width: 26.5306%;">name</td>
<td style="width: 17.449%;">non</td>
<td style="width: 17.551%;">non</td>
<td style="width: 19.3878%;">non</td>
<td style="width: 18.8776%;">non</td>
</tr>
</tbody>
</table>

Le paramètre de chemin d’accès `fieldApiName` obligatoire spécifie le nom d’API du champ à mettre à jour. Le paramètre d’entrée obligatoire est un tableau contenant un objet de champ de prospect avec un ou plusieurs attributs.

### Requête

```http
POST /rest/v1/leads/schema/fields/{fieldApiName}.json
```

### Corps

```json
{
  "input": [
      {
        "displayName": "Acme Access Code",
        "description": "Acme Direct Mail Integration",
        "isHtmlEncodingInEmail": true
      }
  ]
}
```

### Réponse

```json
{
    "requestId": "9f57#1794324f44c",
    "result": [
        {
            "name": "acmeAccessCode",
            "status": "updated"
        }
    ],
    "success": true
}
```

## Transmettre le lead à Marketo

Le lead push est une alternative aux leads de synchronisation et fournit des options de déclenchement supplémentaires, similaires à un formulaire Marketo. Outre la synchronisation des champs de prospect, le point d’entrée peut associer un prospect en fonction d’une valeur de cookie. Transmettez la valeur de `mkt_tok` générée par un clic provenant d’un e-mail Marketo ou transmettez un nom de programme dans l’appel.

Le point d’entrée crée également une activité déclenchable associée à un programme Marketo, à une campagne ou aux deux. Utilisez cette activité pour démarrer des workflows à partir d’événements de capture de pistes attribués à une campagne ou un programme spécifique.

Le lead push utilise les mêmes clés primaires et noms d’API de champ que les leads de synchronisation. Il n’a aucun paramètre d’action, car il effectue toujours un upsert.

Les paramètres d’`programName` et d’entrée sont requis. Le paramètre d’entrée est un tableau d’objets de prospect et l’activité résultante est attribuée au programme nommé. Les paramètres `lookupField`, `source` et `reason` sont facultatifs. Ajoutez des chaînes arbitraires dans `source` et `reason` pour inclure ces valeurs dans les activités résultantes. Vous pouvez utiliser les valeurs comme contraintes dans les déclencheurs correspondants (le prospect est envoyé vers Marketo) et les filtres (le prospect a été envoyé vers Marketo).

Pour associer des activités anonymes antérieures à un prospect nouvellement créé, omettez l’attribut cookies de l’objet de prospect et appelez Associer le prospect après l’intégrer. Pour créer un prospect sans historique d’activité, spécifiez l’attribut cookies dans l’objet de prospect.

### Requête

```http
POST /rest/v1/leads/push.json
```

### Corps

```json
{
    "programName": "Big Blue Thing Product Launch",
    "source": "Cool Sales Site",
    "reason": "Downloaded pricing sheet",
    "lookupField": "email",
    "input": [
        {
             "email": "Theresa.May@westminister.gov.uk",
             "country": "united kingdom",
             "firstName": "Theresa",
             "website": "www.brexit.com",
             "leadScore": 45,
             "jobTitle": "Prime Minister"
         },
         {
             "email": "Justin.Trudeau@ottowa.gov.ca",
             "country": "canada",
             "firstName": "Justin",
             "website": "www.take-off-eh.com",
             "leadScore": 92,
             "jobTitle": "Sonny"
         }
     ]
}
```

### Réponse

```json
{
    "requestId": "939079529805",
    "success": true,
    "warnings": [],
    "result": [
       {
           "id": 483894,
           "status": "created"
       },
       {
           "id": 1087425,
           "status": "updated"
       },
       {
           "id": 3525,
           "reasons": [
                    {
                        "code": "501",
                        "message": "Bad stuff happened"
                    }
           ]
       }
    ]
}
```

Pour transmettre le paramètre `mkt_tok`, affectez sa valeur au membre mktToken dans un enregistrement de prospect au sein du paramètre d’entrée.

### Corps

```json
{
  "programName": "Big Blue Thing Product Launch",
  "source": "Cool Sales Site",
  "reason": "Downloaded pricing sheet",
  "lookupField": "mktToken",
  "input" : [
     {
       "mktToken" : "<tokenValue>",
       "firstName" : "Thelma"
     },
     {
       "mktToken" : "<tokenValue>",
       "firstName" : "Louise"
     }
   ]
}
```

## Envoyer le formulaire

Envoyer le formulaire est une alternative pour synchroniser les prospects et fournit des fonctionnalités équivalentes à un envoi de formulaire Marketo. Utilisez-le pour démarrer des workflows à partir d’événements de capture de prospect attribués à une campagne ou un programme spécifique.

Le point d’entrée Envoyer le formulaire prend en charge les fonctionnalités suivantes :

- Insère un enregistrement de prospect en utilisant le champ d’e-mail comme clé primaire.
- Crée une activité « Remplir le formulaire » associée à un programme, à une campagne ou aux deux.
- Associe un prospect en fonction d’une valeur de cookie.
- Valide les champs de formulaire.

Envoyez un formulaire avec le modèle de base de données de prospect standard. Transmettez un enregistrement d’objet dans le membre d’entrée requis du corps JSON de la requête POST. Le membre de `formId` requis contient l’identifiant du formulaire Marketo cible.

Utilisez le `programId` facultatif pour identifier le programme qui reçoit le prospect, les champs personnalisés des membres du programme ou les deux. Si `programId` est présent, le prospect est ajouté au programme avec tous les champs de membre de programme du formulaire. Le programme doit se trouver dans le même espace de travail que le formulaire.

Si le formulaire ne contient pas de champs personnalisés de membre de programme et que `programId` est omis, le prospect n’est pas ajouté à un programme. Si le formulaire appartient à un programme, contient un ou plusieurs champs personnalisés de membre de programme et omet les `programId`, le point d’entrée utilise le programme du formulaire.

L’objet `leadFormFields` obligatoire contient une ou plusieurs paires nom/valeur à remplir pour les champs. Chaque champ doit être défini dans le formulaire spécifié et chaque nom doit être le nom de l’API REST du champ. Le champ `email` est obligatoire.

L’objet `visitorData` facultatif contient des données sur les visites de page, notamment `pageURL`, `queryString`, `leadClientIpAddress` et `userAgentString`. Utilisez-la pour remplir des champs d’activité supplémentaires pour les filtres et les déclencheurs.

Le membre de cookie facultatif associe un cookie Munchkin à un enregistrement de personne Marketo. Lorsque le point d’entrée crée un prospect, il associe des activités anonymes antérieures à ce prospect, sauf si le cookie a été précédemment associé à un autre enregistrement connu.

Si le cookie a été précédemment associé, les nouvelles activités sont suivies par rapport au nouvel enregistrement, mais les anciennes activités restent avec l’enregistrement connu existant. Pour créer un prospect sans historique d’activité, omettez le membre de cookie.

De nouveaux prospects sont créés dans la partition principale de l’espace de travail dans lequel se trouve le formulaire.

### Requête

```http
POST /rest/v1/leads/submitForm.json
```

### Header

```text
Content-Type: application/json
```

### Corps

```json
{
  "formId": 1029,
  "input": [
    {
      "leadFormFields": {
        "firstName": "Marge",
        "lastName": "Simpson",
        "email": "marge.simpson@fox.com",
        "pMCFField": "PMCF value"
      },
      "visitorData": {
        "pageURL": "https://na-sjst.marketo.com/lp/063-GJP-217/UnsubscribePage.html",
        "queryString": "Unsubscribed=yes",
        "leadClientIpAddress": "192.150.22.5",
        "userAgentString": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.89 Safari/537.36"
      },
      "cookie": "id:063-GJP-217&token:_mch-marketo.com-1594662481190-60776"
    }
  ]
}
```

### Réponse

```json
{
  "requestId": "10667#173bc585ca5",
  "result": [
    {
      "id": 319174,
      "status": "updated"
    }
  ],
  "success": true
}
```

L’image suivante montre les détails de l’activité « Remplir le formulaire » correspondants dans l’interface utilisateur de Marketo Engage :

![Remplir l’interface utilisateur de formulaire](assets/fill_out_form_activity_details.png)

## Fusionner

>[!NOTE]
>
>À compter du 31 mars 2026, les appels qui incluent plus de 25 identifiants dans le paramètre `leadIds` d’un appel de l’API Merge Leads entraîneront un code d’erreur 1080 et l’appel sera ignoré. Les tâches nécessitant la fusion de plus de 25 enregistrements en un seul doivent être divisées en plusieurs tâches pour assurer le succès de ces appels.
>

Utilisez l’API Merge Leads pour combiner les enregistrements en double en un seul enregistrement. Une fusion combine des journaux d’activité, des abonnements de programme, de campagne et de liste, des informations CRM et des valeurs de champ.

Transmettez l’ID de lead gagnant comme paramètre de chemin d’accès. Transmettez un `leadId` en tant que paramètre de requête ou jusqu’à 25 identifiants séparés par des virgules dans le paramètre de `leadIds`.


### Requête

```http
POST /rest/v1/leads/{id}/merge.json?leadId=1324
```

### Réponse

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Le prospect du paramètre de chemin d’accès est le prospect gagnant. Lorsque les valeurs de champ entrent en conflit, la fusion utilise la valeur du gagnant, sauf si cette valeur est vide et que la valeur de l&#39;enregistrement perdant ne l&#39;est pas. Les prospects du paramètre `leadId` ou `leadIds` sont les prospects perdus.

Pour un abonnement activé pour la synchronisation SFDC, utilisez le paramètre `mergeInCRM` pour effectuer également la fusion dans le CRM. Si les deux enregistrements se trouvent dans SFDC et que l’un est un prospect CRM tandis que l’autre est un contact CRM, le contact CRM gagne, quel que soit le gagnant spécifié. Si un enregistrement est dans SFDC et que l’autre n’existe que dans Marketo, le prospect SFDC gagne, quel que soit le gagnant spécifié.

## Associer l&#39;activité Web

Le suivi des leads (Munchkin) enregistre les visites et les clics des visiteurs de votre site web et des pages de destination Marketo. Ces activités utilisent une clé qui correspond au cookie « _mkto_trk » dans le navigateur du prospect, ce qui permet à Marketo de suivre les activités de la même personne.

L’association à un enregistrement de prospect se produit généralement lorsqu’un prospect suit un lien provenant d’un e-mail Marketo ou envoie un formulaire Marketo. Pour associer un prospect après un autre type d’événement, utilisez le point d’entrée Associer le prospect. Transmettez l’ID d’enregistrement de prospect connu en tant que paramètre de chemin d’accès et la valeur du cookie « _mkto_trk » dans le paramètre de requête de cookie.

### Requête

```http
POST /rest/v1/leads/{id}/associate.json?cookie=id:287-GTJ-838%26token:_mch-marketo.com-1396310362214-46169
```

### Réponse

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Si le cookie est déjà associé à un prospect connu, l’utilisation de cette API pour un autre prospect enregistre une nouvelle activité web par rapport au nouvel enregistrement. L’activité web existante ne passe pas au nouvel enregistrement.
Appartenance

Récupérez les enregistrements de prospect en fonction de l’appartenance à une liste ou un programme statique. Vous pouvez également récupérer toutes les listes statiques, les programmes ou les campagnes intelligentes qui incluent un prospect spécifique.

La structure de réponse et les paramètres facultatifs correspondent à Obtenir les prospects par type de filtre, mais cette API n’accepte ni les `filterType` ni les `filterValues`.

Pour trouver l’identifiant de liste dans l’interface utilisateur de Marketo, accédez à la liste et inspectez son URL. En `https://app-****.marketo.com/#ST1001A1`, 1001 est la liste `id`.

## Obtenir les programmes par ID de lead

### Requête

```http
GET /rest/v1/list/{listId}/leads.json?batchSize=3
```

### Réponse

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "nextPageToken":
"PS5VL5WD4UOWGOUCJR6VY7JQO2KUXL7BGBYXL4XH4BYZVPYSFBAONP4V4KQKN4SSBS55U4LEMAKE6===",
    "result":[
       {
            "id":50,
            "email":"kjashaedd@klooblept.com",
            "firstName":"Kataldar",
             "postalCode":"04828"
       },
       {
           "id":2343,
           "email":"kjashaedd@klooblept.com",
           "firstName":"Kataldar",
           "postalCode":"04828"
       },
      {
           "id":88498,
           "email":"kjashaedd@klooblept.com",
           "firstName":"Kataldar",
         "postalCode":"04828"
         }
    ]
}
```

## Obtenir des listes par ID de lead

Le point d’entrée Get Lists by Lead Id prend un paramètre de chemin de `id` d’enregistrement de lead et renvoie chaque liste statique qui inclut le lead.

### Requête

```http
GET /rest/v1/leads/{id}/listMembership.json?batchSize=3
```

### Réponse

```json
{
    "requestId": "1184b#1706f0ec23f",
    "result": [
        {
            "listId": 3379,
            "createdAt": "2016-05-17T19:32:44Z",
            "updatedAt": "2016-05-17T19:32:44Z"
        },
        {
            "listId": 2792,
            "createdAt": "2009-05-19T18:29:15Z",
            "updatedAt": "2009-05-19T18:29:15Z"
        },
        {
            "listId": 42,
            "createdAt": "2009-04-22T19:24:22Z",
            "updatedAt": "2009-04-22T19:24:22Z"
        }
    ],
    "success": true,
    "nextPageToken": "BFRV7OMVSNJWDVKVTUFS3XHT4E======",
    "moreResult": true
}
```

## Programmes

Récupérez l’appartenance à un programme de la même manière que l’appartenance à une liste. L’option Obtenir les prospects par ID de programme accepte les mêmes paramètres de requête facultatifs et nécessite le paramètre de chemin d’accès `programId`.

Vous pouvez éventuellement transmettre un paramètre de champs contenant une liste de noms de champs séparés par des virgules. Si des champs sont omis, la réponse inclut `email`, `updatedAt`, `createdAt`, `lastName`, `firstName`, `membership` et `id`. Si un champ demandé n’est pas renvoyé, sa valeur est implicitement nulle.

Chaque élément du tableau de résultats est un prospect avec un objet enfant appelé « appartenance ». Cet objet décrit la relation du prospect avec le programme demandé et inclut toujours `progressionStatus`, `acquiredBy`, `reachedSuccess` et `membershipDate`.

Si le programme parent est un programme d&#39;engagement, les membres comprennent également des `stream`, des `nurtureCadence` et des `isExhausted` pour décrire la position et l&#39;activité du responsable dans ce programme.

### Requête

```http
GET /rest/v1/leads/programs/{programId}.json?batchSize=3
```

### Réponse

```json
{
    "requestId": "13ad4#1727b748a17",
    "result": [
        {
            "id": 319141,
            "firstName": "Meera",
            "lastName": "Reed",
            "email": "mree@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        },
        {
            "id": 319142,
            "firstName": "Jon",
            "lastName": "Umber",
            "email": "jumb@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        },
        {
            "id": 319143,
            "firstName": "Lyanna",
            "lastName": "Mormont",
            "email": "lmor@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        }
    ],
    "success": true,
    "nextPageToken": "SW3PTMBVFCNHSHJGZ7LQH3ZWNUOHKADJZ3MOQ2LOZZVNO3WEIUPDKPRTTHBSMW756KOCWURTOF2XS==="
}
```

Le point d’entrée Get Programmes by Lead Id prend un paramètre de chemin d’accès à l’ID d’enregistrement du prospect et renvoie chaque programme qui inclut le prospect. Utilisez les paramètres facultatifs `filterType` et `filterValues` pour filtrer par ID de programme.

### Requête

```http
GET /rest/v1/leads/{id}/programMembership.json
```

### Réponse

```json
{
    "requestId": "12e84#1706f13a379",
    "result": [
        {
            "id": 1044,
            "progressionStatus": "Sent",
            "isExhausted": false,
            "acquiredBy": false,
            "reachedSuccess": false,
            "membershipDate": "2016-05-27T19:50:29Z",
            "updatedAt": "2016-05-27T19:50:29Z"
        }
    ],
    "success": true,
    "moreResult": false
}
```

## Campagnes intelligentes

Le point d’entrée Get Smart Campaign by Lead Id prend un paramètre de chemin d’accès à l’identifiant d’enregistrement du prospect et renvoie chaque campagne intelligente qui inclut le prospect.

### Requête

```http
GET /rest/v1/leads/{id}/smartCampaignMembership.json?batchSize=3
```

### Réponse

```json
{
    "requestId": "e7b0#1706f163632",
    "result": [
        {
            "smartCampaignId": 3746,
            "createdAt": "2018-06-01T18:00:04Z",
            "updatedAt": "2018-06-01T18:00:06Z"
        },
        {
            "smartCampaignId": 3678,
            "createdAt": "2015-04-06T18:37:30Z",
            "updatedAt": "2015-04-06T18:37:41Z"
        },
        {
            "smartCampaignId": 3680,
            "createdAt": "2015-04-06T18:37:30Z",
            "updatedAt": "2015-04-06T18:37:40Z"
        }
    ],
    "success": true,
    "nextPageToken": "TNGAH3NKDUFDHNXUVGTNBXJCQM======",
    "moreResult": true
}
```

## Supprimer

Utilisez le point d’entrée Supprimer les prospects pour supprimer les enregistrements de prospect. Spécifiez les ID de prospect dans le corps avec les attributs d’ID. Une requête peut supprimer jusqu’à 300 prospects. Envoyez l’en-tête Content-Type : application/json .

### Requête

```http
POST /rest/v1/leads/delete.json
```

### Corps

```json
{
   "input":[
      {
         "id": 235
      },
      {
         "id":766
      }
   ]
}
```

### Réponse

```json
{
  "requestId":"3608#16664333670",
  "result":[
    {
      "id":235,
      "status":"deleted"
    },
    {
      "id":766,
      "status":"deleted"
    }
  ],
  "success":true
}
```

## Relations

- Sociétés via le champ externalCompanyId dans l’enregistrement de prospect
- SalesPersons via le champ externalSalesPersonId dans l’enregistrement du prospect
- Programmes via l’appartenance à un programme
- Listes via l’appartenance à une liste
- Activités via le champ leadId dans l’activité
- Segmentation par champs de segment individuels sur l’enregistrement de prospect
- Partitionne par le biais du champ leadPartitionId dans l’enregistrement de prospect

## Délais dépassés

Les points d’entrée des leads ont un délai d’expiration de 30 s, à l’exception des points d’entrée suivants :

- Leads de synchronisation : 90s
- Responsable associé : années 60
- Fusionner les leads : 180s
- Mise à jour de la partition de lead : années 60
- Intégrer le lead à Marketo : années 90
- Get Leads by Filter Type : 60s
- Get Leads by List ID : 60s
