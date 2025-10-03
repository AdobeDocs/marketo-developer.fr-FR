---
title: Prospects
feature: REST API
description: Explorez les fonctionnalités de l’API REST des leads Marketo, notamment la description, la requête par ID ou filtre, les champs par défaut, les limites et la récupération des ECID.
exl-id: 0a2f7c38-02ae-4d97-acfe-9dd108a1f733
source-git-commit: cc4bd7c18124bb039386a1cec06b9f1da0d047cb
workflow-type: tm+mt
source-wordcount: '3411'
ht-degree: 3%

---

# Prospects

[Référence du point d’entrée des leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads)

L’API du prospect Marketo offre un large éventail de fonctionnalités pour les applications CRUD simples par rapport aux enregistrements de prospect, ainsi que la possibilité de modifier l’appartenance d’un prospect à des listes et programmes statiques et de lancer un traitement Smart Campaign pour les prospects.

## Décrire

L’une des fonctionnalités clés de l’API Leads est la méthode Describe. Utilisez Décrire les leads pour récupérer une liste complète des champs disponibles pour l’interaction via l’API REST, ainsi que des métadonnées pour chacun :

* Type de données
* Noms d’API REST
* Longueur (le cas échéant)
* Lecture Seule
* Libellé convivial

Description est la source principale de vérité pour savoir si les champs sont disponibles et pour connaître les métadonnées de ces champs.

### Requête

```
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

Normalement, les réponses incluent un ensemble de champs beaucoup plus grand dans le tableau de résultats, mais nous les omettons à des fins de démonstration. Chaque élément du tableau de résultats correspond à un champ disponible sur l’enregistrement du prospect et possède au minimum un id, un displayName et un type de données. Les objets enfants rest et soap peuvent être présents ou non pour un champ donné et leur présence indique si le champ peut être utilisé dans les API REST ou SOAP. La propriété `readOnly` indique si le champ est en lecture seule via l’API correspondante (REST ou SOAP). La propriété length indique la longueur maximale du champ, le cas échéant. La propriété dataType indique le type de données du champ.

## Requête

Il existe deux méthodes principales pour la récupération des prospects : les méthodes Obtenir le prospect par ID et Obtenir les prospects par type de filtre . L’option Obtenir le prospect par ID utilise un ID de prospect unique comme paramètre de chemin d’accès et renvoie un enregistrement de prospect unique.

Vous pouvez éventuellement transmettre un paramètre de champs contenant une liste de noms de champs séparés par des virgules à renvoyer. Si le paramètre fields n’est pas inclus dans cette requête, les champs par défaut suivants sont renvoyés : `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` et `id`. Lors de la demande d’une liste de champs, si un champ particulier est demandé, mais n’est pas renvoyé, la valeur est implicitement nulle.

### Requête

```
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

Pour cette méthode, il y aura toujours un seul enregistrement à la première position du tableau de résultats.

L’option Obtenir les leads par type de filtre renvoie le même type d’enregistrements, mais peut renvoyer jusqu’à 300 par page. Elle nécessite les paramètres de requête `filterType` et `filterValues`.

`filterType` accepte n’importe quel champ personnalisé ou la plupart des champs couramment utilisés. Appelez le point d’entrée `Describe2` pour obtenir une liste complète des champs pouvant faire l’objet d’une recherche et pouvant être utilisés dans `filterType`. Lors d’une recherche par champ personnalisé, seuls les types de données suivants sont pris en charge : `string`, `email`, `integer`. Vous pouvez obtenir les détails du champ (description, type, etc.) à l’aide de la méthode Describe mentionnée ci-dessus.

`filterValues` accepte jusqu’à 300 valeurs au format séparé par des virgules. L’appel recherche les enregistrements pour lesquels le champ du prospect correspond à l’un des `filterValues` inclus. Si le nombre de leads correspondant au filtre de lead est supérieur à 1 000, une erreur est renvoyée : « 1 003, Trop de résultats correspondent au filtre ».

Si la longueur totale de votre requête GET dépasse 8 Ko, une erreur HTTP est renvoyée : « 414, URI trop long » (par RFC 7231). Pour pallier ce problème, vous pouvez remplacer votre GET par POST, ajouter le paramètre _method=GET et placer une chaîne de requête dans le corps de la requête.

### Requête

