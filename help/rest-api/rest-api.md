---
title: "API REST"
feature: REST API
description: "Présentation de l’API REST"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 1%

---


# API REST

Marketo expose une API REST qui permet l’exécution à distance de nombreuses fonctionnalités du système. De la création de programmes à l’importation de pistes en bloc, il existe de nombreuses options qui permettent un contrôle précis d’une instance Marketo.

Ces API se divisent généralement en deux grandes catégories : [Base de données de piste](https://developer.adobe.com/marketo-apis/api/mapi/), et [Ressource](https://developer.adobe.com/marketo-apis/api/asset/). Les API de base de données de piste permettent de récupérer les enregistrements de personne Marketo et les types d’objets associés, tels que les opportunités et les entreprises, ainsi que d’interagir avec ces derniers. Les API de ressources permettent l’interaction avec les documents marketing et les enregistrements liés aux workflows.

- **Quota quotidien :** Les abonnements se voient attribuer 50 000 appels d’API par jour (qui se réinitialisent tous les jours à 00h00 du matin de l’après-midi). Vous pouvez augmenter votre quota quotidien par l’intermédiaire de votre gestionnaire de compte.
- **Limite de taux :** Accès à l’API par instance limité à 100 appels par 20 secondes.
- **Limite de simultanéité :**  Nombre maximum de dix appels API simultanés.

La taille des appels standard est limitée à une longueur d’URI de 8 Ko et une taille de corps de 1 Mo, bien que le corps puisse être de 10 Mo pour nos API en bloc. En cas d’erreur lors de l’appel, l’API renvoie généralement toujours un code d’état de 200, mais la réponse JSON contient un membre &quot;success&quot; avec la valeur de `false`et un tableau d’erreurs dans le membre &quot;errors&quot;. Plus sur les erreurs [here](error-codes.md).

## Prise en main

Les étapes suivantes nécessitent des privilèges d’administrateur dans votre instance Marketo.

Pour votre premier appel à Marketo, vous récupérerez un enregistrement de piste. Pour commencer à utiliser Marketo, vous devez obtenir les informations d’identification d’API pour effectuer des appels authentifiés vers votre instance. Connectez-vous à votre instance et accédez au **[!UICONTROL Administration]** -> **[!UICONTROL Utilisateurs et rôles]**.

![Utilisateurs et rôles d’administration](assets/admin-users-and-roles.png)

Cliquez sur le bouton [!UICONTROL Rôles] puis sélectionnez Nouveau rôle et attribuez au moins l’autorisation &quot;Lecture seule&quot; (ou &quot;Lecture seule personne&quot;) au rôle dans le groupe API d’accès. Veillez à lui donner un nom explicite, puis cliquez sur [!UICONTROL Créer].

![Nouveau rôle](assets/new-role.png)

Revenez à l’onglet Utilisateurs et cliquez sur Inviter un nouvel utilisateur. Attribuez à votre utilisateur un nom explicite indiquant qu’il s’agit d’un utilisateur de l’API, ainsi qu’une adresse électronique, puis cliquez sur **[!UICONTROL Suivant]**.

![Informations sur le nouvel utilisateur](assets/new-user-info.png)

Cochez ensuite l’option API uniquement et attribuez à votre utilisateur le rôle API que vous avez créé, puis cliquez sur **[!UICONTROL Suivant]**.

![Nouvelles autorisations d’utilisateurs](assets/new-user-permissions.png)

Pour terminer la création de l’utilisateur, cliquez sur **[!UICONTROL Envoyer]**.

![Nouveau message d’utilisateur](assets/new-user-message.png)

Ensuite, accédez au menu Admin et cliquez sur **[!UICONTROL LaunchPoint]**.

![Launchpoint](assets/admin-launchpoint.png)

Cliquez sur le menu Nouveau et sélectionnez [!UICONTROL Nouveau service]. Donnez un nom explicite à votre service et sélectionnez &quot;Personnalisé&quot; dans le menu déroulant Service . Donnez-lui une description, puis sélectionnez votre nouvel utilisateur dans le menu déroulant API Only User et cliquez sur [!UICONTROL Créer].

![Nouveau service Launchpoint](assets/admin-launchpoint-new-service.png)

Cliquez sur Afficher les détails pour votre nouveau service afin d’accéder à l’identifiant du client et au secret du client. Pour l’instant, vous pouvez cliquer sur le bouton [!UICONTROL Obtenir un jeton] pour générer un jeton d’accès valide pendant une heure. Enregistrez le jeton dans une note pour l’instant.

![Obtenir un jeton](assets/get-token.png)

Accédez ensuite au menu Admin , puis à **[!UICONTROL Services web]**.

![Services web](assets/admin-web-services.png)

Recherchez le point de terminaison dans la zone API REST et enregistrez-le dans une note pour l’instant.

![Point de terminaison REST](assets/admin-web-services-rest-endpoint-1.png)

Ouvrez un nouvel onglet du navigateur et saisissez les informations suivantes à l’aide des informations appropriées pour appeler . [Obtenir des pistes par type de filtre](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET):

```
<Your Endpoint URL>/rest/v1/leads.json?access_token=<Your Access Token>&filterType=email&filterValues=<Your Email Address>
```

Si vous ne disposez pas d’un enregistrement de piste avec votre adresse électronique dans votre base de données, remplacez-le par un enregistrement que vous connaissez. Appuyez sur Entrée dans votre barre d’URL, et vous devez récupérer une réponse JSON semblable à celle-ci :

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

Chacun de vos utilisateurs d’API est signalé individuellement dans le rapport d’utilisation de l’API. Par conséquent, le partage de vos services web par utilisateur vous permet de comptabiliser facilement l’utilisation de chacune de vos intégrations. Si le nombre d’appels API à votre instance dépasse la limite et provoquent l’échec des appels suivants, cette pratique vous permet de comptabiliser le volume de chacun de vos services et vous permet d’évaluer la manière de résoudre le problème. Consultez votre utilisation en accédant à **[!UICONTROL Administration]** -> **[!UICONTROL Intégration]** > **[!UICONTROL Services web]** et cliquer sur le nombre d’appels au cours des sept derniers jours.
