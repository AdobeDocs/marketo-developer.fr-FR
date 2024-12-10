---
title: Prospects
feature: REST API
description: Détails sur les appels de l’API Leads
exl-id: 0a2f7c38-02ae-4d97-acfe-9dd108a1f733
source-git-commit: 7a3df193e47e7ee363c156bf24f0941879c6bd13
workflow-type: tm+mt
source-wordcount: '3338'
ht-degree: 3%

---

# Prospects

[Référence du point d’entrée Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads)

L’API de piste Marketo offre un large éventail de fonctionnalités pour les applications CRUD simples par rapport aux enregistrements de piste, ainsi que la possibilité de modifier l’appartenance d’une piste dans des listes et programmes statiques et d’initier le traitement de campagne dynamique pour les pistes.

## Description

L’une des fonctionnalités clés de l’API Leads est la méthode Description . Utilisez Description des pistes pour récupérer une liste complète des champs disponibles pour l’interaction via l’API REST, ainsi que des métadonnées pour chacune d’elles :

* Type de données
* Noms d’API REST
* Longueur (le cas échéant)
* Lecture seule
* Libellé convivial

La description est la principale source de vérité pour savoir si des champs sont disponibles et si des métadonnées les concernant sont disponibles.

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

Normalement, les réponses incluent un ensemble de champs beaucoup plus grand dans le tableau de résultats, mais nous les omettons à des fins de démonstration. Chaque élément du tableau de résultat correspond à un champ disponible sur l’enregistrement de piste et comporte au minimum un identifiant, un displayName et un type de données. Les autres objets enfants et soap peuvent être présents ou non pour un champ donné et leur présence indique si le champ est valide pour une utilisation dans les API REST ou SOAP. La propriété `readOnly` indique si le champ est en lecture seule via l’API correspondante (REST ou SOAP). La propriété length indique la longueur maximale du champ, le cas échéant. La propriété dataType indique le type de données du champ.

## Requête

Il existe deux méthodes principales pour la récupération des pistes : les méthodes Get Lead by Id et Get Leads by Filter Type . L’identifiant Get Lead by utilise un seul identifiant de piste comme paramètre de chemin d’accès et renvoie un seul enregistrement de piste.

Vous pouvez éventuellement transmettre un paramètre de champ contenant une liste de noms de champ séparés par des virgules à renvoyer. Si le paramètre fields n&#39;est pas inclus dans cette requête, les champs par défaut suivants sont renvoyés : `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` et `id`. Lorsque vous demandez une liste de champs, si un champ particulier est demandé, mais n’est pas renvoyé, la valeur est implicitement nulle.

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

Pour cette méthode, il y aura toujours un seul enregistrement à la première position du tableau de résultat.

Obtenir des pistes par type de filtre renvoie le même type d’enregistrements, mais peut renvoyer jusqu’à 300 valeurs par page. Elle nécessite les paramètres de requête `filterType` et `filterValues`.

`filterType` accepte n’importe quel champ personnalisé ou la plupart des champs couramment utilisés. Appelez le point d’entrée `Describe2` pour obtenir une liste complète des champs pouvant faire l’objet d’une recherche et pouvant être utilisés dans `filterType`. Lors de la recherche par champ personnalisé, seuls les types de données suivants sont pris en charge : `string`, `email`, `integer`. Vous pouvez obtenir les détails du champ (description, type, etc.) à l&#39;aide de la méthode décrite ci-dessus.

`filterValues` accepte jusqu’à 300 valeurs dans un format séparé par des virgules. L’appel recherche des enregistrements dont le champ de piste correspond à l’un des `filterValues` inclus. Si le nombre de pistes correspondant au filtre de piste est supérieur à 1 000, une erreur est renvoyée : &quot;1003, trop de résultats correspondent au filtre&quot;.

Si la longueur totale de votre demande de GET dépasse 8 Ko, une erreur HTTP est renvoyée : &quot;414, URI trop long&quot; (selon la norme RFC 7231). Pour pallier ce problème, vous pouvez remplacer votre GET par POST, ajouter le paramètre _method=GET et placer une chaîne de requête dans le corps de la requête.

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

Cet appel recherche des enregistrements correspondant aux identifiants inclus dans `filterValues` et renvoie les enregistrements correspondants.