```
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

Cet appel recherche les enregistrements correspondant aux ID inclus dans `filterValues` et renvoie tous les enregistrements correspondants.

Si aucun enregistrement n’est trouvé, la réponse indique la réussite, mais le tableau de résultats est vide.

### Réponse

```json
{
"requestId": "177a1#1578b643357",
"result": [],
"success": true
}
```

Les options Obtenir les prospects par ID et Obtenir les prospects par type de filtre acceptent également un paramètre de requête de champ, qui accepte une liste de champs API séparés par des virgules. Si cela est inclus, alors chaque enregistrement de la réponse inclura ces champs répertoriés.  Si cet attribut est omis, un ensemble de champs par défaut est renvoyé : `id`, `email`, `updatedAt`, `createdAt`, `firstName` et `lastName`.

## ADOBE ECID

Lorsque la fonction Partage d’audience Adobe Experience Cloud est activée, un processus de synchronisation des cookies se produit qui associe l’identifiant Adobe Experience Cloud ID (ECID) aux prospects Marketo.  Les méthodes de récupération de prospect mentionnées ci-dessus peuvent être utilisées pour récupérer les valeurs ECID associées.  Pour ce faire, spécifiez `ecids` dans le paramètre fields . Par exemple : `&fields=email,firstName,lastName,ecids`.

## Créer et mettre à jour

Outre la récupération des données de prospect, vous pouvez créer, mettre à jour et supprimer l’enregistrement de prospect via l’API. La création et la mise à jour de leads partagent le même point d’entrée avec le type d’opération défini dans la requête, et jusqu’à 300 enregistrements peuvent être créés ou mis à jour en même temps.

>[!NOTE]
>
> La mise à jour des champs Société à l’aide du point d’entrée [Leads de synchronisation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) n’est pas prise en charge. Utilisez plutôt le point d’entrée [Synchroniser les entreprises](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST).

>[!NOTE]
>
> Lors de la création ou de la mise à jour de la valeur d’e-mail sur un enregistrement Personne, seuls les caractères ASCII sont pris en charge dans le champ d’adresse e-mail.

### Requête

```
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

Dans cette requête, vous voyez deux champs importants : `action` et `lookupField`.  `action` indique le type d&#39;opération de la demande et peut être `createOrUpdate`, `createOnly`, `updateOnly` ou `createDuplicate`. S’il est omis, l’action est définie par défaut sur `createOrUpdate`.  Le paramètre `lookupField` spécifie la clé à utiliser lorsque l’action est `createOrUpdate` ou `updateOnly`. Si `lookupField` est omis, la clé par défaut est `email`.

Par défaut, la partition par défaut est utilisée. Vous pouvez éventuellement spécifier le paramètre `partitionName`, qui ne fonctionne que si l’action est `createOnly` ou `createOrUpdate`. Pour que `partitionName` fonctionne en tant que critère de déduplication supplémentaire, il doit faire partie du type de source dans les règles de déduplication personnalisées. Lors d’une opération de mise à jour, si un prospect n’existe pas dans la partition spécifiée, une erreur est renvoyée. Si l’utilisateur API uniquement n’a pas l’autorisation d’accéder à la partition spécifiée, une erreur est renvoyée.

Le champ `id` ne peut être inclus en tant que paramètre que lors de l’utilisation de l’action `updateOnly`, car `id` s’agit d’une clé unique gérée par le système.

La requête doit également comporter un paramètre `input`, qui est un tableau d’enregistrements de prospect. Chaque enregistrement de prospect est un objet JSON comportant un nombre illimité de champs de prospect. Les clés incluses dans un enregistrement doivent être uniques pour cet enregistrement et toutes les chaînes JSON doivent être codées au format UTF-8. Le champ `externalCompanyId` peut être utilisé pour lier l’enregistrement du prospect à un enregistrement d’entreprise. Le champ `externalSalesPersonId` peut être utilisé pour lier l&#39;enregistrement du prospect à un enregistrement de vendeur.

Remarque : lors de l’exécution simultanée ou en succession rapide de demandes d’upsert de lead, des enregistrements en double peuvent se produire lors de l’exécution de plusieurs demandes avec la même valeur de clé si un appel suivant de la même valeur est effectué avant le premier retour. Cela peut être évité en utilisant la `createOnly` ou la `updateOnly` appropriée, ou en mettant les appels en file d’attente et en attendant que votre appel revienne avant d’effectuer des appels upsert suivants avec la même clé.

## Champs

