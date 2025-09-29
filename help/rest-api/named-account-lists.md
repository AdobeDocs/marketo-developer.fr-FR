---
title: Listes de comptes nommés
feature: REST API
description: Découvrez comment gérer les listes de comptes nommés Marketo avec l’API REST, y compris les autorisations, les champs, le filtrage et les points d’entrée pour la requête, la création, la mise à jour et la suppression.
exl-id: 98f42780-8329-42fb-9cd8-58e5dbea3809
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 2%

---

# Listes de comptes nommés

[Référence des points d’entrée des listes de comptes nommés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Account-Lists)

[Les listes de comptes nommés](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/target-account-management/target/account-lists) dans Marketo représentent des ensembles de comptes nommés. Elles peuvent être utilisées dans de nombreux cas, notamment pour la catégorisation, l’enrichissement des données et le filtrage intelligent des campagnes. Les API de liste des comptes nommés permettent la gestion à distance de ces ressources de liste et de leur appartenance.
`Content`

## Autorisations

Pour interroger les listes de comptes nommés, vous devez disposer de l&#39;autorisation Lecture seule liste des comptes nommés ou Lecture-écriture liste des comptes nommés . Pour créer, mettre à jour ou supprimer des listes, l’autorisation Lecture-écriture sur la liste des comptes nommés est requise. L’appartenance à une liste de requêtes nécessite les autorisations de compte nommé en lecture seule ou de compte nommé en lecture-écriture, tandis que la gestion de l’appartenance nécessite les autorisations de compte nommé en lecture-écriture.

## Modèle

Les listes de comptes nommés comportent un nombre limité de champs standard et ne sont pas extensibles avec des champs personnalisés.
`Named Account List Field`

| Nom | Type de données | Mise à jour possible | Notes |
|---|---|---|---|
| marketoGUID | Chaîne | false | Identifiant de chaîne unique de la liste des comptes nommés. Ce champ est géré par le système et n’est pas autorisé en tant que champ lors de la création d’un enregistrement. Champ utilisé par « dedupeBy »:« idField » lors d&#39;une création ou d&#39;une mise à jour. |
| name | Chaîne | True | Nom de la liste. Champ utilisé par « dedupeBy »:« dedupeFields » lors d&#39;une création ou d&#39;une mise à jour. |
| createdAt | Datetime | false | Date et heure de création de la liste. Ce champ est géré par le système et n’est pas autorisé en tant que champ lors de la création ou de la mise à jour d’un enregistrement. |
| updatedAt | Datetime | false | Date et heure de la dernière mise à jour de la liste. Ce champ est géré par le système et n’est pas autorisé en tant que champ lors de la création ou de la mise à jour d’un enregistrement. |
| type | Chaîne | false | Type de la liste. Peut avoir une valeur « default » ou « external ». Les listes externes sont celles créées par la vue Compte CRM . |

## Requête

L’interrogation des listes de comptes est simple et facile. Actuellement, il n’existe que deux filterTypes valides pour l’interrogation des listes de comptes nommés : « dedupeFields » et « idField ». Le champ sur lequel appliquer un filtre est défini dans le paramètre `filterType` de la requête et les valeurs sont définies `filterValues as` une liste séparée par des virgules. Les filtres `nextPageToken` et `batchSize` sont également des paramètres facultatifs.

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

La création et la mise à jour des enregistrements de liste de comptes nommés suivent les modèles établis pour les autres opérations de création et de mise à jour de la base de données de leads. Gardez à l’esprit que les listes de comptes nommés ne comportent qu’un seul champ modifiable, `name`.

Le point d’entrée autorise deux types d’actions standard : « createOnly » et « updateOnly ».  `action defaults` de « createOnly ».

Le `dedupeBy parameter` facultatif peut être spécifié si l’action est `updateOnly`.  Les valeurs autorisées sont « dedupeFields » (correspondant à « name ») ou « idField » (correspondant à « marketoGUID »).  Dans les modes de `createOnly`, seul le champ « name » est autorisé comme `dedupeBy`. Vous pouvez envoyer jusqu’à 300 enregistrements à la fois.

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

La suppression des listes de comptes nommés est simple et peut être effectuée en fonction du `name` ou de la `marketoGUID` de la liste. Pour sélectionner la clé que vous souhaitez utiliser, transmettez « dedupeFields » pour le nom ou « idField » pour marketoGUID dans le membre `deleteB` de votre requête. Si cette option n’est pas définie, les champs dédupliqués seront utilisés par défaut. Vous pouvez supprimer jusqu’à 300 enregistrements à la fois.

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

Dans le cas où aucun enregistrement n’est trouvé pour une clé donnée, l’élément de résultat correspondant comporte un `status` de « ignoré » et un motif avec un code et un message décrivant l’échec, comme illustré dans l’exemple ci-dessus.

## Gestion de l’appartenance

### Appartenance à la requête

La requête d’appartenance à une liste de comptes nommée est simple et ne nécessite que le `i` de la liste de comptes. Les paramètres facultatifs sont les suivants :

-`field` - liste de champs séparés par des virgules à inclure dans les enregistrements de réponse
-`nextPageToke` - pour paginer dans le jeu de résultats
-`batchSiz` - pour indiquer le nombre d&#39;enregistrements à renvoyer

Si `field` n’est pas défini`marketoGUI`,`nam`, `createdA` et `updatedA` sont renvoyés. `batchSiz` a une valeur maximale et une valeur par défaut de 300.

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

Les comptes nommés peuvent facilement être ajoutés à une liste de comptes nommés. Les comptes ne peuvent être ajoutés qu’en utilisant leur marketoGUID. Vous pouvez ajouter jusqu’à 300 enregistrements à la fois.

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

### Supprimer les membres

La suppression d’enregistrements d’une liste de comptes comporte un chemin d’accès différent, mais la même interface, nécessitant un `marketoGUI` pour chaque enregistrement que vous souhaitez supprimer. Vous pouvez supprimer jusqu’à 300 enregistrements à la fois.

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

## Délais dépassés

- Le délai d’expiration des points d’entrée de liste de comptes nommés est de 30, sauf indication ci-dessous
   - Synchroniser les listes de comptes nommés : 60s
   - Supprimer les listes de comptes nommés : 60s
   - Obtenir les listes de comptes nommés : 60s
   - Ajouter des membres de la liste des comptes nommés : 60s
   - Supprimer les membres de la liste des comptes nommés : 60s
   - Obtenir les membres de la liste des comptes nommés : 60s