Si aucun enregistrement n’est trouvé, la réponse indique la réussite, mais le tableau de résultat est vide.

### Réponse

```json
{
"requestId": "177a1#1578b643357",
"result": [],
"success": true
}
```

Les options Get Lead by Id et Get Leads by Filter Type acceptent également un paramètre de requête de champ qui accepte une liste de champs d’API séparés par des virgules. Si celle-ci est incluse, chaque enregistrement de la réponse inclut ces champs répertoriés.  S’il est omis, un ensemble de champs par défaut est renvoyé : `id`, `email`, `updatedAt`, `createdAt`, `firstName` et `lastName`.

## Adobe ECID

Lorsque la fonction de partage d’audience Adobe Experience Cloud est activée, un processus de synchronisation des cookies se produit qui associe Adobe Experience Cloud ID (ECID) aux pistes Marketo.  Les méthodes de récupération de pistes mentionnées ci-dessus peuvent être utilisées pour récupérer les valeurs ECID associées.  Pour ce faire, spécifiez `ecids` dans le paramètre des champs. Par exemple : `&fields=email,firstName,lastName,ecids`.

## Créer et mettre à jour

Outre la récupération des données de piste, vous pouvez créer, mettre à jour et supprimer des enregistrements de piste via l’API. La création et la mise à jour des pistes partagent le même point de terminaison avec le type d’opération défini dans la requête et jusqu’à 300 enregistrements peuvent être créés ou mis à jour en même temps.

>[!NOTE]
>
> La mise à jour des champs d’entreprise à l’aide du point de terminaison [Sync Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) n’est pas prise en charge. Utilisez plutôt le point de terminaison [Sync Entreprises](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) .

>[!NOTE]
>
> Lors de la création ou de la mise à jour de la valeur d’un email sur un enregistrement Personne, seuls les caractères ASCII sont pris en charge dans le champ Adresse email .

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

Dans cette requête, vous voyez deux champs importants, `action` et `lookupField`.  `action` spécifie le type d’opération de la requête et peut être `createOrUpdate`, `createOnly`, `updateOnly` ou `createDuplicate`. S’il est omis, l’action est définie par défaut sur `createOrUpdate`.  Le paramètre `lookupField` spécifie la clé à utiliser lorsque l’action est `createOrUpdate` ou `updateOnly`. Si `lookupField` est omis, la clé par défaut est `email`.

Par défaut, la partition par défaut est utilisée. Vous pouvez éventuellement spécifier le paramètre `partitionName`, qui ne fonctionne que si l’action est `createOnly` ou `createOrUpdate`. Pour que `partitionName` fonctionne comme critère de déduplication supplémentaire, il doit faire partie du type de source dans les règles de déduplication personnalisées. Lors d’une opération de mise à jour, si une piste n’existe pas dans la partition spécifiée, une erreur est renvoyée. Si l’utilisateur de l’API uniquement n’est pas autorisé à accéder à la partition spécifiée, une erreur est renvoyée.

Le champ `id` ne peut être inclus qu’en tant que paramètre lors de l’utilisation de l’action `updateOnly`, car `id` est une clé unique gérée par le système.

La requête doit également comporter un paramètre `input`, qui est un tableau d’enregistrements de piste. Chaque enregistrement de piste est un objet JSON comportant un nombre indéfini de champs de piste. Les clés incluses dans un enregistrement doivent être uniques pour cet enregistrement et toutes les chaînes JSON doivent être codées au format UTF-8. Le champ `externalCompanyId` peut être utilisé pour lier l’enregistrement de piste à un enregistrement de société. Le champ `externalSalesPersonId` peut être utilisé pour lier l’enregistrement de piste à un enregistrement de personne effectuant le vente.

Remarque : lorsque vous effectuez des requêtes de suppression de prospect simultanément ou dans une succession rapide, des enregistrements en double peuvent se produire lors de l’exécution de plusieurs requêtes avec la même valeur de clé si un appel ultérieur avec la même valeur est effectué avant le premier renvoi. Cela peut être évité en utilisant `createOnly` ou `updateOnly` selon le cas, ou en mettant en file d’attente des appels et en attendant que votre appel revienne avant d’effectuer les appels de restauration suivants avec la même clé.

## Champs