L’objet de prospect contient des champs standard et éventuellement des champs personnalisés. Des champs standard sont présents dans chaque abonnement Marketo Engage, tandis que des champs personnalisés sont créés par l’utilisateur selon les besoins. Chaque définition de champ se compose d’un ensemble d’attributs qui décrivent le champ. Les exemples d’attributs sont le nom d’affichage, le nom de l’API et dataType. Ces attributs sont collectivement appelés métadonnées.

Les points d’entrée suivants vous permettent d’interroger, de créer et de mettre à jour des champs sur l’objet de prospect. Ces API nécessitent que l&#39;utilisateur propriétaire de l&#39;API dispose d&#39;un rôle avec l&#39;une ou l&#39;autre des autorisations Read-Write Schema Standard Field ou Read-Write Schema Custom Field , ou les deux.

## Champs de requête

L’interrogation des champs de prospect est simple. Vous pouvez interroger un seul champ de prospect par nom d’API ou interroger l’ensemble de tous les champs de prospect. Les champs standard et personnalisés peuvent être récupérés, selon les autorisations de rôle utilisées. Les champs masqués sont également récupérés.

## Par nom

Le point d’entrée Get Lead Field by Name récupère les métadonnées d’un seul champ sur l’objet du prospect. Le paramètre de chemin d’accès fieldApiName obligatoire spécifie le nom d’API du champ. La réponse est similaire au point d’entrée Décrire le prospect, mais contient des métadonnées supplémentaires telles que l’attribut isCustom, qui indique si le champ est un champ personnalisé.

### Requête

```
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

Le point d’entrée Get Lead Fields récupère les métadonnées de tous les champs de l’objet de prospect, y compris. Par défaut, un maximum de 300 enregistrements est renvoyé. Vous pouvez utiliser le paramètre de requête `batchSize` pour réduire ce nombre. Si l’attribut `moreResult` est défini sur « true », cela signifie que d’autres résultats sont disponibles. Continuez à appeler ce point d’entrée jusqu’à ce que l’attribut `moreResult` renvoie false, ce qui signifie qu’aucun résultat n’est disponible. Les `nextPageToken` renvoyés par cette API doivent toujours être réutilisés pour l’itération suivante de cet appel.

### Requête

```
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

Le point d’entrée Créer des champs de prospect crée un ou plusieurs champs personnalisés sur l’objet de prospect. Ce point d’entrée fournit des fonctionnalités comparables à celles disponibles dans l’interface utilisateur de Marketo Engage. Vous pouvez créer jusqu’à 100 champs personnalisés à l’aide de ce point d’entrée.
Examinez attentivement chaque champ que vous créez dans votre instance de production de Marketo Engage à l’aide de l’API.  Une fois qu’un champ a été créé, vous ne pouvez pas le supprimer (vous pouvez uniquement le masquer). La prolifération des champs inutilisés est une mauvaise pratique qui encombrera votre instance.

Le paramètre d’entrée requis est un tableau d’objets de champ de prospect. Chaque objet contient un ou plusieurs attributs. Les attributs obligatoires sont les `displayName`, `name` et `dataType` qui correspondent respectivement au nom d’affichage de l’interface utilisateur du champ, au nom d’API du champ et au type de champ.  Vous pouvez éventuellement spécifier `description`, `isHidden`, `isHtmlEncodingInEmail` et `isSensitive`.

Quelques règles sont associées au nom et à la dénomination des `displayName`. L’attribut name doit être unique, commencer par une lettre et contenir uniquement des lettres, des chiffres ou des traits de soulignement. Le `displayName` doit être unique et ne peut pas contenir de caractères spéciaux.  Une convention d’affectation des noms courante consiste à appliquer la casse mixte aux `displayName` pour produire le nom. Par exemple, une `displayName` de « Mon champ personnalisé » génère le nom « myCustomField ».

### Requête

```
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

Le point d’entrée Mettre à jour le champ de prospect met à jour un seul champ personnalisé sur l’objet de prospect. Pour la plupart, les opérations de mise à jour des champs effectuées à l’aide de l’interface utilisateur de Marketo Engage sont réalisables à l’aide de l’API . Quelques différences sont résumées dans le tableau ci-dessous.

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

Le paramètre de chemin d’accès `fieldApiName` obligatoire spécifie le nom d’API du champ à mettre à jour. Le paramètre d’entrée requis est un tableau qui contient un seul objet de champ de prospect.  L’objet de champ contient un ou plusieurs attributs.

### Requête

```
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

