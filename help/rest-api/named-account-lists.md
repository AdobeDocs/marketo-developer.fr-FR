---
title: Listes de comptes nommés
feature: REST API
description: Découvrez comment gérer les listes de comptes nommés Marketo avec l’API REST, y compris les autorisations, les champs, le filtrage et les points d’entrée pour la requête, la création, la mise à jour et la suppression.
exl-id: 98f42780-8329-42fb-9cd8-58e5dbea3809
TQID: https://experienceleague.adobe.com/18lMhheW21Gz1-3TMHwleHhmLTOqJsZSQ5aqkbbchhM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 686
ht-degree: 3%

---

# Listes de comptes nommés

[Référence des points d’entrée des listes de comptes nommés](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Account-Lists)

Les [listes de comptes nommés](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/target-account-management/target/account-lists) sont des ensembles de comptes nommés dans Marketo. Utilisez-les pour la catégorisation, l’enrichissement des données et le filtrage intelligent des campagnes.

Les API Named Account List vous permettent de gérer à distance des ressources de liste et leur appartenance.
`Content`

## Autorisations

L’autorisation requise dépend de l’opération :

- Listes des comptes nommés par la requête : liste des comptes nommés en lecture seule ou liste des comptes nommés en lecture-écriture.
- Créer, mettre à jour ou supprimer des listes : liste des comptes nommés en lecture-écriture.
- Appartenance à la liste de requête : compte nommé en lecture seule ou compte nommé en lecture-écriture.
- Gérer l’appartenance à une liste : compte nommé en lecture-écriture.

## Modèle

Les listes de comptes nommés comportent un ensemble limité de champs standard et ne prennent pas en charge les champs personnalisés.
`Named Account List Field`

| Nom | Type de données | Mise à jour possible | Notes |
| --- | --- | --- | --- |
| marketoGUID | Chaîne | false | Identifiant de chaîne unique de la liste des comptes nommés. Ce champ est géré par le système et n’est pas autorisé en tant que champ lors de la création d’un enregistrement. Champ utilisé par « dedupeBy »:« idField » lors d&#39;une création ou d&#39;une mise à jour. |
| name | Chaîne | True | Nom de la liste. Champ utilisé par « dedupeBy »:« dedupeFields » lors d&#39;une création ou d&#39;une mise à jour. |
| createdAt | Datetime | false | Date et heure de création de la liste. Ce champ est géré par le système et n’est pas autorisé en tant que champ lors de la création ou de la mise à jour d’un enregistrement. |
| updatedAt | Datetime | false | Date et heure de la dernière mise à jour de la liste. Ce champ est géré par le système et n’est pas autorisé en tant que champ lors de la création ou de la mise à jour d’un enregistrement. |
| type | Chaîne | false | Type de la liste. Peut avoir une valeur « default » ou « external ». Les listes externes sont celles créées par la vue Compte CRM . |

## Requête

Les requêtes de liste de comptes nommés prennent en charge deux filterTypes : « dedupeFields » et « idField ». Définissez le champ dans le paramètre de requête `filterType` et indiquez les valeurs dans `filterValues as` une liste séparée par des virgules.

Les filtres `nextPageToken` et `batchSize` sont facultatifs.

```http
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

Créez et mettez à jour des enregistrements de liste de comptes nommés à l’aide du modèle de base de données de leads standard. Les listes de comptes nommés ne comportent qu’un seul champ modifiable : `name`.

Le point d’entrée prend en charge deux types d’actions standard : « createOnly » et « updateOnly ». `action defaults` de « createOnly ».

Vous pouvez spécifier la `dedupeBy parameter` facultative lorsque l’action est `updateOnly`. Les valeurs autorisées sont « dedupeFields », qui correspond à « name », et « idField », qui correspond à « marketoGUID ».

Dans les modes de `createOnly`, seul le champ « name » est autorisé comme `dedupeBy`. Vous pouvez envoyer jusqu’à 300 enregistrements à la fois.

```http
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

Supprimez les listes de comptes nommés à l’aide de la `name` ou de la `marketoGUID` de la liste. Pour sélectionner la clé, transmettez « dedupeFields » pour le nom ou « idField » pour marketoGUID dans le membre `deleteB` de la requête.

Si cette valeur n’est pas définie, la valeur par défaut est dedupeFields. Vous pouvez supprimer jusqu’à 300 enregistrements à la fois.

```http
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

Si aucun enregistrement n’est trouvé pour une clé, l’élément de résultat correspondant comporte un `status` de « ignoré ». Elle comprend également une raison avec un code et un message qui décrivent l’échec.

## Gestion de l’appartenance

### Appartenance à la requête

Interrogez l’appartenance à la liste des comptes nommés en fournissant la `i` de la liste des comptes. Les paramètres facultatifs sont les suivants :

-`field` - liste de champs séparés par des virgules à inclure dans les enregistrements de réponse
-`nextPageToke` - pour paginer dans le jeu de résultats
-`batchSiz` - pour indiquer le nombre d&#39;enregistrements à renvoyer

Si `field` n’est pas défini`marketoGUI`,`nam`, `createdA` et `updatedA` sont renvoyés. `batchSiz` a une valeur maximale et une valeur par défaut de 300.

```http
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

Ajoutez des comptes nommés à une liste de comptes nommés à l’aide de leur marketoGUID. Vous pouvez ajouter jusqu’à 300 enregistrements à la fois.

```http
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

La suppression d’enregistrements d’une liste de comptes utilise un chemin d’accès différent, mais la même interface. Fournissez un `marketoGUI` pour chaque enregistrement à supprimer. Vous pouvez supprimer jusqu’à 300 enregistrements à la fois.

```http
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

- Le délai d’expiration des points d’entrée de liste de comptes nommés est de 30, sauf indication contraire.
- Le délai d’expiration de la synchronisation des listes de comptes nommés est de 60 s.
- Le délai d’expiration de la suppression des listes de comptes nommés est de 60 s.
- Le délai d’expiration de l’option Obtenir les listes de comptes nommés est de 60 ans.
- Le délai d’expiration de l’option Ajouter des membres de liste de comptes nommés est de 60 s.
- Le délai d’expiration de la suppression des membres de la liste des comptes nommés est de 60 s.
- Le délai d’expiration de la fonction Obtenir les membres de la liste des comptes nommés est de 60 ans.