L’objet de piste contient des champs standard et éventuellement des champs personnalisés. Les champs standard sont présents dans chaque abonnement de Marketo Engage, tandis que les champs personnalisés sont créés par l’utilisateur selon les besoins. Chaque définition de champ comprend un ensemble d’attributs qui décrivent le champ. Les attributs sont par exemple le nom d’affichage, le nom de l’API et dataType. Ces attributs sont connus collectivement sous le nom de métadonnées.

Les points de terminaison suivants vous permettent d’interroger, de créer et de mettre à jour des champs sur l’objet de piste. Ces API exigent que l’utilisateur de l’API propriétaire dispose d’un rôle avec l’une des autorisations de champ standard de schéma de lecture-écriture ou de champ personnalisé de schéma de lecture-écriture.

## Champs de requête

La requête sur les champs de piste est simple. Vous pouvez interroger un champ de piste unique par nom d’API ou interroger l’ensemble de tous les champs de piste. Il est possible de récupérer les champs standard et personnalisés en fonction des autorisations de rôle utilisées. Les champs masqués sont également récupérés.

## Par nom

Le point de fin Get Lead Field by Name récupère les métadonnées d’un champ unique sur l’objet lead. Le paramètre de chemin d’accès fieldApiName obligatoire spécifie le nom d’API du champ. La réponse est semblable au point de terminaison Décrire le prospect , mais contient des métadonnées supplémentaires telles que l’attribut isCustom , qui indique si le champ est un champ personnalisé.

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

Le point de fin Get Lead Fields récupère les métadonnées de tous les champs de l’objet de piste, y compris. Par défaut, un maximum de 300 enregistrements est renvoyé. Vous pouvez utiliser le paramètre de requête `batchSize` pour réduire ce nombre. Si l’attribut `moreResult` est défini sur true, cela signifie que d’autres résultats sont disponibles. Continuez à appeler ce point de terminaison jusqu’à ce que l’attribut `moreResult` renvoie false, ce qui signifie qu’il n’y a aucun résultat disponible. Les `nextPageToken` renvoyés par cette API doivent toujours être réutilisés pour la prochaine itération de cet appel.

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

Le point de fin Créer des champs de piste crée un ou plusieurs champs personnalisés sur l’objet de piste. Ce point de terminaison fournit des fonctionnalités comparables à celles disponibles dans l’interface utilisateur de Marketo Engage. Vous pouvez créer jusqu’à 100 champs personnalisés à l’aide de ce point de terminaison.
Examinez attentivement chaque champ que vous créez dans votre instance de production de Marketo Engage à l’aide de l’API.  Une fois un champ créé, vous ne pouvez plus le supprimer (vous pouvez uniquement le masquer). La prolifération des champs inutilisés est une mauvaise pratique qui encombrera votre instance.

Le paramètre d’entrée requis est un tableau d’objets de champ de piste. Chaque objet contient un ou plusieurs attributs. Les attributs requis sont `displayName`, `name` et `dataType` qui correspondent respectivement au nom d’affichage de l’interface utilisateur du champ, au nom de l’API du champ et au type de champ.  Vous pouvez éventuellement spécifier `description`, `isHidden`, `isHtmlEncodingInEmail` et `isSensitive`.

Il existe quelques règles associées au nom et à l’attribution de noms `displayName`. L’attribut name doit être unique, commencer par une lettre et contenir uniquement des lettres, des chiffres ou un trait de soulignement. `displayName` doit être unique et ne peut pas contenir de caractères spéciaux.  Une convention d’affectation de nom courante consiste à appliquer une casse de chameau à `displayName` pour produire un nom. Par exemple, un `displayName` de &quot;Mon champ personnalisé&quot; produirait un nom de &quot;myCustomField&quot;.

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

Le point de terminaison Mettre à jour le champ de piste met à jour un seul champ personnalisé sur l’objet de piste. La plupart du temps, les opérations de mise à jour de champ effectuées à l’aide de l’interface utilisateur de Marketo Engage sont réalisables à l’aide de l’API. Quelques différences sont résumées dans le tableau ci-dessous.