Push Lead est une alternative pour la synchronisation des leads avec Marketo, principalement conçue pour permettre un plus grand degré de capacité de déclenchement que les leads de synchronisation standard (similaire en utilisation à un formulaire Marketo). Outre la synchronisation des champs de prospect, ce point d’entrée permet l’association de prospects basée sur les valeurs de cookie transmises au point d’entrée . Pour ce faire, transmettez la valeur de `mkt_tok` générée en cliquant sur un e-mail Marketo ou en transmettant un nom de programme dans l’appel. Ce point d’entrée crée également une activité déclenchable unique, qui est associée à un programme et/ou à une campagne dans Marketo. Cela permet de déclencher des événements de capture de prospect attribués à une campagne ou un programme spécifique afin de lancer les workflows associés depuis Marketo.

L’interface des leads push est très similaire aux leads de synchronisation. Toutes les mêmes clés primaires sont valides et les mêmes noms d’API sont utilisés pour les champs (il n’existe aucun paramètre d’action car il s’agit toujours d’une opération d’upsert). Les paramètres d’`programName` et d’entrée sont obligatoires, et les paramètres `lookupField`, `source` et `reason` sont facultatifs. Le paramètre d’entrée est un tableau d’objets de prospect. L’activité résultante est attribuée au programme nommé correspondant. Les paramètres `source` et `reason` sont des champs de chaîne arbitraires qui peuvent être ajoutés à la requête pour incorporer ces valeurs dans les activités résultantes. Ils peuvent être utilisés comme contraintes dans les déclencheurs correspondants (le prospect est envoyé vers Marketo) et dans les filtres (le prospect a été envoyé vers Marketo).

Remarque concernant les activités anonymes. Si vous souhaitez associer des activités anonymes antérieures au prospect nouvellement créé, ne spécifiez pas l’attribut cookies dans l’objet de prospect et appelez le prospect associé à la suite de l’envoi du prospect. Si vous souhaitez créer un prospect sans historique d’activité, il vous suffit de spécifier l’attribut cookies dans l’objet de prospect.

### Requête

```
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
             "marketoSocialFacebookProfileURL": "http://www.facebook.com/id/23434456",
             "jobTitle": "Prime Minister"
         },
         {
             "email": "Justin.Trudeau@ottowa.gov.ca",
             "country": "canada",
             "firstName": "Justin",
             "website": "www.take-off-eh.com",
             "leadScore": 92,
             "marketoSocialFacebookProfileURL": "http://www.facebook.com/id/42434",
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

Pour transmettre le paramètre `mkt_tok`, affectez la valeur au membre mktToken dans un enregistrement de prospect dans le paramètre d’entrée comme suit.

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

Envoi de formulaire constitue une alternative pour la synchronisation des prospects vers Marketo et a été conçu pour fournir une fonctionnalité équivalente à un envoi de formulaire Marketo. Cela permet de déclencher des événements de capture de prospect attribués à une campagne ou un programme spécifique afin de lancer les workflows associés depuis Marketo.

Le point d’entrée Envoyer le formulaire prend en charge les fonctionnalités suivantes :

* Insère un enregistrement de prospect en utilisant le champ d’e-mail comme clé primaire
* Crée une activité « Remplir le formulaire » associée à un programme et/ou à une campagne
* Autorise l’association de prospects en fonction de la valeur du cookie
* Effectue la validation du champ de formulaire

L’envoi d’un formulaire suit le modèle standard de la base de données de prospects. Un seul enregistrement d’objet est transmis dans le membre d’entrée requis du corps JSON d’une requête POST. Le membre de `formId` requis contient l’identifiant du formulaire Marketo cible.

Le `programId` facultatif peut être utilisé pour spécifier le programme auquel ajouter le prospect et/ou spécifier le programme auquel ajouter des champs personnalisés de membre de programme. Si `programId` est fourni, le prospect est ajouté au programme et tous les champs de membre de programme présents dans le formulaire sont également ajoutés. Notez que le programme spécifié doit se trouver dans le même espace de travail que le formulaire. Si le formulaire ne contient pas de champs personnalisés de membre de programme et que `programId` n’est pas fourni, le prospect n’est pas ajouté à un programme. Si le formulaire réside dans un programme et `programId` n’est pas fourni, ce programme est utilisé lorsqu’un ou plusieurs champs personnalisés membres du programme sont présents dans le formulaire.

Dans l’enregistrement d’entrée, l’objet `leadFormFields` est obligatoire. Cet objet contient une ou plusieurs paires nom/valeur qui correspondent aux champs de formulaire à remplir.  Tous les champs spécifiés doivent être définis dans le formulaire spécifié. Le nom correspond au nom de l’API REST pour le champ . Notez que le champ `email` est obligatoire.

L’objet membre de `visitorData` est facultatif et contient des paires nom/valeur qui correspondent aux données de page-visite, y compris `pageURL`, `queryString`, `leadClientIpAddress` et `userAgentString`. Peut être utilisé pour remplir des champs d’activité supplémentaires à des fins de filtrage et de déclenchement.

La chaîne de membre du cookie est facultative et vous permet d’associer un cookie Munchkin à un enregistrement de personne dans Marketo. Lorsqu’un nouveau prospect est créé, toutes les activités anonymes antérieures sont associées à ce prospect, sauf si la valeur du cookie avait été précédemment associée à un autre enregistrement connu. Si la valeur du cookie a été précédemment associée, les nouvelles activités sont suivies par rapport à l’enregistrement, mais les anciennes activités ne seront pas migrées loin de l’enregistrement connu existant. Pour créer un prospect sans historique d’activité, omettez simplement le membre de cookie.

De nouveaux prospects sont créés dans la partition principale de l’espace de travail dans lequel se trouve le formulaire.

### Requête

```
POST /rest/v1/leads/submitForm.json
```

### Header

```
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

