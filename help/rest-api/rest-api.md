---
title: API REST
feature: REST API
description: Découvrez comment utiliser l’API REST Marketo, configurer les utilisateurs d’API et LaunchPoint, afficher les quotas et les limites, vous authentifier avec l’en-tête d’autorisation et récupérer les prospects.
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
TQID: https://experienceleague.adobe.com/GqhWI816wWX-2zf89wWj-GXpg9i615HRFVl2ljdYVj0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 803
ht-degree: 2%

---

# API REST

L’API REST Marketo permet d’accéder à distance à de nombreuses fonctionnalités système. Vous pouvez l’utiliser pour créer des programmes, importer des prospects en bloc et contrôler une instance Marketo à un niveau détaillé.

Les API REST se répartissent en deux grandes catégories :

- [Base de données de leads](https://developer.adobe.com/marketo-apis/api/mapi) les API récupèrent et interagissent avec les enregistrements de personne Marketo et les types d’objet associés, tels que les opportunités et les entreprises.
- Les API [Assets](https://developer.adobe.com/marketo-apis/api/asset) interagissent avec des dérivés marketing et des enregistrements liés aux workflows.

>[!NOTE]
>
>L’API SOAP sera abandonnée et ne sera plus disponible après le 31 juillet 2026. Tout nouveau développement doit être effectué avec l’API Marketo [REST](./rest-api.md) et les services existants doivent être migrés avant cette date pour éviter toute interruption de service. Si un service utilise l’API SOAP, consultez le [&#x200B; Guide de migration de l’API SOAP &#x200B;](../soap-api/migration.md) pour plus d’informations sur la migration.
>

>[!IMPORTANT]
>
>Consultez cette publication [Nation](https://nation.marketo.com/t5/product-blogs/rest-api-double-slash-deprecation/ba-p/358616) à propos de l’obsolescence de la double barre oblique dans les URL de passerelle d’API.
>

- **Quota quotidien :** 50 000 appels d’API par jour sont attribués à chaque abonnement. Le quota est réinitialisé tous les jours à 00:00 CST. Contactez votre gestionnaire de compte pour augmenter le quota quotidien.
- **Limite de débit :** chaque instance est limitée à 100 appels API par période de 20 secondes.
- **Limite de simultanéité :** chaque instance autorise un maximum de dix appels d’API simultanés.

Les appels d’API standard ont une longueur d’URI maximale de 8 Ko et une taille de corps maximale de 1 Mo. Les appels API en bloc prennent en charge une taille de corps maximale de 10 Mo.

Lorsqu’un appel contient une erreur, l’API renvoie généralement toujours le code d’état HTTP 200. La réponse JSON contient un membre de `success` avec une valeur de `false` et un tableau d’erreurs dans le membre de `errors`. Vous trouverez plus d’informations sur les erreurs [ici](error-codes.md).

## Prise en main

Vous avez besoin de privilèges d’administrateur dans votre instance Marketo pour effectuer les étapes suivantes. Ce workflow crée des informations d’identification d’API et les utilise pour récupérer un enregistrement de prospect.

Créez tout d’abord un utilisateur d’API et obtenez les informations d’identification des appels authentifiés. Connectez-vous à votre instance et accédez à **[!UICONTROL Admin]** > **[!UICONTROL Utilisateurs et rôles]**.

![Utilisateurs et rôles d’administration](assets/admin-users-and-roles.png)

Sélectionnez l’onglet **[!UICONTROL Rôles]**, puis sélectionnez Nouveau rôle. Attribuez au rôle au moins l’autorisation « Lead en lecture seule » (ou « Personne en lecture seule ») à partir du groupe d’API Access. Attribuez un nom explicite au rôle et sélectionnez **[!UICONTROL Créer]**.

![Nouveau rôle](assets/new-role.png)

Revenez à l’onglet [!UICONTROL &#x200B; Utilisateurs &#x200B;] et sélectionnez **[!UICONTROL Inviter un nouvel utilisateur]**. Saisissez un nom explicite qui identifie l’utilisateur en tant qu’utilisateur de l’API, saisissez une adresse e-mail, puis sélectionnez **[!UICONTROL Suivant]**.

![Nouvelles informations sur l’utilisateur](assets/new-user-info.png)

Sélectionnez l’option [!UICONTROL API uniquement], attribuez le rôle d’API que vous avez créé, puis sélectionnez **[!UICONTROL Suivant]**.

![Nouvelles autorisations d’utilisateurs](assets/new-user-permissions.png)

Sélectionnez **[!UICONTROL Envoyer]** pour créer l’utilisateur.

![Nouveau message utilisateur](assets/new-user-message.png)

Ensuite, accédez au menu [!UICONTROL Admin] et sélectionnez **[!UICONTROL LaunchPoint]**.

![Point de lancement](assets/admin-launchpoint.png)

Sélectionnez **[!UICONTROL Nouveau]** > **[!UICONTROL Nouveau service]**. Saisissez un nom et une description descriptifs, puis sélectionnez **[!UICONTROL Personnalisé]** dans le menu [!UICONTROL Service]. Sélectionnez votre nouvel utilisateur dans le menu [!UICONTROL Utilisateur API uniquement], puis sélectionnez **[!UICONTROL Créer]**.

![Nouveau service Launchpoint](assets/admin-launchpoint-new-service.png)

Sélectionnez **[!UICONTROL Afficher les détails]** pour que le nouveau service accède à l’ID client et au secret client. Sélectionnez **[!UICONTROL Obtenir le jeton]** pour générer un jeton d’accès valide pendant une heure. Enregistrez le jeton pour le premier appel API.

![Obtenir un jeton](assets/get-token.png)

Accédez à **[!UICONTROL Admin]** > **[!UICONTROL Services web]**.

![Services web](assets/admin-web-services.png)

Recherchez le [!UICONTROL Point d’entrée] dans la zone API REST et enregistrez-le pour le premier appel API.

![&#x200B; Point d’entrée REST &#x200B;](assets/admin-web-services-rest-endpoint-1.png)

Chaque appel de l’API REST doit inclure un jeton d’accès dans un en-tête HTTP.

```text
Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int
```

>[!IMPORTANT]
>
>La prise en charge de l’authentification à l’aide du paramètre de requête **access_token** sera supprimée le 30 juin 2025. Si votre projet utilise un paramètre de requête pour transmettre le jeton d’accès, il doit être mis à jour afin d’utiliser l’en-tête **Authorization** dès que possible. Le nouveau développement doit utiliser exclusivement l’en-tête **Authorization**.

Ouvrez un nouvel onglet du navigateur et saisissez l’URL suivante. Remplacez les espaces réservés par le point d’entrée et l’adresse e-mail de votre instance à appeler [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/getLeadsByFilterUsingGET).

```text
<Your Endpoint URL>/rest/v1/leads.json?&filterType=email&filterValues=<Your Email Address>
```

Si votre base de données ne contient pas d’enregistrement de prospect avec votre adresse électronique, utilisez l’adresse électronique d’un prospect existant. Envoyez l’URL pour recevoir une réponse JSON similaire à l’exemple suivant :

```json
{
    "requestId":"c493#1511ca2b184",
    "result":[
       {
           "id":1,
           "updatedAt":"2015-08-24T20:17:23Z",
           "lastName":"Elkington",
           "email":"developerfeedback@marketo.com",
           "createdAt":"2013-02-19T23:17:04Z",
           "firstName":"Kenneth"
        }
    ],
    "success":true
}
```

## Utilisation de l’API

Le rapport d’utilisation de l’API suit séparément chaque utilisateur de l’API. L’affectation d’un utilisateur distinct à chaque service web vous permet d’identifier l’utilisation de l’API de chaque intégration.

Si les appels dépassent votre limite d’instance et que les appels suivants échouent, utilisez le rapport pour identifier le volume d’appels de chaque service. Accédez à **[!UICONTROL Admin]** > **[!UICONTROL Intégration]** > **[!UICONTROL Services web]**, puis sélectionnez le nombre d’appels effectués au cours des sept derniers jours.

Pour les points d’entrée REST qui renvoient des statistiques d’utilisation et d’erreur quotidiennes et des 7 derniers jours, voir [Utilisation](usage.md).
