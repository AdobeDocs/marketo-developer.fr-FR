---
title: API REST
feature: REST API
description: Découvrez comment utiliser l’API REST Marketo, configurer les utilisateurs d’API et LaunchPoint, afficher les quotas et les limites, vous authentifier avec l’en-tête d’autorisation et récupérer les prospects.
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
source-git-commit: 5f2dcb4864cdcd110ba9f199ef9c86dcee522335
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 1%

---

# API REST

Marketo propose une API REST qui permet l’exécution à distance de nombreuses fonctionnalités du système. De la création de programmes à l’importation de leads en bloc, il existe de nombreuses options permettant un contrôle affiné d’une instance Marketo.

Ces API se répartissent généralement en deux grandes catégories : [Base de données de lead](https://developer.adobe.com/marketo-apis/api/mapi/) et [Ressource](https://developer.adobe.com/marketo-apis/api/asset/). Les API de base de données de leads permettent de récupérer les enregistrements de personne Marketo et les types d’objet associés, tels que les opportunités et les entreprises, ainsi que d’interagir avec eux. Les API de ressources permettent l’interaction avec des dérivés marketing et des enregistrements liés aux workflows.

>[!NOTE]
>L’API SOAP sera abandonnée et ne sera plus disponible après le 31 janvier 2026. Tout nouveau développement doit être effectué avec l’API Marketo [REST](./rest-api.md) et les services existants doivent être migrés avant cette date pour éviter toute interruption de service. Si un service utilise l’API SOAP, consultez le [&#x200B; Guide de migration de l’API SOAP &#x200B;](../soap-api/migration.md) pour plus d’informations sur la migration.
>

>[!IMPORTANT]
>Consultez cette publication [Nation](https://nation.marketo.com/t5/product-blogs/rest-api-double-slash-deprecation/ba-p/358616) à propos de l’obsolescence de la double barre oblique dans les URL de passerelle d’API.
>

- **Quota quotidien :** les abonnements reçoivent 50 000 appels API par jour (qui sont réinitialisés tous les jours à 12 :00AM CST). Vous pouvez augmenter votre quota quotidien par l&#39;intermédiaire de votre gestionnaire de compte.
- **Limite de débit :** l’accès à l’API par instance est limité à 100 appels par 20 secondes.
- **Limite de simultanéité :**  Maximum de dix appels API simultanés.

La taille des appels standard est limitée à une longueur d’URI de 8 Ko et à une taille de corps de 1 Mo, bien que le corps puisse être de 10 Mo pour nos API en bloc. En cas d’erreur dans votre appel , l’API renvoie généralement un code d’état de 200, mais la réponse JSON contient un membre « success » avec une valeur de `false`, ainsi qu’un tableau d’erreurs dans le membre « errors ». En savoir plus sur les erreurs [ici](error-codes.md).

## Prise en main

Les étapes suivantes nécessitent des privilèges d’administrateur dans votre instance Marketo.

Lors de votre premier appel à Marketo, vous récupérez un enregistrement de prospect. Pour commencer à utiliser Marketo, vous devez obtenir des informations d’identification d’API afin d’effectuer des appels authentifiés vers votre instance . Connectez-vous à votre instance et accédez à **[!UICONTROL Admin]** -> **[!UICONTROL Utilisateurs et rôles]**.

![Utilisateurs et rôles d’administration](assets/admin-users-and-roles.png)

Cliquez sur l’onglet **[!UICONTROL Rôles]**, puis sur Nouveau rôle et attribuez au moins l’autorisation « Lead en lecture seule » (ou « Personne en lecture seule ») au rôle dans le groupe API Access . Veillez à lui donner un nom explicite et à cliquer sur **[!UICONTROL Créer]**.

![Nouveau rôle](assets/new-role.png)

Revenez maintenant à l’onglet [!UICONTROL Utilisateurs] et cliquez sur **[!UICONTROL Inviter un nouvel utilisateur]**. Donnez à votre utilisateur un nom explicite qui indique qu’il s’agit d’un utilisateur d’API, ainsi qu’une adresse électronique, puis cliquez sur **[!UICONTROL Suivant]**.

![Nouvelles informations sur l’utilisateur](assets/new-user-info.png)

Ensuite, cochez l’option [!UICONTROL API uniquement] et attribuez à votre utilisateur le rôle d’API que vous avez créé, puis cliquez sur **[!UICONTROL Suivant]**.

![Nouvelles autorisations d’utilisateurs](assets/new-user-permissions.png)

Pour terminer le processus de création d’utilisateurs, cliquez sur **[!UICONTROL Envoyer]**.

![Nouveau message utilisateur](assets/new-user-message.png)

Ensuite, accédez au menu [!UICONTROL Admin] et cliquez sur **[!UICONTROL LaunchPoint]**.

![Point de lancement](assets/admin-launchpoint.png)

Cliquez sur le menu **[!UICONTROL Nouveau]** et sélectionnez **[!UICONTROL Nouveau service]**. Attribuez un nom explicite à votre service et sélectionnez **[!UICONTROL Personnalisé]** dans le menu déroulant [!UICONTROL Service]. Donnez-lui une description, puis sélectionnez votre nouvel utilisateur dans le menu déroulant [!UICONTROL Utilisateur API uniquement] et cliquez sur **[!UICONTROL Créer]**.

![Nouveau service Launchpoint](assets/admin-launchpoint-new-service.png)

Cliquez sur **[!UICONTROL Afficher les détails]** pour que votre nouveau service accède à l’ID client et au secret client. Pour l’instant, vous pouvez cliquer sur le bouton **[!UICONTROL Obtenir le jeton]** pour générer un jeton d’accès valide pendant une heure. Enregistrez le jeton dans une note pour l’instant.

![Obtenir un jeton](assets/get-token.png)

Accédez ensuite au menu **[!UICONTROL Admin]**, puis à **[!UICONTROL Services web]**.

![Services web](assets/admin-web-services.png)

Recherchez le [!UICONTROL &#x200B; Point d’entrée &#x200B;] dans la zone API REST et enregistrez-le dans une note pour l’instant.

![&#x200B; Point d’entrée REST &#x200B;](assets/admin-web-services-rest-endpoint-1.png)

Lors d’appels aux méthodes de l’API REST, un jeton d’accès doit être inclus dans chaque appel pour que l’appel réussisse. Le jeton d’accès doit être envoyé en tant qu’en-tête HTTP.

```
Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int
```

>[!IMPORTANT]
>
>La prise en charge de l’authentification à l’aide du paramètre de requête **access_token** sera supprimée le 30 juin 2025. Si votre projet utilise un paramètre de requête pour transmettre le jeton d’accès, il doit être mis à jour afin d’utiliser l’en-tête **Authorization** dès que possible. Le nouveau développement doit utiliser exclusivement l’en-tête **Authorization**.

Ouvrez un nouvel onglet du navigateur et saisissez les informations suivantes en utilisant les informations appropriées pour appeler [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET)

```
<Your Endpoint URL>/rest/v1/leads.json?&filterType=email&filterValues=<Your Email Address>
```

Si vous n’avez pas d’enregistrement de prospect avec votre adresse e-mail dans votre base de données, remplacez-le par un enregistrement dont vous savez qu’il existe. Appuyez sur Entrée dans votre barre d’URL et vous devriez récupérer une réponse JSON ressemblant à ceci :

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

Chacun de vos utilisateurs d’API est signalé individuellement dans le rapport d’utilisation de l’API. La division de vos services web par utilisateur vous permet donc de rendre compte facilement de l’utilisation de chacune de vos intégrations. Si le nombre d’appels d’API à votre instance dépasse la limite et provoque l’échec des appels suivants, l’utilisation de cette pratique vous permet de tenir compte du volume de chacun de vos services et d’évaluer comment résoudre le problème. Consultez votre utilisation en accédant à **[!UICONTROL Admin]** -> **[!UICONTROL Intégration]** > **[!UICONTROL Services web]** et en cliquant sur le nombre d’appels au cours des sept derniers jours.
