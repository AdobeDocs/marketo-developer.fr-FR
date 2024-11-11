---
title: API REST
feature: REST API
description: Présentation de l’API REST
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
source-git-commit: ade3216f04c822de14dc0bbcbc08bfa3a4b17cb3
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 1%

---

# API REST

Marketo expose une API REST qui permet l’exécution à distance de nombreuses fonctionnalités du système. De la création de programmes à l’importation de pistes en bloc, il existe de nombreuses options qui permettent un contrôle précis d’une instance Marketo.

Ces API se répartissent généralement en deux grandes catégories : [Base de données de piste](https://developer.adobe.com/marketo-apis/api/mapi/) et [Ressource](https://developer.adobe.com/marketo-apis/api/asset/). Les API de base de données de piste permettent de récupérer les enregistrements de personne Marketo et les types d’objets associés, tels que les opportunités et les entreprises, ainsi que d’interagir avec ces derniers. Les API de ressources permettent l’interaction avec les documents marketing et les enregistrements liés aux workflows.

- **Quota quotidien :** Les abonnements reçoivent 50 000 appels d’API par jour (qui se réinitialise tous les jours à 00h00 heure du Pacifique). Vous pouvez augmenter votre quota quotidien par l’intermédiaire de votre gestionnaire de compte.
- **Limite de débit :** l’accès à l’API par instance est limité à 100 appels par 20 secondes.
- **Limite de simultanéité :**  Nombre maximum de dix appels API simultanés.

La taille des appels standard est limitée à une longueur d’URI de 8 Ko et une taille de corps de 1 Mo, bien que le corps puisse être de 10 Mo pour nos API en bloc. En cas d’erreur avec votre appel, l’API renvoie généralement toujours un code d’état de 200, mais la réponse JSON contient un membre &quot;success&quot; avec une valeur de `false` et un tableau d’erreurs dans le membre &quot;errors&quot;. Plus d&#39;informations sur les erreurs [ici](error-codes.md).

## Prise en main

Les étapes suivantes nécessitent des privilèges d’administrateur dans votre instance Marketo.

Pour votre premier appel à Marketo, vous récupérez un enregistrement de piste. Pour commencer à utiliser Marketo, vous devez obtenir les informations d’identification d’API pour effectuer des appels authentifiés vers votre instance. Connectez-vous à votre instance et accédez à **[!UICONTROL Admin]** -> **[!UICONTROL Utilisateurs et rôles]**.

![Utilisateurs et rôles d’administrateur](assets/admin-users-and-roles.png)

Cliquez sur l’onglet **[!UICONTROL Rôles]**, puis sur Nouveau rôle et affectez au moins l’autorisation &quot;Lecture seule&quot; (ou &quot;Lecture seule personne&quot;) au rôle dans le groupe API d’accès. Veillez à lui donner un nom explicite et cliquez sur **[!UICONTROL Créer]**.

![Nouveau rôle](assets/new-role.png)

Maintenant, revenez à l’onglet [!UICONTROL Utilisateurs] et cliquez sur **[!UICONTROL Inviter un nouvel utilisateur]**. Attribuez à votre utilisateur un nom explicite indiquant qu’il s’agit d’un utilisateur de l’API, ainsi qu’une adresse électronique, puis cliquez sur **[!UICONTROL Suivant]**.

![Informations sur le nouvel utilisateur](assets/new-user-info.png)

Ensuite, cochez l’option [!UICONTROL API Only] et attribuez à votre utilisateur le rôle API que vous avez créé, puis cliquez sur **[!UICONTROL Suivant]**.

![Nouvelles autorisations d’utilisateurs](assets/new-user-permissions.png)

Pour terminer le processus de création de l’utilisateur, cliquez sur **[!UICONTROL Envoyer]**.

![Message du nouvel utilisateur](assets/new-user-message.png)

Ensuite, accédez au menu [!UICONTROL Admin] et cliquez sur **[!UICONTROL LaunchPoint]**.

![Launchpoint](assets/admin-launchpoint.png)

Cliquez sur le menu **[!UICONTROL New]** et sélectionnez **[!UICONTROL New Service]**. Donnez un nom explicite à votre service et sélectionnez **[!UICONTROL Personnalisé]** dans le menu déroulant [!UICONTROL Service]. Donnez-lui une description, puis sélectionnez votre nouvel utilisateur dans le menu déroulant [!UICONTROL API Only User] et cliquez sur **[!UICONTROL Créer]**.

![Nouveau service Launchpoint](assets/admin-launchpoint-new-service.png)

Cliquez sur **[!UICONTROL Afficher les détails]** pour votre nouveau service afin d’accéder à l’ID client et au secret client. Pour l’instant, vous pouvez cliquer sur le bouton **[!UICONTROL Obtenir le jeton]** pour générer un jeton d’accès valide pendant une heure. Enregistrez le jeton dans une note pour l’instant.

![Obtenir un jeton](assets/get-token.png)

Ensuite, accédez au menu **[!UICONTROL Admin]**, puis à **[!UICONTROL Services web]**.

![Services web](assets/admin-web-services.png)

Recherchez le [!UICONTROL point d&#39;entrée] dans la zone API REST et enregistrez-le dans une note pour l&#39;instant.

![Point d’entrée REST](assets/admin-web-services-rest-endpoint-1.png)

Lors d’appels aux méthodes de l’API REST, un jeton d’accès doit être inclus dans chaque appel pour que l’appel réussisse. Le jeton d’accès doit être envoyé en tant qu’en-tête HTTP.

```
Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int
```

>[!IMPORTANT]
>
>La prise en charge de l’authentification à l’aide du paramètre de requête **access_token** a été supprimée le 30 juin 2025. Si votre projet utilise un paramètre de requête pour transmettre le jeton d’accès, il doit être mis à jour afin d’utiliser l’en-tête **Authorization** dès que possible. Le nouveau développement doit utiliser exclusivement l’en-tête **Authorization**.

Ouvrez un nouvel onglet du navigateur et saisissez les informations suivantes à l’aide des informations appropriées pour appeler [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET)

```
<Your Endpoint URL>/rest/v1/leads.json?&filterType=email&filterValues=<Your Email Address>
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

Chacun de vos utilisateurs d’API est signalé individuellement dans le rapport d’utilisation de l’API. Par conséquent, le partage de vos services web par utilisateur vous permet de comptabiliser facilement l’utilisation de chacune de vos intégrations. Si le nombre d’appels API à votre instance dépasse la limite et provoquent l’échec des appels suivants, cette pratique vous permet de comptabiliser le volume de chacun de vos services et vous permet d’évaluer la manière de résoudre le problème. Pour afficher votre utilisation, accédez à **[!UICONTROL Admin]** -> **[!UICONTROL Intégration]** > **[!UICONTROL Services web]** et cliquez sur le nombre d’appels au cours des sept derniers jours.