<table>
<tbody>
<tr>
<td style="width: 26.5306%;" rowspan="2"><strong>Attribut</strong></td>
<td style="width: 35%;" colspan="2"><strong>Champ standard</strong></td>
<td style="width: 38.2654%;" colspan="2"><strong>Champ personnalisé</strong></td>
</tr>
<tr>
<td style="width: 17.449%;"><strong>Mise à jour par API ?</strong></td>
<td style="width: 17.551%;"><strong>Mise à jour par l’interface utilisateur ?</strong></td>
<td style="width: 19.3878%;"><strong>Mise à jour par API ?</strong></td>
<td style="width: 18.8776%;"><strong>Mise à jour par l’interface utilisateur ?</strong></td>
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
<td style="width: 26.5306%;">isSensitive</td>
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

Le paramètre de chemin `fieldApiName` requis spécifie le nom de l’API du champ à mettre à jour. Le paramètre d’entrée requis est un tableau contenant un seul objet de champ de piste.  L’objet de champ contient un ou plusieurs attributs.

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

Push Lead est une alternative à la synchronisation des pistes vers Marketo, principalement conçue pour permettre un plus grand degré de déclenchement que les pistes de synchronisation standard (similaire à l’utilisation d’un formulaire Marketo). Outre la synchronisation des champs de piste, ce point de terminaison permet l’association de pistes en fonction des valeurs de cookie, qui sont transmises au point de terminaison . Pour ce faire, transmettez la valeur `mkt_tok` générée en cliquant sur un email Marketo ou en transmettant un nom de programme dans l’appel . Ce point de terminaison crée également une activité déclenchable unique, associée à un programme et/ou à une campagne dans Marketo. Cela permet au déclenchement des événements de capture de piste attribués à une campagne ou un programme spécifique de déclencher les workflows associés depuis Marketo.

L’interface Push Lead est très similaire aux pistes de synchronisation. Toutes les mêmes clés primaires sont valides et les mêmes noms d’API sont utilisés pour les champs (il n’y a aucun paramètre d’action, car il s’agit toujours d’une opération upsert). Les paramètres `programName` et d’entrée sont requis, et les paramètres `lookupField`, `source` et `reason` sont facultatifs. Le paramètre d’entrée est un tableau d’objets de piste. L&#39;activité résultante est attribuée au programme nommé correspondant. Les paramètres `source` et `reason` sont des champs de chaîne arbitraires qui peuvent être ajoutés à la requête pour incorporer ces valeurs dans les activités résultantes. Elles peuvent être utilisées comme contraintes dans les déclencheurs correspondants (le prospect est transféré vers Marketo) et les filtres (le prospect a été transféré vers Marketo).

Remarque concernant les activités anonymes. Si vous souhaitez associer des activités anonymes antérieures à la piste nouvellement créée, ne spécifiez pas d’attribut de cookies dans l’objet de piste, puis appelez Associer la piste après la piste push. Si vous souhaitez créer une piste sans historique d’activité, indiquez simplement l’attribut cookies dans l’objet de piste.

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

Pour transmettre le paramètre `mkt_tok`, affectez la valeur au membre mktToken dans un enregistrement de piste dans le paramètre d’entrée comme suit.

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

Submit Form est une alternative de synchronisation des pistes vers Marketo. Il est conçu pour offrir des fonctionnalités équivalentes à un envoi de formulaire Marketo. Cela permet au déclenchement des événements de capture de piste attribués à une campagne ou un programme spécifique de déclencher les workflows associés depuis Marketo.

Le point de fin Submit Form prend en charge les fonctionnalités suivantes :

* Met à niveau un enregistrement de piste à l’aide du champ de courrier électronique comme clé primaire
* Crée une activité &quot;Remplir le formulaire&quot; associée à un programme et/ou à une campagne
* Permet l’association de pistes en fonction de la valeur du cookie.
* Validation des champs de formulaire

L’envoi d’un formulaire suit le modèle standard de la base de données de piste. Un seul enregistrement d’objet est transmis dans le membre d’entrée requis du corps JSON d’une requête de POST. Le membre `formId` requis contient l’identifiant de formulaire Marketo cible.

Le `programId` facultatif peut être utilisé pour spécifier le programme auquel ajouter le prospect et/ou spécifier le programme auquel ajouter des champs personnalisés de membre du programme. Si `programId` est fourni, le prospect est ajouté au programme et tous les champs de membre du programme présents dans le formulaire sont également ajoutés. Notez que le programme spécifié doit se trouver dans le même espace de travail que le formulaire. Si le formulaire ne contient pas de champs personnalisés de membre de programme et que `programId` n’est pas fourni, le prospect n’est pas ajouté à un programme. Si le formulaire réside dans un programme et que `programId` n’est pas fourni, ce programme est utilisé lorsqu’un ou plusieurs champs personnalisés de membre du programme sont présents dans le formulaire.