Vous trouverez ici les détails de l’activité « Remplir le formulaire » correspondante dans l’interface utilisateur de Marketo Engage :

![Remplir l’interface utilisateur de formulaire](assets/fill_out_form_activity_details.png)

## Fusionner

>[!NOTE]
>À compter du 31 mars 2026, les appels qui incluent plus de 25 identifiants dans le paramètre `leadIds` d’un appel de l’API Merge Leads entraîneront un code d’erreur 1080 et l’appel sera ignoré. Les tâches nécessitant la fusion de plus de 25 enregistrements en un seul doivent être divisées en plusieurs tâches pour assurer le succès de ces appels.
>

Il est parfois nécessaire de fusionner des enregistrements en double. Pour ce faire, Marketo utilise l’API Merge Leads. La fusion des prospects combinera leurs journaux d’activités, programmes, campagnes et listes d’appartenances et informations CRM, et fusionnera toutes leurs valeurs de champ en un seul enregistrement. La fusion des prospects prend un ID de prospect comme paramètre de chemin d’accès et soit un seul `leadId` comme paramètre de requête, soit une liste de 25 ID séparés par des virgules ou moins dans le paramètre de `leadIds`


### Requête

```
POST /rest/v1/leads/{id}/merge.json?leadId=1324
```

### Réponse

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Le prospect spécifié dans le paramètre de chemin d’accès est le prospect gagnant. Par conséquent, s’il existe des champs en conflit entre les enregistrements fusionnés, la valeur du gagnant est récupérée, sauf si le champ de l’enregistrement gagnant est vide et que le champ correspondant de l’enregistrement perdant ne l’est pas. Les prospects spécifiés dans le paramètre `leadId` ou `leadIds` sont les prospects perdus.

Si vous disposez d’un abonnement activé pour la synchronisation de SFDC, vous pouvez également utiliser le paramètre `mergeInCRM` dans votre requête. Si la valeur est définie sur true, la fusion correspondante dans votre CRM est également effectuée. Si les deux prospects sont dans SFDC et que l’un est un prospect CRM et que l’autre est un contact CRM, le gagnant est le contact CRM (quel que soit le prospect spécifié comme gagnant). Si l’un des prospects se trouve dans SFDC et que l’autre est Marketo uniquement, le gagnant est le prospect SFDC (quel que soit le prospect spécifié comme gagnant).

## Associer l&#39;activité Web

Grâce au suivi des leads (Munchkin), Marketo enregistre l’activité web des visiteurs de votre site web et de vos pages de destination Marketo. Ces activités, Visites et Clics, sont enregistrées avec une clé qui correspond à un cookie « _mkto_trk » défini dans le navigateur du prospect. Marketo l’utilise pour suivre les activités de la même personne. Normalement, l’association aux enregistrements de prospect se produit lorsqu’un prospect clique sur un e-mail de Marketo ou remplit un formulaire Marketo, mais une association peut parfois être déclenchée par un autre type d’événement, ce que vous pouvez faire à l’aide du point d’entrée Associer le prospect. Le point d’entrée prend l’identifiant d’enregistrement de prospect connu comme paramètre de chemin d’accès et la valeur du cookie « _mkto_trk » dans le paramètre de requête de cookie.

