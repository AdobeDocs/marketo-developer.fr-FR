---
title: Gestion des utilisateurs
feature: REST API
description: Guide des API User Management de Marketo pour CRUD sur les utilisateurs, l’authentification basée sur l’en-tête, les rôles et les espaces de travail, la gestion du code d’état, le format de date et d’heure et les points d’entrée de requête.
exl-id: 2a58f496-0fe6-4f7e-98ef-e9e5a017c2de
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1197'
ht-degree: 1%

---

# Gestion des utilisateurs

[Référence du point d’entrée User Management](https://developer.adobe.com/marketo-apis/api/user/)

Marketo fournit un ensemble de points d’entrée User Management qui vous permettent d’effectuer des opérations CRUD sur des enregistrements d’utilisateurs dans Marketo. Les utilisateurs sont créés en envoyant une invitation à un utilisateur, qui définit ensuite un mot de passe et accède pour la première fois à Marketo.

Contrairement aux autres API REST Marketo, lors de l’utilisation des API User Management :

- Vous devez utiliser la méthode d’en-tête HTTP pour envoyer le jeton d’accès à authentifier. Vous ne pouvez pas transmettre le jeton d’accès en tant que paramètre de chaîne de requête. Vous trouverez [ici](authentication.md) plus d’informations sur l’authentification.
- Vous devez sélectionner une autorisation de rôle dans deux groupes différents lors de la création du rôle d’utilisateur pour [Service personnalisé](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) pour l’API REST :
   1. Autorisation « Accéder aux utilisateurs » à partir du groupe [Accéder aux administrateurs](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
   1. « Accéder à l’API User Management » à partir du groupe [API Access](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
- Les corps de la réponse ne contiennent pas l’attribut booléen « success » indiquant le succès ou l’échec d’un appel. Vous devez plutôt évaluer le code de statut de la réponse HTTP. Si un appel réussit, un code d’état 200 est renvoyé. Si un appel échoue, un code d’état de niveau non 200 est renvoyé et le corps de la réponse contient le tableau « errors » standard avec le code d’erreur et le message d’erreur descriptif.
- Le format des chaînes datetime est `yyyyMMdd'T'HH:mm:ss.SSS't'+|-hhmm`. Cela s’applique aux attributs suivants : `createdAt`, `updatedAt`, `expiresAt`.
- Les points d’entrée de l’API User Management ne comportent pas le préfixe « /rest » comme les autres points d’entrée.

## Requête

La prise en charge des requêtes pour User Management offre la possibilité de récupérer tous les utilisateurs, rôles et espaces de travail. Vous pouvez également récupérer un enregistrement utilisateur unique par ID utilisateur ou un enregistrement rôle/espace de travail par ID utilisateur.

### Utilisateur par ID

Le point d’entrée [Obtenir l’utilisateur par ID](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserUsingGET) prend un seul paramètre de chemin d’accès `userid` et renvoie un seul enregistrement utilisateur pour un utilisateur qui a accepté son invitation.

```
GET /userservice/management/v1/users/{userid}/user.json
```

```json
{
  "userid": "jamie@houselannister.com",
  "firstName": "Jamie",
  "lastName": "Lannister",
  "emailAddress": "jamie@lannister.com",
  "optedIn": false,
  "failedLogins": 0,
  "failedDeviceCode": 0,
  "isLocked": false,
  "lockedReason": null,
  "id": 0,
  "apiOnly": false,
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "accessRoleName": "Admin",
      "workspaceId": 0,
      "workspaceName": "AllZones"
    },
    {
      "accessRoleId": 2,
      "accessRoleName":
      "Standard User",
      "workspaceId": 1008,
      "workspaceName": "World"
    }
  ],
  "expiresAt": "2020-12-31T08:00:00.000t+0000",
  "lastLoginAt": "2020-02-05T01:02:23.000t+0000"
}
```

### Utilisateur invité par ID

Le point d’entrée [Obtenir l’utilisateur invité par ID](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getInvitedUserUsingGET) prend un seul paramètre de chemin d’accès `userid` et renvoie un seul enregistrement utilisateur pour un utilisateur « en attente » (n’a pas encore accepté son invitation).

```
GET /userservice/management/v1/users/{userid}/invite.json
```

```json
{
    "id": 25112,
    "firstName": "Jamie",
    "lastName": "Lannister",
    "emailAddress": "jamie@lannister.com",
    "userId": "jamie@lannister.com",
    "subscriptionId": 3381,
    "status": "pending",
    "expiresAt": "20200807T20:49:54.0t+0000",
    "createdAt": "20200731T20:49:54.0t+0000",
    "updatedAt": "20200731T20:49:54.0t+0000"
}
```

### Rôles et espaces de travail par ID

Le point d’entrée [Obtenir les rôles et les espaces de travail par ID](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserRolesAndWorkspacesUsingGET) prend un seul paramètre de chemin d’accès `userid` et renvoie une liste d’enregistrements de rôle utilisateur et d’espace de travail. La réponse contient un tableau avec un objet contenant le rôle, l’identifiant d’espace de travail et le nom de l’utilisateur spécifié.

```
GET /userservice/management/v1/users/{userid}/roles.json
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  },
  {
    "accessRoleId": 2,
    "accessRoleName": "Standard User",
    "workspaceId": 1008,
    "workspaceName": "World"
  }
]
```

### Parcourir les utilisateurs

Le point d’entrée [Get Users](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUsersUsingGET) renvoie une liste de tous les enregistrements d’utilisateur. Le paramètre facultatif `pageSize` est un entier qui spécifie le nombre maximal d’entrées à renvoyer. La valeur par défaut est 20. La valeur maximale est 200. Le paramètre facultatif `pageOffset` est un entier qui spécifie où commencer à récupérer les entrées. Peut être utilisé avec `pageSize`. La valeur par défaut est 0.

```
GET /userservice/management/v1/users/allusers.json
```

```json
[
  {
    "userid": "jamie@lannister.com",
    "firstName": "Jamie",
    "lastName": "Lannister",
    "emailAddress": "jamie@houselannister.com",
    "id": 6785,
    "apiOnly": false
  },
  {
    "userid": "jeoffery@housebaratheon.com",
    "firstName": "Jeoffery",
    "lastName": "Baratheon",
    "emailAddress": "jeoffery@housebaratheon.com",
    "id": 7718,
    "apiOnly": false
  },
  {
    "userid": "rickon@housestark.com",
    "firstName": "Rickon",
    "lastName": "Stark",
    "emailAddress": "rickon@housestark.com",
    "id": 8612,
    "apiOnly": false
  }
]
```

>[!NOTE]
>
>Dans l’exemple de code ci-dessus, la `userid` affichée concerne un client qui a été migré vers Adobe IMS. Les clients qui n’ont pas encore migré verront une adresse e-mail régulière dans le champ `userid` .

### Parcourir les rôles

Le point d’entrée [Get Roles](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getRolesUsingGET) renvoie une liste de tous les enregistrements de rôle.

```
GET /userservice/management/v1/users/roles.json
```

```json
[
    {
        "id": 1,
        "name": "Admin",
        "description": "All permissions",
        "type": "system",
        "hidden": false,
        "onlyAllZones": true,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20100327T18:27:42.0t+0000"
    },
    {
        "id": 2,
        "name": "Standard User",
        "description": "All permissions except Admin",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    },
    {
        "id": 24,
        "name": "RTP Launcher",
        "description": "Role required for launcher in RTP",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20151024T01:45:40.0t+0000",
        "updatedAt": "20171024T23:41:24.0t+0000"
    },
    {
        "id": 25,
        "name": "RTP Editor",
        "description": "Role required for editor in RTP",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20151024T01:45:40.0t+0000",
        "updatedAt": "20171024T23:41:24.0t+0000"
    },
    {
        "id": 101,
        "name": "Analytics User",
        "description": "Has access to Analytics",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    },
    {
        "id": 102,
        "name": "Marketing User",
        "description": "All permissions except Admin",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20100327T18:27:42.0t+0000"
    },
    {
        "id": 103,
        "name": "Web Designer",
        "description": "Has access to Design Studio except approval permission",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    }
]
```

### Parcourir les espaces de travail

Le point d’entrée [Get Workspaces](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getWorkspacesUsingGET) renvoie une liste de tous les enregistrements d’espace de travail.

```
GET /userservice/management/v1/users/workspaces.json
```

```json
[
  {
    "id": 1,
    "name": "Default",
    "description": "Initial workspace for Marketing Activities, Design Studio, and so on.",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20160910T23:08:05.0t+0000",
    "updatedAt": "20160910T23:08:05.0t+0000"
  },
  {
    "id": 1008,
    "name": "World",
    "description": "",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20181119T21:59:36.0t+0000",
    "updatedAt": "20181119T21:59:36.0t+0000"
  },
  {
    "id": 1009,
    "name": "Reproduction - US English - All Leads",
    "description": "A Workspace for recreating customer-reported problems.",
    "globalViz": 1,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20190129T23:36:37.0t+0000",
    "updatedAt": "20190129T23:36:37.0t+0000"
  },
  {
    "id": 1010,
    "name": "US",
    "description": "United States - Qualified Leads",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20190322T15:55:40.0t+0000",
    "updatedAt": "20190322T15:55:40.0t+0000"
  }
]
```

## Inviter un utilisateur

Sur les [abonnements intégrés à Adobe IMS](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), ce point d’entrée prend uniquement en charge les invitations des [utilisateurs API uniquement](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Pour inviter des [utilisateurs standard](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), utilisez plutôt l’API [Adobe User Management](https://developer.adobe.com/umapi/).

Le point d’entrée [Inviter un utilisateur](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/inviteUserUsingPOST) pour envoyer une invitation par e-mail « Bienvenue dans Marketo » au nouvel utilisateur. Le corps de l’e-mail contient un lien « Connexion à Marketo » qui permet à l’utilisateur d’accéder à Marketo pour la première fois. Pour accepter l’invitation, le destinataire de l’e-mail clique sur le lien « Se connecter à Marketo », crée son mot de passe et accède à Marketo. Tant que le processus d’acceptation n’est pas terminé, l’invitation est « en attente » et l’enregistrement de l’utilisateur ne peut pas être modifié. Une invitation en attente expire sept jours après avoir été envoyée. Vous trouverez plus d’informations sur la gestion des utilisateurs [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users).

Les paramètres sont transmis dans le corps de la requête au format `application/json` .

Les paramètres requis sont les suivants :  `emailAddress`, `firstName`, `lastName, userRoleWorkspaces`. Le paramètre `userRoleWorkspaces` est un tableau d’objets contenant des attributs `accessRoleId` et `workspaceId`.

Le paramètre `userid` est une valeur de chaîne d’identifiant utilisateur unique utilisée pour la connexion de l’utilisateur et doit être formaté comme une adresse e-mail. Si elle n’est pas fournie dans la requête, la valeur de `userid` est définie par défaut sur la valeur fournie dans `emailAddress` paramètre .

Le paramètre de `apiOnly` booléen indique si l’utilisateur est un utilisateur [API uniquement](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Le paramètre `expiresAt` spécifie le moment auquel la connexion de l’utilisateur expire et est formaté au format W3C ISO-8601 (sans millisecondes). S’il n’est pas fourni dans la requête, il n’expire jamais. Le paramètre `reason` est une chaîne qui décrit la raison de l’invitation de l’utilisateur.

Le point d’entrée renvoie une valeur « true » en cas de réussite, sinon un message d’erreur est renvoyé.

```
POST /userservice/management/v1/users/invite.json
```

```
Content-Type: application/json
```

```json
{
  "emailAddress": "daenerys@housetargaryen.com",
  "firstName": "Daenerys",
  "lastName": "Targaryen",
  "expiresAt": "2020-12-31T23:59:59-05:00",
  "reason": "Keeper of dragons",
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "workspaceId": 0
    }
  ]
}
```

```
true
```

Vous trouverez ci-dessous un exemple d’invitation par e-mail « Bienvenue dans Marketo » envoyée au nouvel utilisateur. L’objet de l’e-mail est « Informations de connexion Marketo », l’expéditeur est l’adresse e-mail de l’utilisateur API uniquement associé au [service personnalisé de l’API REST](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) et le destinataire est tel que spécifié via les paramètres firstName, lastName et emailAddress.

![Inviter un utilisateur par e-mail](assets/invite-user-email.png)

L’utilisateur accepte l’invitation par e-mail en saisissant deux fois son mot de passe et en cliquant sur le bouton « CRÉER UN MOT DE PASSE ». Elle obtient ensuite un accès à Marketo pour la première fois.

## Mettre à jour l’utilisateur

La prise en charge des utilisateurs inclut la possibilité de mettre à jour les attributs utilisateur ou de supprimer un utilisateur. Seuls les utilisateurs ayant accepté leur invitation peuvent être mis à jour. Les attributs sont transmis en tant que paramètres au corps de la requête au format application/json .

### Mettre à jour les attributs utilisateur

Sur les [abonnements intégrés à Adobe IMS](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), ce point d’entrée prend uniquement en charge la mise à jour des attributs des [utilisateurs API uniquement](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Pour mettre à jour les attributs pour [utilisateurs standard](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), utilisez plutôt l’API [Adobe User Management](https://developer.adobe.com/umapi/).

Le point d’entrée [Mettre à jour les attributs utilisateur](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/updateUserAttributeUsingPOST) prend un seul paramètre de chemin d’accès `userid` et renvoie un seul enregistrement utilisateur. Le corps de la requête contient un ou plusieurs attributs utilisateur à mettre à jour : `emailAddress`, `firstName`, `lastName`, `expiresAt`.

```
POST /userservice/management/v1/users/{userid}/update.json
```

```
Content-Type: application/json
```

```json
{
  "firstName": "JAMIE",
  "lastName": "LANISTER",
  "expiresAt": "20211231T08:00:00.000t+0000"
}
```

```json
{
  "userid": "jamie@houselannister.com",
  "firstName": "JAMIE",
  "lastName": "LANISTER",
  "emailAddress": "jamie@houselannister.com",
  "optedIn": false,
  "failedLogins": 0,
  "failedDeviceCode": 0,
  "isLocked": false,
  "lockedReason": null,
  "id": 0,
  "apiOnly": false,
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "accessRoleName": "Admin",
      "workspaceId": 0,
      "workspaceName": "AllZones"
    },
    {
      "accessRoleId": 2,
      "accessRoleName":
      "Standard User",
      "workspaceId": 1008,
      "workspaceName": "World"
    }
  ],
  "expiresAt": "2021-12-31T08:00:00.000t+0000"
  "lastLoginAt": "2020-02-05T01:02:23.000t+0000"
}
```

#### Supprimer l’utilisateur

Sur les [abonnements intégrés à Adobe IMS](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), ce point d’entrée prend uniquement en charge la suppression des [utilisateurs utilisant uniquement l’API](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Pour supprimer [Utilisateurs standard](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), utilisez plutôt l’API [Adobe User Management](https://developer.adobe.com/umapi/).

Le point d’entrée [Supprimer l’utilisateur](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteUserUsingPOST) prend un seul paramètre de chemin d’accès `userid` et supprime l’utilisateur correspondant de l’instance. Il s’agit d’une suppression destructrice qui ne peut pas être annulée. En cas de réussite, un code d’état 200 est renvoyé, sinon un message d’erreur est renvoyé.

```
POST /userservice/management/v1/users/{userid}/delete.json
```

#### Supprimer utilisateur invité

Le point d’entrée [Supprimer l’utilisateur invité](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteInvitedUserUsingPOST) prend un seul paramètre de chemin d’accès `userid` et supprime l’utilisateur « en attente » correspondant de l’instance (l’utilisateur n’avait pas encore accepté son invitation). Il s’agit d’une suppression destructrice qui ne peut pas être annulée. En cas de réussite, un code d’état 200 est renvoyé, sinon un message d’erreur est renvoyé.

```
POST /userservice/management/v1/users/{userid}/invite/delete.json
```

## Mettre à jour les rôles

La mise à jour de la prise en charge des rôles inclut la possibilité d’ajouter et de supprimer des rôles. Les attributs sont transmis en tant que paramètres au corps de la requête au format application/json.

## Ajouter rôles

Le point d’entrée [Ajouter des rôles](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/addRolesUsingPOST) prend un seul paramètre de chemin d’accès `userid` et ajoute un ou plusieurs rôles utilisateur à l’utilisateur correspondant. Le corps de la requête contient une liste d’un ou de plusieurs objets contenant chacun une  `accessRoleId` et un attribut `workspaceId`. En cas de réussite, la liste complète des paires de `accessRoleId/workspaceId` pour l’utilisateur spécifié est renvoyée.

```
POST /userservice/management/v1/users/{userid}/roles/create.json
```

```
Content-Type: application/json
```

```json
[
  {
    "accessRoleId": 2,
    "workspaceId": 1008
  }
]
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  },
  {
    "accessRoleId": 2,
    "accessRoleName": "Standard User",
    "workspaceId": 1008,
    "workspaceName": "World"
  }
]
```

## Supprimer rôles

Le point d’entrée [Supprimer des rôles](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteRolesUsingPOST) prend un seul paramètre de chemin d’accès `userid` et supprime un ou plusieurs rôles utilisateur de l’utilisateur correspondant. Le corps de la requête contient une liste d’un ou de plusieurs objets contenant chacun une  `accessRoleId` et un attribut `workspaceId`. En cas de réussite, la liste restante des paires accessRoleId/workspaceId pour l’utilisateur spécifié est renvoyée.

```
POST /userservice/management/v1/users/{userid}/roles/delete.json
```

```
Content-Type: application/json
```

```json
[
  {
    "accessRoleId": 2,
    "workspaceId": 1008
  }
]
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  }
]
```