Dans l’enregistrement d’entrée, l’objet `leadFormFields` est requis. Cet objet contient une ou plusieurs paires nom/valeur qui correspondent aux champs de formulaire à renseigner.  Tous les champs spécifiés doivent être définis dans le formulaire spécifié. Le nom est le nom de l’API REST pour le champ. Notez que le champ `email` est obligatoire.

L’objet membre `visitorData` est facultatif et contient des paires nom/valeur qui correspondent aux données de visite de page `pageURL`, `queryString`, `leadClientIpAddress` et `userAgentString`. Peut être utilisé pour renseigner des champs d’activité supplémentaires à des fins de filtrage et de déclenchement.

La chaîne du membre du cookie est facultative et vous permet d’associer un cookie Munchkin à un enregistrement de personne dans Marketo. Lorsqu’une nouvelle piste est créée, toute activité anonyme antérieure est associée à cette piste, sauf si la valeur du cookie avait été précédemment associée à un autre enregistrement connu. Si la valeur du cookie a été précédemment associée, les nouvelles activités sont suivies par rapport à l’enregistrement, mais les anciennes activités ne seront pas migrées hors de l’enregistrement connu existant. Pour créer une piste sans historique d’activité, il vous suffit d’omettre le membre du cookie.

De nouveaux pistes sont créés dans la partition principale de l’espace de travail dans lequel se trouve le formulaire.

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

Ici, nous pouvons voir les détails correspondants de l’activité &quot;Remplir le formulaire&quot; depuis l’interface utilisateur du Marketo Engage :

![Remplissage de l’interface utilisateur du formulaire](assets/fill_out_form_activity_details.png)

## Fusionner

Il est parfois nécessaire de fusionner des enregistrements en double et Marketo facilite cette opération via l’API Merge Leads. La fusion des pistes combinera leurs logs d’activité, programmes, campagnes et listes, ainsi que les informations CRM, et fusionnera toutes leurs valeurs de champ en un seul enregistrement. Fusionner les pistes prend un ID de piste comme paramètre de chemin d’accès, et soit un seul `leadId` comme paramètre de requête, soit une liste d’identifiants séparés par des virgules dans le paramètre `leadIds`.

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

La piste spécifiée dans le paramètre de chemin est la piste gagnante. Par conséquent, si des champs sont en conflit entre les enregistrements fusionnés, la valeur du gagnant est prise, sauf si le champ de l’enregistrement gagnant est vide et que le champ correspondant de l’enregistrement perdant ne l’est pas. Les pistes spécifiées dans les paramètres `leadId` ou `leadIds` sont les pistes perdantes.

Si vous avez un abonnement activé pour la synchronisation SFDC, vous pouvez également utiliser le paramètre `mergeInCRM` dans votre requête. Si la valeur est définie sur true, la fusion correspondante dans votre CRM est également effectuée. Si les deux pistes se trouvent dans SFDC et que l’une est une piste CRM et l’autre est un contact CRM, alors le gagnant est le contact CRM (quel que soit le prospect spécifié comme gagnant). Si l’une des pistes se trouve dans SFDC et que l’autre est Marketo uniquement, le gagnant est le prospect SFDC (quelle que soit la piste spécifiée comme gagnante).

## Associer l’activité web

Grâce au suivi des pistes (Munchkin), Marketo enregistre l’activité web des visiteurs sur votre site web et vos pages d’entrée Marketo. Ces activités, Visites et Clics, sont enregistrées avec une clé qui correspond à un cookie &quot;_mkto_trk&quot; défini dans le navigateur de l’prospect, et Marketo l’utilise pour effectuer le suivi des activités de la même personne. En règle générale, l’association à des enregistrements de piste survient lorsqu’un prospect clique à partir d’un courrier électronique Marketo ou remplit un formulaire Marketo, mais il arrive qu’une association puisse être déclenchée par un autre type d’événement et vous pouvez utiliser le point de terminaison Associer le prospect pour ce faire. Le point de terminaison utilise l’identifiant de l’enregistrement de piste connu comme paramètre de chemin d’accès et la valeur du cookie &quot;_mkto_trk&quot; dans le paramètre de requête du cookie.

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

