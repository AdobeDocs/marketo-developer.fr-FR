---
title: "Listes de comptes nommés"
feature: REST API
description: "Configurer des listes de comptes nommés."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 3%

---


# Listes de comptes nommés

[Référence des points de fin des listes de comptes nommés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Account-Lists)

[Listes de comptes nommés](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/target-account-management/target/account-lists) dans Marketo représentent des collections de comptes nommés. Ils peuvent être utilisés pour un large éventail de cas, notamment la catégorisation, l’enrichissement des données et le filtrage de campagne intelligent. Les API de liste de comptes nommés permettent de gérer à distance ces ressources de liste et leur appartenance.
`Content`

## Permissions

Pour interroger les listes de comptes nommés, vous devez disposer de l’autorisation Lecture seule de la liste des comptes nommés ou Lecture-Écriture de la liste des comptes nommés . Pour créer, mettre à jour ou supprimer des listes, l’autorisation Lecture-Écriture sur liste de comptes nommés est requise. L’appartenance à une liste de requêtes nécessite les autorisations de compte nommé en lecture seule ou de compte nommé en lecture-écriture, tandis que la gestion de l’appartenance nécessite les autorisations de compte nommé en lecture-écriture.

## Modèle

Les listes de comptes nommés comportent un nombre limité de champs standard et ne sont pas extensibles avec des champs personnalisés.
`Named Account List Field`

| Nom | Type de données | Mise à jour possible | Notes |
|---|---|---|---|
| marketoGUID | Chaîne | false | Identifiant de chaîne unique de la liste de comptes nommés. Ce champ est géré par le système et n’est pas autorisé en tant que champ lors de la création d’un enregistrement. Champ utilisé par &quot;dedupeBy&quot;:&quot;idField&quot; lors de l’exécution d’une création ou d’une mise à jour. |
| name | Chaîne | Vrai | Nom de la liste. Champ utilisé par &quot;dedupeBy&quot;:&quot;dedupeFields&quot; lors de la création ou de la mise à jour. |
| createdAt | Datetime | false | Date et heure de la création de la liste. Ce champ est géré par le système et n’est pas autorisé en tant que champ lors de la création ou de la mise à jour d’un enregistrement. |
| updatedAt | Datetime | false | Date et heure de la dernière mise à jour de la liste. Ce champ est géré par le système et n’est pas autorisé en tant que champ lors de la création ou de la mise à jour d’un enregistrement. |
| type | Chaîne | false | Type de la liste. Peut avoir une valeur &quot;default&quot; ou &quot;external&quot;. Les listes externes sont celles créées par la vue Compte CRM. |


## Requête

Interroger des listes de comptes est simple et facile. Actuellement, il n’existe que deux filterTypes valides pour interroger les listes de comptes nommés : &quot;dedupeFields&quot; et &quot;idField&quot;. Le champ sur lequel filtrer est défini dans la variable `filterType` du paramètre de la requête, et les valeurs sont définies dans `filterValues as` une liste séparée par des virgules. La variable `nextPageToken` et `batchSize` les filtres sont également des paramètres facultatifs.

```
GET /rest/v1/namedAccountLists.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fb,dff23271-f996-47d7-984f-f2676861b5fc
```

```json
{ 
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [ 
      { 
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "name": "Saas List",
         "createdAt": "xxxxxxxx",
         "updatedAt": "xxxxxxxx",
         "type": "default",
         "updateable": true
      },
      { 
         "seq": 1,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc",
         "name": "My Account List",
         "createdAt": "xxxxxxxx",
         "updatedAt": "xxxxxxxx",
         "type": "default",
         "updateable": true
      }
   ]
}
```

## Créer et mettre à jour

La création et la mise à jour d’enregistrements de liste de comptes nommés suit les schémas établis pour d’autres opérations de création et de mise à jour de la base de données de piste. Gardez à l’esprit que les listes de comptes nommés ne comportent qu’un seul champ modifiable, `name`.

Le point de terminaison autorise les deux types d’action standard : &quot;createOnly&quot; et &quot;updateOnly&quot;.  La variable `action defaults` à &quot;createOnly&quot;.

Le paramètre facultatif `dedupeBy parameter` peut être spécifié si l’action est `updateOnly`.  Les valeurs admises sont &quot;dedupeFields&quot; (correspondant à &quot;name&quot;) ou &quot;idField&quot; (correspondant à &quot;marketoGUID&quot;).  Dans `createOnly` modes, seul &quot;name&quot; est autorisé en tant que `dedupeBy` champ . Vous pouvez envoyer jusqu’à 300 enregistrements à la fois.

```
POST /rest/v1/namedAccountLists.json
```

