---
title: Authentification
feature: REST API
description: Authentification des utilisateurs Marketo pour l’utilisation de l’API.
exl-id: f89a8389-b50c-4e86-a9e4-6f6acfa98e7e
source-git-commit: 2bea5277a80ca99d98eb9b774f8cbea24cb6705f
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 0%

---

# Authentification

Les API REST Marketo sont authentifiées avec OAuth 2.0 à 2 jambes. Les ID de client et les secrets de client sont fournis par les services personnalisés que vous définissez. Chaque service personnalisé est détenu par un utilisateur API uniquement, qui dispose d’un ensemble de rôles et d’autorisations qui autorisent le service à effectuer des actions spécifiques. Un jeton d’accès est associé à un seul service personnalisé. L’expiration des jetons d’accès est indépendante des jetons associés à d’autres services personnalisés qui peuvent être présents dans une instance.

## Création d’un jeton d’accès

Les `Client ID` et `Client Secret` se trouvent dans le menu **[!UICONTROL Admin]** > **[!UICONTROL Intégration]** > **[!UICONTROL LaunchPoint]** en sélectionnant le service personnalisé, puis en cliquant sur **[!UICONTROL Afficher les détails]**.

![Obtenir les détails du service REST](assets/authentication-service-view-details.png)

![Informations d’identification Launchpoint](assets/admin-launchpoint-credentials.png)

Le `Identity URL` se trouve dans le menu **[!UICONTROL Admin]** > **[!UICONTROL Intégration]** > **[!UICONTROL Services web]** de la section API REST.

Créez un jeton d’accès à l’aide d’une demande de GET HTTP (ou de POST) de la manière suivante :

```
GET <Identity URL>/oauth/token?grant_type=client_credentials&client_id=<Client Id>&client_secret=<Client Secret>
```

Si votre requête était valide, vous recevez une réponse JSON similaire à ce qui suit :

```json
{
    "access_token": "cdf01657-110d-4155-99a7-f986b2ff13a0:int",
    "token_type": "bearer",
    "expires_in": 3599,
    "scope": "apis@acmeinc.com"
}
```

Définition de la réponse

- `access_token` - Jeton que vous transmettez avec les appels suivants pour vous authentifier auprès de l’instance cible.
- `token_type` - Méthode d’authentification OAuth.
- `expires_in` - Durée de vie restante du jeton actif en secondes (après laquelle il n’est pas valide). Lorsqu’un jeton d’accès est créé à l’origine, sa durée de vie est de 3 600 secondes ou une heure.
- `scope` - Utilisateur propriétaire du service personnalisé utilisé pour l’authentification.

## Utilisation d’un jeton d’accès

Lors d’appels aux méthodes de l’API REST, un jeton d’accès doit être inclus dans chaque appel pour que l’appel réussisse.

Vous pouvez utiliser deux méthodes pour inclure un jeton dans vos appels, sous la forme d’un en-tête HTTP ou d’un paramètre de chaîne de requête :

1. En-tête HTTP

   `Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int`

1. Paramètre de requête

   `access_token=cdf01657-110d-4155-99a7-f986b2ff13a0:int`

   >[!IMPORTANT]
   >
   >La prise en charge de l’authentification à l’aide du paramètre de requête **access_token** a été supprimée le 30 juin 2025. Si votre projet utilise un paramètre de requête pour transmettre le jeton d’accès, il doit être mis à jour afin d’utiliser l’en-tête **Authorization** dès que possible. Le nouveau développement doit utiliser exclusivement l’en-tête **Authorization**.

## Conseils et bonnes pratiques

La gestion de l’expiration des jetons d’accès est importante pour vous assurer que votre intégration fonctionne correctement et empêche les erreurs d’authentification inattendues de se produire pendant le fonctionnement normal. Lors de la conception de l’authentification pour votre intégration, veillez à stocker le jeton et la période d’expiration contenus dans la réponse Identity.

Avant tout appel REST, vous devez vérifier la validité du jeton en fonction de sa durée de vie restante. Si le jeton a expiré, renouvelez-le en appelant le point de terminaison [Identity](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET). Cela permet de s’assurer que votre appel REST n’échoue jamais en raison d’un jeton expiré. Vous pouvez ainsi gérer la latence de vos appels REST de manière prévisible, ce qui est essentiel pour les applications destinées aux utilisateurs finaux.

Si un jeton expiré est utilisé pour authentifier un appel REST, l’appel REST échoue et renvoie un code d’erreur 602. Si un jeton non valide est utilisé pour authentifier un appel REST, un code d’erreur 601 est renvoyé. Si l’un de ces codes est reçu, le client doit renouveler le jeton en appelant le point de terminaison Identity.

Si vous appelez le point de terminaison d’identité avant l’expiration de votre jeton, le même jeton et la durée de vie restante sont renvoyés dans la réponse.

N’oubliez pas que vos jetons d’accès sont détenus selon le service personnalisé et non selon l’utilisateur. Même si deux réponses d’identité peuvent être incluses dans la portée d’un même utilisateur, les jetons d’accès et les périodes d’expiration sont indépendants les uns des autres s’ils ont été créés avec des informations d’identification provenant de deux services différents. Gardez cela à l’esprit lorsque plusieurs jeux d’informations d’identification figurent dans la même application. L’identifiant du client peut s’avérer utile pour les gérer indépendamment.
