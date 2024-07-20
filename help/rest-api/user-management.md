---
title: Gestion des utilisateurs
feature: REST API
description: Exécutez des opérations CRUD sur les enregistrements d’utilisateur.
exl-id: 2a58f496-0fe6-4f7e-98ef-e9e5a017c2de
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 0%

---

# Gestion des utilisateurs

[Référence du point d’entrée User Management](https://developer.adobe.com/marketo-apis/api/user/)

Marketo fournit un ensemble de points de fin User Management qui vous permettent d’effectuer des opérations CRUD sur les enregistrements d’utilisateurs dans Marketo. Les utilisateurs sont créés en envoyant une invitation à un utilisateur, qui définit ensuite un mot de passe et accède pour la première fois à Marketo.

Contrairement aux autres API REST Marketo, lors de l’utilisation des API User Management :

- Vous devez utiliser la méthode d’en-tête HTTP pour envoyer le jeton d’accès à authentifier. Vous ne pouvez pas transmettre le jeton d’accès en tant que paramètre de chaîne de requête. Pour plus d&#39;informations sur l&#39;authentification, consultez [ici](authentication.md).
- Vous devez sélectionner une autorisation de rôle de deux groupes différents lors de la création du rôle utilisateur pour [Service personnalisé](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) pour l’API REST :
   1. Autorisation &quot;Accéder aux utilisateurs&quot; du groupe [Accès à l’administrateur](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
   1. &quot;Accès à l’API User Management&quot; à partir du groupe [Access API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
- Les corps de réponse ne contiennent pas l’attribut booléen &quot;success&quot; indiquant le succès ou l’échec d’un appel. Vous devez plutôt évaluer le code d’état de réponse HTTP. Si un appel réussit, un code d’état 200 est renvoyé. Si un appel échoue, un code d’état de niveau non 200 est renvoyé et le corps de la réponse contient le tableau standard &quot;errors&quot; avec le code d’erreur et un message d’erreur descriptif.
- Le format des chaînes datetime est &quot;aaaajj&#39;T&#39;HH:mm:ss.SSS&#39;t&#39;+|-hmm&quot;. Cela s’applique aux attributs suivants : createdAt, updatedAt, expiresAt.
- Les points de terminaison de l’API User Management ne comportent pas le préfixe &quot;/rest&quot; comme les autres points de terminaison.

## Requête

La prise en charge des requêtes pour la gestion des utilisateurs inclut la possibilité de récupérer tous les utilisateurs, rôles et espaces de travail. Vous pouvez également récupérer un seul enregistrement utilisateur par identifiant utilisateur ou un enregistrement rôle/espace de mots par identifiant utilisateur.

### Utilisateur par identifiant

Le point d’entrée [Get User by Id](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserUsingGET) utilise un seul paramètre de chemin `userid` et renvoie un seul enregistrement d’utilisateur pour un utilisateur qui a accepté son invitation.

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

### Utilisateur invité par identifiant

Le point de terminaison [Get Invited User by Id](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getInvitedUserUsingGET) utilise un seul paramètre de chemin `userid` et renvoie un enregistrement d’utilisateur unique pour un utilisateur &quot;en attente&quot; (n’a pas encore accepté son invitation).

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

### Rôles et espaces de travail par identifiant

Le point de terminaison [Get Roles and Workspaces by Id](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserRolesAndWorkspacesUsingGET) prend un seul paramètre de chemin `userid` et renvoie une liste d’enregistrements de rôle utilisateur et d’espace de travail. La réponse contient un tableau avec un objet contenant l’identifiant de rôle et d’espace de travail et le nom de l’utilisateur spécifié.

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

Le point d’entrée [Get Users](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUsersUsingGET) renvoie une liste de tous les enregistrements d’utilisateur. Le paramètre facultatif `pageSize` est un entier qui spécifie le nombre maximal d’entrées à renvoyer. La valeur par défaut est 20. 200 au maximum. Le paramètre facultatif `pageOffset` est un entier qui spécifie où commencer la récupération des entrées. Peut être utilisé avec `pageSize`. La valeur par défaut est 0.

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

Le point d’entrée [Get Workspaces](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getWorkspacesUsingGET) renvoie une liste de tous les enregistrements de l’espace de travail.

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

Sur les [abonnements intégrés à Adobe IMS](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), ce point de terminaison prend uniquement en charge l’invitation des [utilisateurs d’API uniquement](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Pour inviter [des utilisateurs standard](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), utilisez plutôt l’ [API de gestion des utilisateurs d’Adobe](https://developer.adobe.com/umapi/).

Le point de terminaison [Invite User](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/inviteUserUsingPOST) pour envoyer une invitation par courrier électronique &quot;Bienvenue dans Marketo&quot; à un nouvel utilisateur. Le corps du courrier électronique contient un lien &quot;Connexion à Marketo&quot; qui permet à l’utilisateur d’accéder à Marketo pour la première fois. Pour accepter l’invitation, le destinataire du courrier électronique clique sur le lien &quot;Se connecter à Marketo&quot;, crée son mot de passe et accède à Marketo. Tant que le processus d’acceptation n’est pas terminé, l’invitation est &quot;en attente&quot; et l’enregistrement de l’utilisateur ne peut pas être modifié. Une invitation en attente expire sept jours après son envoi. Vous trouverez plus d&#39;informations sur la gestion des utilisateurs [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users).

Les paramètres sont transmis dans le corps de la requête au format application/json .

Les paramètres suivants sont requis :  `emailAddress`, `firstName`, `lastName, userRoleWorkspaces`. Le paramètre `userRoleWorkspaces` est un tableau d’objets qui contient des attributs `accessRoleId` et `workspaceId`.

Le paramètre `userid` est une valeur de chaîne d’identifiant utilisateur unique utilisée à des fins de connexion utilisateur et doit être formatée en tant qu’adresse électronique. Si elle n’est pas fournie dans la requête, la valeur de `userid` correspond par défaut à la valeur fournie dans le paramètre `emailAddress`.

Le paramètre booléen `apiOnly` indique si l’utilisateur est un [utilisateur API-Only](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Le paramètre `expiresAt` indique le moment où la connexion de l’utilisateur expire et est formaté au format W3C ISO-8601 (sans millisecondes). S’il n’est pas fourni dans la requête, l’utilisateur n’expire jamais. Le paramètre `reason` est une chaîne qui décrit la raison de l’invitation de l’utilisateur.

Le point de terminaison renvoie la valeur &quot;true&quot; en cas de réussite, sinon un message d’erreur est renvoyé.

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

Vous trouverez ci-dessous un exemple d’invitation par courrier électronique &quot;Bienvenue dans Marketo&quot; envoyée au nouvel utilisateur. L’objet de l’email est &quot;Informations de connexion Marketo&quot;, l’expéditeur est l’adresse email de l’utilisateur API uniquement associé au [service personnalisé de l’API REST](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api), et le destinataire est tel que spécifié via les paramètres firstName, lastName et emailAddress.

![Invite User Email](assets/invite-user-email.png)

L’utilisateur accepte l’invitation par courrier électronique en saisissant son mot de passe à deux reprises et en cliquant sur le bouton &quot;CRÉER UN MOT DE PASSE&quot;. Elle obtient ensuite pour la première fois l’accès à Marketo.

## Mettre à jour l’utilisateur

La mise à jour de la prise en charge des utilisateurs inclut la possibilité de mettre à jour les attributs utilisateur ou de supprimer un utilisateur. Seuls les utilisateurs qui ont accepté leur invitation peuvent être mis à jour. Les attributs sont transmis en tant que paramètres dans le corps de la requête au format application/json .

### Mise à jour des attributs utilisateur

Sur les [abonnements intégrés à Adobe IMS](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), ce point de terminaison prend uniquement en charge la mise à jour des attributs de [ utilisateurs API uniquement](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Pour mettre à jour les attributs de [utilisateurs standard](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), utilisez plutôt l’ [API de gestion des utilisateurs d’Adobe](https://developer.adobe.com/umapi/).

Le point d’entrée [Mettre à jour les attributs utilisateur](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/updateUserAttributeUsingPOST) utilise un seul paramètre de chemin `userid` et renvoie un seul enregistrement d’utilisateur. Le corps de la requête contient un ou plusieurs attributs utilisateur à mettre à jour : `emailAddress`, `firstName`, `lastName`, `expiresAt`.

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

Sur les [abonnements intégrés à Adobe IMS](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), ce point de terminaison prend uniquement en charge la suppression des [utilisateurs d’API uniquement](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Pour supprimer les [utilisateurs standard](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), utilisez plutôt l’ [API de gestion des utilisateurs d’Adobe](https://developer.adobe.com/umapi/).

Le point d’entrée [Delete User](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteUserUsingPOST) utilise un seul paramètre de chemin `userid` et supprime l’utilisateur correspondant de l’instance. Il s’agit d’une suppression destructrice qui ne peut pas être annulée. En cas de réussite, un code d’état 200 est renvoyé, sinon un message d’erreur est renvoyé.

```
POST /userservice/management/v1/users/{userid}/delete.json
```

#### Supprimer un utilisateur invité

Le point d’entrée [Supprimer l’utilisateur invité](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteInvitedUserUsingPOST) utilise un seul paramètre de chemin d’accès `userid` et supprime l’utilisateur &quot;en attente&quot; correspondant de l’instance (l’utilisateur n’avait pas encore accepté son invitation). Il s’agit d’une suppression destructrice qui ne peut pas être annulée. En cas de réussite, un code d’état 200 est renvoyé, sinon un message d’erreur est renvoyé.

```
POST /userservice/management/v1/users/{userid}/invite/delete.json
```

## Mise à jour des rôles

La mise à jour de la prise en charge des rôles inclut la possibilité d’ajouter et de supprimer des rôles. Les attributs sont transmis en tant que paramètres au corps de la requête au format application/json.

## Ajouter des rôles

Le point d’entrée [Ajouter des rôles](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/addRolesUsingPOST) utilise un seul paramètre de chemin `userid` et ajoute un ou plusieurs rôles d’utilisateur à l’utilisateur correspondant. Le corps de la requête contient une liste d’un ou de plusieurs objets contenant chacun une  `accessRoleId` et un attribut `workspaceId`. En cas de réussite, la liste complète de `accessRoleId/workspaceId` paires pour l’utilisateur spécifié est renvoyée.

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

## Suppression de rôles

Le point d’entrée [Supprimer les rôles](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteRolesUsingPOST) utilise un seul paramètre de chemin d’accès `userid` et supprime un ou plusieurs rôles d’utilisateur de l’utilisateur correspondant. Le corps de la requête contient une liste d’un ou de plusieurs objets contenant chacun une  `accessRoleId` et un attribut `workspaceId`. En cas de succès, la liste restante des paires accessRoleId/workspaceId pour l’utilisateur spécifié est renvoyée.

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