```json
{ 
   "action": "createOnly",
   "dedupeBy": "dedupeFields",
   "input": [ 
      { 
         "name": "SAAS List"
      },
      { 
         "name": "Manufacturing (Domestic)"
      }
   ]
}
```

```json
{ 
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [ 
      { 
         "seq": 0,
         "status": "created",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
      },
      { 
         "seq": 1,
         "status": "created",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc"
      }
   ]
}
```

## Supprimer

La suppression des listes de comptes nommés est simple et peut se faire en fonction de l’une des options suivantes : `name`, ou la variable `marketoGUID` de la liste. Pour sélectionner la clé que vous souhaitez utiliser, transmettez &quot;dedupeFields&quot; pour le nom ou &quot;idField&quot; pour marketoGUID dans la variable`deleteB` membre de votre requête. Si elle est désactivée, cette valeur est définie par défaut sur dedupeFields. Vous pouvez supprimer jusqu’à 300 enregistrements à la fois.

```
POST /rest/v1/namedAccountLists/delete.json
```

```json
{ 
   "deleteBy": "dedupeFields",
   "input": [ 
      { 
         "name": "Saas List"
      },
      { 
         "name": "B2C List"
      },
      { 
         "name": "Launchpoint Partner List"
      }
   ]
}
```

```json
{ 
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [ 
      { 
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      { 
         "seq": 1,
         "id": "dff23271-f996-47d7-984f-f2676861b5fc",
         "status": "deleted"
      },
      { 
         "seq": 2,
         "status": "skipped",
         "reasons": [ 
            { 
               "code": "1013",
               "message": "Record not found"
            }
         ]
      }
   ]
}
```

Si un enregistrement est introuvable pour une clé donnée, l’élément de résultat correspondant comporte une`status` de &quot;saut&quot; et d’une raison avec un code et un message décrivant l’échec, comme illustré dans l’exemple ci-dessus.

## Gestion de l’appartenance

### Appartenance à une requête

L’interrogation de l’appartenance à une liste de comptes nommés est simple et nécessite uniquement la variable`i` de la liste des comptes. Les paramètres facultatifs sont les suivants :

-`field` - une liste de champs séparés par des virgules à inclure dans les enregistrements de réponse -`nextPageToke` - pour la pagination via le jeu de résultats -`batchSiz` - pour spécifier le nombre d&#39;enregistrements à renvoyer

If`field` n’est pas défini, puis`marketoGUI`,`nam`, `createdA`, et`updatedA` est renvoyée. `batchSiz` a une valeur maximale et une valeur par défaut de 300.

```
GET /rest/v1/namedAccountList/{id}/namedAccounts.json
```

```json
{ 
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [ 
      { 
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "name": "Saas List",
         "createdAt": "2017-02-01T00:00:00Z",
         "updatedAt": "2017-03-05T17:21:15Z"
      },
      { 
         "seq": 1,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc",
         "name": "My Account List",
         "createdAt": "2017-02-01T00:00:00Z",
         "updatedAt": "2017-03-05T17:21:15Z"
      }
   ]
}
```

### Ajouter des membres

Les comptes nommés peuvent facilement être ajoutés à une liste de comptes nommés. Les comptes ne peuvent être ajoutés qu’à l’aide de leur marketoGUID. Vous pouvez ajouter jusqu’à 300 enregistrements à la fois.

```
POST /rest/v1/namedAccountList/{id}/namedAccounts.json
```

```json
{
    "input": [
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb" 
        },
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        }
    ]
}
```

```json
{
    "requestId": "string",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        },
        {
            "seq": 1,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        }
    ],
    "success": true,
}
```

### Supprimer des membres

La suppression d’enregistrements d’une liste de comptes a un chemin d’accès différent, mais la même interface, qui nécessite une`marketoGUI` pour chaque enregistrement à supprimer. Vous pouvez supprimer jusqu’à 300 enregistrements à la fois.

```
POST /rest/v1/namedAccountList/{id}/namedAccounts/remove.json
```

```json
{
    "input": [
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb" 
        },
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        }
    ]
}
```

```json
{
    "requestId": "string",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        },
        {
            "seq": 1,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        }
    ],
    "success": true
}
```

## Délais d’expiration

- Les points de fin de liste de comptes nommés ont un délai d’expiration de 30 s, sauf indication ci-dessous.
   - Synchroniser les listes de comptes nommés : 60 s 
   - Supprimer les listes de comptes nommés : 60 s 
   - Listes de comptes nommés : 60 s 
   - Ajouter des membres de liste de comptes nommés : 60 s 
   - Supprimer les membres de la liste de comptes nommés : 60 s 
   - Obtenir les membres de la liste de comptes nommés : 60 s