Si un cookie est déjà associé à un enregistrement de piste connu, l’utilisation de cette API sur un autre enregistrement de piste entraîne l’enregistrement d’une nouvelle activité web par rapport à cet enregistrement, mais ne déplace aucune activité web existante vers le nouvel enregistrement.
Adhésion

Les enregistrements de piste peuvent également être récupérés en fonction de l’appartenance à une liste statique ou à un programme. De plus, vous pouvez récupérer toutes les listes statiques, tous les programmes ou toutes les campagnes intelligentes dont un prospect est membre.

La structure de la réponse et les paramètres facultatifs sont identiques à ceux de Get Leads by Filter Type, bien que filterType et filterValues ne puissent pas être utilisés avec cette API.
Pour accéder à l’ID de liste via l’interface utilisateur de Marketo, accédez à la liste. La liste `id` se trouve dans l’URL de la liste statique, `https://app-****.marketo.com/#ST1001A1`. Dans cet exemple, 1001 est le `id` de la liste.

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

Le point de terminaison Get Lists by Lead Id utilise un paramètre de chemin d’accès `id` d’enregistrement de piste et renvoie tous les enregistrements de liste statique dont le prospect est membre.

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

L’appartenance au programme peut être récupérée de la même manière que les listes. Les mêmes paramètres de requête facultatifs sont disponibles lors de l’appel du point de terminaison Get Leads by Program Id et de la transmission du paramètre de chemin d’accès `programId`.

Vous pouvez éventuellement transmettre un paramètre de champ contenant une liste de noms de champ séparés par des virgules à renvoyer. Si le paramètre fields n&#39;est pas inclus dans cette requête, les champs par défaut suivants seront renvoyés : `email`, `updatedAt`, `createdAt`, `lastName`, `firstName`, `membership` et `id`. Lorsque vous demandez une liste de champs, si un champ particulier est demandé, mais n’est pas renvoyé, la valeur est implicitement nulle.

La structure de la réponse est très similaire, car chaque élément du tableau de résultats est une piste, sauf que chaque enregistrement comporte également un objet enfant appelé &quot;membership&quot;. Cet objet d’appartenance comprend des données sur la relation du prospect avec le programme indiqué dans l’appel, affichant toujours ses `progressionStatus`, `acquiredBy`, `reachedSuccess` et `membershipDate`. Si le programme parent est également un programme d’engagement, l’appartenance aura des membres `stream`, `nurtureCadence` et `isExhausted` pour indiquer sa position et son activité dans le programme d’engagement.

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

Le point de terminaison Get Programmes by Lead Id utilise un paramètre de chemin d’accès d’identifiant d’enregistrement de piste et renvoie tous les enregistrements de programme dont le prospect est membre. Les paramètres facultatifs `filterType` et `filterValues` vous permettent de filtrer selon l’ID de programme.

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

Le point de terminaison Get Smart Campaigns by Lead Id applique un paramètre de chemin d’accès à l’ID d’enregistrement de piste et renvoie tous les enregistrements de campagne dynamique dont il fait partie.

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

La suppression de pistes est simple à l’aide du point de terminaison Supprimer les pistes .  Spécifiez les ID de piste à supprimer à l’aide des attributs id dans le corps.  Le maximum est de 300 pistes par requête.  Utilisez l’en-tête Content-Type: application/json .

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

* Sociétés via le champ externalCompanyId sur l’enregistrement de piste
* Salesjeudi par l’intermédiaire du champ externalSalesPersonId dans l’enregistrement de piste
* Programmes par appartenance à un programme
* Listes par abonnement à une liste
* Activités via le champ leadId dans l’activité
* Segmentation par le biais de champs de segment individuels dans l’enregistrement de piste
* Partitions via leadPartitionId sur l’enregistrement de piste

## Délais d’expiration

Les points de fin de pistes ont un délai d’expiration de 30 s, sauf indication ci-dessous :

* Pistes de synchronisation : 90 s
* Responsable associé : 60 s
* Fusionner les pistes : 180 s
* Mise à jour de la partition de piste : 60 s
* Push Lead to Marketo : 90 s
* Obtenir des pistes par type de filtre : 60 s
* Obtenir des pistes par identifiant de liste : 60 s