### Requête

```
POST /rest/v1/leads/{id}/associate.json?cookie=id:287-GTJ-838%26token:_mch-marketo.com-1396310362214-46169
```

### Réponse

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Si un cookie est déjà associé à un enregistrement de prospect connu, l’utilisation de cette API sur un autre enregistrement de prospect entraîne l’enregistrement d’une nouvelle activité web par rapport à cet enregistrement, mais ne déplacera aucune activité web existante vers le nouvel enregistrement.
Adhésion

Les enregistrements de lead peuvent également être récupérés en fonction de l’appartenance à une liste statique ou à un programme. De plus, vous pouvez récupérer toutes les listes statiques, les programmes ou les campagnes intelligentes dont un prospect est membre.

La structure de réponse et les paramètres facultatifs sont identiques à ceux de l’option Get Leads by Filter Type, bien que filterType et filterValues ne puissent pas être utilisés avec cette API.
Pour accéder à l’ID de liste via l’interface utilisateur de Marketo, accédez à la liste. La liste `id` se trouve dans l’URL de la liste statique, `https://app-**&#x200B;**.marketo.com/#ST1001A1`. Dans cet exemple, 1001 est la `id` de la liste.

### Requête

```
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

Le point d’entrée Get Lists by Lead Id prend un enregistrement de lead `id` un paramètre de chemin d’accès et renvoie tous les enregistrements de liste statiques dont le lead est membre.

### Requête

```
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

L’appartenance à un programme peut être récupérée de la même manière que les listes. Les mêmes paramètres de requête facultatifs sont disponibles lors de l’appel du point d’entrée Get Leads by Program Id et de la transmission du paramètre de chemin d’accès `programId`.

Vous pouvez éventuellement transmettre un paramètre de champs contenant une liste de noms de champs séparés par des virgules à renvoyer. Si le paramètre fields n’est pas inclus dans cette requête, les champs par défaut suivants seront renvoyés : `email`, `updatedAt`, `createdAt`, `lastName`, `firstName`, `membership` et `id`. Lors de la demande d’une liste de champs, si un champ particulier est demandé, mais n’est pas renvoyé, la valeur est implicitement nulle.

La structure de la réponse est très similaire, car chaque élément du tableau de résultats est un prospect, excepté que chaque enregistrement possède également un objet enfant appelé « appartenance ». Cet objet d’abonnement inclut des données sur la relation du prospect avec le programme indiqué dans l’appel, affichant toujours ses `progressionStatus`, `acquiredBy`, `reachedSuccess` et `membershipDate`. Si le programme parent est également un programme de participation, les membres auront des `stream`, des `nurtureCadence` et des `isExhausted` pour indiquer leur position et leur activité dans le programme de participation.

### Requête

```
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

Le point d’entrée Get Programmes by Lead Id prend un paramètre de chemin d’accès à l’ID d’enregistrement du prospect et renvoie tous les enregistrements de programme dont le prospect est membre. Les paramètres facultatifs `filterType` et `filterValues` vous permettent de filtrer par ID de programme.

### Requête

```
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

Le point d’entrée Get Smart Campaign by Lead Id prend un paramètre de chemin d’accès à l’identifiant d’enregistrement du prospect et renvoie tous les enregistrements de campagne intelligente dont le prospect est membre.

### Requête

```
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

La suppression des prospects est simple grâce au point d’entrée Supprimer les prospects.  Spécifiez les ID de prospect à supprimer à l’aide des attributs d’ID du corps.  La limite maximale est de 300 prospects par requête.  Utilisez Content-Type : en-tête application/json .

### Requête

```
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

* Sociétés via le champ externalCompanyId dans l’enregistrement de prospect
* SalesPersons via le champ externalSalesPersonId dans l&#39;enregistrement du lead
* Programmes via l’appartenance à un programme
* Listes via l’appartenance à une liste
* Activités via le champ leadId dans l’activité
* Segmentation par champs de segment individuels sur l’enregistrement de prospect
* Partitions via leadPartitionId dans l’enregistrement de prospect

## Délais dépassés

Les points d’entrée des leads ont un délai d’expiration de 30 s, sauf indication ci-dessous :

* Leads de synchronisation : 90s
* Responsable associé : années 60
* Fusionner les leads : 180s
* Mise à jour de la partition de lead : années 60
* Intégrer le lead à Marketo : années 90
* Get Leads by Filter Type : 60s
* Get Leads by List ID : 60s
